# Tổng hợp lý thuyết Flutter & bộ câu hỏi phỏng vấn — rút từ project Musaic

Nguồn: `mobile-app-image-music` (Flutter 3.7+, ~21.500 dòng Dart / 194 file, Clean Architecture + BLoC).
Mỗi mục đều neo vào code thật trong repo để bạn có thể mở ra đối chiếu khi phỏng vấn.

---

# PHẦN A — BẢN ĐỒ LÝ THUYẾT CÓ TRONG PROJECT

## A1. Dart ngôn ngữ (nền tảng)
Code thật dùng khá nhiều feature Dart 3:
- **Sealed class + exhaustive switch**: `sealed class MyMusicShareState` / `final class ...` trong `my_music_share_cubit.dart`.
- **Switch expression + pattern matching**: `_mapProcessingState` (`app_audio_handler.dart`), `final FormData formData => formData.clone()` (`auth_interceptor.dart`), `AppEnvironment.baseUrl`.
- **Null safety**: `?.`, `??`, `late final`, không dùng `!` (theo convention repo).
- **`const` vs `final` vs `static const`**: token design system, `String.fromEnvironment` (compile-time const).
- **Future / Stream / async-await**: khắp data layer; `Completer` để bridge callback → Future (`rewarded_interstitial_ads_service.dart`, `_refreshToken`).
- **Isolate**: `Isolate.run` cho decode/resize ảnh (`core/utils/image_compression_utils.dart`).
- **Extension / mixin**: `with QueueHandler, SeekHandler` (`AppAudioHandler`), extension của `responsive_sizer` (`.w/.h/.sp`).
- **Functional error handling**: `Either<String, T>` (dartz) + `fold`.
q
## A2. Widget & rendering
- `StatelessWidget` vs `StatefulWidget` (27 file stateful), `initState/dispose` (17 file `dispose()`), `mounted` guard (16 file).
- `WidgetsBinding.instance.addPostFrameCallback` (5 chỗ) — vì sao cần: đọc size/hiển thị overlay sau frame đầu.
- `CustomPainter`: mask lỗ khoét onboarding (`core/guide/app_guide_overlay.dart`), bubble tail (`photo_insert_hint_chip.dart`).
- `Overlay`/`OverlayEntry`: toast (`core/utils/app_toast.dart`), guide overlay.
- `LayoutBuilder` (8 file), `MediaQuery` (16 file), `SafeArea` (15 file), `Stack/Positioned` cho layout "pixel-tuned".
- **Clamp text scale toàn app**: `MediaQuery.textScaler.clamp(maxScaleFactor: 1.3)` trong `main.dart` — chủ đề accessibility + layout overflow rất đáng hỏi.
- `AnimationController` + `SingleTickerProviderStateMixin` (3 file), `AnimatedBuilder`, `AnimatedSwitcher`.
- `GestureDetector` (20 file), long-press ẩn để đổi môi trường dev/prod.
- `flutter_staggered_grid_view`, `flutter_svg`, `screenshot` (render widget → PNG off-screen).

## A3. State management (BLoC/Cubit) — vùng mạnh nhất
- **Bloc (event-driven) vs Cubit (method-driven)**: `MusicGenerationBloc`, `MyMusicPlayerBloc` vs `MusicCreationCubit`, `CreditPurchaseCubit`, `MyMusicShareCubit`.
- `MultiBlocProvider` + `MultiBlocListener` ở root (`main.dart`) để cross-bloc communication (profile → credits, logout → clear).
- **Private event pattern**: stream ngoài (`playbackState`, `AudioService.position`) → `add(_PlaybackStateChanged(...))` thay vì emit trực tiếp (`my_music_player_bloc.dart`).
- **Cleanup**: override `close()` cancel `StreamSubscription` (3 file).
- `emit.isDone` guard trong vòng polling dài (`music_generation_bloc.dart`) — chống "emit after close".
- `Equatable` (14 file) cho state comparison; `copyWith` + `clearXxx` flag để phân biệt "không đổi" vs "set null".
- `buildWhen`, `context.select` để giảm rebuild (có nhưng ít — chỉ 1–2 chỗ).

