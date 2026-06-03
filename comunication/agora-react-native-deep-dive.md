# 🎙 Agora SDK trong React Native — Deep Dive toàn diện

> **🎯 Mục đích:** Hiểu sâu `react-native-agora` từ kiến trúc đến triển khai thực tế —
> dùng cho phỏng vấn tại Zenify (Contact Center VoIP) và dự án **recbook** (recording app).
>
> **📦 Package:** `react-native-agora` v4.x (wrapper chính thức của Agora cho React Native)
>
> **💡 Ghi chú:** Toàn bộ giải thích bằng tiếng Việt, thuật ngữ kỹ thuật giữ nguyên tiếng Anh.

---

## 📋 Mục lục

```
┌─────────────────────────────────────────────────────────────┐
│  1. Kiến trúc tổng quan                                     │
│  2. Vòng đời RtcEngine (lifecycle)                          │
│  3. Token authentication — luồng bảo mật                   │
│  4. Channel configuration — COMMUNICATION vs LIVE          │
│  5. Event system — toàn bộ callback quan trọng             │
│  6. Audio management — profile, routing, echo cancel       │
│  7. Xử lý mạng yếu & reconnection                         │
│  8. Background mode — iOS & Android                        │
│  9. Server-side Cloud Recording (quan trọng cho recbook)   │
│  10. CallKit integration — Agora + iOS native              │
│  11. Pattern React Hooks — cách tổ chức đúng              │
│  12. Bẫy & lỗi hay gặp                                     │
│  13. Câu hỏi phỏng vấn chuyên sâu về Agora               │
└─────────────────────────────────────────────────────────────┘
```

---

# 1. Kiến trúc tổng quan

## Sơ đồ lớp — react-native-agora nằm ở đâu

```
┌──────────────────────────────────────────────────────────────┐
│                    📝 JavaScript / TypeScript                 │
│                                                              │
│   const engine = createAgoraRtcEngine()                      │
│   engine.joinChannel(token, channelId, uid, options)         │
│   engine.on('userJoined', (uid) => { ... })                  │
└─────────────────────┬────────────────────────────────────────┘
                      │  JSI / Old Bridge
                      ▼
┌──────────────────────────────────────────────────────────────┐
│             📦 react-native-agora (Native Module)            │
│    TypeScript API ←→ C++/ObjC++ bridge ←→ Kotlin/Swift      │
└──────────┬───────────────────────────────────┬───────────────┘
           │                                   │
           ▼                                   ▼
┌──────────────────────┐           ┌──────────────────────────┐
│  🍎 iOS Native        │           │  🤖 Android Native        │
│                      │           │                          │
│  AgoraRtcEngineKit   │           │  RtcEngine (Java/Kotlin) │
│  (Objective-C++)     │           │  IRtcEngineEventHandler  │
│                      │           │                          │
│  AVAudioSession      │           │  AudioManager            │
│  CallKit (nếu dùng)  │           │  Telecom API (nếu dùng)  │
└──────────────────────┘           └──────────────────────────┘
           │                                   │
           └──────────────┬────────────────────┘
                          ▼
┌──────────────────────────────────────────────────────────────┐
│                  🌐 Agora Media Server (Cloud)               │
│                                                              │
│   SD-RTN (Software Defined Real-time Network)               │
│   ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│   │ Edge Node│  │ Edge Node│  │ Edge Node│  │ Edge Node│  │
│   │ Singapore│  │  Tokyo   │  │  Sydney  │  │  Mumbai  │  │
│   └──────────┘  └──────────┘  └──────────┘  └──────────┘  │
│                                                              │
│   Dùng cho: audio routing, recording, transcoding           │
└──────────────────────────────────────────────────────────────┘
```

## SD-RTN là gì — tại sao Agora tốt cho VoIP

**SD-RTN (Software Defined Real-time Network)** là mạng lưới edge node riêng của Agora, phủ khắp toàn cầu. Khi gọi điện:
- Thiết bị kết nối đến edge node **gần nhất** (~20–50ms latency)
- Agora định tuyến gói tin qua mạng riêng của họ thay vì public internet
- Kết quả: độ trễ end-to-end thường dưới **300ms** cho VoIP, so sánh với WebRTC thuần qua STUN/TURN có thể lên đến 500–1000ms

---

# 2. Vòng đời RtcEngine — Lifecycle đúng chuẩn

## Sơ đồ lifecycle toàn bộ

```
App khởi động
      │
      ▼
┌─────────────────────────────────────────────────────────┐
│  KHỞI TẠO                                               │
│                                                         │
│  const engine = createAgoraRtcEngine()                  │
│  engine.initialize({                                    │
│    appId: APP_ID,         ← lấy từ Keychain/Keystore   │
│    channelProfile:        ← COMMUNICATION hoặc LIVE    │
│      ChannelProfileType.ChannelProfileCommunication,   │
│    audioScenario:                                       │
│      AudioScenarioType.AudioScenarioDefault,           │
│    logConfig: { level: LogLevel.LogLevelWarn }          │
│  })                                                     │
│  engine.enableAudio()     ← bật audio TRƯỚC khi join   │
│  engine.registerEventHandler(eventHandler)  ← đăng ký  │
└─────────────────────┬───────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────┐
│  CHỜ (IDLE)                                             │
│  engine chạy nhưng chưa tham gia channel nào            │
└─────────────────────┬───────────────────────────────────┘
                      │ joinChannel()
                      ▼
┌─────────────────────────────────────────────────────────┐
│  ĐANG JOIN                                              │
│                                                         │
│  engine.joinChannel(                                    │
│    token,       ← từ Token Server của bạn              │
│    channelName, ← ID phòng / ID cuộc gọi               │
│    uid,         ← 0 = Agora tự assign, hoặc fixed UID  │
│    options: {                                           │
│      clientRoleType: ClientRoleType.ClientRoleBroadcaster│
│      autoSubscribeAudio: true,                         │
│      publishMicrophoneTrack: true,                     │
│    }                                                    │
│  )                                                      │
└─────────────────────┬───────────────────────────────────┘
                      │ onJoinChannelSuccess()
                      ▼
┌─────────────────────────────────────────────────────────┐
│  ĐANG TRONG CUỘC GỌI (IN-CALL)                         │
│                                                         │
│  ● muteLocalAudioStream(true/false)  → tắt/bật mic     │
│  ● adjustRecordingSignalVolume(100)  → điều chỉnh volume│
│  ● setEnableSpeakerphone(true/false) → loa ngoài       │
│  ● renewToken(newToken)              → gia hạn token   │
└─────────────────────┬───────────────────────────────────┘
                      │ leaveChannel() / đối phương hang up
                      ▼
┌─────────────────────────────────────────────────────────┐
│  RỜI CHANNEL                                            │
│                                                         │
│  engine.leaveChannel()     ← bắt buộc gọi              │
│  // onLeaveChannel() callback được trigger              │
└─────────────────────┬───────────────────────────────────┘
                      │ app đóng / không cần VoIP nữa
                      ▼
┌─────────────────────────────────────────────────────────┐
│  GIẢI PHÓNG                                             │
│                                                         │
│  engine.release()          ← QUAN TRỌNG: giải phóng    │
│                               tài nguyên audio phần cứng│
│  // sau release(), engine không dùng được nữa          │
└─────────────────────────────────────────────────────────┘
```

