# Drive Grader — The Napkin

## 1. Shape
A real-time telemetry logging and event-tagging system wrapped around a compliance form generator — not a CRUD app, though it has a CRUD admin panel bolted on.

```
[Phone GPS/Accel] --\
                      >--[Session Recorder]--[Local buffer]--[Sync]--[MySQL]
[OBD2 via BLE]  ----/                              |
                                                [DL-40 PDF generator]
```

## 2. The Hard Part
Bluetooth OBD2 reliability from a phone during a live drive — intermittent connection, PID support that varies wildly by vehicle make/year, and the turn-signal/brake-light question probably resolving to "no, standard OBD2 PIDs don't expose that" (those are body control module signals, not powertrain, and aren't on the standard PID list most cheap Bluetooth dongles support). This is the thing that can quietly eat half the semester if the team treats it as a known quantity instead of a spike.

## 3. Bottleneck
Not load, not team size — it's the PWA-to-native decision. If Bluetooth Classic/BLE device access genuinely requires Capacitor, that's not a tweak, it's a second build target, a second deploy pipeline, and possibly a second round of platform-specific permission/testing pain, discovered mid-semester instead of week 2.

## 4. Stack
Keep Quasar/Node/MySQL — boring and already chosen, no reason to relitigate it. The one real decision is PWA vs. Capacitor, and that should be answered by a spike, not a preference.

## 5. Kill Risks (mechanisms, not categories)
- OBD2 dongles don't expose brake/signal data on standard PIDs, so that feature is cut, not delayed — the team needs to know this by week 2, not week 10.
- Web Bluetooth/GPS backgrounding is unreliable on iOS Safari specifically (Apple restricts background BLE and has historically been slow on Web Bluetooth support at all), so "PWA works" may mean "PWA works on Android only," silently narrowing the deliverable.
- EVs often report far fewer standard PIDs (no engine RPM, different speed source), so "OBD2 support" may need to become "OBD2 support, gas vehicles only" without anyone deciding that on purpose.

## 6. Verdict
Feasible as an MVP if the team treats hardware integration as the risk to retire first, not last.

**What to cut first if time runs short:**
- Turn-signal/brake-light capture (nice-to-have, not core to DL-40 grading)
- EV support

Ship gas-vehicle OBD2 + manual grading + DL-40 export as the floor, and add anything else only after the spike proves it's cheap.

---

## Diff Worth Having
Did anyone already burn a day plugging in those 4 dongles, or is "we'll test it" still theoretical? That's the fact that should have driven prompt 2 and didn't yet.
