# Virtusa - Mobile (Flutter) Interview Prep

> **Vị trí:** Mobile Developer (Flutter) - Virtusa Hanoi - Banking client
> **Yêu cầu chính:** 3+ năm Dart, Flutter SDK, Widgets, Flutter CLI, Flutter Test, Mockito, DevTools, Inspector, Dart DevTools, design patterns
> **Thời gian học:** ~2 giờ — học theo thứ tự, mỗi block 10–15 phút
> **Ngôn ngữ trả lời:** mặc định Tiếng Anh ngắn gọn (PRC/JR Talent người Ấn), có sẵn pattern câu trả lời

---

## 0. Cách dùng tài liệu này (đọc đầu tiên — 2 phút)

| Block | Nội dung | Ưu tiên |
|---|---|---|
| 1 | Self-introduction (1 phút) | ⭐⭐⭐ thuộc lòng |
| 2 | Dart core (null safety, async, Future/Stream, Isolate) | ⭐⭐⭐ |
| 3 | Flutter Widgets & Lifecycle | ⭐⭐⭐ |
| 4 | Design Patterns (BLoC, Provider, Repository, Clean Arch) | ⭐⭐⭐ |
| 5 | Flutter Test + Mockito | ⭐⭐⭐ |
| 6 | Flutter CLI + DevTools/Inspector | ⭐⭐ |
| 7 | Banking domain (security) | ⭐⭐ |
| 8 | Behavioral + câu mình hỏi lại | ⭐⭐ |

**Mẹo nhớ nhanh:** mỗi câu nhớ 1 từ-khoá → kéo theo 3 ý → ví dụ ngắn.

---

## 1. Self-Introduction (60–75 giây)

> Hãy tự giới thiệu - đây luôn là câu mở màn

### Khung trả lời (English) — phiên bản Flutter-fintech-heavy

> "Hi, I'm Long. I have **7+ years** of mobile and cross-platform experience, with deep production work in **Flutter**, React Native, and native iOS.
>
> Right now I'm a **Flutter / iOS Mobile Developer at Finhay**, a Vietnamese fintech. The product is a **hybrid Flutter + Native iOS/Android app**, and I'm building the **eKYC and authentication layer** — guided ID capture, **NFC card reading**, **liveness verification**, social sign-in, secure token handling. The architecture is **feature-based Clean Architecture** with **flutter_bloc** and **get_it**, navigation with **go_router**, and I designed the **MethodChannel/EventChannel** contracts that bridge Flutter UI with the native NFC and anti-spoofing SDKs. I also drove the migration plan from native eKYC screens to Flutter with feature-flag rollout via Remote Config.
>
> Before Finhay I spent almost a year as a **mobile developer at MSB — Vietnam Maritime Bank** — on Internet Banking. So on top of fintech, I have **direct retail-banking domain experience**: transfers, bill payment, savings, card activation, micro-frontend / super-app architecture, and the security/compliance discipline that banking demands.
>
> Across earlier roles I shipped **four more production Flutter apps**: **Netchat** (AI chatbot, BLoC + Cubit + Clean Architecture + Dio + EventFlux), **Raise 2.0** (parental-education, Riverpod + go_router + dartz), **QRCheckin** (enterprise OT/check-in, MobX + Dio + OneSignal + Sentry), and a **MyViettel contract module** integrated into an existing Android-Java host using platform channels.
>
> I'm comfortable owning a feature end-to-end — requirement clarification, architecture, implementation, tests with **mocktail / bloc_test**, and release. I've also led mobile teams as Tech Lead and Project Manager.
>
> I'm interested in this Virtusa role because the banking client matches exactly the combo I've been building — **Flutter + fintech + banking**."

### Khung trả lời (Tiếng Việt — backup nếu interviewer chuyển sang tiếng Việt)

> "Em là Long, hơn 7 năm kinh nghiệm mobile. Hiện tại em đang là **Flutter/iOS Mobile Developer tại Finhay** — fintech của Việt Nam. App là hybrid Flutter + Native iOS/Android, em phụ trách mảng **eKYC và authentication**: chụp giấy tờ, đọc **NFC**, **liveness**, social sign-in, xử lý secure token. Stack là **Clean Architecture feature-based + flutter_bloc + get_it + go_router**, và em là người thiết kế các **MethodChannel/EventChannel** để Flutter giao tiếp với SDK NFC và anti-spoofing native, đồng thời lên blueprint migrate eKYC từ native sang Flutter có phased rollout qua Remote Config.
>
> Trước Finhay em làm gần 1 năm tại **MSB Internet Banking** với vai trò mobile developer — chuyển khoản, thanh toán hoá đơn, tiết kiệm, kích hoạt thẻ trong kiến trúc micro-frontend / super-app. Nên em có cả **fintech lẫn retail banking domain experience**.
>
> Ngoài ra em đã ship thêm 4 app Flutter production: **Netchat** (chatbot AI, BLoC+Clean), **Raise 2.0** (Riverpod+go_router), **QRCheckin** (MobX+Dio), **MyViettel module contract** (Flutter platform channel với Android Java host).
>
> Em quan tâm vị trí Virtusa vì combo **Flutter + fintech + banking** đúng đường em đang đi."