## A4. Dependency Injection
- `get_it` thủ công trong `core/dependency_injection.dart`; **không** dùng `injectable`.
- `registerSingleton` vs `registerLazySingleton` vs `registerFactory` — factory cho Cubit theo màn hình (`MyMusicShareCubit`, `CreditPurchaseCubit`), lazySingleton cho repo/datasource.
- Thứ tự init có phụ thuộc: `AudioService.init` phải chạy trong `init()` và trả về `AudioHandler` + `AppAudioHandler` (2 registration cho 1 object).
- Mock swap bằng compile-time flag: `bool.fromEnvironment('USE_MOCK_CREDIT')`.
- Service locator bị gọi *ngay trong interceptor* (`sl<AuthRepository>()`) → chủ đề circular dependency / anti-pattern để tranh luận.

## A5. Navigation (go_router)
- `GoRouter` với `redirect` guard (auth), `refreshListenable` = `StreamListenable extends ChangeNotifier` bọc `authBloc.stream`.
- `ShellRoute` bọc toàn bộ route đã đăng nhập.
- `CustomTransitionPage` + `FadeTransition` + `opaque: false` cho modal-page (`AppRoutes.generatedSong`).
- Truyền tham số bằng `state.extra as MusicTrack` — **không type-safe**, chủ đề đáng hỏi (so với path/query param hoặc typed routes).

## A6. Networking (Dio) — vùng mạnh thứ hai
`core/network/auth_interceptor.dart` (338 dòng) là mỏ vàng câu hỏi:
- `Interceptor` vs **`QueuedInterceptor`** (tuần tự hóa request để refresh token không bị race).
- **Single-flight refresh token**: `static Future<bool>? _refreshingTokenFuture` + `Completer` → nhiều 401 đồng thời chỉ refresh 1 lần.
- **Chống retry loop vô hạn**: cờ `extra['isRetry']`.
- **`FormData.clone()` khi retry** — stream đã bị listen không thể replay.
- Chuyển `skipAuth` từ header → `options.extra` (không leak flag nội bộ ra backend).
- **Unwrap envelope** `{code, data, message}` ở `onResponse`, coi `code` là status thật kể cả HTTP 200.
- Map `DioExceptionType` → exception có ngữ nghĩa: `ConnectException`, `TimeoutException`, `CancelException`, `ResponseException` (override `toString()` để UI hiển thị message backend).
- Chỉ refresh với 401, **cố ý không** refresh 403 (403 = business error như insufficient credits).
- Timeout tách biệt: connect 30s / send-receive 45s.
- Multipart upload (`FormData` + `MultipartFile.fromFile`), filter logger để không dump binary.

## A7. Data layer & codegen
- Clean Architecture 3 lớp: `domain/` (entity freezed + abstract repository) → `data/` (model + datasource + impl) → `presentation/`.
- **Model ≠ Entity**: `SongStatusModel` (JSON) vs `SongStatus` (domain) — vì sao tách.
- Codegen: `freezed` (9 file), `json_serializable` (16 file), `retrofit` `@RestApi` (2 file: `auth_api`, `credit_api`), `build_runner` — file `.g.dart`/`.freezed.dart` commit vào git.
- Hai style datasource cùng tồn tại: retrofit (`AuthApi`) và Dio thủ công (`MusicRemoteDataSourceImpl`) — hỏi trade-off.
- Mock repository (`mock_music_repository.dart`, `mock_credit_repository.dart`) cho offline dev.

## A8. Luồng async dài (ticket + polling)
`music_generation_bloc.dart`: `POST /songs/generate` → ticket → poll `/songs/{id}/status` mỗi 5s, tối đa 120 lần (10 phút) → success/failed/timeout.
- Vì sao polling chứ không WebSocket/FCM; fixed interval vs exponential backoff.
- `shouldRefundCredits` khi lỗi — nghiệp vụ trừ credit trước/sau.
- Side-effect không được làm fail luồng chính (`try { save source images } catch (_) {}`).
- Bloc sống lâu hơn màn hình → cần `emit.isDone`, và `MusicGenerationListener` ở root để tiếp tục lắng nghe khi user rời màn.

