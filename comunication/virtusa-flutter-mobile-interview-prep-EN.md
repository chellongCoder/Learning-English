# Virtusa Flutter Mobile — Interview Prep (English, SpeakFun-recycled)

> **Position:** Mobile Developer (Flutter) — Virtusa Hanoi — Banking client
> **Interviewer:** Josphine, Talent Acquisition (Indian English)
> **Format:** English answers built almost entirely from the **120 words + 79 structures** anh đã học trong `speakfun-english-it-professionals-vocabulary.md` và `speakfun-english-it-professionals-structures.md`.
> **Highlight rule:** **bold** = từ vựng SpeakFun đã học · *italic* = cấu trúc SpeakFun đã học · 🇻🇳 = chú thích nhanh.

---

## How to use / Cách dùng

🇻🇳 **Đọc to thành tiếng** từng answer. Ô bold = từ phải nhấn đúng (anh đã thuộc pronunciation rồi). Ô italic = cấu trúc đã luyện trong SpeakFun, bật ra natural. Tránh sáng tạo từ mới — bám sát vốn đã có.

| Block | Topic | Priority |
|---|---|---|
| 1 | Self-introduction (60–75s) | ⭐⭐⭐ |
| 2 | Dart core | ⭐⭐⭐ |
| 3 | Flutter widgets & lifecycle | ⭐⭐⭐ |
| 4 | Design patterns | ⭐⭐⭐ |
| 5 | Testing & Mockito | ⭐⭐⭐ |
| 6 | CLI & DevTools | ⭐⭐ |
| 7 | Banking / Security (Finhay) | ⭐⭐⭐ |
| 8 | Behavioral STAR | ⭐⭐⭐ |
| 9 | Questions to ask back | ⭐⭐ |
| 10 | Cheat-sheet | ⭐⭐⭐ |

**Speaking tactics for Indian English interviewer:**
- *I see your point* / *That makes sense* — show understanding before continuing.
- *Just to clarify, does this mean that …?* — confirm before answering.
- *I'd like to point out that …* — emphasize a key fact.
- *So just to summarize …* — close every long answer.

---

## §1. Self-Introduction (60–75 seconds)

### English script (memorize, recycled from SpeakFun)

> "Hi Josphine, thank you for the call. I'm Long, a **Software Engineer** with over seven years of experience.
>
> *My main responsibility* right now is at **Finhay**, a Vietnamese fintech, where *I work on* the **Flutter** side of a hybrid Flutter plus Native iOS and Android App with Kotlin. *A typical day involves* **implementing** **features** for the eKYC and authentication **interface** — guided ID capture, **NFC** card reading, liveness verification, social sign-in, and secure token handling. The **framework** is **scalable**, feature-based Clean Architecture with **flutter_bloc** and **get_it**, and *a key aspect of this solution is* the **MethodChannel** and **EventChannel** **components** that **integrate** the Flutter UI with the native NFC and anti-spoofing SDKs. *I'd like to point out that* I also led the **migration** plan from native eKYC screens to Flutter, **deploying** a phased rollout through Remote Config so we could **roll back** if anything went wrong.
>
> Before Finhay, I spent almost a year as a mobile developer at **MSB — Vietnam Maritime Bank** — *I worked on* the Internet Banking app: transfers, bill payment, savings, **card activation**. So I have **direct banking-domain experience**, and I understand how **critical** **reliability** and security are in this space.
>
> Earlier, I shipped four more production Flutter apps: **Netchat** — an AI chatbot built with BLoC, Cubit, and Clean Architecture; **Raise 2.0** — a parental-education app on Riverpod and go_router; **QRCheckin** — an enterprise OT and check-in app on MobX, Dio, and OneSignal; and a **MyViettel contract module** **integrated** into an existing Android-Java host through platform channels.
>
> *I really enjoy* owning a **feature** end-to-end — *clarifying requirements*, **breaking down** the work, **implementing**, **testing**, and **handing off** for release. I've also led mobile teams as Tech Lead and Project Manager.
>
> *I'm excited to be joining the team as a* Flutter Mobile Developer, because the Virtusa banking client *makes sense* with everything I've been building — **Flutter**, fintech, and banking."

🇻🇳 Câu mở: My main responsibility / I work on / A typical day involves / I really enjoy / I'm excited to be joining the team as. Đây là 5 cấu trúc đã học, dùng đúng vị trí.

### One-sentence pitch (if rushed)

> "Seven years of mobile, currently *working on* Flutter eKYC at Finhay with banking experience from MSB — that's exactly the **Flutter** plus banking combo this role needs."

---

## §2. Dart Core

### Q1. What is null safety in Dart 3?

> "Dart has sound null safety, which means types are non-nullable by default. *I'd like to point out that* the **compiler** **identifies** null **issues** at build time, so you **encounter** fewer crashes at runtime. To allow null I write `Type?`. The common operators are `??` for null-coalescing, `??=` for assign-if-null, `?.` for safe-call, and `late` to defer initialization for fields like dependency-injected services. I avoid the `!` bang operator unless I've **identified** that the value is non-null, because otherwise the **application crashes** at runtime."

🇻🇳 Vocab tái sử dụng: identify, issue, encounter, crash, application, compiler.

```dart
String? name;
String age = '17';
late final ApiClient api;

final display = name ?? 'Guest';
name ??= 'default';
final length = name?.length ?? 0;
```

