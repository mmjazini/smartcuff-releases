# Smart Cuff v3.2.46

Bundled firmware wire ID: **rev27v-gauge-33**.
Clean source commit: `25521d3558c34f6d26ebb967cff7589317902e71`.

Valinor retains all fifteen Max 80-220 presets. No PID gains, feedforward, valve limits, calibration equations, or Max-preset control transitions changed. GUI and standalone continue to use the same Valinor control profile.

This is Valinor's small online installer, by design. First-time setup needs internet to provision Python and the Arduino tooling. After setup, the GUI works without internet or attached hardware. NI-DAQmx remains optional and separate.

## Validation

- Full suite: **751 passed, 7 skipped**; legacy checks, layout audit and offscreen GUI smoke passed.
- Device C protected flash and calibration/control-setting read-back passed.
- Seed-213 ten-minute soak: **29,682 / 29,682 frames = 100.00% delivery**, 44.41 Hz received cadence; scorer **HARD-GATE PASS**.
- Three CRC failures recovered. Soak exit code 1, scorer exit code 0. One emission gap reached 0.89 s; no emitted-frame loss or repressurization was observed.
- OLED counters advanced from 1,041 to 604,026, with a final draw age of 924 ms. Physical pixels were not inspected by software.

| Cycle | Preset | Deflation mmHg/s | Valve reversals/s | Repressurization |
|---|---|---:|---:|---:|
| 1 | Max 140 | 2.24 | 0.089 | 0 |
| 2 | Max 200 | 2.45 | 0.030 | 0 |
| 3 | Max 140 | 2.27 | 0.043 | 0 |
| 4 | Max 180 | 2.26 | 0.063 | 0 |
| 5 | Max 120 | 2.25 | 0.101 | 0 |
| 6 | Max 160 | 2.33 | 0.075 | 0 |
| 7 | Max 200 | 2.47 | 0.061 | 0 |
| 8 | Max 220 | 2.52 | 0.055 | 0 |

## What changed

- OLED runtime frames use frozen 32-byte chunks at 100 kHz, returning to the main loop between chunks. Boot reset timing, periodic controller-state repair, bounded recovery, and a static-screen heartbeat are retained.
- Flash now offers **Recover Unresponsive Controller**. It requires exactly one Due and an explicitly selected Device A-H profile, tries a boot-flag repair without erase first, and uses protected flash/restore/read-back only when needed. Active work and fresh unsafe telemetry block recovery. Remove the cuff from any person and physically vent it before recovering a silent controller.
- Operational commands and automatic Live View wait for the exact bundled firmware. The GUI cannot reclaim the serial port while its uploader owns it.
- Installed shortcuts use pythonw without a persistent console. An existing developer PowerShell window returns to its prompt; it is not closed by the application.
- Current validated A-D profiles are packaged separately from the clean source commit. PROFILE_BUILD_INFO.json records exact profile sizes and SHA-256 hashes. A newer local profile is retained; prior local JSON is archived before a newer release profile is adopted.
- The heart logo and **Smart Cuff** application name are unchanged.

## Updates and rollback

Latest remains the initial update choice. The GUI verifies the branch-specific installer URL, size, and SHA-256, waits for a safe idle/pump-off/vented state, installs automatically, and relaunches. Attached firmware uses the protected snapshot/flash/restore path before operational use. No GitHub write credential is included.

Help -> Select Update Version can pin an older verified same-branch release. Selecting Latest resumes automatic upgrades. If a run develops unexpected actuator behavior, stop and vent, preserve its logs, and use a known-good same-branch release at a safe idle point. Missing internet/GitHub does not prevent using an already installed GUI.

## Windows Unknown publisher

This official installer is intentionally unsigned while a trusted institutional certificate is unavailable. If Windows shows **Windows protected your PC** or **Unknown publisher**, select **More info**, verify **SmartCuff_Setup.exe** came from **mmjazini/smartcuff-releases**, and select **Run anyway**. Do not proceed for an unrelated file or download source. Setup opens Smart Cuff after its required checks succeed.

## Evidence limits

Successful I2C transfers do not prove correct physical pixels. Both branches resumed drawing after a bounded five-second idle master-side bus-hang injection without a watchdog reset. That is not a cold-power test or a real slave electrical fault test. Repeated cold-power visual checks, rail captures, and SCL/SDA rise-time measurements remain operator-owned; this release does not claim that the OLED can never freeze or that wiring is the proven root cause.

The USB link still has recovered checksum/retry warnings. Exact delivery describes received versus emitted frames, not achievement of a nominal 50 Hz cadence. The automatic fleet-calibration intake service is not active; reviewed calibration distribution through these application bundles is active.