## A9. Platform channel & native
- `MethodChannel('com.algorz.musaic.app/downloads')` (Dart: `my_music_player_bloc.dart`; Kotlin: `MainActivity.kt`).
- Native: **MediaStore Downloads API** (scoped storage Android 10+, không cần `WRITE_EXTERNAL_STORAGE`), `result.success/error/notImplemented`.
- `MainActivity` phải extends **`AudioServiceActivity`** (yêu cầu của audio_service) — kiến thức plugin-native coupling.
- `Platform.isIOS` (7 file) cho ad unit / IAP khác nhau; `kIsWeb` (3 file).
- iOS: `AppDelegate.swift`, `Podfile`, `sign_in_with_apple` + `crypto` (nonce SHA256).

## A10. Audio nền + OS media controls
`core/audio/app_audio_handler.dart`: `BaseAudioHandler with QueueHandler, SeekHandler`.
- `audio_service` (foreground service / notification / lockscreen) + `just_audio` (engine) + `audio_session` (`AudioSessionConfiguration.music()` — ducking, interruption).
- `ConcatenatingAudioSource` cho queue, `currentIndexStream`, `durationStream` (duration chỉ biết sau khi load → update `MediaItem`).
- `_broadcastState`: map `ProcessingState` → `AudioProcessingState`, `MediaControl`, `androidCompactActionIndices`.
- `AudioServiceConfig`: notification channel, `androidStopForegroundOnPause`.

## A11. Media processing / FFmpeg (đặc sản project)
`ffmpeg_video_composer.dart` (963 dòng) — ảnh + mp3 → mp4 share:
- Một pass `filter_complex`: background → xfade slideshow → alpha mask bo góc → chrome overlay → `drawtext` countdown.
- **Tối ưu hiệu năng có đo lường**: downscale ảnh gốc 1 lần + cache thay vì để FFmpeg re-decode mỗi frame; flatten overlay tĩnh thành 1 PNG để bỏ 1 filter/frame; probe bằng `-f null` để tách chi phí filter graph vs encode.
- Fallback nhiều encoder khi thiết bị không hỗ trợ.
- `Stopwatch` timing log, timeout từng bước (`_withStepTimeout`).
- Asset → file thật (`rootBundle` → `path_provider` temp) vì FFmpeg native không đọc được asset bundle.
- `drawtext` + `textfile=` + `expansion=none` để tránh bug UTF-8 multi-byte của build vendored.
- Vendored plugin ở `third_party/` (path dependency) do package upstream bị retire.

## A12. In-App Purchase (production-grade)
`iap_datasource.dart` + `credit_purchase_cubit.dart` (363 dòng):
- **Consumable** vs non-consumable; `buyConsumable(autoConsume)`; **bắt buộc `completePurchase`** nếu không store redeliver mãi.
- `purchaseStream` phải listen sớm (purchase pending từ lần chạy trước được redeliver).
- **Server-side verification**: verify ở backend trước, chỉ `completePurchase` sau khi đã cộng credit.
- `applicationUserName` = userId → backend đối chiếu `appAccountToken` chống gán sai user.
- `notFoundIDs` → ẩn pack thay vì crash (lỗi cấu hình store).
- **Race condition thật**: purchase update về trước khi `/products` load xong → buffer `_bufferedUpdates` rồi replay (`_drainBufferedUpdates`).
- Play Billing v8 constraint ghi rõ trong `pubspec.yaml` (deadline 2026-08-31) — kiến thức release/policy.

## A13. Ads (AdMob) + reward
`rewarded_interstitial_ads_service.dart`:
- Rewarded interstitial, `FullScreenContentCallback`, `onUserEarnedReward`, `Completer` chờ dismiss, `ad.dispose()`.
- **SSV (server-side verification)**: `ServerSideVerificationOptions(userId, customData)`; callback đi thẳng Google → backend, client chỉ báo "đã xem".
- Test device IDs vs test ad unit — vì sao phải dùng ad unit thật để SSV fire.
- Load timeout 5s.

