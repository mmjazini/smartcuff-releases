# SmartCuff Releases

Public download channel for the **SmartCuff** GUI installer.

University of Pittsburgh CHTL (Cardiovascular Monitoring Research Lab).

## Download

**[Download the latest verified Default Smart Cuff installer](https://github.com/mmjazini/smartcuff-releases/releases/download/v3.1.255/SmartCuff_Setup.exe)**

Current Default release: [`v3.1.255`](https://github.com/mmjazini/smartcuff-releases/releases/tag/v3.1.255)

- Installer size: `223,608,133` bytes
- SHA-256: `f4e9c8f2e94e94da1b4905c21db4fb24017d67d73738ed7ef33d312feee20117`
- Bundled firmware: `rev27v-followup-237`
- Signature: unsigned; Windows displays **Unknown publisher**

**[Download the latest verified Valinor Smart Cuff installer](https://github.com/mmjazini/smartcuff-releases/releases/download/v3.2.56/SmartCuff_Setup.exe)**

Current Valinor release: [`v3.2.56`](https://github.com/mmjazini/smartcuff-releases/releases/tag/v3.2.56)

- Installer size: `5,772,501` bytes
- SHA-256: `c7a8b2e6b239539deb0f84c239c8c8772746d5866c889a880544e7b53dab2170`
- Bundled firmware: `rev27v-gauge-34`
- Signature: unsigned; Windows displays **Unknown publisher**

Default includes its offline Python/Arduino runtime. Valinor is a small
online installer; first-time setup needs internet. Both GUIs support offline
operation after installation; NI-DAQmx is installed separately.

The installed Default update was byte-verified and retained all four existing
local calibration hashes. Its protected firmware gate restored and verified
Device D values. The installed ten-minute seed-213 soak passed seven cycles
with zero flags, 100% telemetry delivery, zero pump reversals and valve
reversals at or below 0.158/s (gate 0.5/s). The operational launcher confirmed
firmware 237 and opened the GUI. Final Valinor layout and mock smoke passed.

Flow calibration uses a 2.9-3.1 V rest band for five continuous seconds on NI
sample time. The GUI has clearer workspace navigation, collapsible sequence
details and connection/event panels, and pulse-envelope diagnostics with pause
and figure export. Live and offline BPF defaults use the same causal NI-voltage
filter; alternate zero-phase pressure filtering is explicitly labelled.
NI time stays the reference. Only Arduino voltage timing may align to it.

Default Smart Const accepts deflation oscillogram peaks from 30-140 mmHg and
reinflates to peak +10. Regular remains 80 mmHg nominal / 85 ceiling. Arm/Wrist
CVP gain pump authority below 1 mmHg/s and smooth regional transitions. Raw
pressure and safety checks remain available while pulse-aware feedback limits
controller response at the simulator frequency. Valinor keeps its Max 80-220
menu, gauge 34 firmware and 3 mmHg/s SysID calibration slope.

Default offline preflight passed 13/13 gates with 1,146 tests passed and six
skipped; Valinor passed 13/13 with 1,012 passed and ten skipped. Focused GUI,
protocol parity, layout and smoke checks supplement those suites.

Device D completed a protected firmware 237 flash/readback and ten-minute
seed-213 soak: six cycles passed, zero flags, 100% telemetry delivery, and
valve reversals 0.043-0.276/s against a 0.5/s gate. All five actual GUI protocols
also completed two cycles with the 50 bpm, 1.25 mL simulator. Four had 100%
delivery; High Max had 99.992% (one missing sequence). No candidate raw-envelope
steps were detected at 30/80 mmHg range crossings. Standalone Smart Const
peak +10 was verified. NI timestamps had no backtracks; observed Arduino
voltage residual lag was approximately 33-57 ms.

SysID runs 0092/0093 failed high-pressure-decay / HIGH-band quality gates;
the GUI blocked pushes and retained known-good Device D calibration and
control values. This is not a 1,000-device manufacturing-yield validation.
Unchanged pneumatic pulse amplitude and shape need a matched passive run at
the same pressure. The historical 1.52-second telemetry blackout is not
claimed resolved across all runs. Default simulator findings are not Valinor
hardware validation. Release assets were downloaded and verified by size and
SHA-256 before the application feeds were promoted.

### Windows SmartScreen

The installers are currently unsigned. Windows may show **Windows protected
your PC** and **Unknown publisher**. If—and only if—you downloaded the official
`SmartCuff_Setup.exe` from this GitHub repository:

1. Select **More info**.
2. Verify the app name is `SmartCuff_Setup.exe`.
3. Select **Run anyway**.

Do not continue with a differently named file or an installer from another
source. Each release includes a SHA-256 checksum for independent verification.

### What happens after an update

Smart Cuff closes briefly while the verified installer replaces the
application. A separate supervisor waits for Setup to finish and then reopens
the new copy automatically. If exactly one Arduino Due is attached, the
launcher aligns that controller with the bundled branch firmware before the GUI
opens. With no Due, the GUI opens offline; multiple Dues or a failed upload stop
startup rather than guessing a hardware target.

---

## What this repository is

This repo contains a public version manifest and release documentation. Built
installers are attached as **GitHub Release assets**, not committed as Git
blobs. The channel exists so the installed GUI can check for updates and
download them over plain public HTTPS, with **no credentials of any kind in the
client**.

Source code lives in the private development repository. Only release artifacts
are published here.

## Why it is separate, and why there is no token

An earlier plan was to embed a GitHub token in the installer so every user could
fetch updates. That does not work, and it is worth writing down why so nobody
tries it again:

- **A token in a distributed installer is not a secret.** Anyone who installs the
  GUI can extract it with a text editor.
- Any **write** scope would let every user push to the repo, delete branches, or
  publish releases.
- Even **read-only on a private repo** hands every installer a key to all
  unpublished work.
- **GitHub scans for leaked credentials and auto-revokes them**, so the updater
  would break without warning at an unpredictable time.
- One shared credential cannot be revoked per user, and gives no audit trail.

An updater needs to **read a public thing**, not **authenticate as the
developer**. Those are different problems and only the second one needs a secret.
So: this repo is public, the manifest and installers are fetched by URL, and the
client holds nothing sensitive.

If the requirement is ever *"users must get OUR build, not a substituted one"* —
that is **integrity**, not access, and the answer is a signature the client
verifies, not a token it presents. See `SHA256` in the manifest below as the first
step toward that.

---

## Layout

```
version.json                 Default update manifest
version-valinor.json         Valinor update manifest
releases/<version>/          tracked human-readable release notes

GitHub Releases:
  SmartCuff_Setup.exe        versioned release asset used by the GUI
  SmartCuff_Setup.exe.sha256 matching checksum sidecar
```

## Branch-specific manifests

The GUI selects exactly one manifest from its installed application version.
Default accepts its legacy `rev27v-followup-*` releases and semantic `v3.1.*`
releases; Valinor accepts `rev27v-gauge-*` and `v3.2.*`. Served raw over HTTPS:

```
https://raw.githubusercontent.com/mmjazini/smartcuff-releases/main/version.json
https://raw.githubusercontent.com/mmjazini/smartcuff-releases/main/version-valinor.json
```

Fields:

| field | meaning |
|---|---|
| `version` | the published version string, e.g. `rev27v-followup-199` |
| `published` | ISO date the artifact was uploaded |
| `installer_url` | direct download URL for the installer |
| `sha256` | SHA-256 of the installer, for the client to verify after download |
| `size_bytes` | expected size, a cheap sanity check before hashing |
| `min_supported` | oldest version that can upgrade in place |
| `notes_url` | human-readable release notes |
| `mandatory` | if true, the GUI should refuse to run until updated |

`mandatory` exists for the case that matters on this project: if a released build
is found to mis-report pressure or to have a control-path defect, users must not
keep running it. Everything else is advisory.

## Publishing a release

1. Build the installer. Use `installer/Build-Installer.ps1 -AllowUnsigned` only
   when the operator has explicitly accepted Windows' Unknown publisher warning.
2. Verify the generated `.sha256` sidecar against the installer bytes.
3. Publish both files as GitHub Release assets named `SmartCuff_Setup.exe` and
   `SmartCuff_Setup.exe.sha256`. Unsigned publication also requires the explicit
   `Publish-Release.ps1 -AllowUnsigned` switch.
4. Write and commit `releases/<version>/RELEASE_NOTES.md`.
5. Update the matching branch manifest with the version-specific release-asset
   URL, SHA-256, and byte size.
6. Commit, push, then fetch the manifest and installer without authentication
   and recompute the downloaded file's hash.

The GUI picks it up on its next check. No server, no build system, no credential.

## What the client must do

- Fetch the branch-specific manifest over HTTPS. Fail **soft**: missing Git,
  GitHub, DNS, TLS, or internet must never block startup or offline operation.
- Compare versions. Do not assume string ordering — parse the `rev`/`followup`
  numbering.
- Download to a temp path, **verify `sha256` before executing anything**. A
  mismatch means abort and report, not retry silently.
- Install automatically only after the live safety gate proves no active
  protocol/SysID/calibration and either no connected controller or fresh IDLE,
  pump-off, cuff-at-or-below-5-mmHg telemetry. A detached supervisor waits for
  Setup and relaunches the new installed copy. Before that GUI opens, exactly
  one attached Due is passed through the protected snapshot/flash/restore path;
  no Due is a supported offline case and ambiguous hardware fails closed. The
  user may select an older verified same-branch release from Help;
  that exact pin remains active until Latest is selected again.
