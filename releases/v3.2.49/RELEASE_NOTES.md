# Smart Cuff v3.2.49 - complete guide refresh

Valinor application bundle; firmware remains `rev27v-gauge-34`.
Clean source: `e407cc655e26c3ea0f34275f7eb77322bc282b24`.

The complete operator/technical guide and linked PID explainer have been
audited against current source, not just given a new version banner.
All 22 top-level guide anchors remain available through Help -> Smart Cuff Guide.

Corrected guidance includes:

- Valinor's Max 80-220 menu, +10 inflation command margin, five-second peak settle, 3 mmHg/s descent, and continuous terminal vent through the low-pressure confirmation band.
- Protected startup, automatic updates/relaunch, same-branch version pinning,
  offline operation, and non-erasing controller recovery.
- A-H device identity, accepted calibration/SysID persistence, local archives,
  branch-safe snapshots and reviewed newer-only profile distribution.
- Actual recording fields and NI-DAQ reference-clock rules; recording Stop
  is separate from stopping a running protocol.
- OLED bounded recovery and evidence limits, hardware versus GUI indicators,
  current Bench/sinusoid controls, and troubleshooting without factory-reset
  or arbitrary gain-change recipes.

The PID page now uses an explicitly dimensionless PI teaching model. It does
not emulate firmware, recommend physical gains, or promise that every cuff
will be stable. Obsolete revision narratives and operating recipes are
removed; their original history remains in source control.

Validation: 813 passed / 7 skipped, all 13 offline preflight gates,
20 focused guide tests, legacy suites, offscreen GUI smoke and layout audit.
Tests check markup/links, metadata, real preset/constants/configuration
agreement, and execute the model plus all interactive JavaScript controls
using a mocked DOM/canvas. No connected browser was available for visual
rendering. Firmware, control settings and accepted calibration are unchanged;
this documentation-only release does not need another hardware flash/soak.

Automatic fleet upload is not an activated intake service in this release.
Reviewed profiles can still ship through verified application updates.
Separate hardware/measurement/policy tasks are not claimed complete here.

Latest users receive this verified guide bundle through their matching
branch feed at the existing safe idle gate. The installer relaunches Smart Cuff;
attached firmware uses protected snapshot/flash/restore if alignment is needed.
Offline operation and historical same-branch version pins remain supported.
The Valinor installer is intentionally smaller and requires internet for first-time dependency/toolchain setup.

This official installer is unsigned. For **Windows protected your PC**, select
**More info**, verify `SmartCuff_Setup.exe` came from
`mmjazini/smartcuff-releases`, then **Run anyway**. Do not disable Defender
or continue with an installer from a different source. First setup opens
Smart Cuff automatically after its checks succeed.