---

### Q2. Future vs Stream — when do you use which?

> "*The main purpose of* a **Future** is to represent a single asynchronous value — for example an HTTP request that returns one user. A **Stream** represents many values over time — WebSocket messages, BLoC events, Firestore snapshots. *I'd like to propose* a simple rule: **Future** for one-shot calls, **Stream** for anything continuous. *One key aspect* is that streams come in two **flavors** — single-subscription, the default, where only one listener is allowed; and broadcast, which I **go with** for UI states or BLoC because multiple widgets may listen."

🇻🇳 Cấu trúc: The main purpose of / I'd like to propose / One key aspect / go with.

```dart
Future<User> fetchUser(String id) async {
  final res = await dio.get('/users/$id');
  return User.fromJson(res.data);
}

Stream<int> ticks() async* {
  for (var i = 0; i < 5; i++) {
    await Future.delayed(const Duration(seconds: 1));
    yield i;
  }
}
```

---

### Q3. How does async/await work? Why prefer it over `.then()`?

> "`async` marks a **function** that returns a Future. `await` pauses execution until the Future **completes**, so the code reads top-to-bottom like synchronous code. *Compared to* chained `.then()` callbacks, async/await is easier to **review**, easier to wrap in try/catch, and avoids callback nesting. *I'd like to point out that* the most common **bug** I **encounter** is forgetting to await — the **function** still runs, but errors don't **propagate** the way you expect."

🇻🇳 Vocab: function, complete, review, bug, encounter.

```dart
try {
  final user = await fetchUser(id);
  final orders = await fetchOrders(user.id);
} catch (e) {
  log.error(e);
}
```

---

### Q4. What is an Isolate? When do you need one?

> "Dart is single-threaded with an event loop, *so* an **Isolate** is a separate thread with its own memory; isolates communicate by passing messages, not shared memory. *I'd like to propose that we* use Flutter's `compute()` helper when we need CPU-heavy work that would otherwise **slow down** the UI thread and **impact** the 60 fps target. *The main purpose is to* keep the UI **responsive**. Concrete cases: **parsing** a very large JSON list of transactions, running a hash or signing operation, decoding images, or applying expensive regex. *This allows us to* keep the **interface** smooth even under load."

🇻🇳 Vocab: slow down, impact, responsive, parsing, interface. Cấu trúc: The main purpose is to / This allows us to.

```dart
import 'package:flutter/foundation.dart';

Future<List<Tx>> parseTxs(String raw) => compute(_parseTxsSync, raw);

List<Tx> _parseTxsSync(String raw) {
  final json = jsonDecode(raw) as List;
  return json.map((e) => Tx.fromJson(e)).toList();
}
```

---

### Q5. mixins, extensions, sealed classes, and records — quick tour.

> "*I'll walk you through* each one. A **mixin** lets me share behavior across classes without inheritance — for example a Loggable mixin I add to multiple BLoCs. An **extension** adds methods to existing types I don't own — handy for things like `String.isEmail`. A **sealed class**, new in Dart 3, *allows us to* write exhaustive switch — perfect for BLoC state where the **compiler** forces me to handle every variant. A **record** is a lightweight tuple — *I go with* records when I want to return multiple values without defining a class."

🇻🇳 Cấu trúc: walk you through, allows us to, go with.

```dart
mixin Loggable {
  void log(String msg) => debugPrint('[$runtimeType] $msg');
}

extension StringX on String {
  bool get isEmail => RegExp(r'^[\w.]+@[\w.]+\.\w+$').hasMatch(this);
}

sealed class AuthState {}
class AuthInitial extends AuthState {}
class AuthLoading extends AuthState {}
class AuthSuccess extends AuthState { AuthSuccess(this.user); final User user; }
class AuthFailure extends AuthState { AuthFailure(this.msg); final String msg; }

String label(AuthState s) => switch (s) {
  AuthInitial() => 'init',
  AuthLoading() => 'loading',
  AuthSuccess(:final user) => 'hi ${user.name}',
  AuthFailure(:final msg) => 'err $msg',
};
```

---

### Q6. const vs final vs var?

> "`const` is a compile-time constant — instances are shared, *so* Flutter can **optimize** rebuilds. `final` is a runtime constant assigned once. `var` is mutable. *I'd like to point out that* I default to **const** for static widgets to keep the app **responsive** at 60 fps. *That makes sense* because shared instances let the framework skip work."

🇻🇳 Vocab: optimize, responsive. Cấu trúc: I'd like to point out / That makes sense.

---

## §3. Flutter Widgets & Lifecycle

### Q7. StatelessWidget vs StatefulWidget?

> "**StatelessWidget** is a pure function from props to **interface** — no internal state. *I work on* it for buttons, labels, cards. **StatefulWidget** has internal mutable state managed by a State object — *I go with* it when I need a `TextEditingController`, an animation, or any field that **changes** over time. *I'd like to propose that we* push state out of widgets into a Bloc or Cubit so widgets stay stateless and **reliable**, and only the things that truly belong locally — controllers, animation tickers — stay in StatefulWidget."

🇻🇳 Vocab: interface, change, reliable. Cấu trúc: I work on / go with / I'd like to propose that we.

---

### Q8. Walk me through the StatefulWidget lifecycle.