### Mỗi câu mở miệng cần bật ra trong 1 giây

1. *"Currently Flutter/iOS dev at Finhay — hybrid fintech, eKYC + NFC + liveness, flutter_bloc + Clean Architecture + MethodChannel."*
2. *"Banking domain background from MSB Internet Banking — transfers, payments, savings, card activation."*
3. *"Five production Flutter apps total, fluent with mocktail + bloc_test for unit/widget testing."*

---

## 2. DART CORE (15 phút)

### Q1. Null safety trong Dart 3.x là gì? Khác gì so với trước?

**Từ khoá:** sound null safety, `?`, `!`, `late`, `required`, `?.`, `??`, `??=`

**Trả lời:**
- Sound null safety: kiểu mặc định **non-nullable**, muốn nullable phải `Type?`.
- `!` (bang): assert non-null — nếu null sẽ crash → **tránh dùng** trừ khi đã chứng minh non-null.
- `late`: hoãn khởi tạo, phải gán trước khi đọc — phù hợp DI hoặc `initState`.
- `??` null-coalescing, `??=` assign-if-null, `?.` safe-call.

```dart
String? name;             // nullable
String age = '17';        // non-nullable
late final ApiClient api; // sẽ inject sau

final display = name ?? 'Guest';   // ?? coalescing
name ??= 'default';                 // gán nếu null
final length = name?.length ?? 0;   // safe-call + fallback
```

**Bẫy phổ biến:** `late` mà chưa gán đã đọc → `LateInitializationError`. Dùng `late final` nếu chỉ gán 1 lần.

---

### Q2. `Future` vs `Stream` — khi nào dùng cái nào?

| Future | Stream |
|---|---|
| **1 giá trị** trả về bất đồng bộ | **Nhiều giá trị** theo thời gian |
| `await fetchUser()` | `await for (final tick in clock())` |
| HTTP request, đọc file 1 lần | WebSocket, BLoC events, `FirebaseFirestore.snapshots()` |

```dart
// Future
Future<User> fetchUser(String id) async {
  final res = await dio.get('/users/$id');
  return User.fromJson(res.data);
}

// Stream
Stream<int> counter() async* {
  for (var i = 0; i < 5; i++) {
    await Future.delayed(const Duration(seconds: 1));
    yield i;
  }
}
```

**Stream broadcast vs single-subscription:**
- Single (mặc định): chỉ 1 listener, ví dụ HTTP body.
- Broadcast: nhiều listener — UI updates, BLoC state.

---

### Q3. `async`/`await` hoạt động thế nào? Khác gì `.then()`?

- `async` đánh dấu function trả về `Future`.
- `await` pause function tới khi Future hoàn tất, đọc dễ như sync.
- `.then()` chain callback — dễ rơi vào callback hell, khó try/catch.

```dart
// then-chain - khó đọc
fetchUser(id).then((u) {
  return fetchOrders(u.id).then((orders) => print(orders));
}).catchError(print);

// async/await - sạch
try {
  final user = await fetchUser(id);
  final orders = await fetchOrders(user.id);
  print(orders);
} catch (e) {
  print(e);
}
```

**Pitfall:** quên `await` → function chạy nhưng không chờ, lỗi không bắt được.

---

### Q4. Isolate là gì? Khi nào cần?

- Dart single-threaded với **event loop**, nhưng `Isolate` cho phép chạy song song trên **luồng khác** (memory tách rời, giao tiếp qua message).
- Dùng khi: parse JSON khổng lồ, crypto/hash, decode image, computation nặng → **không block UI thread (60fps)**.

```dart
import 'dart:convert';
import 'package:flutter/foundation.dart'; // compute()

// compute() là wrapper Isolate ngắn gọn của Flutter
Future<List<Tx>> parseTxs(String raw) =>
    compute(_parseTxsSync, raw);

List<Tx> _parseTxsSync(String raw) {
  final json = jsonDecode(raw) as List;
  return json.map((e) => Tx.fromJson(e)).toList();
}
```

**Rule banking:** parse list giao dịch lớn, sign request, mã hoá → đẩy vào isolate.

---

### Q5. `mixin`, `extension`, `sealed class`, `record` — kể tên ứng dụng

