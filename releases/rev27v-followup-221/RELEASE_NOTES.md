# SmartCuff rev27v-followup-221

Published 2026-09-06.

## What changed

- Added a public, credential-free installer and update channel at
  `mmjazini/smartcuff-releases`.
- Added **Help > Download / Updates** in the GUI.
- The GUI verifies both the declared installer byte size and SHA-256 before it
  offers to launch a downloaded installer.
- Separated the application bundle version from the firmware compatibility
  version so GUI-only releases no longer claim that firmware changed.

## Firmware and control behavior

Firmware remains `rev27v-followup-219`. This release does not change PID,
feedforward, valve limits, timing, calibration equations, safety thresholds, or
protocol control behavior. Existing calibration and device state are preserved.

## Installer integrity

- File: `SmartCuff_Setup.exe`
- Size: `274,611,577` bytes
- SHA-256: `cc4763909049f765f39d7b187182f8364d523b2a404db30504a1102e40c61de4`
- Source commit: `70a8e0c240207f6c2f3e9215487d96a1fd9cd185`

The installer was built from a clean `default` source tree. The complete source
test suite passed with 785 tests passed and 3 skipped; the legacy checks, layout
audit, and GUI smoke test also passed.