> "*Let me walk you through* the **lifecycle**. It goes `createState` → `initState`, called once, where I **configure** controllers and subscribe to streams → `didChangeDependencies` when an InheritedWidget changes → `build`, called many times → `didUpdateWidget` when the parent **updates** props → `deactivate` → `dispose`, where I cancel subscriptions and dispose controllers. *I'd like to point out that* the most common **issue** I see in code **review** is forgetting `dispose` for `TextEditingController`, `AnimationController`, `ScrollController`, and `StreamSubscription` — that's a memory leak."

🇻🇳 Cấu trúc đỉnh: walk you through, I'd like to point out. Vocab: configure, update, issue, review.

```dart
@override
void initState() {
  super.initState();
  _sub = bloc.stream.listen(_onState);
}

@override
void dispose() {
  _sub.cancel();
  _scrollCtrl.dispose();
  super.dispose();
}
```

---

### Q9. What is BuildContext?

> "BuildContext is a handle into the element tree — it tells Flutter where the current widget sits and which inherited widgets are above it. *A typical use case involves* reading theme, media query, navigator, and looking up Blocs through `context.read` or `context.watch`. *I'd like to point out* one trap: using context after an async gap. If the widget unmounts, accessing context throws. *The fix is* `if (!context.mounted) return;` after every `await`."

🇻🇳 Cấu trúc: A typical … involves / I'd like to point out / The fix is.

```dart
Future<void> _save() async {
  await api.save();
  if (!context.mounted) return;
  Navigator.of(context).pop();
}
```

---

### Q10. Keys — when do you need ValueKey or GlobalKey?

> "Keys help Flutter **identify** which widget in the new tree corresponds to which element in the old tree, especially when widgets reorder. *I go with* `ValueKey` to **identify** items in a list that can be reordered or filtered, *so* the right state stays attached to the right item. `GlobalKey` lets me reach a widget's state from outside — the typical case is `GlobalKey<FormState>` to call `validate()` on a form. *I'd like to point out that* I don't add keys casually — only when state preservation across rebuilds is **critical**."

🇻🇳 Vocab tái sử dụng nhiều: identify, critical, go with.

---

### Q11. Flutter's layout pipeline?

> "*Let me walk you through* the three phases: **Build**, then **Layout**, then **Paint**. In layout, *constraints flow down* from parent to child, and *sizes flow up* from child to parent. The parent then sets the position. *One challenge* I **encounter** often is `RenderFlex overflowed` — the child wants more space than the parent allows. *The fix is* wrapping with `Expanded`, `Flexible`, or `SingleChildScrollView`."

🇻🇳 Cấu trúc: Let me walk you through / One challenge / encounter / The fix is.

---

### Q12. Why does `const` constructor help performance?

> "When two widgets are `const`, they share the same instance, *so* `==` returns true and Flutter's element diffing skips the rebuild for that subtree. *This allows us to* keep the app **responsive**. *I'd like to point out that* in a banking app where I might re-render a transaction list often, `const` plus `ListView.builder` plus `RepaintBoundary` is what **streamlines** the rendering and keeps frame times under 16 milliseconds."

🇻🇳 Vocab: responsive, streamline. Cấu trúc: This allows us to / I'd like to point out.

---

## §4. Design Patterns

### Q13. Which state management have you used? Why BLoC?

> "*I work on* BLoC, Cubit, Provider, MobX, and Riverpod in production. *My main focus is* BLoC plus Clean Architecture for medium-to-large apps because *the main benefit is* a unidirectional data flow — events in, states out — which makes the logic deterministic and **reliable** to test with `bloc_test`. For smaller features *I go with* **Cubit** to **streamline** the boilerplate. *I'd like to propose that we* use **Bloc** specifically when we need stream transformers — **debounce** on search, **droppable** on submit buttons to **prevent** double-tap. *That makes sense* because BLoC scales best when the team is large."
>
> "*One drawback is* boilerplate, and for a very small screen BLoC can be **overkill**. In that case, *it's straightforward to* use Cubit first, then move to full BLoC when the flow has more events, side effects, or **simultaneous** requests."

🇻🇳 Vocab: reliable, streamline, prevent, drawback, overkill, straightforward, simultaneous. Cấu trúc: I work on / My main focus / The main benefit is / I go with / I'd like to propose / That makes sense / It's straightforward to.