```dart
// mixin - tái sử dụng behavior, không cần kế thừa
mixin Loggable {
  void log(String msg) => print('[$runtimeType] $msg');
}
class AuthBloc extends Bloc<AuthEvent, AuthState> with Loggable { ... }

// extension - thêm method cho type sẵn có
extension StringX on String {
  bool get isEmail => RegExp(r'^[\w.]+@[\w.]+\.\w+$').hasMatch(this);
}

// sealed (Dart 3) - exhaustive switch, ideal cho state
sealed class AuthState {}
class AuthInitial extends AuthState {}
class AuthLoading extends AuthState {}
class AuthSuccess extends AuthState { final User user; AuthSuccess(this.user); }
class AuthFailure extends AuthState { final String msg; AuthFailure(this.msg); }

String label(AuthState s) => switch (s) {
  AuthInitial() => 'init',
  AuthLoading() => 'loading',
  AuthSuccess(:final user) => 'hi ${user.name}',
  AuthFailure(:final msg) => 'err $msg',
};

// record - tuple/multi-return không cần class
({String name, int age}) parseUser() => (name: 'Long', age: 27);
final (name: n, age: a) = parseUser();
```

---

### Q6. `const` vs `final` vs `var`?

- `const`: **compile-time constant**, immutable, share instance — Flutter ưu tiên `const` constructor để tối ưu rebuild.
- `final`: runtime constant, gán 1 lần.
- `var`: biến đổi được — hạn chế dùng.

```dart
const padding = EdgeInsets.all(16);        // 1 instance share toàn app
final now = DateTime.now();                // runtime
var counter = 0;                           // mutable
```

**Banking pitfall:** dùng `const` cho widget tĩnh để Flutter skip rebuild → giữ 60fps.

---

## 3. FLUTTER WIDGETS & LIFECYCLE (15 phút)

### Q7. StatelessWidget vs StatefulWidget — khi nào dùng?

| Stateless | Stateful |
|---|---|
| Pure props → UI, không nhớ gì | Có state nội bộ thay đổi theo thời gian |
| Button, Text, Card | Form field, animation, controller |
| Rebuild khi parent rebuild | `setState()` → rebuild riêng |

```dart
class PriceTag extends StatelessWidget {
  const PriceTag({super.key, required this.amount});
  final int amount;
  @override
  Widget build(BuildContext context) => Text('₫$amount');
}

class CounterButton extends StatefulWidget {
  const CounterButton({super.key});
  @override
  State<CounterButton> createState() => _CounterButtonState();
}

class _CounterButtonState extends State<CounterButton> {
  int _count = 0;
  @override
  Widget build(BuildContext context) => TextButton(
        onPressed: () => setState(() => _count++),
        child: Text('Tap $_count'),
      );
}
```

**Tip phỏng vấn:** ưu tiên stateless + lift state lên BLoC/Cubit → testable + rebuild tối thiểu.

---

### Q8. Lifecycle của StatefulWidget?

```
createState()
   ↓
initState()           ← 1 lần, init controller, subscribe stream
   ↓
didChangeDependencies() ← khi InheritedWidget đổi
   ↓
build()               ← gọi nhiều lần
   ↓
didUpdateWidget(old)  ← khi parent đưa props mới
   ↓
deactivate() → dispose() ← cleanup controller, stream
```

**Mỗi câu hỏi “memory leak” — luôn nhắc:** `dispose()` để hủy `TextEditingController`, `AnimationController`, `StreamSubscription`, `ScrollController`.

```dart
@override
void initState() {
  super.initState();
  _sub = bloc.stream.listen(_onState);
}

@override
void dispose() {
  _sub.cancel();
  super.dispose();
}
```

---

### Q9. `BuildContext` là gì?

- Là **handle vào element tree** — biết widget hiện tại nằm ở đâu, parent là ai, theme nào, route nào.
- Dùng để: `Theme.of(context)`, `MediaQuery.of(context)`, `Navigator.of(context)`, `BlocProvider.of(context)`.

**Bẫy:** dùng `context` sau `await` mà widget đã unmounted → exception.

```dart
Future<void> _save() async {
  await api.save();
  if (!context.mounted) return;     // luôn check
  Navigator.of(context).pop();
}
```

---

### Q10. Key là gì? Khi nào cần `ValueKey`, `GlobalKey`?

- Key giúp Flutter **map widget cũ ↔ element/state cũ** khi cây widget thay đổi vị trí.
- `ValueKey('user-123')`: phân biệt items trong list reorder.
- `GlobalKey<FormState>`: truy cập state từ ngoài (validate form).
- Không thêm key bừa — chỉ khi list reorder hoặc cần giữ state.

```dart
final _formKey = GlobalKey<FormState>();
Form(key: _formKey, child: ...);
if (_formKey.currentState!.validate()) {...}
```

---

### Q11. Layout pipeline 3 phases?

`Build → Layout (constraints down, sizes up) → Paint`

- **Constraints go down:** parent đưa min/max size.
- **Sizes go up:** child quyết định size trong constraints.
- **Position set by parent.**

```
Container(width: 200) → child Text('hi')
  Constraint: w∈[0,200]
  Text trả size cần thiết, ví dụ (40,18)
  Container đặt Text ở (0,0).
```

**Lỗi điển hình:** `RenderFlex overflowed` → child cứng hơn parent. Fix: `Expanded`, `Flexible`, `SingleChildScrollView`.

---

### Q12. Tại sao `const` constructor giúp tối ưu performance?

