<style>
:root {
  --theme-color: #0c6a7a;
  --theme-color-soft: #d9eef2;
  --main-color: #111827;
  --muted-color: #5f6b7a;
  --global-font-size: 9.3pt;
  --header-font-family: 'Georgia', 'Times New Roman', serif;
  --global-font-family: 'Times New Roman', 'Georgia', serif;
  --underline-color: #d9e3e8;
  --page-margin: 10mm;
}

html, body {
  margin:0;
  padding:0;
  font-size: var(--global-font-size)!important;
  color: #1f2937;
  font-weight: 400;
  font-family: var(--global-font-family)!important;
  background: #ffffff;
}

body {
  max-width: 900px;
  margin: 0 auto;
  padding: 9mm 11mm 11mm;
}

@media print {
  @page
  {
    size: A4;
    margin: var(--page-margin);
  }
  body {
    padding: 0;
    margin: 0;
    max-width: none;
    background: white;
  }
}

a{
  text-decoration: none;
  color: var(--main-color);
}
p a{
  text-decoration: underline;
  text-decoration-color: var(--underline-color);
}
b, strong {font-weight: 700}
p, blockquote {margin: 0;}

h1 {
  font-size: 21.5pt;
  font-weight: 700;
  text-align: center;
  letter-spacing: 0.2px;
  margin: 0 0 4pt!important;
}

h1, h2, h3, h4, h5 {
  font-family: var(--header-font-family);
  border: none;
  line-height: 1.2;
  margin: 10pt 0 2pt;
}

h1, h2, h3, h4, h5, h2 a, h3 a, h3 strong, h3 b {
  color: var(--theme-color);
}

h1 + p,
h1 + p strong,
h1 + p a {
  font-size: 8.7pt;
  color: var(--muted-color);
}

h1 + p {
  text-align: center;
  line-height: 1.55;
  margin-bottom: 10pt;
}

h2 {
  font-size: 13pt;
  font-weight: 700;
  border-bottom: 1px solid var(--underline-color);
  text-transform: uppercase;
  letter-spacing: 0.55px;
  margin: 18pt 0 4pt!important;
  padding-bottom: 2pt;
}

h3, h4, h5, h6 {
  font-size: var(--global-font-size);
  font-weight: 600;
}

em {
  float: right;
  font-weight: 200;
  font-style: normal;
}

h3 + blockquote p {
  font-weight: bold;
}

p, ul li, ol li {
  line-height: 1.62;
}

/* List */
ul,
ol,
ul > li,
ol > li {
  margin-left: 0;
  padding-left: 0;
}

ul, ol {
  padding-left: 1rem;
  margin-top: 0.22rem;
  margin-bottom: 0.3rem;
}

ul {
  columns: 2;
  -webkit-columns: 2;
  -moz-columns: 2;
}

img {
  display: block;
  margin: 8pt auto 14pt;
  border-radius: 10px;
  border: 1px solid #d8e2e8;
  box-shadow: 0 8px 20px rgba(15, 23, 42, 0.08);
  object-fit: cover;
}

hr {
  border: 0;
  border-top: 1px solid var(--underline-color);
  margin: 10pt 0 14pt;
}

blockquote {
  padding-left: 10pt;
  border-left: 3px solid var(--theme-color-soft);
  color: var(--muted-color);
}

strong, b {
  color: #111827;
}

li::marker {
  color: var(--theme-color);
}