## A14. Firebase / Auth
`firebase_core`, `firebase_auth`, `google_sign_in` v7, `sign_in_with_apple`, OTP, `firebase_messaging`.
- Push: `PushNotificationService.registerCurrentDevice()` chỉ gọi sau `AuthAuthenticated` (BlocListener ở root) — device token gắn với user.
- Deactivate account flow (commit gần đây), refresh token + logout khi refresh fail.

## A15. Storage & cache
- `flutter_secure_storage` (Keychain/Keystore) cho token vs `shared_preferences` cho preference (`AppSP`, `AppEnvironmentStore`).
- `path_provider`: temp vs app support vs cache dir; cache share artifacts (`my_music_share`) và script clear cache trong `pubspec.yaml`.
- `SongSourceImagesStore` — map songId → ảnh gốc local để tái tạo slideshow lúc share.

## A16. Theme / Design system
- Material 3 (`useMaterial3: true`), `ColorScheme` light/dark, `ThemeData` component themes (`elevatedButtonTheme`, `inputDecorationTheme`, `dividerTheme`).
- Design tokens tách riêng (`design_system/tokens`: colors/spacing/typography) + component library (`buttons`, `chips`, `inputs`, `diamond`...).
- `google_fonts` + `TextTheme` mapping; `responsive_sizer` cho scale theo màn hình.
- Lưu ý thực tế: `darkTheme` đã viết nhưng `themeMode: ThemeMode.light` hard-code — chủ đề hay để hỏi "dead config".

## A17. Build, config, release
- **`--dart-define-from-file=dart_define.json`** + `String.fromEnvironment` cho secret (Shake key, AdMob unit) — không hardcode, không commit.
- Environment switch runtime (dev/prod) lưu vào SharedPreferences, đổi bằng gesture ẩn — thay cho flavor.
- `version: 1.1.19+53` → versionName/versionCode & CFBundleShortVersionString/CFBundleVersion.
- `flutter_native_splash`, `flutter_launcher_icons`, keystore signing (`key.properties`), Gradle.
- **CI/CD**: `.gitlab-ci.yml` (stages test/build/deploy, fvm pin 3.44.8, keystore từ base64 CI var) + `fastlane` cho cả Android & iOS.
- `flutter_native_splash` phải là dependency thường (không dev_dependencies) vì `GeneratedPluginRegistrant` — kiến thức plugin native registration.
- FVM (`.fvmrc`) để pin SDK.

## A18. Testing (vùng yếu)
Chỉ 4 file test, đều là unit/pure-Dart + widget test nhẹ:
`music_options_model_test.dart`, `music_option_selection_test.dart`, `widget_test.dart`, `choose_music_genre_selector_test.dart`.
- Có: `group/test/expect`, JSON parsing test, default-value test, widget test cơ bản.
- Không có: `bloc_test`, `mocktail/mockito`, golden test, `integration_test`, test cho interceptor/polling/IAP (chính là phần logic khó nhất).

---

# PHẦN B — BỘ CÂU HỎI PHỎNG VẤN (kèm đáp án gợi ý)