- `const` widget → cùng identity → `==` true → `Element.update()` skip rebuild.
- `flutter analyze` luôn cảnh báo nên thêm `const`.

```dart
// Tốt
return const Padding(
  padding: EdgeInsets.all(16),
  child: Text('Balance'),
);
```

Banking interview hay hỏi: list giao dịch render chậm → answer: `const`, `ListView.builder`, `RepaintBoundary`, key ổn định.

---

## 4. DESIGN PATTERNS (15 phút)

### Q13. Bạn dùng pattern nào trong Flutter? Ưu nhược điểm?

> **Trả lời mở:** "I default to **BLoC + Clean Architecture** for medium-to-large apps because of testability and clear boundaries. For smaller features I use **Cubit** to reduce boilerplate. I've also used Provider, Riverpod, and MobX in production."

#### a) BLoC / Cubit

- **Why:** unidirectional flow, dễ test (input event → expect state).
- **Cubit:** không cần event class — `emit(newState)` trực tiếp.
- **Bloc:** dùng khi cần debounce, throttle, transform stream events.

```dart
// Cubit (đơn giản)
class CounterCubit extends Cubit<int> {
  CounterCubit() : super(0);
  void increment() => emit(state + 1);
}

// Bloc (event-driven)
sealed class AuthEvent {}
class LoginRequested extends AuthEvent {
  LoginRequested(this.email, this.password);
  final String email, password;
}

class AuthBloc extends Bloc<AuthEvent, AuthState> {
  AuthBloc(this._repo) : super(AuthInitial()) {
    on<LoginRequested>(_onLogin, transformer: droppable());
  }
  final AuthRepository _repo;

  Future<void> _onLogin(LoginRequested e, Emitter<AuthState> emit) async {
    emit(AuthLoading());
    final res = await _repo.login(e.email, e.password);
    res.fold(
      (f) => emit(AuthFailure(f.message)),
      (u) => emit(AuthSuccess(u)),
    );
  }
}
```

UI:
```dart
BlocProvider(
  create: (_) => AuthBloc(getIt())..add(LoginRequested(...)),
  child: BlocBuilder<AuthBloc, AuthState>(
    builder: (_, state) => switch (state) {
      AuthLoading() => const CircularProgressIndicator(),
      AuthFailure(:final msg) => Text(msg),
      AuthSuccess(:final user) => Text('Hi ${user.name}'),
      _ => const SizedBox.shrink(),
    },
  ),
);
```

#### b) Repository pattern (banking-friendly)

```
UI → Bloc → UseCase → Repository (abstract)
                          ↓
              ┌───────────┴────────────┐
        RemoteDataSource          LocalDataSource
        (Dio / Retrofit)          (Hive / Secure storage)
```

- Repository ẩn data source — UI không biết là remote/cache.
- Dễ mock cho test.

```dart
abstract interface class AuthRepository {
  Future<Either<Failure, User>> login(String email, String pw);
}

class AuthRepositoryImpl implements AuthRepository {
  AuthRepositoryImpl(this._remote, this._secure);
  final AuthApi _remote;
  final SecureStorage _secure;

  @override
  Future<Either<Failure, User>> login(String email, String pw) async {
    try {
      final dto = await _remote.login(email, pw);
      await _secure.write('token', dto.token);
      return Right(dto.toEntity());
    } on DioException catch (e) {
      return Left(NetworkFailure(e.message ?? 'network'));
    }
  }
}
```

#### c) Clean Architecture (4 layers)

```
presentation (UI + Bloc)
   ↓ depends
domain (entity + usecase + repo interface)  ← pure Dart, no Flutter
   ↑ implements
data (repo impl + dto + datasource)
```

**Why banking:** dễ swap data source khi backend đổi (SOAP → REST → gRPC), unit test domain không cần Flutter test.

#### d) Singleton / DI với `get_it` + `injectable`

```dart
final getIt = GetIt.instance;

@module
abstract class AppModule {
  @lazySingleton
  Dio dio() => Dio(BaseOptions(baseUrl: kBaseUrl));
}

@injectable
class AuthApi { AuthApi(this._dio); final Dio _dio; ... }
```

Test: `getIt.registerSingleton<AuthApi>(MockAuthApi());`

#### e) Factory, Adapter, Observer (kinh điển)

- **Factory:** `User.fromJson(map)` — encapsulate construction.
- **Adapter:** convert DTO → Entity.
- **Observer:** Stream/ChangeNotifier — Flutter chính là Observer pattern.

---

### Q14. Tại sao tách presentation / domain / data?

> "Domain is pure Dart, knows nothing about HTTP or Flutter — that means I can run **fast unit tests** without spinning up a widget tree, and if we move from Dio to gRPC tomorrow, only the data layer changes. In a banking app where regulations might force us to swap auth providers or storage backends, this isolation is non-negotiable."

---

## 5. FLUTTER TEST + MOCKITO (15 phút)

### Q15. Có mấy loại test trong Flutter?