## Code pattern đúng chuẩn — khởi tạo và dọn dẹp

```typescript
// hooks/useAgoraEngine.ts
import {
  createAgoraRtcEngine,
  IRtcEngine,
  ChannelProfileType,
  AudioScenarioType,
} from 'react-native-agora';
import { useEffect, useRef } from 'react';
import { getAgoraAppId } from '../services/SecureStorage'; // từ Keychain

export function useAgoraEngine() {
  const engineRef = useRef<IRtcEngine | null>(null);

  useEffect(() => {
    let mounted = true;

    async function initEngine() {
      const appId = await getAgoraAppId(); // KHÔNG hardcode appId
      if (!mounted) return;

      const engine = createAgoraRtcEngine();
      engine.initialize({
        appId,
        channelProfile: ChannelProfileType.ChannelProfileCommunication,
        // AudioScenarioType.AudioScenarioVoiceChatroom → tối ưu 1-1 VoIP
        audioScenario: AudioScenarioType.AudioScenarioVoiceChatroom,
        logConfig: {
          level: __DEV__
            ? LogLevel.LogLevelInfo
            : LogLevel.LogLevelWarn, // ít log hơn ở production
        },
      });
      engine.enableAudio();
      engineRef.current = engine;
    }

    initEngine();

    return () => {
      mounted = false;
      // ⚠️ QUAN TRỌNG: gọi release() khi cleanup
      engineRef.current?.release();
      engineRef.current = null;
    };
  }, []);

  return engineRef;
}
```

> ⚠️ **Bẫy phổ biến:** Không gọi `engine.release()` trong cleanup → engine vẫn giữ micro, loa, audio session của iOS. Lần gọi tiếp theo sẽ conflict.

---

# 3. Token Authentication — Luồng bảo mật

## Tại sao cần Token Server (không dùng App ID trực tiếp)

```
❌ SAI — App ID trong code (development only)
   JS code → engine.joinChannel(null, channelName, uid)
   Vấn đề: App ID trong bundle → ai cũng có thể join bất kỳ channel nào

✅ ĐÚNG — Token từ server của bạn
   JS code → POST /agora/token → Token Server → Agora Token SDK
           ← { token: "006...xyz", expiredTs: 1720000000 }
           → engine.joinChannel(token, channelName, uid)
```

## Luồng token đầy đủ

```
┌─────────────────────────────────────────────────────────────────┐
│                    Token Authentication Flow                      │
└─────────────────────────────────────────────────────────────────┘

📱 App (agent login)
  │
  │  1. Đăng nhập app, lấy JWT của agent
  │
  ▼
🖥️ Backend Contact Center
  │
  │  2. Nhận request: POST /agora/token
  │     body: { channelName, uid, role: 'publisher' }
  │     → Xác thực JWT agent trước
  │
  ▼
🔑 Agora Token Server (BE tự host hoặc Agora Cloud)
  │
  │  3. Tạo RTC Token bằng:
  │     - App ID (secret)
  │     - App Certificate (secret)  ← KHÔNG BAO GIỜ để trong app
  │     - channelName + uid + role
  │     - expiredTs (e.g. 3600s = 1 tiếng)
  │
  ▼
📱 App nhận token → engine.joinChannel(token, channelName, uid)

         ⏰ Sau ~50 giây trước khi hết hạn:
         └── onTokenPrivilegeWillExpire() callback trigger
             → app gọi POST /agora/token một lần nữa
             → engine.renewToken(newToken)   ← KHÔNG join lại
```

## Code xử lý token expiry — không ngắt cuộc gọi

```typescript
// Trong EventHandler của Agora
const eventHandler: IRtcEngineEventHandler = {
  onTokenPrivilegeWillExpire: async (connection, token) => {
    try {
      // Lấy token mới — KHÔNG gọi leaveChannel + joinChannel lại
      const newToken = await fetchNewAgoraToken(channelName, localUid);
      engine.renewToken(newToken);
      // ✅ Cuộc gọi tiếp tục không bị gián đoạn
    } catch (err) {
      // Nếu fetch thất bại → log và handle gracefully
      console.error('[Agora] Token renewal failed:', err);
    }
  },

  onTokenPrivilegeDidExpire: (connection) => {
    // Token đã hết hạn hoàn toàn → phải join lại (cuộc gọi bị ngắt)
    // Hiển thị thông báo cho agent
    store.dispatch(callActions.setError('Phiên hết hạn, vui lòng gọi lại'));
    engine.leaveChannel();
  },
};
```

---

# 4. Channel Configuration — Chọn đúng profile

## So sánh 2 channel profile

```
┌──────────────────────────────────────────────────────────────────┐
│              ChannelProfileType — Chọn loại nào?                 │
├───────────────────────────┬──────────────────────────────────────┤
│  COMMUNICATION            │  LIVE_BROADCASTING                   │
│  (1-1 hoặc group nhỏ)    │  (streaming, audience lớn)           │
├───────────────────────────┼──────────────────────────────────────┤
│ Tất cả user = broadcaster │ Có role: broadcaster / audience      │
│ Mọi người đều publish mic │ Audience không publish               │
│ Tối ưu cho voice call     │ Tối ưu cho livestream                │
│ Latency: ~400ms           │ Latency: có thể cao hơn              │
│                           │                                      │
│ ✅ DÙNG CHO:              │ ✅ DÙNG CHO:                         │
│ - VoIP call 1-1           │ - Livestream bán hàng                │
│ - Group call < 10 người   │ - Webinar, class online              │
│ - Contact Center agent    │ - Recbook (host vs viewer)          │
└───────────────────────────┴──────────────────────────────────────┘
```

## AudioScenario — chọn đúng kịch bản âm thanh