## B1. Junior / Fresher (nền tảng — 15 câu)
1. `StatelessWidget` vs `StatefulWidget`? Khi nào bắt buộc stateful? → giữ state qua rebuild, cần `AnimationController`/controller/subscription.
2. Vòng đời `State`: `initState → didChangeDependencies → build → didUpdateWidget → dispose`. Vì sao phải `dispose()` controller/subscription? (memory leak, `setState` after dispose).
3. `mounted` dùng để làm gì, tại sao code này check `if (!mounted) return` sau `await`?
4. `const` constructor giúp gì cho performance? (canonicalize + skip rebuild subtree).
5. `Key` để làm gì? Khi nào cần `GlobalKey` (project dùng 6 chỗ) và vì sao GlobalKey đắt?
6. `Future` vs `Stream`; `async/await` vs `then`.
7. `Column` bị overflow → xử lý thế nào? (`Expanded`, `Flexible`, `SingleChildScrollView`, `FittedBox`).
8. `MediaQuery.of(context)` gây rebuild khi nào? Khác gì `LayoutBuilder`?
9. `ListView` vs `ListView.builder` — vì sao builder tốt hơn với list dài?
10. Null safety: `?`, `!`, `late`, `??`, `?.`. Vì sao repo cấm dùng `!`?
11. Asset khai báo ở đâu, `rootBundle.load` khác `Image.asset` thế nào?
12. `SafeArea` giải quyết vấn đề gì?
13. `BuildContext` là gì? Vì sao không dùng context sau `await` mà không check `mounted`?
14. Hot reload vs hot restart — cái nào mất state?
15. `pubspec.yaml`: dependencies vs dev_dependencies. Đọc case `flutter_native_splash` trong repo và giải thích vì sao nó **không** được để ở dev_dependencies.

## B2. Middle (kiến trúc & luồng dữ liệu — 20 câu)
16. Clean Architecture 3 lớp trong repo: entity ở đâu, model ở đâu, ai được import ai? Vì sao domain không được biết Dio?
17. Vì sao tách `SongStatusModel` (data) và `SongStatus` (domain) thay vì dùng 1 class?
18. BLoC vs Cubit — trong repo cái nào dùng Bloc, cái nào dùng Cubit, tiêu chí chọn?
19. `BlocProvider` vs `BlocProvider.value`; khi nào bloc bị dispose tự động?
20. `BlocBuilder` vs `BlocListener` vs `BlocConsumer` vs `BlocSelector`. Repo dùng `MultiBlocListener` ở root để làm gì?
21. Giảm rebuild trong BLoC: `buildWhen`, `context.select`, tách widget nhỏ. Repo làm tốt chưa?
22. Vì sao state cần `Equatable`? Bug gì xảy ra nếu state mutable và emit cùng instance?
23. `copyWith` không phân biệt được "không truyền" và "truyền null" — repo giải quyết bằng cách nào? (`clearDownloadMessage`, `clearPurchasing`).
24. get_it: `registerSingleton` vs `registerLazySingleton` vs `registerFactory`. Vì sao Cubit theo màn hình dùng `factory` còn repository dùng `lazySingleton`?
25. `AudioService.init` trả về 1 object nhưng register 2 type (`AudioHandler` + `AppAudioHandler`) — vì sao? Đánh đổi gì?
26. Dio `Interceptor` — thứ tự `onRequest/onResponse/onError`, `handler.next/resolve/reject` khác nhau thế nào?
27. Vì sao interceptor này unwrap `{code, data, message}` ở `onResponse` và reject khi `code` ngoài 2xx dù HTTP là 200? Ưu/nhược?
28. `go_router` `redirect` guard hoạt động ra sao? `refreshListenable` cần gì (`Listenable`) và repo bridge từ `Stream` sang bằng cách nào?
29. `ShellRoute` khác `GoRoute` lồng nhau thế nào? Khi nào cần `StatefulShellRoute`?
30. Truyền object qua `state.extra as MusicTrack` — rủi ro gì (deep link, restore state, cast lỗi)? Cách làm tốt hơn?
31. `Either<String, T>` (dartz) vs throw exception vs `Result` sealed class — trade-off? Repo mất gì khi left chỉ là `String`?
32. `freezed` sinh ra gì (`copyWith`, `==`, `toString`, union)? Vì sao commit file `.g.dart` vào git?
33. Retrofit `@RestApi` vs viết Dio thủ công — repo tồn tại cả hai, khi nào chọn cái nào?
34. Upload nhiều ảnh: `FormData` + `MultipartFile.fromFile`, field lặp key `images` — server nhận thế nào? Vì sao phải filter logger cho multipart?
35. Secure storage vs SharedPreferences — cái gì lưu ở đâu và vì sao token không được để trong SharedPreferences?

