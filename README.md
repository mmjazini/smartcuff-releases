# SmartCuff Releases

Public download channel for the **SmartCuff** GUI installer.

University of Pittsburgh CHTL (Cardiovascular Monitoring Research Lab).

---

## What this repository is

This repo contains **built installers and a version manifest — nothing else**. It
exists so the installed GUI can check for updates and download them over plain
public HTTPS, with **no credentials of any kind in the client**.

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
version.json                 update manifest, read by the GUI
releases/<version>/          one directory per published version
  SmartCuffSetup-<ver>.exe   the installer
  SHA256SUMS                 checksums for everything in that directory
  RELEASE_NOTES.md           what changed
```

## `version.json`

The GUI fetches this single file and compares `version` against its own build.
Served raw over HTTPS:

```
https://raw.githubusercontent.com/mmjazini/smartcuff-releases/main/version.json
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
2. Create `releases/<version>/`, copy the installer in.
3. Generate `SHA256SUMS` in that directory.
4. Write `RELEASE_NOTES.md`.
5. Update `version.json` — including `sha256` and `size_bytes`.
6. Commit and push.

The GUI picks it up on its next check. No server, no build system, no credential.

## What the client must do

- Fetch `version.json` over HTTPS. Fail **soft**: a network error must never block
  a session, because this device is used at a bench and often offline.
- Compare versions. Do not assume string ordering — parse the `rev`/`followup`
  numbering.
- Download to a temp path, **verify `sha256` before executing anything**. A
  mismatch means abort and report, not retry silently.
- Never auto-install mid-session. Offer it, and let the operator choose when.

That last point is not cosmetic. A firmware/GUI version mismatch is already a
known failure mode on this project — the GUI checks
`EXPECTED_FIRMWARE_VERSION` against the device on connect — so an update that
lands between a calibration and a run would produce exactly the confusing
mismatch that check exists to catch.