```dart
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

---

### Q14. Repository pattern in your apps?

> "Repository hides the data source behind an **interface**. The Bloc or use-case talks to an abstract `AuthRepository`; the **implementation** decides whether the data comes from REST, gRPC, secure storage, or a cache. *The main benefit is* threefold: I can **integrate** new data sources without touching presentation, I can mock the repository in unit tests, and I have a single place to **diagnose** network errors and map them into typed `Failure` objects. *That makes sense* in banking, where the backend protocol can **change** — for example I worked on ViettelPay Pro which used SOAP."

🇻🇳 Vocab: interface, integrate, diagnose, change. Cấu trúc: The main benefit is / That makes sense.

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

---

### Q15. Walk me through Clean Architecture.

> "*Let me walk you through* a feature-based Clean Architecture with three layers per **feature**: **presentation** for UI plus Bloc, **domain** for entities, use-cases, and repository **interfaces** — that's pure Dart, no Flutter — and **data** for DTOs, data sources, repository **implementations**. Dependencies always flow inward — presentation and data depend on domain, never the reverse. *The main benefit is* that I can unit-test the domain layer in milliseconds without spinning up a widget tree. *Plus*, when the backend **changes** endpoints, only the data layer **changes** — *this allows us to* **streamline** future migrations."
>
> "*The advantages are* testability and clear ownership. *One drawback is* extra files, so for a small prototype it may be **overkill**, but for banking apps it helps us **address** long-term maintenance and compliance risk."

🇻🇳 Cấu trúc đầy đủ: Let me walk you through / The main benefit is / This allows us to / The advantages are. Vocab: feature, interface, implementation, change, streamline, drawback, overkill, address.

```
lib/features/auth/
├── presentation/    bloc + screen + widgets
├── domain/          entity + usecase + repo interface
└── data/            dto + datasource + repo impl
```

---

### Q16. Other patterns?

> "*I'll summarize* the rest. **Factory** in every `fromJson` constructor; **Adapter** to convert DTO to domain Entity; **Observer** is built into Stream and ChangeNotifier; **Singleton** through `get_it` with `injectable` for DI; **Strategy** for things like multiple eKYC SDK adapters at Finhay or multiple payment **providers** at MSB; **Bridge / Facade** for the `NativeKycBridge` that wraps MethodChannel and EventChannel; and **Coordinator-style routing** through `go_router` typed routes."

🇻🇳 Cấu trúc: I'll summarize.

---

## §5. Testing & Mockito

### Q17. What kinds of tests do you write?

> "*Let me walk you through* four levels. **Unit tests** for pure logic — Blocs, use-cases, validators — they run in milliseconds. **Widget tests** through `tester.pumpWidget`, `pump`, and `pumpAndSettle` to verify a single screen renders the right thing for a given state. **Integration tests** through the `integration_test` package for end-to-end flows on a real device — login, then dashboard. And **golden tests** for pixel-stable UI **components**. *This allows us to* **identify regressions** early and **deploy** with confidence."

🇻🇳 Cấu trúc: Let me walk you through / This allows us to. Vocab: identify, deploy, component.

```yaml
dev_dependencies:
  flutter_test: { sdk: flutter }
  bloc_test: ^9.0.0
  mocktail: ^1.0.0
  mockito: ^5.4.0
  integration_test: { sdk: flutter }
```

---

### Q18. Show me how you test a Bloc.

> "I use `bloc_test` and `mocktail`. *I'll walk you through* the steps: I mock the repository, *configure* it through `when(...).thenAnswer`, then use `blocTest` to send an event and *verify* the emitted state sequence. *That makes sense* because BLoC is deterministic — given the same event, you always get the same states."

🇻🇳 Cấu trúc: I'll walk you through / That makes sense. Vocab: configure, verify.

```dart
import 'package:bloc_test/bloc_test.dart';
import 'package:mocktail/mocktail.dart';
import 'package:test/test.dart';

class MockAuthRepository extends Mock implements AuthRepository {}

void main() {
  late MockAuthRepository repo;
  late AuthBloc bloc;

  setUp(() {
    repo = MockAuthRepository();
    bloc = AuthBloc(repo);
  });
  tearDown(() => bloc.close());

  blocTest<AuthBloc, AuthState>(
    'emits [Loading, Success] on valid login',
    build: () {
      when(() => repo.login(any(), any()))
          .thenAnswer((_) async => Right(User(id: '1', name: 'Long')));
      return bloc;
    },
    act: (b) => b.add(LoginRequested('a@b.com', '123')),
    expect: () => [isA<AuthLoading>(), isA<AuthSuccess>()],
  );
}
```

---

### Q19. Mockito vs mocktail — which do you prefer?

> "*I'd go with* **mocktail** in new code because it doesn't need code generation — no `build_runner` step, the API is simpler, and it works seamlessly with null safety. I still use **mockito** when a project already uses it, in which case I generate mocks through `@GenerateMocks` and `build_runner build`. *The main benefit is* that the core idea is the same — *you could say something like* 'stub method calls with `when`, return with `thenReturn`, `thenAnswer`, or `thenThrow`, and verify with `verify(...).called(n)` or `verifyNever`'."

🇻🇳 Cấu trúc đặc biệt: You could say something like — đây là cấu trúc số 20 trong SpeakFun.

```dart
@GenerateMocks([AuthApi])
import 'auth_test.mocks.dart';

void main() {
  final api = MockAuthApi();
  test('returns user', () async {
    when(api.login('a', 'b')).thenAnswer((_) async => UserDto(id: '1'));
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

---

### Q20. Widget test example?

> "I `pumpWidget` the screen, `pump()` to advance frames, then `find.byType`, `find.byKey`, or `find.text` plus `expect` to verify. For Bloc-driven screens, *I go with* `whenListen` from `bloc_test` to feed a stream of states. *This allows us to* test a screen in isolation, without **deploying** to a real device."

🇻🇳 Cấu trúc + vocab: go with / This allows us to / deploy.

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
      home: BlocProvider.value(value: cubit, child: const BalanceScreen()),
    ),
  );

  await tester.pump();
  expect(find.byType(CircularProgressIndicator), findsOneWidget);

  await tester.pumpAndSettle();
  expect(find.text('₫1,000,000'), findsOneWidget);
});
```

---

### Q21. Integration test setup?

> "I use the `integration_test` package, which gives the same `tester` API but runs against a real device. *A typical setup involves* driving the full app from `main`, simulating user input, and asserting the final screen. *This allows us to* **reproduce** real user flows and **identify** regressions before we **go live**."

🇻🇳 Vocab gold: reproduce, identify, go live.

```dart
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

