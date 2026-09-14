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
  <strong>Senior Mobile Engineer | React Native (TypeScript / JavaScript)</strong><br/>
  Ha Noi, Vietnam · 17/07/1998 · <a href="mailto:longnn17.developer@gmail.com">longnn17.developer@gmail.com</a> · 0868177610<br/>
  <a href="https://chelongcoder.github.io/my-portfolio">Portfolio</a> · <a href="https://github.com/longnn17">GitHub</a>
</p>

<p align="center">
  <img src="https://i.ibb.co/9Hjzkzbk/chandung.jpg" alt="Nguyen Nhat Long" width="220" />
</p>

---

## Professional Summary

Senior mobile engineer with 7+ years of professional experience and deep React Native expertise (TypeScript/JavaScript), building and shipping scalable, production mobile applications for iOS and Android across banking, telecom, fintech, and consumer domains. Co-founder of Algorz, owning full product and engineering delivery end-to-end for its consumer mobile products.

Comfortable owning mobile features from architecture and implementation through deployment, integrating RESTful APIs and managing application state with Redux, MobX, Zustand, and Recoil. Strong bar for clean, well-tested, maintainable code, backed by a track record of code reviews, technical discussions, and cross-functional collaboration with product, design, and backend teams in Agile, remote-first environments.

---

## Core Strengths

- 7+ years of professional mobile engineering experience with deep React Native expertise (JavaScript/TypeScript), building and shipping production apps end to end
- Co-founder of Algorz, owning full product and engineering delivery end-to-end for consumer mobile products released on the App Store and Google Play
- Full development lifecycle ownership: architecture, implementation, testing, code review, release, and deployment across Android and iOS
- Hands-on experience integrating RESTful APIs and managing application state with Redux, MobX, Zustand, and Recoil
- Strong architecture ownership with Clean Architecture, MVVM, and reactive mobile patterns, with a genuine bar for clean, testable, maintainable code
- Comfortable operating autonomously in remote-first, distributed teams, with a strong track record of cross-functional collaboration (product, design, backend, QA)
- Experience delivering large-scale, high-traffic mobile products across telecom, banking, and consumer domains
- Modern git workflows and Agile development practices across every engagement

---

## Technical Skills

### Mobile & Cross-Platform
- React Native, Expo
- Flutter / Dart 3, flutter_bloc / Cubit, get_it
- Feature-based Clean Architecture
- Dio + Retrofit + Freezed (codegen)
- go_router
- in_app_purchase (Apple / Play Billing 8), google_mobile_ads
- flutter_secure_storage
- Native module integration, Flutter MethodChannel / Native bridge

### iOS
- Swift, SwiftUI, UIKit, RxSwift
- Alamofire, Certificate pinning, Auto Layout
- CoreNFC, LocalAuthentication, Keychain Services
- CocoaPods

### Android
- Kotlin, Java, Coroutines
- Retrofit2 + OkHttp, Room
- XML UI, CameraX, BiometricPrompt

### Dev Tools & Cloud
- Git (feature-branch workflows, PR-based code review)
- Firebase, Firebase App Distribution
- Google Cloud Platform (IAM, Service Accounts, Cloud Storage, Pub/Sub)
- Fastlane, TestFlight
- GitLab CI/CD, Jenkins
- Sentry

### API & Integration
- REST API, JWT authentication
- Social SSO (Apple / Google / Facebook)
- Server-side IAP verification (Apple App Store Server API, Google Play Developer API)
- Google Play Real-time Developer Notifications (RTDN) via Cloud Pub/Sub
- FCM push (Firebase Admin)
- Third-party SDK integration (NFC, liveness verification)
- Remote Config based feature rollout

### Architecture & Security
- MVVM, Clean Architecture, Reactive programming
- Biometric authentication, Certificate pinning
- Hybrid architecture (Flutter UI + Native SDK)
- A/B rollout and fallback strategy

