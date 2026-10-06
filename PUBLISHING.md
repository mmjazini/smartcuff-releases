# Publishing verified application updates

User 2026-10-06 (rev27v-followup-256): publish complete Default and Valinor
applications from clean attributable source, preserving their distinct channels.

Commit the intended source changes first. Build in a clean checkout containing
only reviewed source and release profiles. Keep operator recordings, live
calibration, tokens and `.last_flash.json` out of that checkout.

```powershell
powershell -ExecutionPolicy Bypass -File installer/Build-Installer.ps1 -AllowUnsigned
powershell -ExecutionPolicy Bypass -File installer/Publish-Release.ps1 -AllowUnsigned -ValidateOnly
powershell -ExecutionPolicy Bypass -File installer/Publish-Release.ps1 -AllowUnsigned -NotesFile <release-notes> -PromoteFeed
```

Use trusted Authenticode when available. The operator has explicitly accepted
unsigned builds; both commands require `-AllowUnsigned`. On a first install,
Windows SmartScreen may require **More info -> Run anyway** for the official
`SmartCuff_Setup.exe` downloaded from `mmjazini/smartcuff-releases`.

Build writes an ignored `.exe.build.json` receipt binding branch, clean commit,
application version, firmware, size, SHA-256 and UTC build time. Publication
refuses stale bytes or source, wrong channel, existing release tags and draft
feed promotion. It downloads and verifies the published installer before
updating the feed with GitHub's current content SHA. Default `v3.1.*` remains
GitHub Latest; Valinor `v3.2.*` uses `version-valinor.json`. Never replace release
bytes in place. If promotion fails after publication, the release remains
available and the feed is unchanged; verify the release and perform a reviewed
feed update instead of recreating it.

Accepted profile inputs remain `installer/calibration_profiles`; newer reviewed
profiles require explicit `-CalibrationProfilesDir`. Live `gui/calibration`
snapshots stay local and ignored. Preserve the previous installer separately
when retaining historical local outputs, and record build receipts and download
verification evidence under `build/release_verification`.
