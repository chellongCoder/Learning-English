# MEETING Q&A — 5 Must-Know Points (Short)

1. What is Aurora and how many product lines are there?
- Aurora is a React Native app for configuring wireless hardware. There are three product lines: Bunning (ODU, BLE + SIM), Leopard (router, WiFi + REST), and Nest (AurusGate + satellite mesh).

2. Where do device part codes come from?
- Part codes are device model numbers and are hardcoded in the app (DeviceConstants.DeviceList). The Pi/server does not return them.

3. Demo Mode vs Pi emulator — what's the difference?
- Demo Mode fakes data inside the app (`connectionMode = standalone`). The Pi runs DeviceEmulator (Eagle REST + GATT) so the app talks to a fake device on a real network (`connectionMode = device`). Use Pi for QA when you need network realism.

4. Top Pi setup gotchas (quick):
- Use a proper 5V/2.5A supply, enable macOS Internet Sharing for direct Mac↔Pi cable, and add the SSH key to `~/.ssh/config` if it has a non-standard name.

5. How to trace Nest REST calls when you "see no logs"?
- `react-native-ssl-pinning` uses native fetch (not visible in Chrome/Flipper). Trace via `eagle.bash` on the Pi, `screen.Api` logs, or by setting a Hermes breakpoint in `NestAPIService` / `network.js`.

---

If you want, I can translate this short version into Vietnamese or further simplify it for a 5-minute meeting.
