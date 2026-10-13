# MEETING Q&A — Aurora App (English)

This document summarizes the key points about the Aurora React Native app. Rewritten using learned SpeakFun structures and vocabulary.

## A. Architecture & products

Q1. What is Aurora, and how many product lines are there?
- Aurora is a React Native (0.80.2) app for configuring and managing wireless network hardware. There are three separate product lines and they do not connect to each other — the app blocks switching flows.
  - Bunning — outdoor cellular ODU (4G/5G). Communicates over BLE and requires a SIM.
  - Leopard — ISP-branded router. Communicates over WiFi + REST/SSL.
  - Nest — AurusGate (NF71) gateway + satellite mesh. Uses WiFi/Ethernet + REST.

Q2. What do the part codes (CFW-/NFW-/NF71-) mean, and where do they come from?
- They are device model numbers mapped to a specific flow: `CFW-3211/3212`, `NFW-3412` → Bunning; `CFW-4221/4222` → Leopard; `NF71/NF70` → Nest. The list is hardcoded in the app (DeviceConstants.DeviceList) and is not returned by the Pi/server. The Pi is used only after a model is selected.

Q3. AurusGate vs Satellite vs ODU — what's the difference?
- AurusGate: the central Nest gateway/router that connects to the Internet, broadcasts WiFi, manages VoIP and the mesh controller.
- Satellite: a secondary node that extends WiFi and links back to AurusGate; the Gateway must be set up first.
- ODU (Bunning): an outdoor 4G/5G device that does not broadcast WiFi itself — attach a router via a PoE injector.

## B. Two kinds of emulation (easy to confuse)

Q4. How does Demonstration Mode work, and where does the fake data come from?
- Demonstration Mode sets `connectionMode = standalone` (toggle: long-press the house image for 5s). Fake data sources:
  - REST (Nest/Leopard): hardcoded JS objects in `AuroraServer.js`.
  - BLE (Bunning): bundled Realm DB `emulator.realm` replayed by scenario × step.
  => No real network or BLE traffic in Demo Mode.

Q5. What's the Raspberry Pi for, and how does it differ from Demo Mode?
- A Pi 3B runs the DeviceEmulator package: an Eagle REST server (Nest, port :24242) and a GATT server for Bunning (port :5000). The app connects to the Pi like real hardware (`connectionMode = device`). This is used for QA and integration testing close to reality; Demo Mode only fakes data inside the app.

Q6. How does the app connect to the Pi (without real router hardware)?
- Put the Pi IP in `custom-gw-ip.txt`, build the app, and `version/git_info.py` writes `CUSTOM_GW_IP` into `version/index.js` — `network.js` uses that IP in debug builds only. Safety: when `IS_RELEASE=1` this test hack is ignored so it cannot leak into store builds.

## C. Real-world Pi setup — lessons learned

Q7. Any "gotchas" when setting up the Pi?
- Power: a Pi 3B is not powered over Ethernet — provide a proper 5V/2.5A supply.
- Networking: for direct Mac↔Pi cable, enable macOS Internet Sharing so the Pi gets an IP (current example: `aurus-0000` @ 192.168.2.2).
- SSH keys: a non-standard key name (e.g. `id_id_ed25519_zhone_pi`) means plain `ssh` may not offer it — fix with a `~/.ssh/config` entry.

Q8. What's the Pi's hardware status?
- Pi 3B — 1GB RAM (~540MB free), 16GB SD (~9GB free), temperature ~49°C (acceptable). Warning: currently under-voltage (`throttled=0x50005`) → CPU throttled to 600MHz. Action item: switch to a proper 5V/2.5A supply to avoid throttling and possible SD corruption.

## D. Nest flow & network tracing (technical)

Q9. Which screens actually call REST to the device in the Nest flow?
- Instructional screens (ConnectAurusGate, PoweringUp…) make no REST calls. `WifiConnection` is the first screen to call REST (`getStatusId → getConfig → getStatusWAN`). Subsequent REST activity occurs in `ConnectToTheInternet` (updateConfigWAN), `SetupWifiNest` (getConfigWLAN), `SettingUpWifi` (updateConfigWLAN), `AurusGateReady` (updateConfig), and `Dashboard` (periodic polling).

Q10. When are `getStatusId` / `getConfig` / `getStatusWAN` called?
- Calls are chained and conditional: `getStatusId` is called only once an admin password exists. If OK, `getConfig` may run (for a fresh device); if config is incomplete, `getStatusWAN` decides the route to `SetupWifiNest` (WAN up) or `ConnectToTheInternet` (WAN down). Call counts by branch: 0 (no password yet), 1 (wrong password / reconnect), 2 (already configured → Dashboard), 3 (brand-new device).

Q11. Why do you "see no network logs" during testing? How to trace it?
- If no admin password is entered the app reaches the Auth screen before any REST call — so there are no calls by design. Also, `react-native-ssl-pinning` uses a native fetch which does not appear in Chrome/Flipper Network tab. Trace options: use `eagle.bash` logs on the Pi (live requests), check `screen.Api` logs in the app console, or set a Hermes breakpoint in `NestAPIService` / `network.js`.

## E. (If the meeting touches design)

Q12. Can we read the "Nest Flow – DZS INTERNAL" Figma via MCP yet?
- Not yet. The signed-in account (`long.nguyennhat@zhone.com`) is on a Starter/View seat and is not a member of the DZS Org that owns the file, so MCP denies access. Action: a DZS admin must add the account to the Org with a Dev/Full seat. Meanwhile, use screenshots.

---

Action Items / Notes:
- **Deliverable:** Bring this MEETING_QA.md to the meeting and ask the BA/Dev lead to sign off on outstanding Pi setup actions.
- **Blocker:** Pi under-voltage — fix power supply before long QA runs.
- **Out of scope:** Changing hardcoded device lists in production builds (this is a debug/test-only flow).

