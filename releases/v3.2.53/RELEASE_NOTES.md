# Smart Cuff Valinor v3.2.53

Flow calibration consumes every raw Ch2 sample on the actual NI-DAQ clock.
Nominal flow=2*(V-V0) L/min; area times1000/60 gives mL. K=known mL/nominal mL:
300/260=1.153846, dimensionless. Existing coefficients are not rescaled. The
GUI shows measured volume, K and stroke selection. Cancel preserves stored
calibration; invalid/one-sided fits and DAQ discontinuities cannot be applied.
SET/save/device readback precedes snapshot/fleet capture. Baseline, full-precision
strokes, partial traces and selection masks are archived. Physical syringe
accuracy remains to be measured; this release does not push example gains.

Publisher binds installer bytes to a clean source/firmware/version receipt and
verifies downloaded size/SHA before the separate Valinor feed advances. Max80-220
GUI/standalone gauge control and firmware rev27v-gauge-34 are unchanged. Reviewed
profile inputs are unchanged; live calibration snapshots remain local/ignored.

The official online installer is unsigned and first setup needs internet.
Windows may show Windows protected your PC and Unknown publisher. For official
SmartCuff_Setup.exe from mmjazini/smartcuff-releases, choose **More info -> Run
anyway**. Smart Cuff opens automatically after successful setup and supports
offline operation after installation. NI-DAQmx is installed separately.

Verified build: 2026-10-06T16:56:01.9791772Z; source `447201741325a787b408a5806ca561960315d88f`; firmware `rev27v-gauge-34`.
Installer 5,754,999 bytes; SHA-256 `57e8b9e801a7d101ee5f66842bf250c6d71edb456f6d58a995003fe459563a7e`. Unsigned.