| Loại | Mục đích | Tốc độ |
|---|---|---|
| **Unit** | Logic thuần (bloc, usecase, util) | 🚀 nhanh nhất |
| **Widget** | 1 widget tree đơn lẻ, pump + tap | 🏃 nhanh |
| **Integration** | E2E trên device/simulator | 🐢 chậm |
| **Golden** | Snapshot pixel UI | 🏃 |

```yaml
# pubspec.yaml
dev_dependencies:
  flutter_test:
    sdk: flutter
  bloc_test: ^9.0.0
  mocktail: ^1.0.0       # khuyên dùng > mockito (no codegen)
  mockito: ^5.4.0        # nếu interview hỏi cụ thể
  integration_test:
    sdk: flutter
```

---

### Q16. Viết unit test cho Cubit/Bloc — kèm mocktail

```dart
// auth_bloc_test.dart
import 'package:bloc_test/bloc_test.dart';
import 'package:mocktail/mocktail.dart';
import 'package:test/test.dart';

class MockAuthRepository extends Mock implements AuthRepository {}

void main() {
  late MockAuthRepository repo;
  late AuthBloc bloc;
  const email = 'a@b.com';
  const pw = '123';

  setUp(() {
    repo = MockAuthRepository();
    bloc = AuthBloc(repo);
  });

  tearDown(() => bloc.close());

  blocTest<AuthBloc, AuthState>(
    'emits [Loading, Success] when login succeeds',
    build: () {
      when(() => repo.login(email, pw))
          .thenAnswer((_) async => Right(User(id: '1', name: 'Long')));
      return bloc;
    },
    act: (b) => b.add(LoginRequested(email, pw)),
    expect: () => [
      isA<AuthLoading>(),
      isA<AuthSuccess>(),
    ],
  );

  blocTest<AuthBloc, AuthState>(
    'emits [Loading, Failure] when network throws',
    build: () {
      when(() => repo.login(any(), any()))
          .thenAnswer((_) async => Left(NetworkFailure('offline')));
      return bloc;
    },
    act: (b) => b.add(LoginRequested(email, pw)),
    expect: () => [isA<AuthLoading>(), isA<AuthFailure>()],
  );
}
```

---

### Q17. Mockito Dart — cách dùng cơ bản

> Khi interviewer ghi rõ **Mockito Dart**, nhắc cả `mocktail` (modern, no codegen) lẫn `mockito` (legacy, codegen).

```dart
// 1. Khai báo
@GenerateMocks([AuthApi])
import 'auth_test.mocks.dart';

void main() {
  final api = MockAuthApi();

  test('returns user', () async {
    when(api.login('a', 'b'))
        .thenAnswer((_) async => UserDto(id: '1'));

    final res = await api.login('a', 'b');

    expect(res.id, '1');
    verify(api.login('a', 'b')).called(1);
    verifyNoMoreInteractions(api);
  });
}
```

```bash
dart run build_runner build --delete-conflicting-outputs
```

**Phân biệt nhanh:**
- `when(...).thenReturn(x)` — sync.
- `when(...).thenAnswer((_) async => x)` — async/Future.
- `when(...).thenThrow(e)` — ném lỗi.
- `verify(api.x()).called(1)` — đã gọi đúng 1 lần.
- `verifyNever(api.x())` — chưa từng gọi.

---

### Q18. Widget test — test 1 màn hình

```dart
testWidgets('shows balance after load', (tester) async {
  final cubit = MockBalanceCubit();
  whenListen(
    cubit,
    Stream.fromIterable([BalanceLoading(), BalanceLoaded(1_000_000)]),
    initialState: BalanceInitial(),
  );

  await tester.pumpWidget(
    MaterialApp(
      home: BlocProvider.value(
        value: cubit,
        child: const BalanceScreen(),
      ),
    ),
  );

  await tester.pump();                     // process Loading
  expect(find.byType(CircularProgressIndicator), findsOneWidget);

  await tester.pumpAndSettle();            // process Loaded
  expect(find.text('₫1,000,000'), findsOneWidget);
});
```

**API hay hỏi:**
- `tester.pumpWidget(...)` — render tree.
- `tester.pump()` — advance frame 1 lần.
- `tester.pumpAndSettle()` — chờ tới khi không còn frame nào pending (cẩn thận với infinite animation).
- `find.byType`, `find.byKey`, `find.text`, `find.byTooltip`.
- `tester.tap(finder)`, `tester.enterText(finder, 'abc')`, `tester.drag(...)`.

---

### Q19. Integration test (E2E)

```dart
// integration_test/app_test.dart
import 'package:integration_test/integration_test.dart';
import 'package:flutter_test/flutter_test.dart';

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  testWidgets('user can login then see dashboard', (tester) async {
    await app.main();
    await tester.pumpAndSettle();

    await tester.enterText(find.byKey(const Key('email')), 'a@b.com');
    await tester.enterText(find.byKey(const Key('password')), '123');
    await tester.tap(find.byKey(const Key('loginBtn')));
    await tester.pumpAndSettle();

    expect(find.text('Dashboard'), findsOneWidget);
  });
}
```