```typescript
// Zenify Contact Center → VoiceChat (1-1 VoIP)
audioScenario: AudioScenarioType.AudioScenarioVoiceChatroom
// → Agora tự động: AEC (echo cancel), ANS (noise suppression),
//   AGC (auto gain control), tối ưu speech clarity

// Recbook (recording) → GameStreaming hoặc Chatroom
audioScenario: AudioScenarioType.AudioScenarioGameStreaming
// → Ít xử lý âm thanh hơn, giữ nguyên âm thanh đầu vào
//   Phù hợp khi muốn record âm thanh trung thực

// Hội thảo, meeting nhiều người
audioScenario: AudioScenarioType.AudioScenarioMeeting
// → Tối ưu cho nhiều người nói xen kẽ
```

## AudioProfile — codec và chất lượng

```typescript
engine.setAudioProfile(
  AudioProfileType.AudioProfileDefault,    // 32kbps mono (đủ cho VoIP)
  AudioScenarioType.AudioScenarioVoiceChatroom
)

// Các option:
// AudioProfileSpeechStandard  → 18kbps, speech only, tối ưu bandwidth thấp
// AudioProfileMusicStandard   → 48kbps, âm nhạc OK
// AudioProfileMusicHighQuality → 128kbps, stereo (dùng cho recbook nếu cần HQ)
```

---

# 5. Event System — Toàn bộ callback quan trọng

## Sơ đồ event flow

```
Agora Native SDK
  │
  │  (callbacks từ native thread → JS thread)
  ▼
IRtcEngineEventHandler
  │
  ├── 🔗 Connection Events
  │   ├── onJoinChannelSuccess(connection, elapsed)    ← join thành công
  │   ├── onLeaveChannel(connection, stats)            ← rời channel
  │   ├── onConnectionStateChanged(connection, state, reason)
  │   │   States: DISCONNECTED → CONNECTING → CONNECTED → RECONNECTING → FAILED
  │   └── onConnectionLost(connection)                ← mất kết nối hẳn
  │
  ├── 👥 User Events
  │   ├── onUserJoined(connection, remoteUid, elapsed) ← user khác vào
  │   ├── onUserOffline(connection, remoteUid, reason) ← user rời (normal/timeout)
  │   └── onActiveSpeaker(connection, uid)            ← ai đang nói
  │
  ├── 🔊 Audio Events
  │   ├── onAudioVolumeIndication(connection, speakers, speakerNumber, totalVolume)
  │   │   → dùng để vẽ "sóng âm" khi ai đó đang nói
  │   ├── onLocalAudioStateChanged(connection, state, error)
  │   ├── onRemoteAudioStateChanged(connection, uid, state, reason, elapsed)
  │   └── onAudioRoutingChanged(routing)  ← loa ngoài / tai nghe / BT thay đổi
  │
  ├── 📊 Network/Stats Events
  │   ├── onNetworkQuality(connection, uid, txQuality, rxQuality)
  │   │   Quality: UNKNOWN / EXCELLENT / GOOD / POOR / BAD / VERY_BAD / DOWN
  │   ├── onRtcStats(connection, stats)   ← stats tổng (mỗi 2 giây)
  │   └── onLocalAudioStats(connection, stats) ← bitrate, RTT, packet loss
  │
  └── 🔑 Token Events
      ├── onTokenPrivilegeWillExpire(connection, token)  ← ~30s trước khi hết hạn
      └── onTokenPrivilegeDidExpire(connection)          ← hết hạn hoàn toàn
```

## Cách đăng ký event handler đúng — tránh memory leak

```typescript
// ❌ SAI — tạo object mới mỗi render → không unregister được
useEffect(() => {
  engine.registerEventHandler({
    onUserJoined: (connection, uid) => setRemoteUid(uid),
  });
}, []);

// ✅ ĐÚNG — dùng ref để giữ cùng một handler reference
const eventHandlerRef = useRef<IRtcEngineEventHandler>({
  onJoinChannelSuccess: (connection, elapsed) => {
    console.log('[Agora] Joined channel, elapsed:', elapsed, 'ms');
    store.dispatch(callActions.setStatus('in-call'));
  },

  onUserJoined: (connection, uid, elapsed) => {
    console.log('[Agora] Remote user joined:', uid);
    store.dispatch(callActions.setRemoteUid(uid));
  },

  onUserOffline: (connection, uid, reason) => {
    // reason: USER_OFFLINE_QUIT (chủ động) vs USER_OFFLINE_DROPPED (mạng)
    console.log('[Agora] Remote user left:', uid, 'reason:', reason);
    store.dispatch(callActions.handleRemoteHangUp());
  },

  onConnectionStateChanged: (connection, state, reason) => {
    console.log('[Agora] Connection state:', state, 'reason:', reason);
    if (state === ConnectionStateType.ConnectionStateFailed) {
      store.dispatch(callActions.setError('Mất kết nối'));
    }
  },

  onNetworkQuality: (connection, uid, txQuality, rxQuality) => {
    // uid = 0 → local user
    if (uid === 0) {
      store.dispatch(callActions.setNetworkQuality({ tx: txQuality, rx: rxQuality }));
    }
  },

  onTokenPrivilegeWillExpire: async (connection, token) => {
    const newToken = await AgoraTokenService.refresh(channelName, localUid);
    engine.renewToken(newToken);
  },
});

useEffect(() => {
  const handler = eventHandlerRef.current;
  engine.registerEventHandler(handler);
  return () => {
    engine.unregisterEventHandler(handler); // ✅ cleanup đúng cách
  };
}, [engine]);
```

---

# 6. Audio Management — Routing, Echo Cancel, Volume

## Sơ đồ audio routing trên iOS

```
Micro (hardware)
    │
    ▼
AVAudioSession (iOS Audio Layer)
    │  category: .playAndRecord
    │  mode: .voiceChat          ← bật echo cancel phần cứng
    │  options: .allowBluetooth  ← cho phép AirPods / tai nghe BT
    │
    ▼
Agora Audio Module (native)
    │  AEC  → Acoustic Echo Cancellation (khử tiếng vọng)
    │  ANS  → Ambient Noise Suppression (khử tiếng ồn môi trường)
    │  AGC  → Auto Gain Control (tự điều chỉnh âm lượng)
    │
    ▼
Audio Output Routing
    ├── Receiver (loa nhỏ, nghe điện thoại bình thường)   ← mặc định
    ├── Speaker (loa ngoài, handsfree)
    ├── Wired Headset (tai nghe có dây)
    └── Bluetooth Headset (AirPods, tai nghe BT)
```

## Code quản lý audio routing