---

## §6. Flutter CLI & DevTools

### Q22. Which CLI commands do you use daily?

> "*Day to day, I work on* a few core commands. *Let me walk you through* them: `flutter doctor` to **verify** my setup, `flutter pub get` and `pub upgrade --major-versions` for dependencies, `flutter run --flavor prod` to launch with the right environment, `flutter build apk`, `appbundle`, or `ipa` for releases, `flutter analyze --fatal-warnings` and `dart format` before every **pull request**, `flutter test --coverage` plus `genhtml` to see coverage, and `dart run build_runner watch` when I'm using freezed, retrofit, or generated mocks. *I'd like to point out that* for banking releases I always add `--obfuscate --split-debug-info=build/symbols`. *The main benefit is* that it makes reverse-engineering harder."

🇻🇳 Vocab vàng: pull request, verify. Cấu trúc: Let me walk you through / I'd like to point out / The main benefit is.

```bash
flutter doctor -v
flutter pub get
flutter pub upgrade --major-versions
flutter pub outdated

flutter run --flavor prod -t lib/main_prod.dart

flutter build apk --split-per-abi --obfuscate --split-debug-info=build/symbols
flutter build appbundle --obfuscate --split-debug-info=build/symbols
flutter build ipa --obfuscate --split-debug-info=build/symbols

flutter analyze --fatal-warnings
dart format --line-length 80 .
flutter test --coverage
genhtml coverage/lcov.info -o coverage/html

dart run build_runner watch --delete-conflicting-outputs
flutter clean && flutter pub get
```

---

### Q23. What do you use Flutter DevTools for?

> "DevTools is a browser-based suite I open through `dart devtools`. *Let me walk you through* the tabs I use most. **Flutter Inspector** to walk the widget tree and **identify** layout problems. **Performance** to **diagnose** the frame timeline and **dig into** whether jank is on the UI thread or the raster thread. **CPU profiler** to find a slow **function** — usually JSON parsing or regex. **Memory** for heap snapshots and leak detection — typically a controller I forgot to dispose, *and that's a critical issue*. **Network** to **review** HTTP traffic. And **App size** when I need to **streamline** a release IPA or APK."

🇻🇳 Vocab dày đặc: identify, diagnose, dig into, function, critical, issue, review, streamline.

---

### Q24. Flutter Inspector — what do you check?

> "*I'd like to point out* three things mainly. *First*, select-widget mode — I tap the running app to highlight that widget in the tree, *and that's a great way to* **identify** layout overflow. *Second*, the debug-paint and repaint-rainbow options to see which widgets repaint each frame; if a whole screen flashes when only one item should **change**, *I'd propose* wrapping it in a `RepaintBoundary` to **optimize** the rendering. *Third*, slow-animation mode to **diagnose** timing issues in transitions."

🇻🇳 Vocab: identify, change, optimize, diagnose.

---

### Q25. Dart DevTools for non-Flutter projects?

> "*The same DevTools*, attached to a Dart VM. *A typical workflow involves* `dart run --observe my_app.dart` and opening the URL printed in the terminal. *I work on* it for CLI tools or server-side Dart to profile CPU and memory."

🇻🇳 Cấu trúc: A typical … involves / I work on.

---

## §7. Banking Domain & Security — Finhay anchor

### Q26. How do you store tokens and PII in a banking Flutter app?

> "*I'd like to point out* one rule first: never in `SharedPreferences` or plain Hive — those are **compromised** on rooted devices. *I go with* **`flutter_secure_storage`**, which **integrates** with **iOS Keychain** and **Android EncryptedSharedPreferences backed by Keystore**. *On top of that*, *the solution involves* short-lived JWT — under fifteen minutes; refresh-token rotation; biometric gate through `local_auth` before reading the token; **certificate pinning** in Dio so a man-in-the-middle proxy can't intercept; jailbreak and root detection before allowing login; **screenshot blocking** on sensitive screens — `FLAG_SECURE` on Android and a screen-recording observer on iOS; and `--obfuscate` on every release build. *That's how we build a robust and reliable banking client.*"

🇻🇳 Cấu trúc + vocab: I'd like to point out / I go with / integrate / On top of that / The solution involves / robust / reliable.

```dart
final storage = FlutterSecureStorage(
  iOptions: const IOSOptions(
    accessibility: KeychainAccessibility.first_unlock_this_device,
  ),
  aOptions: const AndroidOptions(encryptedSharedPreferences: true),
);
await storage.write(key: 'access_token', value: token);
```

---

### Q27. How do you build a banking-grade network layer?

> "*The solution involves* **Dio** with a stack of interceptors. *Let me walk you through* them: an **AuthInterceptor** that attaches the bearer token; a **RefreshInterceptor** that, on `401`, pauses the request queue, calls `/refresh` exactly once, *so* we **prevent** a thundering herd of refresh calls, then resumes the queue with the new token; a **CertificatePinningInterceptor**; and a **LogInterceptor** *that's only enabled in debug. I'd like to point out that* timeouts are aggressive — ten seconds connect, fifteen seconds receive — *because banking users tap retry quickly and that has a big impact on user perception of reliability*."
>
> "*We roll out* risky network changes behind Remote Config first, then enable them for a small group of users. *This allows us to* **address** failures early and roll back before they have a wide **impact**."