### State Management
- Redux / Redux Toolkit, Redux Saga, Redux Thunk
- MobX, Recoil, Context API, Zustand, Provider
- BLoC / Cubit, Riverpod

### Backend & Data
- Node.js / NestJS
- Prisma / TypeORM
- PostgreSQL / MySQL / MongoDB
- BullMQ + Redis, Transactional Outbox pattern
- Swagger / OpenAPI
- Jest + Supertest

---

## Professional Experience

### Algorz
**Founder / Co-founder**  
**12/2025 - Present**

- Co-founded Algorz and own end-to-end product and engineering delivery for its consumer mobile products, including Musaic (AI image-to-music platform, live on the App Store and Google Play)
- Make independent architecture, technology, and release decisions across mobile, backend, and infrastructure without a dedicated engineering team
- Operate as a fully autonomous, remote-first engineer: define scope, ship, and iterate directly from user feedback

### Finhay Vietnam JSC
**iOS / Mobile Developer**  
**11/2025 - 05/2026**

- Worked on a hybrid fintech mobile product integrating Native iOS and Flutter modules
- Delivered secure onboarding and eKYC-related flows, including card capture guidance, NFC reading, and liveness handoff
- Implemented and maintained authentication journeys, social sign-in integration, and secure token handling
- Planned phased rollout with Remote Config, fallback path to legacy native flow, and risk-based regression control
- Collaborated with backend, QA, product, and design teams to align business rules, API contracts, and release quality

### Vietnam Maritime Bank (MSB)
**Software Developer, FE5**  
**02/2025 - 11/2025**

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
- Worked across logistics, booking, enterprise operations, real estate social products, healthcare, fintech, and e-commerce projects, including early-stage startup clients (e.g. BDS Recbook)
- Helped guide technical direction, coordinate teams, and collaborate closely with PM, BA, QA, design, and clients
- Built strong experience in shipping real-time and media-related mobile applications

### OWS Vietnam
**Software Developer**  
**09/2018 - 09/2019**

- Worked on web and mobile application development using React and React Native
- Gained early full-time product delivery experience while still at university

---

## Selected Project Experience

### Musaic – AI Image-to-Music Platform *(Algorz, live on App Store / Google Play)* <span class="tag tag-hybrid">Flutter + NestJS + Vue</span>
**Founder, Algorz (Solo Fullstack Owner: Flutter / NestJS / Vue SSR)**  
**12/2025 - Present**

Production AI product that turns a user photo into an original music track. Own all three tiers end-to-end as founder: the Flutter mobile app (released, v1.2.2), the NestJS API with async AI generation pipeline and credit monetization, and a Vue 3 SSR admin CMS. Full ownership from architecture through release, with no dedicated engineering team.

**Key contributions**
- Built the mobile app with feature-based Clean Architecture (BLoC + Cubit, `get_it` DI), a type-safe networking layer (Dio + Retrofit + Freezed), and background audio playback
- Delivered dual monetization (`in_app_purchase` credit top-up, `google_mobile_ads` rewarded flow) with server-verified credit grants; migrated to Play Billing 8.0 ahead of the Play Store deadline
- Designed a modular NestJS 11 backend with BullMQ async processors and a transactional outbox pattern (event-driven, at-least-once delivery) for audio generation, notifications, and campaign fan-out
- Built the credit ledger with atomic deduction inside `prisma.$transaction`, plus idempotent automatic refund on terminal provider failure
- Implemented server-side IAP verification and refund reconciliation against the Apple App Store Server API and Google Play Developer API
- Hardened the API with Helmet, rate limiting, and Terminus health checks, covered by 28 Jest/supertest spec suites
- Built a Vue 3 SSR admin CMS for user, campaign, and content management