```typescript
// Bật loa ngoài (handsfree)
engine.setEnableSpeakerphone(true);

// Tắt loa ngoài, quay về receiver (nghe điện thoại bình thường)
engine.setEnableSpeakerphone(false);

// Lấy trạng thái hiện tại của speaker
engine.isSpeakerphoneEnabled().then((enabled) => {
  console.log('Speaker enabled:', enabled);
});

// ⚠️ QUAN TRỌNG — iOS: cấu hình AVAudioSession trước khi join
// react-native-agora v4.x tự xử lý AVAudioSession khi initialize,
// nhưng nếu bạn dùng CallKit thì phải cấu hình thủ công qua native:

// Swift (Native Module) — gọi TRƯỚC khi engine.joinChannel()
let session = AVAudioSession.sharedInstance()
try session.setCategory(.playAndRecord,
                        mode: .voiceChat,
                        options: [.allowBluetooth, .allowBluetoothA2DP])
try session.setActive(true)
```

## Xử lý thay đổi audio route khi tai nghe cắm vào/ra

```typescript
// Event từ Agora khi route thay đổi
onAudioRoutingChanged: (routing) => {
  // routing values:
  // -1: Default, 0: Headset, 1: Earphone, 2: Headset no mic, 3: Speaker
  // 4: Loud Speaker (bt), 5: Headset BT, 6: USB
  
  switch (routing) {
    case 5: // Bluetooth headset
      store.dispatch(callActions.setAudioRoute('bluetooth'));
      break;
    case 0: // Wired headset
      store.dispatch(callActions.setAudioRoute('headset'));
      break;
    case 3: // Speaker
      store.dispatch(callActions.setAudioRoute('speaker'));
      break;
    default:
      store.dispatch(callActions.setAudioRoute('earpiece'));
  }
},
```

## Mute/Unmute — local vs remote

```typescript
// Tắt mic local (agent không muốn bị nghe)
engine.muteLocalAudioStream(true);   // → remote không nghe thấy agent
engine.muteLocalAudioStream(false);  // → mở mic lại

// Tắt nghe remote (agent không muốn nghe — hiếm dùng hơn)
engine.muteRemoteAudioStream(remoteUid, true);

// Tắt TẤT CẢ remote users (khi trong conference call)
engine.muteAllRemoteAudioStreams(true);

// Điều chỉnh volume mic (0-400, default 100)
engine.adjustRecordingSignalVolume(100);

// Điều chỉnh volume nghe (0-400)
engine.adjustPlaybackSignalVolume(100);

// Volume của một remote user cụ thể
engine.adjustUserPlaybackSignalVolume(remoteUid, 150); // to hơn 50%
```

## Visualize "đang nói" bằng onAudioVolumeIndication

```typescript
// Bật tính năng báo volume (mỗi 200ms)
engine.enableAudioVolumeIndication(
  200,   // interval (ms)
  3,     // smooth factor
  false  // report_vad
);

// Lắng nghe event
onAudioVolumeIndication: (connection, speakers, speakerNumber, totalVolume) => {
  speakers.forEach((speaker) => {
    if (speaker.uid === 0) {
      // Local user (agent)
      setLocalVolume(speaker.volume); // 0-255
      setIsSpeaking(speaker.volume > 15); // threshold
    } else {
      // Remote user
      setRemoteVolume(speaker.volume);
    }
  });
},
```

---

# 7. Xử lý mạng yếu & Reconnection

## Sơ đồ connection state machine

```
              DISCONNECTED
                   │
                   │ joinChannel()
                   ▼
              CONNECTING
                   │
         ┌─────────┴─────────┐
         │ success           │ fail
         ▼                   ▼
     CONNECTED           RECONNECTING ◄──────────┐
         │                   │                   │
         │ network drop      │ retry (auto)       │ mạng phục hồi
         │                   └───────────────────┘
         ▼
   RECONNECTING
         │
         │ quá giới hạn retry (~10 giây timeout)
         ▼
       FAILED ──────── onConnectionLost() trigger
```

## Agora tự reconnect — bạn cần làm gì?

```typescript
// Agora SDK tự động thử reconnect khi mất mạng trong <= 10 giây.
// Bạn CHỈ cần:
// 1. Hiển thị UI "Đang kết nối lại..." khi state = RECONNECTING
// 2. Xử lý khi FAILED (mất kết nối lâu hơn 10s)

onConnectionStateChanged: (connection, state, reason) => {
  switch (state) {
    case ConnectionStateType.ConnectionStateReconnecting:
      store.dispatch(callActions.setStatus('reconnecting'));
      // Hiển thị spinner, không leaveChannel()
      break;

    case ConnectionStateType.ConnectionStateConnected:
      // Kết nối lại thành công — cuộc gọi tiếp tục
      store.dispatch(callActions.setStatus('in-call'));
      break;

    case ConnectionStateType.ConnectionStateFailed:
      // Mất kết nối vĩnh viễn — cần xử lý ở app level
      store.dispatch(callActions.setStatus('failed'));
      engine.leaveChannel(); // cleanup
      // Hỏi user muốn gọi lại không
      break;
  }
},

// ⚠️ ĐỪNG gọi leaveChannel() khi RECONNECTING
// — Agora đang tự xử lý, leaveChannel() sẽ ngắt hẳn cuộc gọi
```

## Network quality indicator — hiển thị cho agent

```typescript
// Màu indicator dựa trên quality level
function getNetworkQualityColor(quality: QualityType): string {
  switch (quality) {
    case QualityType.QualityExcellent:
    case QualityType.QualityGood:
      return '#4CAF50'; // xanh
    case QualityType.QualityPoor:
      return '#FF9800'; // vàng cam
    case QualityType.QualityBad:
    case QualityType.QualityVbad:
      return '#F44336'; // đỏ
    case QualityType.QualityDown:
      return '#9E9E9E'; // xám - mất kết nối
    default:
      return '#9E9E9E';
  }
}

// onNetworkQuality cập nhật mỗi 2 giây
// txQuality → upload (micro → server)
// rxQuality → download (server → tai nghe)
// Hiển thị cả hai để agent biết chiều nào đang có vấn đề
```

---

# 8. Background Mode — iOS & Android

## iOS — giữ cuộc gọi khi app vào background

```
Yêu cầu trong Info.plist:
  UIBackgroundModes = ["voip", "audio"]

Khi agent ấn Home button trong khi đang gọi:
  App → Background
  AVAudioSession vẫn active (iOS cho phép nhờ "audio" background mode)
  Agora SDK tiếp tục stream audio
  Engine không bị suspend

⚠️ Nếu THIẾU "voip" background mode:
  → iOS suspend app sau ~3 giây khi vào background
  → Audio bị ngắt
  → Cuộc gọi kết thúc đột ngột
```

