# Smart Cuff v3.2.47 - calibration-safe branch switching

Valinor application update. Bundled firmware remains `rev27v-gauge-33`.

- Live device-state JSON is local data, independent of Default/Valinor source.
  Switching branches no longer requires committing or stashing calibration.
- Reviewed release profiles are separate installer inputs. Accepted newer
  profiles still ship through validated, checksummed updates, with prior local
  files archived and newer local profiles retained.
- The launcher refuses to overwrite ignored data if an old branch still tracks
  it. Genuine source conflicts stop before upload or GUI launch.
- Regression coverage includes new device letters E-H: separate snapshots,
  exact history, optional release staging and cross-device rejection. A-H is
  the supported assigned-ID range; 0/X remain unassigned/unknown. A new physical
  unit needs its own accepted calibration and SysID, not another unit's JSON.

No firmware/control changes, reflash or new soak are needed for this update.
Existing OLED recovery, Max 80-220 menus, and standalone/GUI parity are unchanged.
Recovered USB checksum/retry warnings and fleet intake-service activation remain
separate open tasks; this release does not claim those are resolved.

The GUI defaults to Latest and installs verified updates at its safe idle gate,
then relaunches. Missing GitHub/internet never blocks offline GUI operation.
Help can pin an older same-branch application version.

This official installer is unsigned. If Windows displays **Unknown publisher**
or **Windows protected your PC**, choose **More info**, verify the download is
`SmartCuff_Setup.exe` from `mmjazini/smartcuff-releases`, then **Run anyway**.
Valinor remains a small online installer; first-time setup needs internet.
A successful first setup opens Smart Cuff automatically.

## Verification

- Source commit: `63d30e0cc1f12208edeae185ad64d6f482bf95c6`.
- Installer: 5,709,922 bytes; SHA-256
  `8ee00895dcf806832136a88936792904c79900fd9ba75eb1b88d44b2b96f8638`.
- Full regression suite: 777 passed / 6 skipped on the attached Valinor branch.
- Legacy suite: 7/7; layout and offscreen GUI smoke passed. Both GUIs also
  opened with the packaged runtime with Git/Arduino/NI-DAQ discovery disabled.
- Real Default -> Valinor -> Default round trip preserved every live snapshot
  hash, including unassigned/unknown local files. Failure and new E-H profile
  cases passed in disposable Git repositories without touching hardware.
- Both public installers were downloaded anonymously and matched size/SHA-256.
  Both frozen A-D profile sets match exactly, with per-profile hash receipts.
