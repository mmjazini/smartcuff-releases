# Smart Cuff v3.2.51 - MAP-paper flow calibration, PID read-back, safer operation

Valinor application bundle; firmware remains `rev27v-gauge-34`.
Clean source: `b4cc79f5291c473c4cead51c369610d6c383eea1`.

## Control path

Firmware, valve limits and safety thresholds are unchanged. Three GUI
changes alter what can be sent to the controller:

- **Apply PID defect fixed.** Earlier versions sent all ten PID-tab values on
  every Apply PID press. The inflation gains stored in the controller
  (SysID-tuned) were never read, so pressing Apply PID replaced them with the
  GUI defaults 0.30/0.05/0.00. The deflation gains were read rounded to one
  decimal, and a 15 % correction limit was pushed while the firmware runs 50 %.
  Now the GUI reads both gain sets from the controller on connect (4 decimals)
  and Apply PID sends only the groups the operator changed. If you pressed
  Apply PID on an earlier version, re-apply the accepted SysID PID for that
  device.
- **Actions refuse without a link.** Protocol Apply All, Apply PID, Custom
  Run, Sync from Device and every Bench tool now refuse, with a message,
  when no device is connected. A sweep, leak compensation, Hold, Push-Pump or
  leak-rate measurement left running is stopped when the link drops, so it
  cannot drive the next controller that connects.
- **Flow calibration (Cal 3) follows the MAP paper.** Each stroke's area now
  covers the whole stroke, including its start before the 0.1 V trigger.
  Gains are referred to the mean stroke area. When IN and OUT differ by no
  more than the stroke-to-stroke scatter, one pooled gain is used for both Kp
  and Kn, as in the paper; otherwise each direction keeps its own. The review
  plots every stroke against the resting voltage with its area. Stored
  calibrations do not change until Cal 3 is re-run and accepted. This branch
  also receives the measured-time integration Default made in
  rev27v-followup-234: Cal 3 had read every stroke about 5 % low, so earlier
  Valinor flow gains are about 5 % high. Re-run Cal 3 on each Valinor device.

## Also new to the Valinor feed

v3.2.50 was published as a GitHub Release but never became the feed's Latest,
so automatic updates deliver its changes for the first time here:

- **Help -> Safety, PID and Controller Reference**, a source-backed reference
  for the safety layers, PID equations, limits and handoffs.
- **GitHub-only fleet calibration** (Help -> Fleet Calibration). Accepted
  Cal 1/2/3 and trusted SysID results are read back, saved, archived and queued
  for the private calibration-intake repository with each contributor's own
  Issues-only token; a maintainer reviews every submission before promotion.
  Cal 2 snapshots use the Arduino-domain coefficients.

## Safety and correctness

- **STOP from every tab.** A STOP button sits in the tab bar and **Esc**
  triggers it: it stops any calibration, SysID or Bench tool, then STOP, pump
  off, valve open.
- **Factory Reset needs a connected device** and no longer deletes saved
  presets when the reset was not sent; presets are moved aside, not deleted.
- **Calibration and SysID pushes are device-scoped.** Load previous SysID lists
  only the connected device's runs, and a saved config from another device
  letter is refused.
- **No fake live values.** Without a device the Display tab and Live Status
  show "No device" and dashes instead of IDLE, a lit power lamp and 0.0 mmHg,
  and a cable pull clears them.
- **TX log is truthful.** Commands that never reached the controller are logged
  as `[TX DROPPED]`.
- **Protocol Parameters** open with the selected preset's values, and Run asks
  before replacing edits that were not applied. A Max run ends at 40 mmHg and
  never uses Constant, Duration or Break; those fields sit at their minimums
  and the caption says so.
- The **Steadier CVP** checkbox is hidden: nothing on this branch used it.
- **Log tab** writes under `gui\recordings\<date>` and never overwrites a file.

## Startup, updates and firmware

- The launcher now asks the controller which firmware it runs. If it differs
  from this bundle, the protected flash (snapshot, upload, restore) runs before
  the GUI opens, even when this PC's flash record says it is current.
- A failed automatic update now relaunches the existing Smart Cuff instead of
  leaving no window. After two failures of the same release within 24 hours,
  automatic install of that release pauses; Help -> Check for Updates retries.
  A failed update check is retried after 1/5/15/30/60 minutes, old installers
  are pruned, and uninstall removes the update cache.

## Layout

- **File, Edit and View menus** added (this branch had only Help), matching
  Default: Open Recordings Folder, Save Diagnostic Bundle, Copy Event Log,
  Clear Plot Data, Reset Panel Layout, Show Engineering Tabs, Show
  Notifications.
- Tab controls and Events share a draggable divider (remembered;
  View -> Reset Panel Layout restores it).
- The window fits a 1366x768 laptop and opens maximized on small screens.
- Connection is shown as a coloured pill; View -> Show Engineering Tabs can
  hide PID, Sys ID, Custom, Display, Bench and Flash for a clinical view
  (shown by default).
- NI-DAQ install pages open once per PC instead of on every launch.

## Validation

Offline pytest 916 passed / 35 skipped; the only 2 failures are the
branch-name checks, which fail in any checkout that still has a retired
session branch. Legacy suites 7/7, offscreen GUI smoke without exceptions, and
layout audit CLEAN at 1600x1000. The update supervisor was exercised under
PowerShell 7 with simulated Setup exit codes; a real Windows update run, the
launch gate against a controller with older firmware, PID read-back on a
connected controller and a real Cal 3 on each device still need bench
confirmation before publishing.

Latest users receive this bundle through the Valinor feed at the existing safe
idle gate. The installer relaunches Smart Cuff; attached firmware uses
protected snapshot/flash/restore if alignment is needed. Offline operation and
historical same-branch version pins remain supported. The Valinor installer
is intentionally smaller and requires internet for first-time
dependency/toolchain setup.

This official installer is unsigned. For **Windows protected your PC**, select
**More info**, verify `SmartCuff_Setup.exe` came from
`mmjazini/smartcuff-releases`, then **Run anyway**. Do not disable Defender
or continue with an installer from a different source. First setup opens
Smart Cuff automatically after its checks succeed.