## Android — Foreground Service bắt buộc

```
Android 8+ policy: App trong background không được chạy service > 1 phút
→ Phải dùng Foreground Service để duy trì cuộc gọi

Luồng:
  agent nhận cuộc gọi
      │
      ▼
  startForegroundService(CallService::class)
      │
      ▼
  CallService.onCreate()
      ├── startForeground(NOTIFICATION_ID, notification) ← bắt buộc trong 5 giây
      │   notification: "Đang trong cuộc gọi - Agent Nguyễn Văn A"
      └── AgoraEngine.joinChannel(...)

  agent ấn Home / chuyển app:
      → Service vẫn chạy (foreground)
      → Agora SDK tiếp tục stream
      → Notification bar hiển thị "Đang trong cuộc gọi"

  kết thúc cuộc gọi:
      → engine.leaveChannel()
      → stopForeground(true)
      → stopSelf()
```

---

# 9. Server-side Cloud Recording — Quan trọng cho Recbook

## Tại sao cần Cloud Recording thay vì client-side recording?

```
❌ Client-side recording (ghi trên thiết bị)
   agent ghi trên điện thoại → file .aac / .mp4 trên device
   Vấn đề:
   - App bị kill → file corrupt
   - Network gián đoạn → recording mất sync
   - Khó upload sau: file lớn, batch upload unreliable
   - Không có recording khi agent B gọi từ app khác

✅ Server-side Cloud Recording (Agora Cloud Recording)
   Server backend gọi Agora REST API → Agora spin up recording bot
   Recording bot join channel như một "invisible user"
   Ghi lại toàn bộ audio/video → upload lên S3/GCS của bạn
   Không phụ thuộc vào thiết bị agent
```

## Luồng Cloud Recording end-to-end

```
┌────────────────────────────────────────────────────────────────┐
│                  Cloud Recording Architecture                    │
└────────────────────────────────────────────────────────────────┘

📱 Agent App (React Native)
    │
    │  1. joinChannel(token, channelId, agentUid)
    │
    ▼
🖥️ Backend Contact Center
    │
    │  2. Phát hiện cuộc gọi bắt đầu → gọi Agora Cloud Recording API:
    │     POST https://api.agora.io/v1/apps/{appId}/cloud_recording/acquire
    │     body: { clientRequest: { resourceExpiredHour: 24 } }
    │     ← { resourceId: "abc123..." }
    │
    │  3. Start recording:
    │     POST /v1/apps/{appId}/cloud_recording/resourceid/{resourceId}/mode/mix/start
    │     body: {
    │       clientRequest: {
    │         token: recordingToken,    ← token riêng cho recording bot
    │         storageConfig: {
    │           vendor: 1,             ← 1=AWS S3, 2=GCP, 3=Ali OSS
    │           region: 0,
    │           bucket: "recbook-recordings",
    │           accessKey: "...",      ← IAM credentials (không phải Agora key)
    │           secretKey: "..."
    │         },
    │         recordingConfig: {
    │           channelType: 0,        ← 0=COMMUNICATION
    │           streamTypes: 0,        ← 0=audio only, 1=video, 2=both
    │           audioProfile: 2,       ← 0=LC-AAC, 1=HE-AAC, 2=HE-AAC v2
    │           maxIdleTime: 30        ← dừng recording nếu channel trống 30s
    │         }
    │       }
    │     }
    │     ← { sid: "session123..." }
    │
    │  4. Cuộc gọi kết thúc → Stop recording:
    │     POST /v1/apps/{appId}/cloud_recording/resourceid/{resourceId}/sid/{sid}/mode/mix/stop
    │     ← { serverResponse: { uploadingStatus: "uploaded" } }
    │
    ▼
☁️ Agora Cloud Recording Bot
    │  (join channel với uid = 0, không publish audio)
    │  (ghi lại toàn bộ audio stream của channel)
    │
    ▼
🗄️ S3/GCS — file .aac hoặc .mp4 lưu ở đây
    │  Naming: {appId}_{channelName}_{utcMs}.m3u8 (HLS) hoặc .aac
    │
    ▼
📱 Recbook App — xem lại recording:
    Fetch URL từ backend → play bằng AVPlayer (iOS) / ExoPlayer (Android)
```

## Các chế độ recording của Agora

```
┌──────────────────────────────────────────────────────────────────┐
│                 Agora Cloud Recording Modes                       │
├──────────────────┬───────────────────────────────────────────────┤
│  mix (composite) │ Trộn TẤT CẢ stream thành 1 file duy nhất    │
│                  │ → Dễ phát lại, 1 file .aac cho cuộc gọi     │
│                  │ ✅ DÙNG cho Contact Center (ghi cuộc gọi)    │
├──────────────────┼───────────────────────────────────────────────┤
│  individual      │ Ghi riêng từng user thành file riêng         │
│                  │ → Nhiều file, cần mix lại sau               │
│                  │ ✅ DÙNG khi cần edit riêng từng track        │
├──────────────────┼───────────────────────────────────────────────┤
│  web (page)      │ Ghi lại cả màn hình web app                 │
│                  │ ✅ DÙNG cho screen recording / webinar       │
└──────────────────┴───────────────────────────────────────────────┘
```

## Webhook để biết recording hoàn tất

```typescript
// Backend nhận callback từ Agora khi file upload xong
// POST /webhooks/agora
// body: {
//   eventType: 32,                    ← 32 = upload file hoàn tất
//   serviceType: 4,                   ← 4 = cloud recording
//   details: {
//     msgName: "cloud_recording_file_infos",
//     fileList: [
//       { fileName: "xxx.m3u8", isPlayable: true, trackType: "audio" }
//     ]
//   }
// }

app.post('/webhooks/agora', async (req, res) => {
  const { eventType, details } = req.body;
  if (eventType === 32) {
    const fileUrl = buildS3Url(details.fileList[0].fileName);
    await db.recordings.update({ callId }, { recordingUrl: fileUrl });
    // Notify agent qua WebSocket: recording sẵn sàng xem lại
  }
  res.status(200).send('OK');
});
```

---

# 10. CallKit Integration — Agora + iOS Native

## Sơ đồ tích hợp đầy đủ