.tag {
  display: inline-block;
  font-size: 7.6pt;
  font-weight: 700;
  padding: 1pt 6pt;
  border-radius: 4pt;
  margin-left: 6pt;
  vertical-align: middle;
  letter-spacing: 0.3px;
  text-transform: uppercase;
}
.tag-flutter { background: #d9eef2; color: #0c6a7a; }
.tag-rn      { background: #dbeafe; color: #1d4ed8; }
.tag-native  { background: #fde6cc; color: #a04a00; }
.tag-hybrid  { background: #e6d8f5; color: #5b2a91; }
</style>

# Nguyen Nhat Long

<p align="center">
  <strong>Software Engineer | Mobile Developer | React Native / Flutter / Swift / Kotlin</strong><br/>
  Ha Noi, Vietnam · 17/07/1998 · <a href="mailto:longnn17.developer@gmail.com">longnn17.developer@gmail.com</a> · 0868177610<br/>
  <a href="https://chelongcoder.github.io/my-portfolio">Portfolio</a> · <a href="https://github.com/longnn17">GitHub</a>
</p>

<p align="center">
  <img src="https://i.ibb.co/9Hjzkzbk/chandung.jpg" alt="Nguyen Nhat Long" width="220" />
</p>

---

## Professional Summary

I am a Software Engineer with over 7 years of experience delivering production-grade mobile products across banking, fintech, telecom, IoT/connected devices, healthcare, logistics, education, media, and enterprise domains. I specialize in end-to-end mobile delivery, from architecture design and implementation to release management and long-term product maintenance.

My core technical profile is balanced across major mobile stacks: 7 years of React Native, 4 years of Flutter, and 4 years of native iOS/Android development. In React Native, I focus on scalable architecture, API integration, authentication, state management, native bridge/module integration, and performance optimization. In Flutter, I have delivered robust applications using BLoC, Cubit, Riverpod, Provider, MobX, and Clean Architecture for maintainable and testable codebases.

On the native side, I have hands-on experience with Swift, UIKit, RxSwift, SnapKit, Kotlin, Java, coroutines, and XML-based Android UI for platform-specific optimization. I also bring practical backend collaboration and implementation experience with Node.js (including NestJS and Express), Prisma, TypeORM, MongoDB, MySQL, and PostgreSQL, enabling efficient API design, integration, and cross-team delivery alignment.

In addition to technical implementation, I have experience coordinating delivery across product, design, backend, QA, and client-facing stakeholders. I am comfortable clarifying requirements, breaking down work, assigning tasks, reviewing technical direction, and keeping delivery aligned with scope and timeline. My long-term goal is to continue growing as a Technical Lead and eventually into an Engineering Manager role.

---

## Core Strengths

- 7+ years of experience in software and mobile application development
- Strongest in React Native and mobile product delivery
- Solid production experience with Flutter and clean mobile architecture
- Hands-on experience with iOS and Android native development when needed
- Proven experience delivering secure fintech onboarding and identity verification journeys (OCR, NFC, liveness)
- Strong architecture ownership with MVVM + Clean Architecture and reactive mobile patterns
- Practical expertise in hybrid Native + Flutter integration for modular feature delivery
- Production experience with IoT / connected-device apps: BLE GATT pairing, ESP-IDF BLE Wi-Fi provisioning, mDNS discovery, and local device REST control
- Strong grasp of on-device network security: mTLS client certificates, certificate pinning, and challenge-response device authentication
- Experience designing resumable, checkpointed installation journeys and firmware OTA pipelines for consumer hardware
- Built hardware-free test environments (in-app demo emulator and Raspberry Pi device emulator) to unblock QA and development
- Experienced in cross-platform migration planning (Native iOS/Android to Flutter UI) with phased rollout strategy
- Strong capability in Flutter-Native bridge design (MethodChannel, callback round-trip, and lifecycle-safe navigation)
- Hands-on backend ownership with Node.js and NestJS — queue-based async pipelines (BullMQ), transactional outbox delivery, Prisma/PostgreSQL data modeling, and API hardening
- Practical AI integration experience — image/text-to-music generation pipelines, prompt engineering, and async provider orchestration with retry and refund-on-failure handling
- End-to-end feature ownership from requirement clarification to release
- Solid delivery mindset for high-risk flows with A/B rollout, fallback path, and regression control
- Comfortable leading delivery, coordinating teams, and supporting cross-functional collaboration
- Experience across banking, telecom, healthcare, logistics, education, media, and marketplace products

---

## Technical Skills

### Architecture & Design Patterns
- Clean Architecture (domain / data / presentation, dependency rule)
- Feature-based modularization
- MVVM, MVI (BLoC), MVP, Coordinator / Router navigation
- Repository pattern, DI (get_it, Koin, Resolver)
- Unidirectional data flow, reactive streams (Rx, Flow, Combine)
- Either / Result error modeling
- Micro front-end / super-app (Module Federation + Re.Pack)
- Hybrid architecture: cross-platform UI + native SDK modules

### Native Interop & Platform Integration
- Bridge contract design: MethodChannel / EventChannel, NativeModules, RCTEventEmitter, Expo Modules
- Cross-platform error-code mapping, lifecycle-safe callbacks, replacement navigation
- Native modules for NFC, liveness, FIDO transaction signing, Screen Time, FFmpeg video editing
- BLE GATT, ESP-IDF provisioning, mDNS discovery, firmware OTA

### Security & Reliability
- Keychain / Keystore, Secure Enclave, biometric auth (LocalAuthentication, BiometricPrompt)
- Certificate pinning, mTLS, challenge-response device auth
- Proactive token refresh with single-flight guard
- Idempotency, double-submit protection, transactional outbox
- Feature-flag rollout with rollback path (Remote Config)

### Backend & System Design
- NestJS modular architecture: guards, interceptors, exception filters
- Async job pipelines (BullMQ + Redis) with retry and terminal-failure handling
- Atomic ledger operations (Prisma `$transaction`), idempotent refunds
- Server-side IAP verification and refund reconciliation
- PostgreSQL / MongoDB data modeling

### Core Stack
- React Native (TypeScript), Flutter (Dart 3), Swift, Kotlin
- State: Redux Toolkit, Zustand, MobX, BLoC / Cubit, Riverpod
- Node.js / NestJS, Prisma, PostgreSQL
- CI/CD: Fastlane, GitLab CI, Jenkins, Firebase App Distribution, Sentry

---

## Professional Experience

### NetComm Wireless
**Mobile Developer (React Native / iOS / Android)**  
**06/2026 - Present**

- Working on Aurora, a production React Native companion app for NetComm networking hardware (fixed-wireless outdoor CPE, mesh gateways with satellites, Wi-Fi extender / smart-plug devices) shipped to carrier and retail channels
- Delivering features across five device product lines, each with its own installation flow and transport: BLE GATT, ESP-IDF BLE provisioning, local REST over Wi-Fi with mTLS and certificate pinning, and mDNS/Zeroconf discovery
- Owning the Rex (WiFiHero) product line end to end, from BLE pairing and Wi-Fi provisioning to device password management, firmware OTA, diagnostics, and dashboard
- Building device-management features for the Nest/Osprey gateway lines, including LAN/DHCP, Wi-Fi and Guest Wi-Fi, Internet Backup failover, parental control, VoIP, and network topology
- Introducing an automated AI-assisted merge-request review job on GitLab CI, a Raspberry Pi hardware-emulator test environment, and developer onboarding documentation for the team
- Improving Appium/XCUITest automation hooks and accessibility across shared UI components
- Working in a distributed Australia-Vietnam team on a Jira + GitLab merge-request workflow, coordinating with firmware, QA automation, product, and design

### Finhay Vietnam JSC
**iOS / Mobile Developer**  
**02/2025 - 05/2026**

- Worked on a hybrid fintech mobile product integrating Native iOS and Flutter modules
- Delivered secure onboarding and eKYC-related flows, including card capture guidance, NFC reading, and liveness handoff
- Implemented and maintained authentication journeys, social sign-in integration, and secure token handling
- Defined migration direction for eKYC UI from Native iOS/Android to Flutter while preserving Native NFC/Liveness SDK usage
- Designed bridge contracts for Flutter-Native round-trip communication across NFC scan, liveness result callbacks, and flow completion events
- Standardized navigation behavior across platforms for critical eKYC steps (replacement flow to prevent unsafe back navigation)
- Planned phased rollout with Remote Config, fallback path to legacy native flow, and risk-based regression control
- Collaborated with backend, QA, product, and design teams to align business rules, API contracts, and release quality

### Vietnam Maritime Bank (MSB)
**Software Developer, FE5**  
**02/2025 - 02/2026**

- Working as a React Native developer in the banking domain
- Contributing to Internet Banking features in a micro front-end / super app environment
- Building and maintaining daily banking features including account information, transfers, bill payment, savings, loyalty flows, and card activation
- Working with Repack and modular front-end architecture for scalable delivery

### Viettel Software
**Software Development Engineer**  
**11/2023 - 01/2025**

- Worked across multiple Viettel products spanning mobile media, telecom utility, internal modules, AI/chatbot, and iOS product delivery
- Contributed as a React Native, Flutter, and iOS developer depending on product needs
- Took ownership of architecture setup, feature implementation, bug fixing, internal coordination, and release quality

### Hisoft Vietnam
**Software Developer / Technical Leader**  
**09/2019 - 11/2023**

- Started as a Software Developer and later moved into Technical Leader responsibilities
- Worked across logistics, booking, enterprise operations, real estate social products, healthcare, fintech, and e-commerce projects
- Helped guide technical direction, coordinate teams, and collaborate closely with PM, BA, QA, design, and clients
- Built strong experience in shipping real-time and media-related mobile applications

### OWS Vietnam
**Software Developer**  
**09/2018 - 09/2019**

- Worked on web and mobile application development using React and React Native
- Gained early full-time product delivery experience while still at university

### Technology Academy ACT
**Teaching Support**  
**05/2017 - 05/2019**

- Worked as a teaching assistant for the university computer center
- Supported student learning and technical instruction while continuing formal study

---

## Selected Project Experience

### Aurora - Multi-Product Device Companion App (NetComm) <span class="tag tag-hybrid">RN + Native BLE / Wi-Fi</span>
**Mobile Developer (React Native / iOS / Android)**  
**06/2026 - Present**

Production iOS + Android companion app that guides end users through installing, provisioning, and managing NetComm networking hardware. The app supports **five device product lines**, each with its own setup journey and communication transport, and is distributed through carrier and retail channels.

**Product lines and transports**
- `Bunning` - outdoor fixed-wireless CPE over **BLE GATT** with SHA-256 challenge/response authentication
- `Leopard` / `Nest` / `Osprey` - AurusGATE gateways (NF70 / NF71) over **Wi-Fi + local REST** with **mTLS client certificates and certificate pinning**, plus satellite mesh pairing (NS70 / NS71) and VoIP provisioning
- `Rex` (WiFiHero NR01) - **ESP-IDF BLE provisioning**, **mDNS/Zeroconf discovery**, and pinned REST control channel

**Key responsibilities**
- Owned the Rex (WiFiHero) product line end to end: BLE scan and connect with animated LED-state guidance, ESP BLE Wi-Fi provisioning, device password generation/live validation/factory-reset re-authentication, firmware update, ping monitoring, scheduled power cycle, diagnostic log export, and dashboard
- Built **resumable setup with checkpointing** so an interrupted installation returns the user to the exact step (BLE connected / Wi-Fi provisioned entry points) instead of restarting, delivered for both the Rex flow and the NF71 Nest flow
- Implemented the **firmware update pipeline** across two deliberately separate transports - a public-CA update server for download and a pinned, authenticated device channel for upload and apply - with download deduplication, reboot polling, and phased progress reporting
- Hardened **mDNS device discovery** against stale IP addresses and multi-device networks by verifying device identity (BLE serial / TXT records) before provisioning, preventing cross-device misconfiguration
- Delivered Nest/Osprey gateway management features: LAN/DHCP settings with subnet validation and apply-polling, Wi-Fi and Guest Wi-Fi settings, Share Wi-Fi Access, Internet Backup priority and failover, device map/topology, parental control, firmware upgrade, scheduled reboot, and date/time
- Built **multi-device saved-device sync and switching** with safe BLE teardown, timeout guards, and connect/remove flows to eliminate UI hangs when swapping between saved devices
- Scaffolded the **Osprey (fifth) product flow** and extracted a dedicated API service from the shared gateway service layer to keep flow boundaries clean
- Set up an **automated AI-assisted merge-request review** job on GitLab CI (containerised runner, GitLab API integration, previous-review reconciliation, cross-flow regression tracing, quota and retry handling)
- Authored the **developer onboarding guide, demo-mode guide, screen guide, and Raspberry Pi emulator documentation**, and built a Pi-based emulator workflow (REST + BLE GATT servers) so QA can run full installation journeys without real hardware
- Improved **Appium / XCUITest automation and accessibility** across shared components - individually addressable list rows, switch roles and states, dropdown backdrop handling, and scroll-view structure
- Handled platform compliance and build health: Android **API 36 (Android 16)** targeting for Play Store compliance, Xcode 26 build fixes, and in-app update prompting driven by Firebase Remote Config

**Tech stack**
- React Native 0.80 / React 19
- JavaScript / TypeScript
- Redux Toolkit (11 feature slices)
- React Navigation v7 (stack / drawer / bottom tabs, ~90 routes)
- Swift / Objective-C
- Kotlin / Java
- BLE GATT (SHA-256 challenge/response pairing)
- ESP-IDF BLE provisioning (custom `react-native-esp-provisioning` native module)
- mDNS / Zeroconf service discovery
- Wi-Fi join (NEHotspotConfiguration on iOS, Android Wi-Fi APIs)
- mTLS client certificates + certificate pinning
- Local device REST API
- Realm v20 (demo-mode device emulator)
- Firebase Analytics / Crashlytics / Remote Config
- react-native-vision-camera (QR scan)
- react-native-reanimated
- i18n-js (EN / ES)
- Jest
- Appium + Cucumber (Java) + Selenium Grid
- Fastlane / TestFlight
- GitLab CI/CD (Docker + macOS runners)
- Docker
- Jira
- Raspberry Pi device emulator (Python)

---

### Aimoji – AI Emoji & Sticker Keyboard *(Personal Project, live on App Store / Google Play)* <span class="tag tag-native">Native Android IME + iOS Keyboard Extension</span>
**Solo Fullstack Owner (Kotlin / Compose / SwiftUI / NestJS)**  
**08/2026 - Present**  
**Store:** [App Store](https://apps.apple.com/vn/app/aimoji-keyboard-ai-emojis/id6814914345) · [Google Play](https://play.google.com/store/apps/details?id=com.algorz.aimoji.app)

Custom keyboard that turns a face photo into cartoon stickers with AI and lets the user insert them directly from the keyboard inside any chat app. Includes a card creator for composing stickers, text, audio and video into shareable cards.

**Key responsibilities**
- Built the Android custom keyboard as an `InputMethodService` in Kotlin (QWERTY typing plus a scrollable sticker grid), sharing one process with the main app so the keyboard reads generated stickers straight from app storage without IPC
- Built the iOS keyboard as a separate **Keyboard Extension**, sharing sticker files with the main app through an **App Group** container, keeping the group identifier consistent across both targets' entitlements and the shared config, where a mismatch fails silently instead of crashing
- Started on Flutter, then rewrote the app natively (Kotlin + Jetpack Compose on Android, SwiftUI on iOS) to get full control over the keyboard surfaces; ported all 7 Compose screens to SwiftUI with shared design tokens
- Built the card composer: text styling, sticker resize, audio recording and video export (`AVAssetWriter` on iOS), plus watermarking and share flows
- Designed the NestJS backend as a thin proxy that holds the AI API keys: API-key and throttler guards, upload validation, image captioning with a vision-model fallback chain, prompt building, pluggable image providers (Stability / Picsart) and `sharp` background removal to transparent PNG
- Integrated AdMob behind UMP consent, and shipped both stores with Fastlane (Play internal track, TestFlight)

**Tech stack**
- Kotlin
- Jetpack Compose
- Android InputMethodService
- Swift / SwiftUI
- iOS Keyboard Extension / App Groups
- AVFoundation (AVAssetWriter)
- Flutter (initial version)
- NestJS
- sharp
- Stability AI / Picsart APIs
- Vision-model image captioning
- AdMob + UMP consent
- Fastlane

---

### Aixercise – On-Device AI Exercise Coach *(Personal Project, live on App Store / Google Play)* <span class="tag tag-native">Kotlin Multiplatform</span>
**Solo Mobile Owner (Kotlin Multiplatform / Compose / Swift)**  
**07/2026 - Present**  
**Store:** [App Store](https://apps.apple.com/vn/app/ai-exercise-home-work-out/id6810195874) · [Google Play](https://play.google.com/store/apps/details?id=com.algorz.aiexercise.app)

Offline-first exercise coaching app that watches the user through the front camera, detects body pose in real time, and gives spoken corrections against a reference pose for 27 exercises across yoga, pilates, calisthenics, tai chi and stretching. All pose processing runs on-device: no video or landmark data leaves the phone.

**Key responsibilities**
- Rewrote the app from Flutter to native after diagnosing two Android issues the Flutter layer could not fix: the camera preview never rendered frames in AOT release builds, and the pose pipeline crashed inside the plugin's YUV conversion. Calling CameraX and ML Kit directly removed both failure points
- Migrated the native Android app to **Kotlin Multiplatform**: pose geometry, session state machine, content, localization, usage tracking and networking live in `commonMain`, shared by Android and iOS
- Built the pose pipeline: ML Kit Pose Detection (33 landmarks) → smoothing → frame-bounds and facing checks → joint-angle calculation → per-joint comparison against target angles and tolerances defined in bundled JSON content
- Designed the session state machine (ALIGNING → HOLDING → COMPLETED) with priority-queued voice coaching, a 900 ms persistence window and per-joint cooldown so brief arm movements do not trigger false warnings
- Integrated platform-specific pieces behind shared interfaces: CameraX / AVFoundation, Android TTS / AVSpeechSynthesizer, WorkManager reminders, Play Billing 9 / StoreKit subscriptions, AdMob
- Shipped release tooling: Fastlane for both stores, multi-locale release notes, R8 keep rules for reflection-instantiated classes

**Tech stack**
- Kotlin Multiplatform
- Jetpack Compose / Compose Multiplatform
- SwiftUI
- CameraX
- Google ML Kit Pose Detection
- Ktor client
- kotlinx.serialization / coroutines / datetime
- Navigation Compose
- WorkManager
- Play Billing Library 9
- StoreKit
- AdMob
- Fastlane

---

### Musaic – AI Image-to-Music Platform *(Personal Product, live on App Store / Google Play)* <span class="tag tag-hybrid">Flutter + NestJS + Vue</span>
**Solo Fullstack Owner (Flutter / NestJS / Vue SSR)**  
**12/2025 - Present**  
**Store:** [App Store](https://apps.apple.com/vn/app/lyraic-music-photo-to-song/id6792515277) · [Google Play](https://play.google.com/store/apps/details?id=com.algorz.musaic.app) (published as *Lyraic Music: Photo to Song*) · **1K+ downloads** on Google Play  

Production AI product that turns a user photo into an original music track. I own all three tiers end-to-end: the Flutter mobile app (released, v1.2.2), the NestJS API with async AI generation pipeline and credit monetization, and a Vue 3 SSR admin CMS for content and campaign operations.

**Mobile — Flutter (feature-based Clean Architecture)**
- Structured the app by feature (`auth`, `music_generation`, `credit`, `my_music`, `settings`), each split into `domain / data / application / presentation` layers with BLoC + Cubit, `get_it` DI, and `dartz` Either-based error handling
- Built a type-safe networking layer with Dio + Retrofit + Freezed/json_serializable code generation, and `go_router` guards for auth-protected navigation
- Implemented background audio playback with `just_audio` + `audio_service` + `audio_session`, including lock-screen controls and playback state recovery
- Maintained a **vendored FFmpeg fork** (`ffmpeg_kit_flutter_new_video`) driving a video composer that renders share-frame artifacts (track + artwork → MP4) for social sharing
- Delivered dual monetization: `in_app_purchase` credit top-up and `google_mobile_ads` rewarded-interstitial flow, with server-verified credit grants
- Handled platform compliance proactively — pinned `in_app_purchase_android` to the Play Billing 8.0 line ahead of the Play Store deadline
- Integrated Firebase Auth, Sign in with Apple, Google Sign-In, FCM push, Analytics, plus TikTok Events SDK attribution and `flutter_secure_storage` for token storage

**Backend — NestJS 11 + Prisma 7 + PostgreSQL**
- Designed a modular backend (`songs`, `ai`, `queue`, `credit`, `purchases`, `rewarded-ads`, `notifications`, `internal-admin`) with global exception filters, response transform interceptor, and role/auth guards
- Built the async generation pipeline on **BullMQ**: dedicated processors for audio generation, song-title generation, notification delivery, and campaign fan-out, with retry and terminal-failure handling
- Implemented a **transactional outbox pattern** (`SongTitleOutbox`, `NotificationOutbox`) with dedicated recovery services to guarantee at-least-once delivery when a worker or provider fails mid-flight
- Built the credit ledger with atomic deduction inside `prisma.$transaction`, plus idempotent automatic refund on terminal provider failure (guarded by `creditsRefundedAt`)
- Implemented server-side IAP verification and refund reconciliation against the Apple App Store Server API and Google Play Developer API, persisting store refund events
- Built an FCM notification system with per-category user preferences, campaign scheduling, and delivery-status tracking
- Hardened the API with Helmet, rate limiting, credential-redaction logging interceptor, `nestjs-i18n` (EN/ES), Terminus health checks, and Swagger contracts; covered by **28 Jest/supertest spec suites**

**Admin CMS — Vue 3 SSR**
- Built an SSR admin app (Vue 3 + Vite 7 + Express 5, PM2-deployed) layered as services → Pinia stores → views → components, covering user management, notification campaigns, content management, and a content-sync module with run history, diff review, and conflict resolution

**Tech stack**
- Flutter / Dart 3
- flutter_bloc / Cubit
- get_it
- dartz
- go_router
- Dio + Retrofit + Freezed
- just_audio / audio_service / audio_session
- FFmpeg (vendored native fork)
- in_app_purchase (Apple / Play Billing 8)
- google_mobile_ads (rewarded)
- Firebase Auth / Messaging / Analytics
- Sign in with Apple / Google Sign-In
- flutter_secure_storage
- NestJS 11
- TypeScript
- Prisma 7
- PostgreSQL
- BullMQ (Redis)
- Transactional Outbox pattern
- JWT + Passport + bcrypt
- ElevenLabs Music API
- OpenAI API
- Google Gemini (@google/genai)
- Apple App Store Server API / Google Play Developer API
- Firebase Admin (FCM)
- Google Cloud Storage
- Helmet / rate limiting / nestjs-i18n / Terminus
- Swagger / Jest / supertest
- Vue 3 + Pinia + Vite SSR + Express 5 + PM2

---

### Finhay - Hybrid Native (iOS/Android) + Flutter Fintech App <span class="tag tag-hybrid">Flutter + Native</span>
**iOS / Mobile Developer**  
**1/2026 - 05/2026**  
**Store:** [App Store](https://apps.apple.com/vn/app/finhay-daily-financial-app/id1336942463) · [Google Play](https://play.google.com/store/apps/details?id=vn.finhay.finhay) · **1M+ downloads** on Google Play, 21K+ App Store ratings  

Contributed to a hybrid fintech product where Native iOS and Flutter modules were integrated to deliver secure onboarding, authentication, and identity verification flows for financial users.

**Key responsibilities**
- Implemented end-to-end eKYC journeys including guided ID capture, NFC card reading, and liveness verification handoff
- Integrated and maintained enterprise SDKs for NFC identity reading and anti-spoofing face validation
- Built and maintained bridge flows between Native iOS view controllers and Flutter modules for seamless navigation and data passing
- Defined migration blueprint to move eKYC UI from Native iOS/Android to Flutter while keeping SDK-critical native components
- Designed MethodChannel contracts for NFC read, liveness open/result callbacks, and completion/cancel round-trip to caller screens
- Drove cross-platform flow consistency by enforcing replacement navigation after QR success and aligning behavior between iOS and Android
- Planned phased implementation and feature-flag rollout strategy with rollback-ready fallback to legacy native flow
- Developed and stabilized authentication flows, including social sign-in providers and secure token lifecycle handling
- Applied MVVM + Clean Architecture with clear boundaries between presentation, domain, and data layers
- Collaborated with backend, QA, product, and design teams to align business rules, API contracts, and release readiness
- Contributed to iOS engineering standards, onboarding documentation, and maintainability guidelines for team scaling

**Tech stack**
- Swift
- UIKit
- SwiftUI
- RxSwift
- Kotlin
- Flutter 3.32 / Dart 3
- flutter_bloc
- get_it
- MVVM
- Clean Architecture
- Keychain Services
- KeychainSwift
- Alamofire
- Certificate pinning (Alamofire)
- URLSession
- Retrofit2 + OkHttp
- RxJava2 / RxAndroid
- CocoaPods
- Swift Package Manager
- Fastlane
- Jenkins
- TestFlight
- Firebase App Distribution
- DeployGate
- Firebase Analytics/Crashlytics
- Social SSO (Apple / Google / Facebook)
- Native-Flutter module bridge
- go_router
- mobile_scanner
- Remote Config (feature flag rollout)

---



### Internet Banking - Individual Internet Banking <span class="tag tag-rn">React Native</span>
**Mobile Developer**  
**06/2025 - 1/1026**  
**Store:** [App Store](https://apps.apple.com/vn/app/msb-digital-bank/id6740115468) · [Google Play](https://play.google.com/store/apps/details?id=com.msb.digibank.retail) (*MSB Digital Bank*) · **100K+ downloads** on Google Play  

Contributing to a high-scale Individual Internet Banking product in a super-app ecosystem, where business domains are delivered as independent micro front-end modules and integrated at runtime.

**Key responsibilities**
- Build and maintain account domain features such as account inquiry, transaction history, account settings, and customer self-service flows
- Implement and optimize daily transaction journeys including transfer, bill payment, top-up, and savings flows with strong focus on reliability and UX consistency
- Develop module-level screens and business logic using Clean Architecture + MVVM-style hooks, with clear separation between presentation, domain, and data layers
- Integrate and maintain host-app contracts (navigation, auth context, HTTP client, popup/toast/spinner) for seamless super-app interoperability
- Contribute to release readiness across multiple environments (DEV/SIT/UAT/Pilot/Prod), including build configuration, dependency alignment, and production issue support

**Tech stack**
- TypeScript
- Micro front-end / Super app architecture
- Module Federation (@module-federation/enhanced)
- Repack + Rspack
- React Navigation v7
- Zustand, React Hook Form, Zod
- Firebase Analytics/Crashlytics/Messaging, Notifee

---

### ABC Education App <span class="tag tag-hybrid">RN + Native iOS</span>
**Technical Lead**  
**08/2024 **  
**Store:** [Google Play](https://play.google.com/store/apps/details?id=com.algorz.abeeci.app) (*ABeeCi*) · **1K+ downloads** on Google Play  

Educational application that integrates multiple subjects into a single platform for children, with parental control and in-app purchase support.

Built as a cross-platform education product with a strong native layer for parental control on iOS, including custom Screen Time modules, shield extension, and widget-based control flows.

**Key responsibilities**
- Led technical delivery from early-stage architecture to production rollout across mobile and backend tracks
- Designed and implemented iOS native Screen Time capabilities using custom modules integrated into React Native/Expo Dev Client
- Built and maintained native bridge contracts (NativeModules, RCTEventEmitter, Expo Module) for permission request, app selection, block/unblock state, and JS-native event flow
- Implemented iOS parental-control extension layer, including ShieldConfiguration customization and home-screen widget actions with shared state
- Coordinated app-group based data sharing between app and iOS extensions for reliable blocked-state synchronization
- Worked cross-functionally with design, BA, and backend to align business rules, API contracts, and release quality for child-focused features

**Tech stack**
- React Native
- Expo SDK / Expo Dev Client
- TypeScript
- Swift
- SwiftUI
- FamilyControls, ManagedSettings, DeviceActivity
- WidgetKit, AppIntents, ActivityKit
- RCTEventEmitter / NativeModules / Expo Modules Core
- iOS App Groups (shared UserDefaults), CocoaPods
- Kotlin
- ReactJS
- Vite
- NestJS
- MongoDB
- Google Cloud
- JWT
- Mongoose
- Passport
- Swagger
- Express
- CI/CD

---

### ViettelPay Pro <span class="tag tag-native">Native iOS</span>
**iOS Developer**  
**10/2024 - 02/2025**  
**Store:** [App Store](https://apps.apple.com/vn/app/viettelpay-pro/id1265352466) · [Google Play](https://play.google.com/store/apps/details?id=com.bplus.bankplusbccs) · **1M+ downloads** on Google Play, 4K+ App Store ratings  

Enterprise iOS application for ViettelPay Pro channel and agent operations, supporting service payment, money transfer, recharge, transaction tracking, pending payment records, reporting, support, and operational self-service flows for sales points and business users.

**Key responsibilities**
- Built and refined the waiting and pending payment records feature, including search, add-to-waiting-list, delete, select-all, view-all, and batch payment entry points
- Implemented transaction history screens with date, status, and phone filters, including selectable failed or pending records that can be retried through the shared payment flow
- Refactored payment transaction flow around a shared base payment view controller to reduce repeated screen logic across pending and history transaction modules
- Integrated SOAP/XML and newer Swift API flows through existing `APIClient` and `VTPPApiManager` layers, including commands for telco debit lookup, waiting-payment processing, and payment execution
- Built UIKit/XIB interfaces for transaction list cells, search forms, result states, empty states, maintenance states, and payment action states using the project's existing UI components
- Worked with a hybrid Objective-C/Swift codebase and coordinated with Android, backend, BA, PM, and QA to clarify requirements, fix production issues, and stabilize release behavior

**Representative features**
- Waiting payment records and batch payment flow
- Transaction history search and retry flow
- Telecom debit lookup and payment preparation
- Money transfer, recharge, and Mobile Money-related service journeys
- Agent and channel operational modules including reporting, support, and service management
- Environment-specific enterprise build and distribution workflow support

**Tech stack**
- Swift
- Objective-C
- UIKit
- XIB
- Auto Layout
- SnapKit
- MVC / shared base view controllers
- SOAP/XML API
- Alamofire
- AFNetworking
- Firebase Crashlytics / Analytics / Performance / Messaging
- VDS tracking
- Realm / FMDB
- Keychain
- Fastlane / AppCenter

---

### Netchat <span class="tag tag-flutter">Flutter</span>
**Mobile Developer**  
**09/2024 - 12/2024**

Flutter-based AI chatbot application integrated with server APIs and built around business documentation requirements.

**Key responsibilities**
- Worked independently on the mobile side
- Coordinated with BA, BE, QA, and client stakeholders
- Optimized chatbot-related functionality
- Fixed UI issues based on SRR and design documents
- Maintained and improved production behavior

**Tech stack**
- Flutter
- BLoC
- Cubit
- EventFlux
- Dio
- Clean Architecture
- AI chatbot integration

---
### Mydio (**Viettel Software**) <span class="tag tag-hybrid">RN + Native</span>
**Mobile Developer**  
**04/2024 - 10/2024**

National-scale audio and audiobook streaming product for Viettel Telecom, focused on audio discovery, chapter-based listening, background playback, VIP/package purchase, listening logs, and cross-platform media performance.

**Key responsibilities**
- Designed and maintained a Clean Architecture-style React Native codebase organized by bounded contexts such as audio, home, library, user, authentication, favorite, and explore
- Built audio domain flows including audio detail, chapters, albums, series, similar content, like/rating, VIP checks, pre-signed audio links, and listening log synchronization
- Refined mini player and full player behavior, including app-state synchronization, mini-player visibility, bottom-position animation, chapter-change handling, and playback state recovery
- Worked with native Android playback service using ExoPlayer, media session, notification controls, and background playback behavior
- Integrated subscription/package purchase flow with in-app purchase SDK events, product fetching, transaction completion, and backend subscription updates
- Supported build and release quality through linting, bundle scripts, native configuration, Fastlane-related setup, and cross-functional coordination with product, backend, QA, and mobile teams

**Tech stack**
- **React Native**
- TypeScript
- **Expo Dev Client**
- Swift
- Java
- XML View
- ExoPlayer
- **React Native Track Player**
- MobX
- Context API
- InversifySugar
- Axios
- Firebase Analytics / Crashlytics / Dynamic Links / Messaging
- In-app purchase
- **React Navigation**
- Fastlane

---
 

### MyClip (**Viettel Software**) <span class="tag tag-hybrid">RN + Native Bridge</span>
**Mobile Developer**  
**11/2023 - 03/2024**

Viettel Telecom media product focused on short video, video playback, recording, upload, offline viewing, karaoke, livestream-related experiences, and native video editing capabilities.

**Key responsibilities**
- Built React Native video features across recording, gallery selection, upload, trimming, cut-video flow, text overlay, speed control, preview, and draft/offline handling
- Integrated and maintained a custom native video editor bridge using Swift, Objective-C++, Kotlin, and FFmpeg to support trimming, splitting, merging, quality reduction, text insertion, save-to-library, and platform-specific file handling
- Implemented video playback and interaction flows including video detail, related videos, quality selection, comments, popup settings, fullscreen controls, and streaming behavior
- Worked on feature modules for shorts, channel detail, karaoke, livestream, favorite videos, search, account history, and downloaded/offline videos
- Helped coordinate implementation direction and task allocation for middle and junior developers while keeping UI, hooks, services, and native integration responsibilities separated
- Supported release workflow through native iOS/Android configuration, Fastlane scripts, AppCenter/TestFlight-related commands, patch-package, and GitLab CI conventions

**Tech stack**
- **React Native**
- TypeScript
- Android Kotlin
- iOS Swift
- Objective-C
- Redux
- FFmpeg

---

### Raise 2.0 <span class="tag tag-flutter">Flutter</span>
**Mobile Developer**  
**05/2023 - 08/2023**

Parent-focused educational and activity application that supports child development across multiple learning dimensions.

**Key responsibilities**
- Developed Flutter application features across UI, networking, data, media, and analytics
- Helped maintain a clean modular architecture suitable for long-term product expansion

**Tech stack**
- Flutter
- Dart
- Clean Architecture
- Riverpod
- go_router
- Dio
- dartz
- Firebase
- easy_localization
- flutter_screenutil
- just_audio
- video_player
- chewie
- Branch SDK
- sentry_flutter

---

### OTM System <span class="tag tag-rn">React Native</span>
**Tech Lead / Product Owner**  
**08/2023 - 11/2024**

Personal project combining mobile and backend development with a product ownership mindset.

**Project overview**
- Mobile application built with React Native
- Backend built with Node.js and NestJS
- PostgreSQL as the primary database
- Focused on applying design patterns and clean software principles

**Tech stack**
- React Native
- Node.js
- NestJS
- PostgreSQL
- React Vite

---

### Thanh Hung <span class="tag tag-rn">React Native</span>
**Mobile Team Leader**  
**05/2022 - 05/2023**

Transport and moving service platform supporting immediate and scheduled bookings, survey/pricing assistance, payment, driver tracking, and promotions.

**Key responsibilities**
- Led mobile delivery in a product with booking and logistics workflows
- Worked on team coordination and feature execution for a service-heavy application

**Tech stack**
- TypeScript
- React Native
- Sendbird
- Context API
- Hooks
- Redux Toolkit
- RTK Query

---

### Liên Minh Vận Tải <span class="tag tag-rn">React Native</span>
**Project Manager**  
**04/2023 - 11/2023**

Transportation booking and e-commerce related project where I took project leadership responsibilities.

**Key responsibilities**
- Managed project progress across teams
- Organized meetings and task assignment
- Coordinated delivery and technical support
- Contributed hands-on when needed across mobile, web, testing, and backend workstreams

**Tech stack**
- React Native
- Scrum
- Jira
- Product coordination

---

### BDS Recbook <span class="tag tag-rn">React Native</span>
**Mobile Developer**  
**09/2022 - 11/2022**

Social network-oriented product for the real estate market, aligned with sharing economy concepts.

**Tech stack**
- TypeScript
- React Native
- Android Java
- Context API
- Hooks
- Redux Saga
- Redux Toolkit

---

### QRCheckin - Manager OT, Onsite, Personal Detail <span class="tag tag-flutter">Flutter</span>
**Mobile Developer**  
**06/2022 - 09/2022**

Enterprise Flutter application for company entry/exit workflows, OT management, contractor and guest check-ins, approvals, attachments, and notifications.

**Key responsibilities**
- Owned feature delivery end-to-end: scope, design, implementation, and tests
- Integrated secure networking with Dio and environment-based configuration
- Implemented authentication, session storage, and secure token handling
- Built responsive UI and maintained localization/theming consistency
- Integrated OneSignal across iOS and Android, including iOS extension-related setup
- Set up observability with Sentry and logging
- Managed release activities such as iOS archives, Android builds, app icons, and assets

**Tech stack**
- Flutter
- Nylo
- MobX
- Dio
- flutter_secure_storage
- OneSignal
- sentry_flutter
- build_runner
- horizontal_data_table
- photo_view
- image_picker
- image_cropper

---

### EPS - Manager OT, Onsite, Personal Detail <span class="tag tag-flutter">Flutter</span>
**Mobile Developer**  
**04/2021 - 06/2022**

Application for managing OT, onsite workflows, and personal information using Flutter.

**Tech stack**
- Flutter
- Provider
- BLoC

---

### Hiron iOS <span class="tag tag-native">Native iOS</span>
**Mobile Developer**  
**11/2022 - 04/2023**

iOS application combining e-commerce and medical appointment booking features.

**Key responsibilities**
- Translated Zeplin designs into UIKit-based interfaces
- Worked with SnapKit and structured iOS UI implementation

**Tech stack**
- Swift
- UIKit
- SnapKit
- Alamofire
- MVVM

---

### Radar Index <span class="tag tag-rn">React Native</span>
**Mobile Developer**  
**01/2022 - 06/2022**

Crypto-focused product designed to help traders monitor token signals and market changes.

**Tech stack**
- TypeScript
- React Native
- Android Java
- Context API
- Hooks
- Redux
- Redux Saga
- Redux Toolkit

---

### IBeauty - Shopping & Affiliate <span class="tag tag-rn">React Native</span>
**Mobile Developer**  
**03/2020 - 05/2021**

Beauty commerce and affiliate platform that helps users purchase products and generate income opportunities through a referral-oriented flow.

**Tech stack**
- JavaScript
- React Native
- Redux Thunk
- Firebase Authentication

---

### Mantel - Enterprise Hotline <span class="tag tag-rn">React Native</span>
**Mobile Developer**  
**06/2018 - 07/2019**

Enterprise call center application that provides free calling features for customer service departments.

**Tech stack**
- React Native
- Redux Thunk
- SIP calling integration

---

### Health Record <span class="tag tag-flutter">Flutter</span>
**Personal Project | Software Developer**

Health parameter management application that applies blockchain concepts to medical and patient information management.

**Key responsibilities**
- Built a blockchain proof of concept
- Connected to Sepolia infrastructure to call smart contracts
- Developed UI and application logic
- Wrote smart contracts to interact with the Ether network

**Tech stack**
- Flutter
- Dart
- SQLite
- Provider
- get_it
- shared_preferences
- Solidity

---

## Education

### Vietnam Academy of Cryptography Techniques
**Information Assurance**  
**2016 - 2022**

- Graduation standing: Good
- Academic focus included secure systems and information assurance

---

## Research / Publication

### Graduation Thesis
**Building a Secure Document Management System Using Blockchain Technology**

- Thesis archive score: 9.5 / 10
- Focused on building a secure and stable document management system using blockchain technology
- Prioritized security, stability, immutability, and high availability
- Worked with technologies such as Hyperledger Indy, Aries, Hyperledger Fabric, IPFS, Python, Node.js, TypeScript, React, and related secure-system design concepts

---

## Awards & Recognition

### Employee of the Year
**OWS Vietnam**

- Recognized as Outstanding Employee of the Year during my university period while working full-time

---

## Languages

- Vietnamese: Native
- English: Basic working communication

---

## Interests

- Board games, Pickle Ball, Football, Badminton