```bash
flutter test integration_test/app_test.dart
flutter drive --driver=test_driver/integration_test.dart \
              --target=integration_test/app_test.dart
```

---

## 6. FLUTTER CLI + DEVTOOLS / INSPECTOR (10 phút)

### Q20. Lệnh CLI bạn dùng hằng ngày?

```bash
flutter doctor -v                         # health check môi trường
flutter pub get                           # cài deps
flutter pub upgrade --major-versions      # update major
flutter pub outdated                      # liệt kê outdated

flutter run                               # debug
flutter run --release                     # build release ngay
flutter run -d chrome                     # web
flutter run --flavor prod -t lib/main_prod.dart   # banking flavor

flutter build apk --split-per-abi         # APK nhỏ hơn
flutter build appbundle                    # AAB cho Play Store
flutter build ipa                          # iOS

flutter analyze --fatal-warnings           # static analyse
dart format --line-length 80 .             # format
flutter test --coverage                    # test + coverage
genhtml coverage/lcov.info -o coverage/html  # report

flutter clean && flutter pub get           # khi build kẹt
flutter precache --ios                     # tải sẵn artifact

dart run build_runner watch --delete-conflicting-outputs   # codegen (freezed, mocks, retrofit)
```

**Banking-relevant flags:**
- `--obfuscate --split-debug-info=build/symbols` → giấu symbol release.
- `--flavor` → tách dev/staging/prod (URL backend, API key).

---

### Q21. Flutter DevTools — bạn dùng làm gì?

DevTools là **suite UI** chạy ở browser, mở qua:
```bash
flutter pub global activate devtools
dart devtools
# hoặc khi flutter run: nhấn 'h' → 'v' để mở
```

| Tab | Dùng để |
|---|---|
| **Flutter Inspector** | Soi widget tree, layout, render bounds, slow build |
| **Performance** | Frame timeline, jank, raster vs UI thread |
| **CPU profiler** | Bottleneck function (parse JSON, regex…) |
| **Memory** | Heap snapshot, leak detection (controller chưa dispose) |
| **Network** | Inspect HTTP request (cần `HttpProfilerEvent`) |
| **Logging** | `print`, `developer.log` |
| **App size** | Phân tích bundle để giảm IPA/APK |

---

### Q22. Flutter Inspector — phân biệt vấn đề layout

- **Select widget mode:** click vào widget chạy, thấy tree highlight.
- **Slow animation:** quay slow-motion để debug animation.
- **Show guidelines / baselines / repaint rainbow:** thấy widget nào repaint nhiều.

Pitfall hay debug: 1 widget repaint cả màn → bọc `RepaintBoundary` quanh nó.

---

### Q23. Dart DevTools cho Dart-only project?

- Dart DevTools là cùng 1 bộ DevTools nhưng connect tới Dart VM (CLI/server). Lệnh:
  ```bash
  dart run --observe my_app.dart
  ```
  Mở URL trong terminal để vào DevTools.
- Banking server-side Dart hoặc CLI tool: profile CPU + Memory tương tự.

---

## 7. BANKING DOMAIN & SECURITY (8 phút)

### Q24. Lưu token / PII trong Flutter banking app như thế nào?

> **Trả lời thẳng:** "Never `SharedPreferences` or `Hive` plain. Use **`flutter_secure_storage`** which maps to **iOS Keychain** and **Android Keystore / EncryptedSharedPreferences**."

```dart
final storage = FlutterSecureStorage(
  iOptions: const IOSOptions(accessibility: KeychainAccessibility.first_unlock_this_device),
  aOptions: const AndroidOptions(encryptedSharedPreferences: true),
);
await storage.write(key: 'access_token', value: token);
```

**Plus:**
- Refresh token rotation, JWT short TTL (≤15 min).
- Biometric gate trước khi đọc token: `local_auth`.
- Certificate pinning với Dio: `dio_certificate_pinning` hoặc `BadCertificateCallback`.
- Disable screenshot ở screen nhạy cảm (`FLAG_SECURE` Android, `UIScreen.isCaptured` iOS qua platform channel).
- Detect jailbreak/root trước khi cho login (`flutter_jailbreak_detection`).
- `--obfuscate` build release.

---

### Q25. Network layer cho banking?

```dart
final dio = Dio(BaseOptions(
  baseUrl: kBaseUrl,
  connectTimeout: const Duration(seconds: 10),
  receiveTimeout: const Duration(seconds: 15),
))
  ..interceptors.addAll([
    AuthInterceptor(storage),       // attach Bearer
    RefreshInterceptor(authApi),    // 401 → refresh → retry
    LogInterceptor(),               // chỉ debug
    CertificatePinningInterceptor(allowedSHAFingerprints: [...]),
  ]);
```

Pattern Refresh queue (chống đa request 401):
```
Multiple 401 → pause request queue → call /refresh once → resume queue with new token.
```