🇻🇳 Vocab: prevent, impact, reliability, roll out, address. Cấu trúc: The solution involves / Let me walk you through / I'd like to point out / We roll out / This allows us to.

```dart
final dio = Dio(BaseOptions(
  baseUrl: kBaseUrl,
  connectTimeout: const Duration(seconds: 10),
  receiveTimeout: const Duration(seconds: 15),
))
  ..interceptors.addAll([
    AuthInterceptor(storage),
    RefreshInterceptor(authApi),
    CertificatePinningInterceptor(allowedSHAFingerprints: const [...]),
    if (kDebugMode) LogInterceptor(),
  ]);
```

---

### Q28. *(Finhay anchor)* MethodChannel vs EventChannel — when do you use which?

> "*Let me walk you through* the two. **MethodChannel** is request-response — Dart calls a native method, native returns a single result. *I work on* it for actions like *open the NFC scanner*, *start liveness*, *get the device fingerprint*. **EventChannel** is a stream — native pushes events to Dart over time. *I go with* it for liveness frame events, session-expiry pushes, FCM token rotation. *A key aspect of this solution* at Finhay is that I designed both: a `NativeKycBridge` **interface** in the domain layer hides the channels, *so* my Cubits depend on the abstraction. *This allows us to* mock the bridge in unit tests, *and that's a robust approach*."

🇻🇳 Cấu trúc: Let me walk you through / I work on / I go with / A key aspect of this solution / This allows us to / robust.

```dart
class NativeKycBridge {
  static const _ch = MethodChannel('finhay/kyc');

  Future<NfcResult> readNfcCard({required String docType}) async {
    final raw = await _ch.invokeMethod<Map>('readNfc', {'doc': docType});
    return NfcResult.fromMap(raw!.cast<String, dynamic>());
  }
}

class LivenessEvents {
  static const _ev = EventChannel('finhay/kyc/liveness');
  Stream<LivenessEvent> watch() =>
      _ev.receiveBroadcastStream().map((e) => LivenessEvent.fromMap(e));
}
```

**Traps to mention out loud:**
- *I'd like to point out that* `result.success` / `result.error` must be called exactly once on the native side, otherwise the **app crashes**.
- *Just to clarify*, channel arguments must be `StandardMessageCodec` types — `bool`, `num`, `String`, `List`, `Map`, `Uint8List`. No custom classes — convert with `toMap()`.
- *And the last one*: cancel `EventChannel` subscriptions in `dispose` to avoid leaks.

---

### Q29. How would you test a money-transfer flow?

> "*Let me walk you through* the layers. **Unit:** *verify* amount is positive, beneficiary format, fee calculation. **Bloc test:** I drive `TransferRequested` and *expect* the state sequence — `Loading` → `OtpRequired` → on `OtpSubmitted` valid → `Success`, on invalid → `Failure`. **Widget test:** the OTP pad accepts six digits, *and that enables the submit button*. **Integration:** a mocked backend covering the happy path *plus* edge cases — timeout, wrong OTP, insufficient balance. *And one more thing* — a manual security pass: no transaction details in logs, screenshots blocked, no token in crash reports. *That's how we hand off a feature to QA with confidence.*"

🇻🇳 Vocab vàng: verify, hand off. Cấu trúc: Let me walk you through.

---

## §8. Behavioral / STAR

### Q30. Tell me about the most challenging Flutter problem you've solved.

**Primary STAR — Finhay eKYC migration:**

> **Situation.** "*Let me walk you through* one. At Finhay, the legacy eKYC flow was **implemented** natively on iOS in UIKit and SwiftUI, and on Android in Kotlin, *while* the rest of the new product was Flutter. *We've been receiving complaints that* features ship slowly because we maintain two codebases for the same KYC journey, *and that has a big impact* on delivery speed plus a real compliance risk if behavior drifts between platforms."
>
> **Task.** "*I had to* **migrate** the eKYC UI to Flutter *while preserving* the native NFC reading SDK and the anti-spoofing liveness SDK — those are vendor SDKs that only exist as native libraries. The flow had to stay safe — no unsafe back-navigation after a sensitive step — *and we needed a fallback path* to the legacy native flow if anything broke in production."
>
> **Action.**
> - "*I designed* **MethodChannel** contracts for invocations like *open NFC scanner* and *start liveness*, and **EventChannel** for streaming results back — NFC tag read, liveness pass-or-fail, completion or cancel events."
> - "*I wrapped both* behind a `NativeKycBridge` **interface** in the domain layer, *so* my **flutter_bloc** Cubits depend on the abstraction — *this allows us to* unit-test through mocktail."
> - "*I went with* **go_router** `replace` instead of `push` after critical steps — successful QR scan, passed liveness — *to prevent* users from swiping back into a stale state."
> - "*I structured the feature with* Clean Architecture — presentation, domain, and data — and **deployed** it in phases through **Remote Config** feature flag, *with a fallback path* to the original native flow."
>
> **Result.** "*So just to summarize*, we shipped the new Flutter eKYC behind the flag and ramped from zero to ten to fifty to one hundred percent over two releases — **zero rollback**. Single source of truth across both platforms, the NFC and liveness SDKs untouched, *and we now reuse the same bridge pattern* for other native integrations. *I'd like to point out that* this also **streamlined** the onboarding experience for new mobile engineers, *and that has a big positive impact on team velocity*."
>
> "*We kicked off* with one low-risk KYC step first, then expanded to the full journey. *We can roll out* this kind of migration safely when feature flags, fallback paths, and monitoring are in place."