## B3. Senior (bug thật, race condition, performance — 20 câu)
36. **Refresh token race**: 5 request song song đều 401. Nếu mỗi request tự refresh thì sao? Repo giải quyết bằng `QueuedInterceptor` + `static Future<bool>? _refreshingTokenFuture`/`Completer` — giải thích cơ chế single-flight và điểm còn hở (biến static, `finally` set null).
37. Vì sao phải có cờ `extra['isRetry']`? Không có thì hệ quả gì?
38. **`FormData` retry**: vì sao phải `formData.clone()`? Lỗi cụ thể là gì nếu không clone?
39. Vì sao chỉ refresh với 401 mà **không** với 403? Liên hệ business (insufficient credits).
40. Interceptor gọi `sl<AuthRepository>()` — vấn đề gì về circular dependency/testability? Cách refactor?
41. **`emit` sau khi bloc close**: `MusicGenerationBloc` poll tới 10 phút, user rời màn → repo dùng `emit.isDone`. Còn cách nào (cancel token, `CancelableOperation`, `StreamSubscription` trong `close()`)?
42. Vòng poll 5s × 120: nhược điểm gì (battery, server load, không sống khi app bị kill)? So sánh với WebSocket / FCM data message / `WorkManager`.
43. Bloc sống lâu hơn màn hình: `MusicGenerationListener` bọc ở root `builder` của `MaterialApp`. Ưu/nhược so với đặt provider theo route?
44. Stream ngoài (`playbackState`) → tại sao push qua **private event** `add(_PlaybackStateChanged(...))` thay vì `emit` trực tiếp trong listener?
45. **IAP race thật trong repo**: `purchaseStream` bắn update trước khi `/products` load xong → `packageId` rỗng. Repo buffer rồi replay. Bạn sẽ thiết kế khác thế nào?
46. Vì sao **bắt buộc** `completePurchase` và chỉ gọi *sau* khi backend verify? Nếu gọi trước thì mất mát gì? Nếu không gọi thì sao?
47. Vì sao phải gửi `applicationUserName` = userId? Tấn công nào bị chặn?
48. AdMob **SSV** hoạt động ra sao? Vì sao không tin client báo "đã xem xong"? Vì sao test ad unit không fire SSV?
49. **Isolate**: `Isolate.run` trong `image_compression_utils.dart` — vì sao closure phải capture bytes chứ không phải `File`? Cái gì được truyền qua isolate (SendPort, copy vs TransferableTypedData)? Khi nào `compute` là đủ?
50. **Tối ưu FFmpeg**: bài học "downscale 1 lần + cache" và "flatten overlay tĩnh" — giải thích chi phí per-frame vs one-time. Cách đo (`-f null`, `Stopwatch`).
51. Vì sao asset phải copy ra file thật trước khi đưa cho FFmpeg?
52. `MethodChannel`: luồng Dart ↔ Kotlin, chạy trên thread nào, `result.error` map sang exception gì ở Dart? Làm sao xử lý `notImplemented`/version guard (Android <10)?
53. Scoped storage: vì sao dùng `MediaStore.Downloads` thay vì `WRITE_EXTERNAL_STORAGE`? Fallback cho Android 9 là gì?
54. `MainActivity extends AudioServiceActivity` — vì sao plugin bắt buộc điều này? Điều gì xảy ra nếu quên?
55. Background audio: chuỗi `just_audio` + `audio_service` + `audio_session` mỗi thành phần lo gì? Xử lý interruption (cuộc gọi đến), ducking, và kill process trên iOS/Android khác nhau ra sao?
56. `MediaItem.duration` ban đầu null → repo update qua `durationStream`. Vì sao phải update cả `queue` lẫn `mediaItem`?
57. Clamp `textScaler` toàn app ở `maxScaleFactor: 1.3` — đúng hay là code smell? Đánh đổi accessibility vs layout.
58. Secret qua `String.fromEnvironment` — có an toàn tuyệt đối không? (Vẫn nằm trong binary, có thể strings/reverse). Cách bảo vệ thật.
59. Env switch dev/prod bằng SharedPreferences + gesture ẩn thay vì flavor — rủi ro gì trên build production? Bạn sẽ dùng flavor/scheme thế nào?
60. Codegen (`build_runner`) trong CI: `.g.dart` commit vào git — ưu/nhược so với generate trong pipeline?

