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
  @page { size: A4; margin: var(--page-margin); }
  body { padding: 0; margin: 0; max-width: none; background: white; }
}

a { text-decoration: none; color: var(--main-color); }
p a { text-decoration: underline; text-decoration-color: var(--underline-color); }
b, strong { font-weight: 700 }
p, blockquote { margin: 0; }

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

h1, h2, h3, h4, h5, h2 a, h3 a, h3 strong, h3 b { color: var(--theme-color); }

h1 + p, h1 + p strong, h1 + p a { font-size: 8.7pt; color: var(--muted-color); }
h1 + p { text-align: center; line-height: 1.55; margin-bottom: 10pt; }

h2 {
  font-size: 13pt;
  font-weight: 700;
  border-bottom: 1px solid var(--underline-color);
  text-transform: uppercase;
  letter-spacing: 0.55px;
  margin: 16pt 0 4pt!important;
  padding-bottom: 2pt;
}

h3, h4, h5, h6 { font-size: var(--global-font-size); font-weight: 600; }

p, ul li, ol li { line-height: 1.6; }

ul, ol, ul > li, ol > li { margin-left: 0; padding-left: 0; }
ul, ol { padding-left: 1rem; margin-top: 0.22rem; margin-bottom: 0.3rem; }

img {
  display: block;
  margin: 8pt auto 12pt;
  border-radius: 10px;
  border: 1px solid #d8e2e8;
  box-shadow: 0 8px 20px rgba(15, 23, 42, 0.08);
  object-fit: cover;
}

hr { border: 0; border-top: 1px solid var(--underline-color); margin: 9pt 0 12pt; }