🇻🇳 Cấu trúc xếp lớp:
- S: Let me walk you through / We've been receiving complaints that / impact
- T: I had to / while preserving / fallback path
- A: This allows us to / went with / so / prevent
- R: So just to summarize / I'd like to point out / streamlined / impact / kicked off / roll out

---

### Q31. How do you keep a Flutter app at 60 fps?

> "*Let me walk you through* eight things I check, in order. *First*, `const` constructors wherever a widget is static. *Second*, `ListView.builder` instead of mapping a list into a `Column`. *Third*, **break down** large widgets into smaller ones *so that* rebuild scope shrinks. *Fourth*, `BlocBuilder` with `buildWhen` to skip frames that don't matter. *Fifth*, `RepaintBoundary` around expensive items like images or charts. *Sixth*, heavy parsing goes into `compute()` *so* it runs on an isolate. *Seventh*, images use `cached_network_image` at the right resolution. *Eighth*, I profile through **DevTools Performance** and **identify** whether jank is on the UI thread or the raster thread — *that tells me* whether it's a build problem or a paint problem. *The main benefit is* a **responsive** and **reliable** UX."
>
> "For large feeds or transaction history, *I've come up with* two common options: pagination and **infinite** scrolling. *We fetch* a small page first, then **fetch** more when users scroll. *The advantages are* lower **bandwidth**, lower memory usage, and smoother scrolling. *Pagination alone reduces* initial payload size significantly, especially in banking apps with long transaction histories."

🇻🇳 Vocab dày đặc: break down, identify, responsive, reliable, come up with, infinite, fetch, bandwidth. Cấu trúc mới: I've come up with / We fetch first, then fetch more / The advantages are / Pagination alone reduces.

---

### Q32. The hardest production bug you've debugged?

> "*I'd like to talk about a time when* I debugged a production bug in Mydio — the audio streaming app at Viettel. *At that time, I was working on* the mini-player and background playback flow. *We've been receiving complaints that* the mini-player would **vanish** after the app stayed in the background for more than five minutes on Android.
>
> *The issue seems to come from* Android's aggressive memory policy, which was killing the audio service when the app was no longer active. *I started by* reproducing the issue on a real device, checking logs, and using DevTools Memory to confirm that the service was not stable in the background.
>
> *To fix this*, we migrated playback into a foreground service with a media-session notification and persisted playback state to disk. This allowed us to restore the exact track and position when the user opened the app again. *As a result*, the mini-player became more **reliable**, and users could continue listening after returning to the app. *What I learned was* that background audio needs both a stable native service and a fallback state-recovery plan."

🇻🇳 Cấu trúc mới đã dùng: I'd like to talk about a time when / At that time, I was working on / The issue seems to come from / I started by / To fix this / As a result / What I learned was. Vocab: production bug, vanish, background, reproduce, logs, memory, foreground service, reliable, fallback.

---

## §9. Questions to ask Josphine

*Pick three or four — show genuine interest.*

1. *"Just to clarify*, what does the team look like — *how is the work allocated* between Flutter, native iOS, native Android, and backend?"
2. *"Could you share* which state-management approach the existing codebase uses — BLoC, Riverpod, Provider, or a mix?"
3. *"From the team side, what's the testing culture* — coverage targets, do we run integration tests in CI?"
4. *"For the banking client, are we building a brand-new app, or maintaining and extending an existing one?* I want to understand the **scope** before I start."
5. *"What's the release cadence and CI/CD setup* — Fastlane, App Center, Bitrise, or in-house?"
6. *"Are there compliance constraints I should know about* — PCI DSS, the Vietnam State Bank's regulations? I want to be aware of them upfront."
7. *"What does the first ninety days look like for this role?* Is there a specific **deliverable** you'd like me to focus on?"
8. *"How will success be measured for this position after six months?* I'd like to **align** on expectations early."
9. *"If I join the project, when would we **kick off** the first banking **deliverable**, and how would we **roll out** changes to users?"
10. *"Could you walk me through* the next steps in the interview process and the expected timeline?"

🇻🇳 Cấu trúc: Just to clarify / Could you share / From the team side / Could you walk me through / align. Vocab: allocate, scope, deliverable, kick off, roll out.

---

## §10. 5-Minute Cheat-Sheet (read this last)