**Tech stack**
- Flutter / Dart 3, flutter_bloc, get_it, Dio + Retrofit + Freezed
- NestJS 11, Prisma 7, PostgreSQL, BullMQ (Redis), Transactional Outbox pattern
- Apple App Store Server API / Google Play Developer API
- Firebase Auth / Messaging / Analytics, Google Cloud Storage
- Vue 3 + Pinia + Vite SSR + Express 5

---

### Mydio (Viettel Software) <span class="tag tag-hybrid">RN + Native</span>
**Mobile Developer**  
**04/2024 - 10/2024**

National-scale audio and audiobook streaming product for Viettel Telecom, covering audio discovery, chapter-based listening, background playback, and VIP/package purchase.

**Key contributions**
- Designed a Clean Architecture-style React Native codebase organized by bounded contexts (audio, home, library, user, authentication)
- Built audio domain flows: chapters, albums, VIP checks, pre-signed audio links, and listening log sync
- Refined mini/full player behavior with a native Android ExoPlayer playback service and background playback
- Integrated subscription/package purchase flow with in-app purchase SDK events and backend subscription updates

**Tech stack**
- React Native, TypeScript, Expo Dev Client, React Native Track Player
- ExoPlayer, MobX, Axios
- Firebase Analytics / Crashlytics / Messaging
- Fastlane

---

### MyClip (Viettel Software) <span class="tag tag-hybrid">RN + Native Bridge</span>
**Mobile Developer**  
**11/2023 - 03/2024**

Viettel Telecom media product for short video: playback, recording, upload, offline viewing, and native video editing.

**Key contributions**
- Built React Native video features: recording, trimming, text overlay, speed control, and draft/offline handling
- Integrated a custom native video editor bridge (Swift, Objective-C++, Kotlin, FFmpeg) for trimming, merging, and text insertion
- Helped coordinate implementation direction and task allocation for middle and junior developers, keeping UI, hooks, services, and native integration responsibilities separated
- Supported release workflow through native iOS/Android configuration and GitLab CI conventions

**Tech stack**
- React Native, TypeScript, Kotlin, Swift, Objective-C
- Redux, FFmpeg

---

### Internet Banking - Individual Internet Banking <span class="tag tag-rn">React Native</span>
**Mobile Developer**  
**06/2025 - 1/1026**

High-scale Individual Internet Banking product in a super-app ecosystem, with business domains delivered as independent micro front-end modules integrated at runtime.

**Key contributions**
- Build and maintain account domain features: account inquiry, transaction history, settings, and self-service flows
- Implement daily transaction journeys (transfer, bill payment, top-up, savings) with focus on reliability and UX consistency
- Integrate host-app contracts (navigation, auth context, HTTP client) for super-app interoperability
- Contribute to release readiness across DEV/SIT/UAT/Pilot/Prod environments

**Tech stack**
- TypeScript, Micro front-end / Super app architecture, Module Federation
- Repack + Rspack, React Navigation v7, Zustand
- Firebase Analytics/Crashlytics/Messaging

---

### BDS Recbook <span class="tag tag-rn">React Native</span>
**Mobile Developer**  
**09/2022 - 11/2022**

Social network-oriented product for the real estate market, built for an early-stage real estate startup client, aligned with sharing economy concepts.

**Tech stack**
- TypeScript, React Native, Android Java
- Context API, Hooks
- Redux Saga, Redux Toolkit

---

### IBeauty - Shopping & Affiliate <span class="tag tag-rn">React Native</span>
**Mobile Developer**  
**03/2020 - 05/2021**

Beauty commerce and affiliate platform that helps users purchase products and generate income opportunities through a referral-oriented flow.

**Tech stack**
- JavaScript, React Native, Redux Thunk, Firebase Authentication

---

### Finhay - Hybrid Native (iOS/Android) + Flutter Fintech App <span class="tag tag-hybrid">Flutter + Native</span>
**iOS / Mobile Developer**  
**1/2026 - 05/2026**

Contributed to a hybrid fintech product integrating Native iOS and Flutter modules to deliver secure onboarding, authentication, and identity verification for financial users.

