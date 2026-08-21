Publishing a SmartCuff release
==============================

Checklist. Follow it in order; step 5 is the one that makes the release visible
to users, so nothing after step 5 should be able to fail.

1. Build
   ------
   From the development repo:
       powershell -ExecutionPolicy Bypass -File installer\Build-Installer.ps1

   Confirm installer/SmartCuff.iss AppVersion matches the firmware
   FIRMWARE_VERSION and gui/smartcuff_gui.py EXPECTED_FIRMWARE_VERSION. Those
   three are meant to move in lockstep; a release that ships them out of step
   will make every user's GUI report a firmware mismatch on connect.

2. Stage
   -----
       releases/<version>/SmartCuffSetup-<version>.exe

3. Checksums
   ---------
   From inside releases/<version>/:
       certutil -hashfile SmartCuffSetup-<version>.exe SHA256

   Write SHA256SUMS as:
       <hash>  SmartCuffSetup-<version>.exe

4. Release notes
   -------------
   releases/<version>/RELEASE_NOTES.md

   State plainly what changed in the CONTROL PATH, if anything. Users of this
   device care about deflation behaviour and calibration handling far more than
   about UI polish. If a previous release had a defect that mis-reported
   pressure or mishandled a run, say so and set "mandatory": true in step 5.

5. Manifest
   --------
   Update version.json: version, published, installer_url, sha256, size_bytes,
   min_supported, notes_url, mandatory.

   installer_url form:
       https://github.com/mmjazini/smartcuff-releases/raw/main/releases/<version>/SmartCuffSetup-<version>.exe

   Validate the JSON before committing. A malformed manifest should fail soft in
   the client, but do not lean on that.

6. Push
   ----
       git add -A && git commit -m "release <version>" && git push

7. Verify as a user would
   ----------------------
   Fetch the manifest from the raw URL in a browser or with curl, download the
   installer from installer_url, and check the hash matches. Do this from a
   machine that is NOT logged in to GitHub -- that is the only way to prove the
   channel really is public and no credential is involved.

Things not to do
----------------
* Do not put a token, key, or password in the installer, the manifest, or this
  repo. The whole point of the split is that the client needs no secret.
* Do not force-push or rewrite history here. Clients may be mid-download.
* Do not delete an old release directory. min_supported controls what can
  upgrade in place; removing artifacts breaks users who are behind.
* Do not publish a build that has not been flashed and soaked on hardware. The
  development repo's AGENTS.md requires a 10-minute soak for any firmware
  correction, and a release is the last place to discover a control-path
  regression.