```
PushKit payload đến (app đang bị terminate hoặc background):
{
  "callId": "call-123",
  "channelName": "ch-zenify-456",
  "agoraToken": "006...",
  "callerName": "Khách hàng Nguyễn Văn A",
  "callerNumber": "+84901234567"
}

       │
       ▼
didReceiveIncomingPushWithPayload (AppDelegate.swift)
       │
       │  BƯỚC 1: Báo CallKit ngay — KHÔNG async, KHÔNG delay
       ▼
CXProvider.reportNewIncomingCall(
  UUID(uuidString: callId)!,
  update: { update in
    update.remoteHandle = CXHandle(type: .phoneNumber, value: callerNumber)
    update.hasVideo = false
    update.localizedCallerName = callerName
    update.supportsDTMF = true
    update.supportsHolding = true
    update.supportsGrouping = false
  }
)
       │
       │  BƯỚC 2: Lưu payload vào bộ nhớ tạm
       ▼
CallSessionManager.shared.pendingCall = PendingCallInfo(
  callId: callId,
  channelName: channelName,
  token: agoraToken
)

       │
       │  Agent nhấn "Chấp nhận" (màn khóa hoặc trong app)
       ▼
CXProvider.perform(CXAnswerCallAction)
       │
       │  BƯỚC 3: fulfil action + join Agora channel
       ▼
func provider(_ provider: CXProvider, perform action: CXAnswerCallAction) {
  action.fulfill()   // ← báo CallKit đã chấp nhận

  // Lấy thông tin cuộc gọi từ pendingCall
  let info = CallSessionManager.shared.pendingCall!

  // Emit sự kiện về React Native
  RCTEventEmitter.shared.sendEvent("onCallAnswered", [
    "callId": info.callId,
    "channelName": info.channelName,
    "token": info.token
  ])
}

       │ React Native nhận event
       ▼
// JS side:
useEffect(() => {
  const subscription = callKitModule.addListener('onCallAnswered', (event) => {
    engine.joinChannel(event.token, event.channelName, localUid, {
      clientRoleType: ClientRoleType.ClientRoleBroadcaster,
      publishMicrophoneTrack: true,
      autoSubscribeAudio: true,
    });
    navigation.navigate('ActiveCall', { callId: event.callId });
  });
  return () => subscription.remove();
}, []);
```

## Xử lý khi agent bấm Kết thúc cuộc gọi

```typescript
// JS: agent bấm nút Hang Up
async function handleHangUp() {
  // 1. Rời Agora channel
  await engine.leaveChannel();

  // 2. Báo CallKit kết thúc
  callKitModule.endCall(currentCallId);
  // → Swift: CXCallController.requestTransaction(with: CXEndCallAction(call: uuid))
  // → iOS hiển thị "Cuộc gọi kết thúc" và lưu vào Call History

  // 3. Notify backend
  await api.post(`/calls/${currentCallId}/end`);

  // 4. Reset state
  store.dispatch(callActions.reset());
  navigation.popToTop();
}
```

---

# 11. Pattern React Hooks — Tổ chức code Agora đúng chuẩn

## Kiến trúc module Agora trong app

```
src/
├── features/
│   └── call/
│       ├── hooks/
│       │   ├── useAgoraEngine.ts      ← khởi tạo engine, manage lifecycle
│       │   ├── useAgoraCall.ts        ← join/leave channel, call state
│       │   ├── useAudioControls.ts   ← mute, speaker, volume
│       │   └── useNetworkQuality.ts  ← monitor network
│       │
│       ├── store/
│       │   └── callSlice.ts           ← Zustand hoặc Redux slice
│       │
│       ├── services/
│       │   ├── AgoraTokenService.ts   ← fetch + renew token
│       │   └── CloudRecordingService.ts ← start/stop recording
│       │
│       └── screens/
│           ├── IncomingCallScreen.tsx
│           └── ActiveCallScreen.tsx
```

## useAgoraCall — hook tổng hợp

```typescript
// hooks/useAgoraCall.ts
export function useAgoraCall(channelName: string, token: string) {
  const engineRef = useAgoraEngine(); // khởi tạo 1 lần
  const [callState, setCallState] = useState<CallState>('idle');
  const [remoteUid, setRemoteUid] = useState<number | null>(null);
  const [networkQuality, setNetworkQuality] = useState<QualityType>(QualityType.QualityUnknown);
  const localUidRef = useRef<number>(0);

  // Đăng ký event handlers
  useEffect(() => {
    const engine = engineRef.current;
    if (!engine) return;

    const handler: IRtcEngineEventHandler = {
      onJoinChannelSuccess: (_, elapsed) => {
        setCallState('in-call');
      },
      onUserJoined: (_, uid) => {
        setRemoteUid(uid);
      },
      onUserOffline: (_, uid, reason) => {
        setRemoteUid(null);
        if (reason === UserOfflineReasonType.UserOfflineQuit) {
          setCallState('ended');
        } else {
          // Dropped — có thể là mạng chứ không phải cố ý
          setCallState('remote-dropped');
        }
      },
      onConnectionStateChanged: (_, state) => {
        if (state === ConnectionStateType.ConnectionStateReconnecting) {
          setCallState('reconnecting');
        } else if (state === ConnectionStateType.ConnectionStateFailed) {
          setCallState('failed');
        }
      },
      onNetworkQuality: (_, uid, tx, rx) => {
        if (uid === 0) setNetworkQuality(Math.max(tx, rx)); // worst of two
      },
      onTokenPrivilegeWillExpire: async () => {
        const newToken = await AgoraTokenService.refresh(channelName, localUidRef.current);
        engine.renewToken(newToken);
      },
    };

    engine.registerEventHandler(handler);
    return () => engine.unregisterEventHandler(handler);
  }, [engineRef.current, channelName]);

  const joinCall = useCallback(async () => {
    const engine = engineRef.current;
    if (!engine) return;

    setCallState('joining');
    engine.joinChannel(token, channelName, 0, {
      clientRoleType: ClientRoleType.ClientRoleBroadcaster,
      publishMicrophoneTrack: true,
      autoSubscribeAudio: true,
    });
  }, [token, channelName]);

  const leaveCall = useCallback(() => {
    const engine = engineRef.current;
    if (!engine) return;
    engine.leaveChannel();
    setCallState('idle');
    setRemoteUid(null);
  }, []);

  return { callState, remoteUid, networkQuality, joinCall, leaveCall };
}
```

---

# 12. Bẫy & Lỗi hay gặp khi dùng react-native-agora

