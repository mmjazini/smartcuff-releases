# SmartCuff Releases

Public download channel for the **SmartCuff** GUI installer.

University of Pittsburgh CHTL (Cardiovascular Monitoring Research Lab).

## Download

**[Download the latest verified Default Smart Cuff installer](https://github.com/mmjazini/smartcuff-releases/releases/download/rev27v-followup-227/SmartCuff_Setup.exe)**

Current Default release: [`rev27v-followup-227`](https://github.com/mmjazini/smartcuff-releases/releases/tag/rev27v-followup-227)

- Installer size: `223,779,957` bytes
- SHA-256: `44461ce276ff58e9e79818f448a02304afe40d00fd1ec9c23a4cec3d883e36a3`
- Bundled firmware: `rev27v-followup-223`

**[Download the latest verified Valinor Smart Cuff installer](https://github.com/mmjazini/smartcuff-releases/releases/download/rev27v-gauge-34/SmartCuff_Setup.exe)**

Current Valinor release: [`rev27v-gauge-34`](https://github.com/mmjazini/smartcuff-releases/releases/tag/rev27v-gauge-34)

- Installer size: `5,664,848` bytes
- SHA-256: `8f0fea0188768e97ca2c128ae19f0877e8ba5057089f9e784512fa448b41ccae`
- Bundled firmware: `rev27v-gauge-27`

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
Default accepts only `rev27v-followup-*`; Valinor accepts only
`rev27v-gauge-*`. Served raw over HTTPS:

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

1. Build the installer (`installer/Build-Installer.ps1`).
2. Verify the generated `.sha256` sidecar against the installer bytes.
3. Publish both files as GitHub Release assets named `SmartCuff_Setup.exe` and
   `SmartCuff_Setup.exe.sha256`.
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
  pump-off, cuff-at-or-below-5-mmHg telemetry. The GUI then relaunches and uses
  its protected one-Due snapshot/flash/restore path if firmware alignment is
  required. The user may select an older verified same-branch release from Help;
  that exact pin remains active until Latest is selected again.
