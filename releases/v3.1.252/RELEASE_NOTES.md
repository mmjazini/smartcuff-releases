# Smart Cuff v3.1.252

Default Arm and Wrist CVP share inflation landing, deflation cadence and final
vent handling in GUI and standalone operation. Their command targets remain
55 and 80 mmHg. Device D's protected firmware234 upload preserved Cal1/2/3,
SysID, PID and valve settings. A 10-minute seed213 soak completed seven cycles
without faults: slope ratios 0.93–0.95, valve reversals 0.049–0.152/s, zero pump
reversals, 100% active-control telemetry delivery. Standalone Arm/Wrist macros
also completed at zero pressure, with 0.080/0.098 valve reversals/s and 100%
delivery. Peaks were 56.03–58.75 / 82.70–83.12 mmHg; targets are not hard clamps.
One malformed startup row is preserved; historical link blackout remains a
limitation outside these measured sessions. This is bench evidence, not clinical
validation or a promise of identical performance across cuffs/devices.

Flow calibration integrates every raw Ch2 sample on the actual NI-DAQ clock.
`2*(V-V0)` is nominal L/min. Its area times 1000/60 gives mL; K = known mL /
measured mL, so 300/260 = 1.153846. Existing coefficients are not rescaled.
Full strokes, baseline, selections and partial traces are archived. DAQ restart,
missing/duplicate data and invalid fits are rejected. Apply requires three
consistent strokes per direction; Cancel preserves device calibration. Matching
SET/save/device read-back precede snapshot/fleet capture. A fresh physical syringe
calibration is still needed to establish measured sensor accuracy.

The GUI shows nominal mL and dimensionless K. Publisher receipts bind clean
source/firmware/version to installer bytes; verified download precedes channel
feed promotion. Valinor remains a separate application channel. Reviewed profile
inputs are unchanged; live calibration snapshots remain local and ignored.

Validation: firmware compile, protected flash, scored soak, standalone end-to-end,
offline preflight, calibration/DAQ/publisher regression tests and mocked GUI smoke.
Six pre-existing PID spinner clipping warnings remain.

This official installer is unsigned. Windows may show **Windows protected your
PC** and **Unknown publisher**. For `SmartCuff_Setup.exe` downloaded from
`mmjazini/smartcuff-releases`, choose **More info -> Run anyway**. Setup opens
Smart Cuff automatically after successful checks. NI-DAQmx remains a separate
manual installation; the GUI supports offline operation.

Verified build: 2026-10-06T17:06:49.2266368Z; source `1d2632f8f2972c88a0155276becc6c7574c683e4`; firmware `rev27v-followup-234`.
Installer 223,555,082 bytes; SHA-256 `7882715048bfa169147ffb9d934d547214a3fdd21d6fafb110c9c8f2eb226774`. Unsigned.