---

### Q26. Test một flow chuyển khoản nên test gì?

- **Unit:** validate amount > 0, beneficiary format, fee calc.
- **Bloc test:** `TransferRequested` → `OtpRequired` → `OtpSubmitted` → `TransferSuccess` / `Failure`.
- **Widget test:** OTP screen pad bấm 6 số → state ready, button enabled.
- **Integration:** mock backend (charles) full happy + edge (timeout, wrong OTP, insufficient balance).
- **Manual security:** không log số tiền/account, screenshot blocked.

---

### Q26b. MethodChannel vs EventChannel — khi nào dùng cái nào? *(Finhay case)*

| | MethodChannel | EventChannel |
|---|---|---|
| **Pattern** | Request/response (1 lần) | Stream (nhiều event) |
| **Direction** | 2 chiều: Dart → Native và ngược lại | Native phát → Dart lắng nghe |
| **Use case banking/fintech** | Open NFC scanner, start liveness, get device fingerprint | Liveness frame events, session-expiry, FCM token rotation, real-time balance |

```dart
// MethodChannel — invoke native NFC reader (Finhay case)
class NativeKycBridge {
  static const _ch = MethodChannel('finhay/kyc');

  Future<NfcResult> readNfcCard({required String docType}) async {
    final raw = await _ch.invokeMethod<Map>('readNfc', {'doc': docType});
    return NfcResult.fromMap(raw!.cast<String, dynamic>());
  }

  Future<bool> startLiveness() async =>
      await _ch.invokeMethod<bool>('liveness') ?? false;
}

// EventChannel — stream liveness frames pass/fail
class LivenessEvents {
  static const _ev = EventChannel('finhay/kyc/liveness');
  Stream<LivenessEvent> watch() =>
      _ev.receiveBroadcastStream().map((e) => LivenessEvent.fromMap(e));
}
```

Native (Android Kotlin):
```kotlin
class KycPlugin : MethodChannel.MethodCallHandler, EventChannel.StreamHandler {
  override fun onMethodCall(call: MethodCall, result: MethodChannel.Result) {
    when (call.method) {
      "readNfc" -> startNfc(call.argument("doc"), result)
      "liveness" -> startLiveness(result)
      else -> result.notImplemented()
    }
  }
  override fun onListen(arguments: Any?, events: EventChannel.EventSink) { ... }
  override fun onCancel(arguments: Any?) { ... }
}
```

**Bẫy hay hỏi:**
- Phải gọi `result.success/error` **đúng 1 lần** — gọi 2 lần sẽ crash.
- Args qua channel chỉ chấp nhận `StandardMessageCodec` types: bool/num/string/list/map/`Uint8List` — không truyền custom class trực tiếp, phải `toMap()`.
- Listen EventChannel xong nhớ `cancel()` subscription trong `dispose()` để tránh leak.
- Test: bọc bridge sau interface → mock được, không cần platform.

---

## 8. BEHAVIORAL + CÂU HỎI ĐÁP (10 phút)

### Q27. "Tell me a challenging Flutter problem you solved."

**STAR template — case chính: Finhay eKYC Flutter ↔ Native (NFC + Liveness)**

> **S — Situation:** "At Finhay, the legacy eKYC flow was implemented natively on iOS (UIKit/SwiftUI) and Android (Kotlin), but the rest of the new product was Flutter. We had two separate codebases for the same KYC journey, which slowed feature delivery and risked behavior drift between platforms — a real compliance problem in fintech."
>
> **T — Task:** "Migrate the eKYC UI to Flutter while **preserving the native NFC reading SDK and the anti-spoofing liveness SDK** — those are vendor SDKs that only exist as native libraries. The flow had to stay safe: no unsafe back-navigation after a sensitive step, fallback path to the legacy native flow if anything broke in production."
>
> **A — Action:**
> - Designed **MethodChannel** contracts for invocations (open NFC scanner, start liveness) and **EventChannel** for streaming results back (NFC tag read, liveness pass/fail, completion/cancel).
> - Wrapped both behind a `NativeKycBridge` interface in domain layer, so the **flutter_bloc** Cubit/Bloc only depends on the abstraction → fully unit-testable with mocktail.
> - Used **go_router** with `replace` instead of `push` after critical steps (QR success, liveness pass) to prevent users from swiping back into a stale state.
> - Applied **Clean Architecture**: presentation (UI + Bloc), domain (entities + use-cases + bridge interface), data (bridge implementation, API client, secure storage).
> - Phased rollout via **Remote Config feature flag** with a fallback path to the original native flow — could disable Flutter eKYC remotely if a critical bug surfaced.
>
> **R — Result:** "We shipped the new Flutter eKYC behind the flag, ramped 0% → 10% → 50% → 100% over two releases with zero rollback. Single source of truth for eKYC across iOS and Android, NFC/liveness SDKs untouched, and we now reuse the bridge pattern for other native integrations."

**STAR template phụ — backup từ MyViettel Contract Module (cùng pattern, ngắn hơn):**