**Key contributions**
- Implemented end-to-end eKYC journeys: guided ID capture, NFC card reading, and liveness verification handoff
- Built bridge flows between Native iOS view controllers and Flutter modules, designing MethodChannel contracts for NFC/liveness callbacks
- Planned phased rollout with feature-flag strategy and rollback-ready fallback to legacy native flow
- Applied MVVM + Clean Architecture with clear presentation/domain/data boundaries
- Collaborated with backend, QA, product, and design teams on business rules, API contracts, and release readiness

**Tech stack**
- Swift, UIKit, SwiftUI, RxSwift, Kotlin
- Flutter 3.32 / Dart 3, flutter_bloc, get_it
- Keychain Services, Alamofire, Certificate pinning
- Fastlane, Jenkins, TestFlight

---

### Abeeci <span class="tag tag-hybrid">RN + Native iOS</span>
**Technical Lead**  
**10/2024 - 08/2026**

Educational application integrating multiple subjects for children, with parental control and in-app purchase support, including a native iOS Screen Time layer.

**Key contributions**
- Led technical delivery from early-stage architecture to production rollout across mobile and backend tracks
- Designed native iOS Screen Time capabilities (FamilyControls, ManagedSettings, DeviceActivity) integrated into React Native/Expo Dev Client
- Built native bridge contracts (NativeModules, RCTEventEmitter, Expo Module) for permission, app selection, and block-state flow
- Worked cross-functionally with design, BA, and backend on release quality

**Tech stack**
- React Native, Expo Dev Client, TypeScript, Swift, SwiftUI, Kotlin
- NestJS, MongoDB, Google Cloud, JWT, Swagger
- CI/CD

---

### ViettelPay Pro <span class="tag tag-native">Native iOS</span>
**iOS Developer**  
**10/2024 - 02/2025**

Enterprise iOS application for ViettelPay Pro channel and agent operations, covering service payment, money transfer, recharge, transaction tracking, and reporting for sales points and business users.

**Key contributions**
- Built and refined the waiting and pending payment records feature (search, batch payment entry, retry flow)
- Implemented transaction history screens with date/status/phone filters and a shared retry flow for failed or pending records
- Integrated SOAP/XML and Swift API flows through existing API layers for telco debit lookup and payment execution
- Worked in a hybrid Objective-C/Swift codebase, coordinating with Android, backend, BA, PM, and QA on production issues and release stability

**Tech stack**
- Swift, Objective-C, UIKit, XIB, SnapKit, Alamofire, AFNetworking
- Firebase Crashlytics / Analytics / Performance
- Fastlane / AppCenter

---

### Hiron iOS <span class="tag tag-native">Native iOS</span>
**Mobile Developer**  
**11/2022 - 04/2023**

iOS application combining e-commerce and medical appointment booking features.

**Key contributions**
- Translated Zeplin designs into UIKit-based interfaces
- Worked with SnapKit and structured iOS UI implementation

**Tech stack**
- Swift, UIKit, SnapKit, Alamofire, MVVM

---

### QRCheckin - Manager OT, Onsite, Personal Detail <span class="tag tag-flutter">Flutter</span>
**Mobile Developer**  
**06/2022 - 09/2022**

Enterprise Flutter application for company entry/exit workflows, OT management, and contractor/guest check-ins.

**Key contributions**
- Owned feature delivery end-to-end: scope, design, implementation, and tests
- Implemented authentication, session storage, and secure token handling
- Integrated OneSignal push across iOS and Android
- Set up observability with Sentry and logging

**Tech stack**
- Flutter, MobX, Dio, flutter_secure_storage, OneSignal, sentry_flutter

---

## Education

### Vietnam Academy of Cryptography Techniques
**Information Assurance**  
**2016 - 2022**

- Graduation standing: Good
- Academic focus included secure systems and information assurance

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