## Bảng bug phổ biến và cách fix

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  BUG                              │  NGUYÊN NHÂN          │  FIX            │
├───────────────────────────────────┼───────────────────────┼─────────────────┤
│ Crash khi release bản Android     │ ProGuard obfuscate    │ Thêm keep rules │
│ "NoSuchMethodError"               │ Agora native class    │ cho Agora SDK   │
├───────────────────────────────────┼───────────────────────┼─────────────────┤
│ Micro không release sau cuộc gọi  │ Quên engine.release() │ Gọi release()   │
│ App tiếp theo không thu âm được   │ hoặc leaveChannel()   │ trong cleanup   │
├───────────────────────────────────┼───────────────────────┼─────────────────┤
│ Retain cycle — iOS memory leak    │ AgoraDelegate strong  │ weak var        │
│ Audio resource giữ mãi            │ ref đến AgoraModule   │ delegate:       │
├───────────────────────────────────┼───────────────────────┼─────────────────┤
│ iOS: Audio không hoạt động sau    │ AVAudioSession bị     │ Cấu hình lại    │
│ khi nghe nhạc / app khác dùng     │ interrupt chưa resume │ AVAudioSession  │
│ audio                             │                       │ trong interrupt │
│                                   │                       │ handler         │
├───────────────────────────────────┼───────────────────────┼─────────────────┤
│ Token expired khi cuộc gọi dài    │ Không renewToken()    │ Lắng nghe       │
│ → cuộc gọi bị ngắt giữa chừng    │ khi callback trigger  │ onTokenPrivilege│
│                                   │                       │ WillExpire      │
├───────────────────────────────────┼───────────────────────┼─────────────────┤
│ Echo tiếng vọng khi dùng loa      │ AudioScenario sai     │ Dùng            │
│ ngoài                             │ hoặc thiếu AEC        │ VoiceChatroom   │
│                                   │                       │ scenario        │
├───────────────────────────────────┼───────────────────────┼─────────────────┤
│ Android: cuộc gọi dừng khi app    │ Không dùng            │ Implement       │
│ vào background                    │ Foreground Service    │ CallService     │
├───────────────────────────────────┼───────────────────────┼─────────────────┤
│ Simulator không có âm thanh       │ iOS Simulator không   │ Test trên       │
│                                   │ hỗ trợ audio hardware │ thiết bị thật  │
└───────────────────────────────────┴───────────────────────┴─────────────────┘
```

## ProGuard keep rules cho Android (bắt buộc)

```proguard
# File: android/app/proguard-rules.pro

# Agora RTC SDK
-keep class io.agora.** { *; }
-dontwarn io.agora.**

# Agora native libraries
-keep class io.agora.rtc2.** { *; }
-keep class io.agora.base.** { *; }

# IRtcEngineEventHandler implementations — không obfuscate callback names
-keepclassmembers class * implements io.agora.rtc2.IRtcEngineEventHandler {
    public *;
}
```

## Xử lý AVAudioSession interrupt (iOS)

```swift
// Native Module — cần xử lý khi cuộc gọi điện thoại đến giữa VoIP call
NotificationCenter.default.addObserver(
    self,
    selector: #selector(handleAudioSessionInterrupt),
    name: AVAudioSession.interruptionNotification,
    object: nil
)

@objc func handleAudioSessionInterrupt(notification: Notification) {
    guard let userInfo = notification.userInfo,
          let typeValue = userInfo[AVAudioSessionInterruptionTypeKey] as? UInt,
          let type = AVAudioSession.InterruptionType(rawValue: typeValue) else { return }

    switch type {
    case .began:
        // Cuộc gọi điện thoại native đến → Agora sẽ tự mute
        // Gửi event về React Native để hiển thị UI phù hợp
        sendEvent(withName: "onAudioInterrupted", body: nil)

    case .ended:
        // Cuộc gọi điện thoại kết thúc → resume AVAudioSession
        if let optionsValue = userInfo[AVAudioSessionInterruptionOptionKey] as? UInt {
            let options = AVAudioSession.InterruptionOptions(rawValue: optionsValue)
            if options.contains(.shouldResume) {
                try? AVAudioSession.sharedInstance().setActive(true)
                sendEvent(withName: "onAudioResumed", body: nil)
            }
        }
    default: break
    }
}
```

---

# 13. Câu hỏi phỏng vấn chuyên sâu về Agora

---

## AQ1. Giải thích sự khác biệt giữa Agora App ID và Token — khi nào dùng cái nào?

### 💡 Giải thích

**App ID** — định danh app trong Agora console. Không có expiry. Nếu để trong code app → bất kỳ ai có App ID đều có thể join channel của bạn.

**Token (RTC Token)** — JSON Web Token có chữ ký, chứa:
- App ID
- Channel name cụ thể
- UID cụ thể
- Role (publisher/subscriber)
- Expiry timestamp

**Nguyên tắc:**
- `Production` → **bắt buộc dùng Token**. App ID để trong code là P0 security vulnerability.
- `Development` → Agora cho phép join bằng App ID với token = null, nhưng chỉ khi tắt Authentication trong Agora Console.

```typescript
// ❌ KHÔNG BAO GIỜ làm production:
engine.joinChannel(null, channelName, uid, {})

// ✅ LUÔN dùng token từ server:
const token = await fetchTokenFromBackend(channelName, uid);
engine.joinChannel(token, channelName, uid, {})
```

---

## AQ2. UID trong Agora là gì — cách quản lý UID cho multi-agent Contact Center?

### 💡 Giải thích

**UID (User ID)** — số nguyên 32-bit (uint32) định danh mỗi participant trong channel.
- `0` = Agora tự assign UID ngẫu nhiên (lấy từ `onJoinChannelSuccess.uid`)
- `> 0` = bạn tự chỉ định, **phải duy nhất trong channel**

**Contact Center pattern:**

```typescript
// Cách map agent → UID an toàn:
// Không dùng agentId trực tiếp (có thể đoán được, security risk)
// Backend hash agentId + callId → uint32

// Backend:
function generateUID(agentId: string, callId: string): number {
  const hash = crypto.createHash('sha256')
    .update(`${agentId}:${callId}`)
    .digest('hex');
  // Lấy 8 char hex đầu → parseInt → uint32
  return parseInt(hash.substring(0, 8), 16) % 2147483647; // max uint32/2
}

// Trả về cùng với token
const token = generateToken(channelName, uid);
return { token, uid };
```

**Lý do không dùng UID = 0:**
- Khi Agora tự assign, bạn không biết UID của mình cho đến khi `onJoinChannelSuccess`.
- Backend/cloud recording cần biết UID để xác định "agent này đang ghi cuộc gọi nào".
- Tốt hơn: pre-assign UID từ backend, trả về cùng token.

---

## AQ3. Làm thế nào tối ưu Agora cho mạng 3G/4G yếu ở vùng nông thôn?

### 💡 Giải thích

```typescript
// 1. Bật adaptive bitrate — Agora tự điều chỉnh khi mạng xấu
engine.setAudioProfile(
  AudioProfileType.AudioProfileSpeechStandard, // 18kbps, đủ cho speech
  AudioScenarioType.AudioScenarioVoiceChatroom
);

