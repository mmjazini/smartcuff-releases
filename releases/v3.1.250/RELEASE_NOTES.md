# Smart Cuff v3.1.250 - MAP-paper flow calibration, PID read-back, safer operation

Default application bundle; firmware remains `rev27v-followup-233`.
Clean source: `32a684d7a422eec9f85199217709ef5300959389`.

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
  calibrations do not change until Cal 3 is re-run and accepted. With the
  measured-time integration from the unpublished v3.1.249 (strokes had read
  about 5 % low), a re-run gives gains about 5 % lower than calibrations made
  with v3.1.247 or earlier. Re-run Cal 3 on each Default device.

## Also new since v3.1.247

v3.1.248 and v3.1.249 were never published, so this is their first delivery:

- **Help -> Safety, PID and Controller Reference**, a source-backed reference
  for the safety layers, PID equations, limits and handoffs.
- **GitHub-only fleet calibration** (Help -> Fleet Calibration). Accepted
  Cal 1/2/3 and trusted SysID results are read back, saved, archived and queued
  for the private calibration-intake repository with each contributor's own
  Issues-only token; a maintainer reviews every submission before promotion.
  Cal 2 snapshots use the Arduino-domain coefficients.
- **Cal 3 measured-time integration** (v3.1.249): see Flow calibration above.

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
  before replacing edits that were not applied.
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

- Tab controls and Events share a draggable divider (remembered;
  View -> Reset Panel Layout restores it).
- The window fits a 1366x768 laptop and opens maximized on small screens.
- Connection is shown as a coloured pill; View -> Show Engineering Tabs can
  hide PID, Sys ID, Custom, Display, Bench and Flash for a clinical view
  (shown by default).
- NI-DAQ install pages open once per PC instead of on every launch.

## Validation

Offline pytest 1023 passed / 40 skipped; the only 2 failures are the
branch-name checks, which fail in any checkout that still has a retired
session branch. Legacy suites 7/7, offscreen GUI smoke without exceptions, and
layout audit CLEAN at 1600x1000. The update supervisor was exercised under
PowerShell 7 with simulated Setup exit codes; a real Windows update run, the
launch gate against a controller with older firmware, PID read-back on a
connected controller and a real Cal 3 on each device still need bench
confirmation before publishing.

Latest users receive this bundle through the Default feed at the existing safe
idle gate. The installer relaunches Smart Cuff; attached firmware uses
protected snapshot/flash/restore if alignment is needed. Offline operation and
historical same-branch version pins remain supported. The Default installer
includes its offline Python/Arduino payload.

This official installer is unsigned. For **Windows protected your PC**, select
**More info**, verify `SmartCuff_Setup.exe` came from
`mmjazini/smartcuff-releases`, then **Run anyway**. Do not disable Defender
or continue with an installer from a different source. First setup opens
Smart Cuff automatically after its checks succeed.