## B4. Câu hỏi "code review" (dùng chính repo làm bài test)
61. `themeMode: ThemeMode.light` hard-code trong khi `darkTheme` được build đầy đủ — bug hay ý đồ? Sửa thế nào cho đúng?
62. `debugShowCheckedModeBanner: true` ở release — ảnh hưởng gì?
63. `StreamListenable` listen stream nhưng **không bao giờ cancel** — leak không? Vì sao (mức app-lifetime) và vẫn nên sửa thế nào?
64. `MusicRemoteDataSourceImpl` bọc mọi lỗi thành `Exception(String)`, làm mất status code đã được interceptor map — hệ quả gì ở UI?
65. `ffmpeg_video_composer.dart` 963 dòng trong 1 class — bạn tách thế nào mà không phá performance đã tối ưu?
66. Bloc polling `Future.delayed(5s)` không cancel được — viết lại bằng gì để test được và stop ngay?
67. Test suite hiện tại (4 file) không cover interceptor / polling / IAP. Viết test cho `AuthInterceptor` refresh-race bằng cách nào (`DioAdapter`/mock adapter, fake clock)?
68. `Image.network` không có cache đĩa (không dùng `cached_network_image`) — hệ quả với danh sách bài hát và cách khắc phục.

---

# PHẦN C — ĐÁNH GIÁ ĐỘ BAO TRÙM KIẾN THỨC FLUTTER

## C1. Bảng đánh giá

| Chủ đề | Mức phủ | Bằng chứng trong repo |
|---|---|---|
| Dart 3 (sealed, pattern, isolate, async) | ★★★★☆ | sealed state, switch expression, `Isolate.run` |
| Widget cơ bản, layout, lifecycle | ★★★★☆ | 27 stateful, LayoutBuilder/MediaQuery/SafeArea, Overlay |
| State management (BLoC/Cubit) | ★★★★★ | 10+ bloc/cubit, private event, `emit.isDone`, cross-bloc listener |
| DI (get_it) | ★★★★☆ | 3 loại registration, mock swap, init phụ thuộc thứ tự |
| Navigation (go_router) | ★★★★☆ | guard, refreshListenable, ShellRoute, custom transition |
| Networking / Dio / interceptor | ★★★★★ | QueuedInterceptor, single-flight refresh, error mapping, multipart |
| Codegen (freezed/json/retrofit) | ★★★★☆ | 9 freezed, 16 json, 2 retrofit API |
| Clean Architecture / repository | ★★★★☆ | domain/data/presentation đủ 5 feature |
| Platform channel + native code | ★★★★☆ | MethodChannel + MediaStore Kotlin, AudioServiceActivity |
| Background audio / media | ★★★★★ | audio_service + just_audio + audio_session đầy đủ |
| Media processing / FFmpeg / perf đo được | ★★★★★ | 963 dòng có benchmark, caching, fallback encoder |
| IAP + verify server | ★★★★★ | consumable, SSV, pending redelivery, race buffer |
| Ads + SSV | ★★★★☆ | rewarded interstitial, ServerSideVerificationOptions |
| Firebase auth / social login / FCM | ★★★★☆ | Google/Apple/OTP, push register theo user |
| Storage (secure/prefs/file) | ★★★★☆ | secure storage, AppSP, path_provider cache |
| Theming / design system | ★★★★☆ | M3, tokens, component library, google_fonts |
| Build/release/CI-CD | ★★★★☆ | dart-define, gitlab-ci, fastlane, fvm, signing |
| Animation (nâng cao) | ★★☆☆☆ | chỉ 3 AnimationController, không Hero/staggered/Rive |
| Custom render / Slivers | ★★☆☆☆ | 2 CustomPainter, hầu như không Sliver, không RenderObject |
| Performance tooling (DevTools, profiling UI) | ★★☆☆☆ | tối ưu FFmpeg tốt, nhưng không thấy RepaintBoundary, `const` audit, jank profiling |
| Testing tự động | ★☆☆☆☆ | 4 file, không mock/bloc_test/golden/integration |
| Local database / offline-first | ☆☆☆☆☆ | không sqflite/hive/drift/isar; không cache HTTP |
| i18n / l10n | ☆☆☆☆☆ | không intl/arb — string hardcode toàn bộ |
| Accessibility | ★☆☆☆☆ | 1 chỗ `Semantics`; clamp textScaler là *chặn* a11y |
| Connectivity / retry offline | ☆☆☆☆☆ | không connectivity_plus, không offline queue |
| Deep link / App Link / push routing | ☆☆☆☆☆ | không có deep link handling |
| Crash/analytics/observability | ★☆☆☆☆ | chỉ shake_flutter; không Crashlytics/Sentry/Analytics |
| Flavor đa môi trường chuẩn | ★★☆☆☆ | thay bằng env switch runtime + gesture ẩn |
| App lifecycle (`WidgetsBindingObserver`) | ☆☆☆☆☆ | không xử lý background/foreground |
| Web/Desktop/adaptive | ★☆☆☆☆ | có folder linux/macos nhưng khóa portrait, mobile-only |
| Multi state-management khác (Riverpod/Provider) | ☆☆☆☆☆ | chỉ BLoC |

