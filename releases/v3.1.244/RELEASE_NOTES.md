# Smart Cuff v3.1.244

Bundled firmware wire ID: **rev27v-followup-232**.
Clean source commit: `1e6ddfcb56ae95ce4cc61db17cbfac564d80a83b`.

Default retains all five clinical presets. No PID gains, feedforward, valve limits, calibration equations, or preset-control transitions changed. GUI and standalone continue to use the same branch-specific control rules.

This is the self-contained Default installer: Python, GUI dependencies, Arduino CLI and the Due toolchain are bundled. NI-DAQmx remains a separate optional official installation.

## Validation

- Full suite: **874 passed, 3 skipped**; legacy checks, layout audit and offscreen GUI smoke passed.
- Device C protected flash and calibration/control-setting read-back passed.
- Seed-213 ten-minute soak: **29,536 / 29,536 frames = 100.00% delivery**. All six deflation analyzer verdicts PASS.
- All six cycles have checksum/retry flags, leaving soak exit code 1. This is not a completely clean USB soak.
- Normal display chunk time is about 4.2 ms (maximum observed about 8.3 ms), versus about 113 ms of uninterrupted blocking in the preceding 100 kHz experiment.

| Cycle | Preset | Start -> end mmHg | Valve reversals/s | Verdict |
|---|---|---|---:|---|
| 1 | Smart Const | 166.67 -> 5.04 | 0.000 | PASS |
| 2 | Arm CVP | 53.75 -> 5.02 | 0.037 | PASS |
| 3 | Smart Const | 166.58 -> 5.02 | 0.000 | PASS |
| 4 | Wrist CVP | 78.73 -> 5.00 | 0.050 | PASS |
| 5 | Regular | 166.76 -> 5.08 | 0.022 | PASS |
| 6 | High Max | 196.17 -> 5.01 | 0.019 | PASS |

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
