# Interview Prep: Web3 Mobile Wallet Engineer (React Native)

> **Job context:** Non-custodial wallet, BIP39/44, WalletConnect v2, KYC/KYB, biometrics, multi-chain (ETH/Polygon/Arbitrum/Base), native bridge, CI/CD.
>
> **Strategy:** Lean on fintech eKYC + RN + Native bridge background (Finhay, MSB, ABC Education). Be honest about Web3 — frame it as a fast learn, backed by the blockchain thesis (9.5/10) and Solidity/Hardhat experience on the Health Record project.
>
> Each answer below uses **vocab** and **structures** from the 2 study files in this folder:
> - `speakfun-english-it-professionals-vocabulary.md` → marked **[V#]**
> - `speakfun-english-it-professionals-structures.md` → marked **[S#]**

---

## Q1. Tell me about yourself.

**Answer:**

> "Sure. **I work on** **[S33]** mobile development with over 7 years of experience, mainly in **React Native**, and also Flutter and native iOS/Android. **My main responsibility** **[S32]** in recent projects has been delivering **secure** / **reliable** **[V109]** fintech and banking flows — for example, at Finhay I built end-to-end eKYC journeys including NFC card reading, liveness verification, and **Keychain**-based token storage. Before that, at MSB, I worked on Internet Banking in a **scalable** **[V59]** micro front-end **architecture** **[V56]**. **One thing I really enjoy** **[S36]** is bridging **React Native with native modules** — Swift and Kotlin — to **integrate** **[V107]** platform-specific SDKs. I'm now looking for a role where I can apply this mobile + security background to Web3, which I've been studying since my graduation thesis on blockchain document management."

**Why this works:** Hits exactly what the JD asks (RN + native bridging + mobile security) in 45 seconds.

---

## Q2. Why are you interested in this role / Web3?

**Answer:**

> "Two reasons. First, **from a technical perspective** **[V100]**, this role aligns with my strongest skills: React Native, native bridging for biometrics, and secure key storage — patterns I already **implement** **[V13]** in fintech. Second, Web3 isn't new to me conceptually — my graduation thesis was on a blockchain-based document system using Hyperledger and IPFS, and I've **deployed** **[V53]** Solidity smart contracts to Sepolia for a personal Health Record project. **The main benefit** **[S38]** for the team is that I can ship production-grade mobile features from day one, while ramping up on BIP32/44 and WalletConnect v2 quickly."

---

## Q3. Walk us through your experience with React Native + Native modules.

**Answer:**

> "Let me **walk you through** **[V29 / S7]** a concrete example. At ABC Education, I built an iOS Screen Time parental control feature in **React Native + Expo Dev Client**. **The solution involved** **[S37]** writing a custom Swift module exposing FamilyControls, ManagedSettings, and DeviceActivity through `NativeModules` and `RCTEventEmitter`. **A key aspect** **[S41]** was the bi-directional bridge — JS would request permission, the native side would handle the system prompt, and the result was emitted back via events. I also used **App Groups** for shared `UserDefaults` between the app and a shield extension widget. **It's designed to be** **[S40]** **robust** **[V7]** enough to survive app restarts and **reliable** **[V109]** across iOS versions."

**For Web3 mapping:** Mention that the same bridge pattern can be applied for native crypto signers (Secure Enclave, Android Keystore).

---

## Q4. How would you securely store a private key on mobile?

**Answer:**

> "**The solution consists of** **[S37]** three layers. First, the seed phrase is generated using BIP39 standard libraries on-device — never transmitted. Second, the derived private key is stored in **iOS Keychain** with `kSecAttrAccessibleWhenUnlockedThisDeviceOnly` and `SecAccessControl` requiring biometric **authentication** — on Android, the equivalent is **Android Keystore** with `setUserAuthenticationRequired(true)`. Third, **a key aspect** **[S41]** is hardware-backed protection: on iOS we leverage Secure Enclave for keys that never leave the chip; on Android, StrongBox where available. **We plan to implement this through** **[S39]** a thin native module exposed to React Native, so JS code only ever sees a key handle, never the raw key. **It's designed to be** **[S40]** **robust** **[V7]** against jailbreak/root and resilient to OS upgrades."

> **Honest note if pushed deeper on BIP44:** "I haven't shipped BIP44 derivation in production yet, but the path structure `m/44'/60'/0'/0/n` and HD wallet concepts are clear from my blockchain thesis work. **I'll work on** **[S23 / V43]** reproducing this in a small prototype before we talk again if helpful."

---

## Q5. How do you handle biometric authentication (FaceID/TouchID)?

**Answer:**

> "On iOS, **I work on** **[S33]** biometrics through the **LocalAuthentication** framework — `LAContext.evaluatePolicy` with `.deviceOwnerAuthenticationWithBiometrics`. **One challenge** **[S35]** is fallback handling: if biometrics fail or aren't enrolled, the **interface** **[V54]** should gracefully degrade to a passcode flow. On Android, the equivalent is **BiometricPrompt**, which I used in a Viettel project. **A key aspect** **[S41]** for a wallet is binding the biometric check to the Keychain access control itself — so the OS, not your app, enforces the gate. That makes the system **robust** **[V7]** even if the JS layer is compromised."

---

## Q6. Describe a critical bug you've debugged. (STAR-style)

**Answer:**

> "**I'd like to point out** **[S49]** one from MSB. **There was an issue with** **[S58]** transfers occasionally failing with a generic timeout. **It was an intermittent** **[V92]** problem, **so** **[S62]** **reproducing** **[V93]** it was hard. **I started by digging into** **[V95]** the network logs in Sentry **and identified** **[V39]** that the issue happened only when users switched from Wi-Fi to 4G mid-request. **From the mobile side** **[S64]**, the Repack micro-frontend was holding stale auth headers. **The cause seemed to come from** **[S66]** the host app not propagating a re-issued token to remote modules. **We could consider** **[S67]** adding a token-refresh interceptor at the host level — that's what **I proposed** **[V70]**. After we **deployed** **[V53]** the fix, the **failure** **[V87]** rate dropped to near zero. **So just to summarize** **[S68]**, the lesson was: in micro-frontend architectures, auth context must be a single source of truth."

---

## Q7. How do you set up CI/CD for a mobile app?

**Answer:**

> "**A typical setup involves** **[S34]** Fastlane for build/sign/upload, plus a CI provider — I've used Jenkins at Finhay and GitLab CI elsewhere. **The main purpose** **[S42]** is to remove manual steps. The pipeline I built at ABC Education had four stages: lint + type-check, unit tests, EAS Build for iOS/Android, and distribution via TestFlight + Firebase App Distribution. **It's designed to be** **[S40]** **scalable** **[V59]** — every PR triggers a preview build, and main branch auto-deploys to internal testers. **One key aspect** **[S41]** for a wallet app would be reproducible builds and signed artifacts — so users can verify the binary matches the open-source code."

---

## Q8. How do you collaborate with a remote, global engineering team?

**Answer:**

> "**A typical day involves** **[S34]** async updates first — I write a daily standup note covering: *yesterday I worked on X* **[S1]**, *today I'll focus on Y* **[S2]**, and any **blockers** **[V11]**. **One thing I learned** at Finhay is over-communicating context — instead of saying 'I fixed the bug', I share what was wrong, why, and the impact. For complex topics, I create short Loom walk-throughs or sequence diagrams. When there's disagreement, **I'd like to propose** **[S50]** we **align on** **[V73 / S52]** the interface contract first, then implementation can proceed in parallel. If something is **out of scope** **[V21]** or escalating, **I escalate** **[V83]** early rather than late."

---

## Q9. What do you know about WalletConnect v2?

**Answer (honest + structured):**

> "I know the high-level model: WalletConnect v2 uses a **relay infrastructure** **[V57]** where the wallet and dApp pair via a URI, then exchange JSON-RPC requests over an end-to-end-encrypted channel using the Sign / Auth / Push namespaces. The v2 improvements over v1 are multi-chain sessions in a single pairing and a more **scalable** **[V59]** relay. **I haven't shipped a WalletConnect integration yet** — but **I'll work on** **[S23]** a proof-of-concept this week using `@walletconnect/react-native-v2` to make sure I can speak to it concretely in the next round. Given my React Native + native bridging background, **I'm confident** the integration pattern will be familiar."

> **Why this works:** Honesty + initiative is far stronger than bluffing. You showed you understand the **architecture** **[V56]**.

---

## Q10. How would you approach multi-chain support (ETH/Polygon/Arbitrum/Base)?

**Answer:**

> "**The solution consists of** **[S37]** an abstraction layer where chain-specific logic is isolated. Since all four are EVM-compatible, the signing logic is identical — only the chain ID, RPC endpoint, gas model, and explorer URL change. **A key aspect** **[S41]** is using a `ChainConfig` registry as a single source of truth, with **Ethers.js** or **Viem** as the client. **It's designed to be** **[S40]** **scalable** **[V59]** — adding a new EVM chain becomes a config change, not a code change. **One challenge** **[S35]** is gas estimation: Arbitrum and Base have different fee mechanics from mainnet, so we'd need chain-aware estimators. For real-time balances, **we could consider** **[S67]** a multicall pattern to **cut down on** **[V99]** RPC calls and **optimize** **[V8]** latency."

---

## Q11. How do you document complex security architecture for a remote team?

**Answer:**

> "**The main purpose** **[S42]** of the doc is for someone in another timezone to act on it without a meeting. **My typical approach involves** **[S34]** three artifacts: (1) a sequence diagram in Mermaid showing the trust boundaries — user → app → Keychain → blockchain; (2) a threat-model table listing each risk and mitigation; (3) a runbook covering what to do during an **outage** **[V91]** or key-compromise incident. **It's essential to ensure that** **[S44]** the doc is versioned in the repo next to the code it describes, so it doesn't drift. **One thing I learned** at Finhay: short, diagrammed docs get **reviewed** **[V31]**; long prose docs don't."

---

## Q12. Where do you see yourself in 2–3 years?

**Answer:**

> "**My goal** **[V44]** is to grow from senior IC into a Technical Lead role — owning the **architecture** **[V56]** of a product area end-to-end and mentoring 2–3 engineers. I've already had a taste of this at Hisoft and on the ABC Education project, and **I really enjoy** **[S36]** the mix of hands-on coding and team enablement. Long-term, I want to move toward Engineering Manager, but I want to be a **robust** **[V7]** technical leader first."

---

## Q13. Questions to Ask Them (always have 3+ ready)

1. *"Could you* **walk me through** **[S7]** *your current security model for key derivation and storage? Are you using Secure Enclave / StrongBox today?"*
2. *"What does the CI/CD pipeline look like for the mobile wallet — are builds reproducible from a public repo?"*
3. *"How is the team structured across regions, and what does a typical sprint cadence look like?"*
4. *"What are the biggest* **blockers** **[V11]** *for the team right now, and where would my first 30/60/90 days have the most* **impact** **[V98]***?"*
5. *"Are you planning to support non-EVM chains (Solana, Bitcoin) — does that affect how the wallet abstraction is built today?"*

---

## Quick Vocab / Structure Cheat Sheet for the Interview

**Power phrases to weave in naturally:**

| Situation | Phrase |
|---|---|
| Open an answer | *"Sure, let me walk you through…"* **[V29 / S7]** |
| Describe a solution | *"The solution consists of… A key aspect is…"* **[S37 / S41]** |
| Describe quality | *"It's designed to be robust / scalable / reliable"* **[S40]** |
| Show ownership | *"My main responsibility is… I work on…"* **[S32 / S33]** |
| Talk about a problem | *"One challenge was… I dug into… and identified…"* **[S35 / V95 / V39]** |
| Suggest cause | *"That seemed to come from…"* **[S66]** |
| Propose | *"I'd like to propose… / We could consider…"* **[S50 / S67]** |
| Wrap up | *"So just to summarize…"* **[S68]** |
| Show willingness | *"I'll work on…"* **[S23]** |
| Show understanding | *"That makes sense / I see your point"* **[S21 / S22]** |

---

## Practice Plan

1. **Mock interview drill** — pick 5 of these Qs and record yourself in English, time-box to 90 seconds each.
2. **Build a 1-day WalletConnect v2 + Ethers.js demo** in RN — even a minimal "connect → sign message" flow gives a real talking point in Q9.
3. **Refresh BIP39/44 hands-on** — install `bip39` and `ethers` npm packages, generate a mnemonic, derive keys, sign a transaction. ~2 hours.
4. **Prepare 2 STAR stories** in detail (Finhay eKYC + ABC Screen Time) — these map almost 1:1 to JD requirements.
5. **Optional:** draft a **30/60/90-day plan** for the role and bring it as a leave-behind document.

---

## Vocab Index (used in this doc)

| Tag | Word / Phrase | Quick meaning |
|---|---|---|
| **[V7]** | robust | very strong, not easily broken |
| **[V8]** | optimize | make as effective as possible |
| **[V11]** | blocker | something stopping your work |
| **[V13]** | implement | put a plan/system into practice |
| **[V21]** | out of scope | not part of agreed plan |
| **[V29]** | walk through | explain step by step |
| **[V31]** | review | look closely and give opinion |
| **[V39]** | identify | find / pinpoint |
| **[V43]** | work on | spend time improving |
| **[V44]** | goal | main thing to achieve |
| **[V53]** | deploy | make ready and available for users |
| **[V54]** | interface | the part users see and use |
| **[V56]** | architecture | basic plan of a system |
| **[V57]** | infrastructure | basic services / structures |
| **[V59]** | scalable | can grow without problems |
| **[V70]** | propose | offer an idea for consideration |
| **[V73]** | align | come to shared agreement |
| **[V83]** | escalate | send a problem higher up |
| **[V87]** | failure | not succeeding / not working |
| **[V91]** | outage | service is not working |
| **[V92]** | intermittent | happens sometimes, not regularly |
| **[V93]** | reproduce | make a bug happen again on purpose |
| **[V95]** | dig into | examine carefully and in detail |
| **[V98]** | impact | strong effect on a situation |
| **[V99]** | cut down on | use / do less of something |
| **[V100]** | perspective | a particular point of view |
| **[V107]** | integrate | bring different parts to work as one |
| **[V109]** | reliable | trusted to work well every time |

## Structure Index (used in this doc)

| Tag | Structure | Use |
|---|---|---|
| **[S1]** | "Yesterday, I worked on [task] and [outcome]." | Report past work |
| **[S2]** | "Today, I will focus on [task]." | Report planned work |
| **[S7]** | "Walk [someone] through [something]" | Present step by step |
| **[S21]** | "I see your point." | Show understanding |
| **[S22]** | "That makes sense." | Show agreement |
| **[S23]** | "I'll work on …" | Show willingness to improve |
| **[S32]** | "My main responsibility is…" | State primary duty |
| **[S33]** | "I work on…" | Describe current focus |
| **[S34]** | "A typical day involves…" | Describe usual workday |
| **[S35]** | "One challenge is…" | Talk about a difficulty |
| **[S36]** | "I really enjoy…" | Express what you like |
| **[S37]** | "The solution involves / consists of…" | Introduce solution components |
| **[S38]** | "The main benefit is…" | Explain advantages |
| **[S39]** | "We plan to implement this through…" | Outline methodology |
| **[S40]** | "It's designed to be… (scalable / robust)" | Describe characteristics |
| **[S41]** | "A key aspect of this solution is…" | Highlight important parts |
| **[S42]** | "The main purpose of this [section] is to…" | State primary goal |
| **[S44]** | "It's essential to ensure that [element] is …" | Emphasize importance |
| **[S49]** | "I'd like to point out that …" | Emphasize something important |
| **[S50]** | "I'd like to propose that we …" | Suggest improvements |
| **[S52]** | "Let's align on …" | Decide action items |
| **[S58]** | "There's an issue with…" | State a problem exists |
| **[S62]** | "It's an intermittent problem, so…" | Describe inconsistent problem |
| **[S64]** | "From the [team] side, …" | Give perspective |
| **[S66]** | "… seems to come from …" | Identify source |
| **[S67]** | "We could consider + V-ing" | Suggest solution softly |
| **[S68]** | "So just to summarize, …" | Wrap up |