strong, b { color: #111827; }
li::marker { color: var(--theme-color); }

table { width: 100%; border-collapse: collapse; margin: 6pt 0 8pt; }
table th, table td {
  text-align: left;
  padding: 3.5pt 8pt;
  border-bottom: 1px solid var(--underline-color);
  vertical-align: top;
}
table th {
  font-family: var(--header-font-family);
  color: var(--theme-color);
  font-size: 9pt;
  text-transform: uppercase;
  letter-spacing: 0.4px;
}

.highlight {
  background: var(--theme-color-soft);
  border-left: 3px solid var(--theme-color);
  padding: 6pt 10pt;
  margin: 6pt 0 10pt;
}
</style>

# Nguyen Nhat Long

<p align="center">
  <strong>Senior React Native Engineer · Mobile</strong><br/>
  Ha Noi, Vietnam (UTC+7) · <a href="mailto:longnn17.developer@gmail.com">longnn17.developer@gmail.com</a> · +84 868 177 610<br/>
  <a href="https://chelongcoder.github.io/my-portfolio">Portfolio</a> · <a href="https://github.com/longnn17">GitHub</a> · Remote-ready, available for 5–6h daily PST overlap
</p>

<p align="center">
  <img src="https://i.ibb.co/9Hjzkzbk/chandung.jpg" alt="Nguyen Nhat Long" width="150" height="190" />
</p>

---

## Professional Summary

Senior React Native engineer with **7+ years shipping production mobile apps** used at national scale in banking, fintech, and telecom. I own features end-to-end , from architecture decisions through App Store / Google Play release , and I am usually the person the team turns to when JS needs to talk to native code.

Two things I do that most RN engineers do not: I run **major RN version upgrades on legacy codebases** (0.73.6 → 0.81, including the New Architecture migration), and I write the **native modules** (Swift / Kotlin / Objective-C++) that the RN layer depends on.

<div class="highlight">
<strong>Direct match to this role:</strong> proven legacy RN upgrade experience · Redux + MobX in production at national scale · deep native iOS/Android interop · self-hosted CodePush OTA server · shipped to both stores · daily use of AI-assisted development (Claude Code, Cursor).
</div>

---

## Technical Skills

- **React Native (core)**: React Native 0.67 → 0.81, Expo SDK / Expo Dev Client, TypeScript, React 18/19, Hooks, Hermes, **New Architecture (Fabric / TurboModules) migration**
- **RN version upgrades**: legacy-to-current upgrades, native dependency resolution, Gradle / CocoaPods migration, `patch-package`, Android 15 API 35 + 16 KB page-size compliance, iOS Privacy Manifest
- **State & data**: **Redux, Redux Toolkit, Redux Saga, RTK Query, MobX**, Zustand, Context API, React Hook Form + Zod, REST API integration, Axios
- **Native interop**: **NativeModules, RCTEventEmitter, Expo Modules Core**, Swift / SwiftUI / UIKit, Kotlin / Java, Objective-C / Objective-C++, App Groups, FFmpeg, ExoPlayer
- **Architecture**: Clean Architecture, MVVM-style hooks, feature-first modularization, **Module Federation (@module-federation/enhanced) + Re.Pack** micro front-end / super-app
- **Navigation & performance**: React Navigation v6 / v7, deep linking, FlatList virtualization, re-render profiling, memoization, startup and playback optimization
- **Testing & quality**: Jest, React Native Testing Library, supertest, code review, trunk-based git workflow, Agile / Scrum
- **Release & OTA**: **CodePush (self-hosted on Google Cloud VPS)**, Fastlane, TestFlight, App Store Connect, Google Play Console, Firebase App Distribution, GitLab CI/CD, Jenkins
- **AI-assisted development**: Claude Code, Cursor, Copilot - daily use for refactoring, test scaffolding, and codebase exploration
- **Backend (supporting)**: Node.js, NestJS, Prisma, PostgreSQL, MongoDB, BullMQ
- **Also**: Flutter / Dart (secondary stack, 4 years)

---

## Work Experience

| Period | Company | Role |
| --- | --- | --- |
| **06/2026 – Present** | Zhone Technologies Vietnam | Mobile App Software Engineer |
| **11/2025 – 05/2026** | Finhay Vietnam JSC | Mobile Developer (React Native / iOS) |
| **02/2025 – 11/2025** | Vietnam Maritime Bank (MSB) | Mobile Developer, FE5 (React Native) |
| **11/2023 – 01/2025** | Viettel Software | Software Development Engineer (React Native / iOS) |
| **09/2019 – 11/2023** | Hisoft Vietnam | Software Developer → Technical Leader |
| **09/2018 – 09/2019** | OWS Vietnam | Software Developer (Web + React Native) |

---

## Selected Projects

### 1. ABeeCi : Cross-Platform Education App with Native iOS Parental Control
**08/2024 – Present · Technical Lead · Team size: ~6 (Mobile, BE, Design, BA)**

React Native + Expo education product for children, with an unusually deep native iOS layer for Screen Time parental control. Published on the App Store and Google Play.

**My role : tech lead, RN upgrade owner, native module author**
- **Led the React Native upgrade from 0.73.6 to 0.81** on a live production codebase: resolved breaking native-module changes, migrated Gradle / CocoaPods configuration, replaced unmaintained dependencies, and validated the **New Architecture (Fabric / TurboModules)** path on Android
- Handled platform-compliance upgrades ahead of deadlines: **Android 15 (API 35) targeting**, **16 KB page-size support**, and the iOS Privacy Manifest requirement
- Authored iOS parental-control capabilities as **custom Expo Modules** (FamilyControls, ManagedSettings, DeviceActivity) and the JS ↔ native bridge contracts (NativeModules, RCTEventEmitter) for permission requests, app selection, and block/unblock state events
- Built the Shield extension and home-screen widget layer, using **App Groups** to keep blocked-state synchronized between the app and its iOS extensions
- **Deployed and self-hosted a CodePush server on a Google Cloud VPS** after App Center retirement, and wired OTA release channels into the app
- Led technical delivery across mobile and backend tracks, and ran code review for the team

**Key tech**: React Native 0.73 → 0.81, Expo Dev Client, TypeScript, React 19, Swift, SwiftUI, FamilyControls, WidgetKit, App Groups, `react-native-code-push` (self-hosted), NestJS, MongoDB, Google Cloud

---

### 2. Finhay : Fintech App with Native eKYC Integration
**11/2025 – 05/2026 · Mobile Developer · Team size: ~10 (Mobile, BE, QA, Design, PM)**

Investment and savings fintech app combining a cross-platform UI layer with SDK-critical native identity verification.

**My role : owner of the cross-platform ↔ native interop layer**
- Designed the bridge contracts between the JS layer and native iOS view controllers / Android activities - NFC scan, liveness open/result callbacks, and completion/cancel round-trip back to the calling screen
- Delivered the eKYC journey end-to-end (guided ID capture, NFC card reading, liveness handoff) and enforced replacement-flow navigation to eliminate unsafe back-stacks in a regulated flow
- Drove the plan to migrate eKYC UI off native while keeping the NFC and anti-spoofing SDKs in native code
- Built authentication, social SSO (Apple / Google / Facebook), and secure token lifecycle handling; shipped behind a Remote Config phased rollout with a rollback path to the legacy native flow

**Key tech**: React Native, TypeScript, Swift, UIKit, RxSwift, Kotlin, NativeModules / RCTEventEmitter, NFC SDK, Liveness SDK, Firebase, Remote Config, Fastlane, TestFlight

---

### 3. MSB Internet Banking : Super-App Micro Front-End
**02/2025 – 11/2025 · Mobile Developer, FE5 · Team size: ~15 across module teams**

Retail internet banking app for one of Vietnam's largest commercial banks, serving millions of customers. Business domains ship as independent micro front-end modules loaded at runtime.

**My role : module owner, account & daily-transaction domain**
- Owned the account domain: account inquiry, transaction history, account settings, and customer self-service flows
- Built and stabilized daily transaction journeys : transfer, bill payment, top-up, savings where correctness and UX consistency are non-negotiable
- Integrated host-app contracts (auth context, HTTP client, navigation, popup / toast / spinner) so independently-built modules interoperate inside the super-app shell
- Delivered across DEV / SIT / UAT / Pilot / Prod environments, covering build configuration, dependency alignment, and production issue triage

**Key tech**: React Native, TypeScript, **Module Federation (@module-federation/enhanced)**, **Re.Pack + Rspack**, React Navigation v7, Zustand, React Hook Form, Zod, Firebase, Notifee

---

### 4. Mydio : National-Scale Audio Streaming (Viettel Telecom)
**04/2024 – 10/2024 · Mobile Developer · Team size: ~10 (RN, Native, BE, QA, Design)**

Audio and audiobook streaming product for Viettel Telecom, Vietnam's largest telecom operator - discovery, chapter-based listening, background playback, VIP subscription, listening logs.

**My role : audio domain core developer (RN side)**
- Structured the RN codebase by bounded context (audio, home, library, user, auth, favorite, explore) with **MobX** and dependency injection
- Built the audio domain: detail, chapters, albums, series, like/rating, VIP entitlement checks, pre-signed audio links, and listening-log synchronization
- Owned mini-player and full-player behavior : app-state synchronization, visibility and position animation, chapter-change handling, and playback state recovery, the highest-traffic surface in the app
- Integrated the native Android playback service (ExoPlayer, media session, notification controls) and the IAP subscription flow with backend entitlement updates

**Key tech**: React Native, TypeScript, Expo Dev Client, **MobX**, ExoPlayer, React Native Track Player, Swift, Java, Firebase, In-App Purchase, Fastlane

---

### 5. MyClip : Short Video Platform & Native Video Editor (Viettel Telecom)
**11/2023 – 03/2024 · Mobile Developer · Team size: ~12 (RN, iOS, Android, BE, QA)**

Short-video and media product for Viettel Telecom on a **legacy React Native 0.67 codebase** - playback, recording, upload, offline viewing, karaoke, livestream, and in-app video editing.

**My role : native video-editor bridge owner**
- Authored a custom native video-editor bridge (Swift, Objective-C++, Kotlin, **FFmpeg**) supporting trim, split, merge, quality reduction, text overlay, and save-to-library, with platform-specific file handling on both sides
- Built the RN video feature set: recording, gallery selection, upload, cut-flow, text overlay, speed control, preview, and draft / offline handling
- Implemented playback and interaction flows : video detail, related videos, quality selection, comments, fullscreen controls, streaming behavior
- Worked within an aging dependency tree using `patch-package` and pinned native builds; coordinated implementation direction for middle and junior developers
- Supported the release pipeline: Fastlane, AppCenter / TestFlight, GitLab CI

**Key tech**: React Native 0.67, TypeScript, **Redux**, Swift, Objective-C++, Kotlin, FFmpeg, Fastlane, GitLab CI/CD

---

### 6. Musaic : AI Image-to-Music App *(solo product, live on both stores)*
**12/2025 – Present · Solo Fullstack Owner**

A personal product I designed, built, shipped, and now operate alone: a mobile app that turns a photo into an original music track, backed by my own API and admin CMS.

- Shipped and maintain the mobile app on **App Store and Google Play** (v1.2.2), including IAP credit purchases and a rewarded-ads flow with server-verified credit grants
- Built the **NestJS + Prisma + PostgreSQL** backend: async generation pipeline on **BullMQ**, a transactional-outbox pattern with recovery workers for at-least-once delivery, an atomic credit ledger with idempotent refund on provider failure, and server-side IAP receipt verification against the Apple and Google APIs
- Integrated the AI pipeline (**ElevenLabs Music**, OpenAI, Google Gemini) with prompt engineering and retry handling
- Covered the backend with **28 Jest / supertest spec suites**; hardened with Helmet, rate limiting, and credential-redaction logging

**Key tech**: Flutter, NestJS, TypeScript, Prisma, PostgreSQL, BullMQ, ElevenLabs / OpenAI / Gemini APIs, Google Cloud Storage, Firebase, Vue 3 SSR (admin CMS)

---

## Education

**Vietnam Academy of Cryptography Techniques** : Information Assurance · 2016 - 2022 · Good standing

Graduation thesis on secure document management using blockchain - scored **9.5 / 10**.

---

## Additional

- **Languages**: Vietnamese (native) · **English — professional working proficiency**, comfortable in written and verbal communication with distributed teams
- **Availability**: fully remote, able to sustain **5–6 hours of daily overlap with PST**
- **Award**: Employee of the Year — OWS Vietnam
