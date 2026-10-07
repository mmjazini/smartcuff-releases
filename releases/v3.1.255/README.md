Smart Cuff v3.1.255 improves protocol navigation, flow calibration and pulse diagnostics while preserving saved device calibration.

- Flow rest is 2.9–3.1 V for five continuous seconds on NI sample time.
- Smart Const accepts oscillogram peaks from 30–140 mmHg and approaches peak +10 over the configured ramp. Regular remains 80 nominal / 85 ceiling. Standalone and GUI use the same controller path.
- Raw pulse-envelope diagnostics include controller feedback, actual pump/valve duty and phase markers, with pause and figure export. Connections and Events expand on demand; sequence details can collapse.
- Offline BPF defaults to the live causal NI-voltage filter. Alternate zero-phase pressure filtering is explicitly labelled. NI timestamps remain the reference; reference zero requires ten vented seconds.
- Rejected SysID results cannot offer default PID gains or replace snapshots. Diagnostic override cannot bypass the high-decay push gate. Push acceptance requires command ACKs and save/readback verification.

Device D bench: protected firmware 237 flash/readback and ten-minute seed-213 soak passed all six cycles with 100% telemetry delivery and zero flags. All five actual GUI protocols completed two cycles with the 50 bpm simulator: four had 100% delivery; High Max had 99.992% (one missing sequence). No candidate raw-envelope steps appeared at 30/80 mmHg region crossings. Standalone Smart Const peak +10 was verified. NI timestamps had no backtracks; observed Arduino-voltage residual lag was approximately 33–57 ms.

SysID runs 0092/0093 were rejected for high-pressure decay and unreliable HIGH-band characterization; the previous Device D calibration, PID, feedforward and valve limits were confirmed unchanged. This does not establish 1,000-device manufacturing yield or unchanged pneumatic pulse shape without a matched passive baseline. The historical telemetry blackout is not claimed resolved across all runs.

Installed-app verification: 49 changed files and BUILD_INFO matched the release; all four existing local calibration files retained their hashes. The installed protected flash restored and read back Device D values. Its ten-minute seed-213 soak passed seven cycles, zero flags, 100% delivery, zero pump reversals and valve reversals at or below 0.158/s (gate 0.5/s). The installed launcher confirmed firmware 237 before opening the GUI. Default preflight passed 13/13 gates with 1,146 tests passed and six skipped; final focused checks, layout and smoke passed.

Unsigned installer: Windows may show Unknown publisher. On a first install choose More info → Run anyway. Default includes its Python/Arduino offline runtime; NI-DAQmx remains separately installed. Published bytes are verified by size and SHA-256.