```
DART     : null safety (?, !, late) | Future vs Stream | async/await | Isolate (compute) |
           sealed/record/extension/mixin | const > final > var
WIDGETS  : Stateless vs Stateful | initState → build → dispose | const | Key | constraints flow down, sizes flow up
PATTERNS : BLoC/Cubit | Repository | Clean Arch | get_it DI |
           Factory/Adapter/Observer/Singleton/Strategy/Bridge
CHANNELS : MethodChannel (req/res) | EventChannel (stream) | StandardMessageCodec types |
           wrap behind interface to mock
TEST     : unit (bloc_test) | widget (pump/pumpAndSettle/find) | integration_test | golden |
           mocktail (no codegen) > mockito @GenerateMocks
CLI      : doctor | pub get | run --flavor | build apk/appbundle/ipa | analyze | format |
           test --coverage | --obfuscate
DEVTOOLS : Inspector | Performance | CPU | Memory | Network | App size
SECURITY : flutter_secure_storage | cert pinning | jailbreak detect | obfuscate |
           refresh-token queue | screenshot blocking
TRADEOFF : drawback | overkill | straightforward | address | roll out | kick off
LISTS    : infinite scrolling | fetch small page first | cut down bandwidth
```

### Project anchors

```
★ Finhay   — Flutter + Native eKYC | NFC | liveness | MethodChannel/EventChannel |
              flutter_bloc + get_it + go_router | Remote Config phased rollout
★ MSB      — RN Internet Banking | transfer | bill | savings | card activation | micro-frontend
· Netchat  — Flutter | BLoC + Cubit + Clean Arch + EventFlux + Dio
· Raise2.0 — Flutter | Riverpod + go_router + dartz
· QRCheckin— Flutter | MobX + Dio + OneSignal + Sentry
· MyViettel— Flutter inside Android-Java host through MethodChannel
```

### Three sentences to fire when the call opens

1. *"My main responsibility right now is* Flutter and iOS at **Finhay** — hybrid fintech, eKYC plus NFC plus liveness, on **flutter_bloc**, Clean Architecture, MethodChannel and EventChannel."
2. *"Before that, I worked on* Internet Banking at MSB — transfers, bill payment, savings, card activation."
3. *"Five production Flutter apps total. I work on testing through* mocktail with bloc_test, *targeting over* eighty percent on domain and bloc layers."

---

## SpeakFun reuse map (đã dùng)

### Vocabulary đã tái sử dụng (≥ 60 / 120 từ)

architecture · component · robust · optimize · interface · feature · functionality · framework · scalable · streamline · responsive · reliable · compatible · integrate · implement · deploy · troubleshoot · resolve · configure · provision · onboard · familiarize · allocate · escalate · repository · credentials · function · variable · bug · review · comment · change · pull request · merge · approve · fix · update · pending · out of scope · sign off · hand off · go live · wrap up · walk through · feedback · acceptance criteria · deliverable · release notes · issue · blocker · progress · encounter · clarify · break down · estimate · dependency · prioritize · commit · align · action item · point out · propose · reflect · input · summarize · perspective · go with · make sense · identify · diagnose · reproduce · dig into · slow down · impact · cut down on · critical · intermittent · crash · failure · glitch · freeze · lag · outage · drawback · overkill · come up with · fetch · infinite · bandwidth · roll out · kick off · straightforward · simultaneous · address

### Structures đã tái sử dụng (≥ 40 / 79 cấu trúc)

- *I work on …* (#33)
- *My main responsibility is …* (#32)
- *A typical day involves …* (#34)
- *One challenge is …* (#35)
- *I really enjoy …* (#36)
- *The solution involves / consists of …* (#37)
- *This allows us to … / The main benefit is …* (#38)
- *We plan to implement this by/through …* (#39)
- *It's designed to be …* (#40)
- *A key aspect of this solution is …* (#41)
- *The main purpose of this … is to …* (#42)
- *I've encountered a … with …* (#3)
- *I'll look into …* (#5)
- *Walk you through …* (#7)
- *I'd like to point out that …* (#49)
- *I'd like to propose that we …* (#50)
- *Let's align on …* (#52)
- *We could improve by …* (#51)
- *I see your point* / *That makes sense* (#21, #22)
- *I'll work on …* (#23)
- *Just to clarify, does this mean that …?* (#26)
- *Maybe we can break this down into …* (#29)
- *One dependency here is …* (#30)
- *I think we should prioritize … because …* (#31)
- *We've been receiving complaints that …* (#63)
- *From the team side, …* (#64)
- *That might be causing/slowing down …* (#65)
- *… seems to come from …* (#66)
- *We could consider + V-ing* (#67)
- *So just to summarize, …* (#68)
- *I'm excited to be joining the team as a …* (#53)
- *My main focus in the first few weeks will be to …* (#54)
- *Could you please show me how to …?* (#55)
- *You could say something like …* (#20)
- *The interface is [adjective] and [adjective]* (#72)
- *I've come up with [number] possible [noun].* (#74)
- *It's straightforward to [verb].* (#75)
- *The advantages are [noun phrase 1] and [noun phrase 2].* (#76)
- *[Solution] alone reduces [noun phrase] by [percentage or number].* (#77)
- *We [verb 1] first, then [verb 2].* (#78)
- *We can roll out [noun] in [time].* (#79)

🇻🇳 Anh **không cần học từ mới** — bám sát danh sách trên là an toàn. Khi không nghĩ ra cách diễn đạt → quay về 1 trong các structures này.

---

**Good luck, Long. Speak slowly, breathe between clauses, smile through the camera. *You've got this.***

🇻🇳 Chúc anh may mắn! Nói chậm, hít thở giữa các vế, mỉm cười qua camera. Bí thì hỏi lại "*Could you repeat that, please?*" — đừng đoán.