> Same pattern earlier at Viettel: a Flutter Contract module was added inside an existing Android-Java host. I used MethodChannel for session/auth handoff and EventChannel for session-expiry events, keeping Flutter behind a `NativeBridge` abstraction. Module shipped on time with >80% unit-test coverage on the Cubit and bridge layer.

### Q28. "How do you make sure your Flutter app stays at 60fps?"

1. `const` constructor everywhere static.
2. `ListView.builder` thay vì map `.toList()` rồi `Column`.
3. Tách widget nhỏ → rebuild scope nhỏ.
4. `BlocBuilder` + `buildWhen` để skip frame thừa.
5. `RepaintBoundary` cho item phức tạp (image, chart).
6. Heavy parse → `compute()` (Isolate).
7. Image: `cached_network_image`, `precacheImage`, đúng kích thước.
8. Profile bằng DevTools Performance, kiểm tra **Raster vs UI thread**.

### Q29. "What design patterns have you actually used in production?"

> "BLoC / Cubit (Finhay, Netchat, EPS), Provider (EPS, Health Record), MobX (QRCheckin, Mydio), Riverpod (Raise 2.0). Beyond state management: **Repository** (ubiquitous), **Factory** (`fromJson`/`fromMap`), **Adapter** (DTO ↔ domain Entity), **Observer** (Stream / ChangeNotifier), **Singleton via DI** (`get_it`+`injectable`), **Strategy** (e.g., multiple payment-provider adapters at MSB, multiple eKYC SDK adapters at Finhay), **Bridge / Facade** (Finhay `NativeKycBridge` and MyViettel `NativeBridge` over MethodChannel/EventChannel), **Coordinator-like routing** (`go_router` typed routes / `auto_route`)."

### Q30. Câu mình nên hỏi nhà tuyển dụng

1. "What's the team size and how is the work split between Flutter, iOS, Android, and backend?"
2. "Which **state management** does the existing codebase use — BLoC, Riverpod, Provider, or a mix?"
3. "What's the **testing culture** — coverage targets, do we run integration tests in CI?"
4. "Banking client — are we building a brand-new app or maintaining/extending an existing one? Greenfield or legacy?"
5. "Release cadence and CI/CD setup? Fastlane, App Center, Bitrise, or in-house?"
6. "Any compliance constraints I should know about — PCI DSS, local banking regulator (SBV)?"
7. "What does the first 90 days look like for this role?"
8. "How do you measure success for this position after 6 months?"

---

## 9. CHEAT-SHEET 5-PHÚT CUỐI (đọc lại trước khi vào)

```
DART        : null safety (?, !, late) | Future vs Stream | async/await | Isolate (compute) | sealed/record/extension/mixin | const>final>var
WIDGETS     : Stateless vs Stateful | initState→build→dispose | const | Key (Value/Global) | constraints down sizes up
PATTERNS    : BLoC/Cubit | Repository | Clean Arch (presentation/domain/data) | get_it DI | Factory/Adapter/Observer/Singleton/Bridge
CHANNELS    : MethodChannel (req/res) | EventChannel (stream) | StandardMessageCodec types | bridge interface để test
TEST        : unit (bloc_test) | widget (pump/pumpAndSettle/find) | integration_test | golden | mocktail (no codegen) / mockito @GenerateMocks
CLI         : doctor, pub get, run --flavor, build apk/appbundle/ipa, analyze, format, test --coverage, build_runner watch, --obfuscate
DEVTOOLS    : Inspector (widget tree) | Performance (frame) | CPU | Memory | Network | App size
SECURITY    : flutter_secure_storage (Keychain/Keystore) | cert pinning | jailbreak detect | obfuscate | refresh token rotation | block screenshot
ANSWERS PROJ: ★ Finhay (Flutter+Native eKYC, NFC, liveness, MethodChannel/EventChannel, flutter_bloc+get_it+go_router, Remote Config rollout)
              ★ MSB (RN banking — transfer/payment/savings/card, micro-frontend)
              · Netchat (BLoC+Cubit+Clean+EventFlux+Dio)
              · Raise2.0 (Riverpod+go_router+dartz+Dio)
              · QRCheckin (MobX+Dio+OneSignal+Sentry)
              · MyViettel Contract (Flutter MethodChannel inside Android-Java host)
```

**3 câu mở miệng phải bật ra trong 1 giây:**
1. *"Currently Flutter/iOS dev at Finhay — hybrid fintech, eKYC + NFC + liveness, flutter_bloc + Clean Architecture + MethodChannel."*
2. *"Before that, banking at MSB Internet Banking — transfers, payments, savings, card activation."*
3. *"Five production Flutter apps total. For testing I use mocktail + bloc_test, target >80% on domain and bloc layers."*

---

> **Chúc may mắn buổi phỏng vấn với Virtusa!** Trước khi vào: deep-breath, slow speech, mỉm cười. Khi không hiểu — hỏi lại để confirm rather than assume.
