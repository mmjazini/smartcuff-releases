Publishing a SmartCuff release
==============================

Checklist. Follow it in order; step 5 is the one that makes the release visible
to users, so nothing after step 5 should be able to fail.

1. Build
   ------
   From the development repo:
       powershell -ExecutionPolicy Bypass -File installer\Build-Installer.ps1 -AllowUnsigned

   `-AllowUnsigned` is intentional while the operator accepts Windows'
   **Unknown publisher** warning. Omit it when a trusted Authenticode signing
   identity is configured.

   Confirm installer/SmartCuff.iss AppVersion matches
   gui/firmware_version.py APPLICATION_VERSION. Separately confirm its
   FirmwareVersion matches EXPECTED_FIRMWARE_VERSION and the firmware's
   FIRMWARE_VERSION. GUI-only installer releases intentionally advance the app
   version without pretending the firmware changed.

2. Stage
   -----
   Keep the built file named SmartCuff_Setup.exe. Do not copy this large binary
   into the Git repository; attach it to a GitHub Release.

3. Checksums
   ---------
   From the development repository:
       certutil -hashfile installer\SmartCuff_Setup.exe SHA256

   Confirm that result matches installer\SmartCuff_Setup.exe.sha256 exactly.

4. Release notes
   -------------
   Write releases/<version>/RELEASE_NOTES.md in this repository and use the
   same text as the GitHub Release description.

   State plainly what changed in the CONTROL PATH, if anything. Users of this
   device care about deflation behaviour and calibration handling far more than
   about UI polish. If a previous release had a defect that mis-reported
   pressure or mishandled a run, say so and set "mandatory": true in step 5.

5. Publish release assets
   ----------------------
   Create the GitHub Release and upload SmartCuff_Setup.exe plus its `.sha256`
   sidecar. Unsigned publication requires the explicit `-AllowUnsigned` switch
   in `installer\Publish-Release.ps1`. Never replace an existing tag's bytes;
   bump the version instead.

6. Manifest
   --------
   Only after the assets exist, update `version.json` for Default or
   `version-valinor.json` for Valinor: version, published, installer_url,
   sha256, size_bytes, min_supported, notes_url, mandatory. Never point one
   branch at the other branch's release family.

   installer_url form:
       https://github.com/mmjazini/smartcuff-releases/releases/download/<version>/SmartCuff_Setup.exe

   Validate the JSON before committing. A malformed manifest should fail soft in
   the client, but do not lean on that.

7. Push
   ----
       git add -A && git commit -m "release <version>" && git push

8. Verify as a user would
   ----------------------
   Fetch the manifest from the raw URL in a browser or with curl, download the
   installer from installer_url, and check the hash matches. Do this from a
   machine that is NOT logged in to GitHub -- that is the only way to prove the
   channel really is public and no credential is involved.

Things not to do
----------------
* Do not put a token, key, or password in the installer, the manifest, or this
  repo. The whole point of the split is that the client needs no secret.
* Do not commit the installer binary to Git. GitHub Release assets are the
  supported large-file download path.
* Do not force-push or rewrite history here. Clients may be mid-download.
* Do not delete an old release directory. min_supported controls what can
  upgrade in place; removing artifacts breaks users who are behind.
* Do not publish a build that has not been flashed and soaked on hardware. The
  development repo's AGENTS.md requires a 10-minute soak for any firmware
  correction, and a release is the last place to discover a control-path
  regression.