// 2. Bật dual-stream (chủ yếu cho video, nhưng cũng áp dụng cho audio quality)
// Agora tự chuyển giữa high/low quality dựa trên bandwidth

// 3. Monitor packet loss và cảnh báo sớm
onLocalAudioStats: (connection, stats) => {
  if (stats.txPacketLossRate > 15) {
    // Mất trên 15% packet → chất lượng nghe sẽ xấu
    showNetworkWarning('Mạng yếu, chất lượng cuộc gọi có thể giảm');
  }
},

// 4. Dùng AudioScenario phù hợp với mạng yếu
// AudioScenarioVoiceChatroom → FEC (Forward Error Correction) tự động
// → Agora gửi redundant packet để recover khi mất gói
```

---

## AQ4. Giải thích cơ chế AEC (Acoustic Echo Cancellation) trong Agora hoạt động thế nào ở mức SDK?

### Sơ đồ

```
Micro thu âm → Tín hiệu "bẩn" (voice + echo từ loa)
                    │
                    ▼
         ┌──────────────────────┐
         │  AEC Module (native) │
         │                      │
         │  Reference signal    │← lấy từ playback (biết chính xác
         │  (cái loa đang phát) │  cái loa đang phát ra cái gì)
         │          │           │
         │          ▼           │
         │  Adaptive Filter     │← tính toán echo ước lượng
         │  (LMS/NLMS algo)     │
         │          │           │
         │  Subtract echo từ    │← tín hiệu micro - echo estimate
         │  tín hiệu micro      │
         └──────────────────────┘
                    │
                    ▼
         Voice sạch → gửi đến Agora server
```

### 💡 Giải thích

AEC hoạt động theo nguyên tắc: loa phát âm thanh gì, micro sẽ thu lại âm đó (echo). AEC biết chính xác tín hiệu đang phát ra loa (reference signal), dùng thuật toán adaptive filter để ước lượng phần echo trong tín hiệu micro, rồi trừ đi.

**Tại sao cần `mode: .voiceChat` trên iOS?**
iOS hardware AEC chỉ hoạt động khi `AVAudioSession.mode = .voiceChat`. Nếu dùng mode khác, AEC phần cứng bị tắt → phải rely on software AEC của Agora (kém hiệu quả hơn).

**Agora có thêm ANS (Ambient Noise Suppression):** lọc tiếng ồn không liên tục (tiếng xe, tiếng gõ bàn phím) bằng machine learning model chạy trên device.

---

## AQ5. Trong recbook, làm thế nào đảm bảo recording không bị mất khi agent mất mạng giữa chừng?

### 💡 Giải thích

```
Vấn đề:
   Agent đang gọi, Cloud Recording đang chạy trên Agora server.
   Agent mất mạng 30 giây rồi kết nối lại.
   Recording: bị gián đoạn hay tiếp tục?

Câu trả lời: Cloud Recording vẫn chạy trên Agora server
   → Server không biết agent mất mạng
   → Recording tiếp tục, nhưng sẽ có khoảng im lặng trong file recording
   → Khi agent reconnect và join lại channel, audio lại được ghi

Vấn đề nghiêm trọng hơn: channel trống hoàn toàn (cả 2 bên mất kết nối)
   → maxIdleTime: 30 (cấu hình lúc start recording)
   → Nếu channel trống > 30 giây, Agora tự stop recording và upload file
   → File bị cắt ngang

Giải pháp cho recbook:
   1. maxIdleTime: 60 thay vì 30 (thời gian buffer dài hơn)
   2. Sau khi reconnect: query Agora API kiểm tra recording có còn chạy không
      GET /v1/apps/{appId}/cloud_recording/resourceid/{rid}/sid/{sid}/mode/mix/query
      → { status: 5 } = đang chạy ✅
      → { status: 4 } = đã stop ❌ → phải start recording mới
   3. Ghép file: nếu recording bị split, backend ghép các file .aac lại
      bằng ffmpeg: ffmpeg -i concat:file1.aac|file2.aac -c copy merged.aac
```

---

## AQ6. Làm sao test Agora integration khi không có 2 thiết bị thật?

### 💡 Giải thích

```typescript
// Cách 1: Agora Web Demo
// Vào https://webdemo.agora.io/basicVoiceCall/
// Nhập App ID + Channel Name → join từ trình duyệt
// → Thiết bị mobile của bạn join cùng channel → nghe và nói được

// Cách 2: Mock AgoraEngine trong tests
// Tạo mock implementation cho IRtcEngine
jest.mock('react-native-agora', () => ({
  createAgoraRtcEngine: jest.fn(() => ({
    initialize: jest.fn(),
    enableAudio: jest.fn(),
    joinChannel: jest.fn(),
    leaveChannel: jest.fn(),
    release: jest.fn(),
    registerEventHandler: jest.fn(),
    unregisterEventHandler: jest.fn(),
    muteLocalAudioStream: jest.fn(),
    setEnableSpeakerphone: jest.fn(),
    renewToken: jest.fn(),
  })),
  ChannelProfileType: { ChannelProfileCommunication: 0 },
  ClientRoleType: { ClientRoleBroadcaster: 1 },
  // ... other enums
}));

// Cách 3: Agora có Testing Token (không expire trong 24h)
// Agora Console → Project → Temporary Token (chỉ dùng cho development)

// ⚠️ LUÔN test trên thiết bị thật trước khi release
// Simulator không có audio hardware, không phản ánh thực tế
```

---

## ⚠️ Checklist trước khi lên production (Agora)

```
□ App ID lấy từ Keychain/Keystore — KHÔNG hardcode trong code
□ Token Server được setup — token có expiry
□ onTokenPrivilegeWillExpire được handle — renewToken() gọi đúng cách
□ engine.release() được gọi khi cleanup
□ AVAudioSession mode = .voiceChat trên iOS
□ Foreground Service được implement trên Android
□ ProGuard keep rules cho Agora SDK (Android release)
□ maxIdleTime phù hợp cho Cloud Recording
□ Agora webhook endpoint setup để nhận recording complete event
□ Test trên thiết bị thật (không phải simulator)
□ Background mode "voip" và "audio" trong Info.plist (iOS)
□ AEC/ANS/AGC được bật — dùng AudioScenarioVoiceChatroom
□ UID management: backend generate UID an toàn, không dùng UID=0 ở production
□ onNetworkQuality → hiển thị indicator cho user
□ onConnectionStateChanged → handle RECONNECTING và FAILED khác nhau
```