## C2. Kết luận

**Có, project này bao trùm khá rộng — nhưng lệch rõ về hướng "product app tích hợp nặng", không phải "UI/foundation".**

Ước lượng: phủ khoảng **70–75%** kiến thức phỏng vấn Flutter mid–senior; riêng nhóm *integration & production concerns* thì mạnh hơn phần lớn project mẫu (đủ để trả lời tự tin cho vị trí **Mid → Senior**).

**Điểm khác biệt (rất "ăn điểm" khi phỏng vấn)** — đây là những thứ project công ty thật mới có, tutorial không có:
- Refresh-token single-flight + retry an toàn với multipart.
- IAP có verify server, pending redelivery, và một race condition thật đã fix.
- AdMob SSV (không tin client).
- Background audio đầy đủ 3 tầng + native activity coupling.
- Tối ưu FFmpeg **có đo lường** (probe `-f null`, tách filter-graph vs encode cost).
- MethodChannel + scoped storage Kotlin.
- CI/CD GitLab + fastlane + FVM + signing từ CI variable.

**Lỗ hổng cần bù trước khi phỏng vấn** (nếu bị hỏi là sẽ trả lời yếu):
1. **Testing** — lớn nhất. Không có `bloc_test`, `mocktail`, golden, integration test. Nên tự viết vài test cho `AuthInterceptor` + `MusicGenerationBloc` để có câu chuyện kể.
2. **Local DB / offline-first** — không có sqflite/hive/drift, không cache HTTP. Đây là câu hỏi rất phổ biến.
3. **i18n/l10n** — hoàn toàn không có; nên đọc trước `flutter_localizations` + arb + plural/gender.
4. **Animation nâng cao & custom RenderObject / Slivers** — chỉ ở mức cơ bản.
5. **Performance UI (jank)** — biết tối ưu native/FFmpeg nhưng không có câu chuyện về `RepaintBoundary`, rebuild profiling, DevTools timeline, shader jank.
6. **App lifecycle & deep link & connectivity** — trắng.
7. **Riverpod / Provider / GetX** — chỉ biết BLoC; nhiều JD vẫn hỏi so sánh.
8. **Accessibility** — biết `textScaler` nhưng theo hướng *chặn*, cần hiểu Semantics/TalkBack.

**Gợi ý chiến lược kể chuyện phỏng vấn**: dùng 3 case của project làm "signature story" —
(1) race condition refresh token, (2) race condition IAP + verify server, (3) tối ưu FFmpeg từ đo lường thực tế.
Ba câu chuyện này chứng minh được cả debugging, concurrency, và performance mindset, mạnh hơn nhiều so với kể "em dùng BLoC và Clean Architecture".