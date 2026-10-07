Smart Cuff Valinor v3.2.56 improves flow calibration, workspace navigation and pulse diagnostics. The Max 80–220 menu, gauge 34 firmware and 3 mmHg/s SysID calibration slope are preserved.

Flow calibration uses the fixed 2.9–3.1 V rest band for five continuous NI sample seconds. Pulse diagnostics show raw pressure envelopes, feedback, actual actuators and phase markers, with pause and figure export. Connections and Events expand on demand; sequence details can collapse. Offline BPF defaults to the live causal NI-voltage filter, with the alternative pressure zero-phase mode labelled. NI timestamps remain the reference; Fluke zero requires ten vented seconds.

Rejected SysID results cannot offer default PID gains or replace snapshots. Diagnostic override cannot bypass the high-decay push gate, and accepted push provenance requires ACK/save/readback verification. Saved local calibration is preserved. Offline preflight passed 13/13 gates with 1,012 tests passed and ten skipped; common host fixes have focused GUI coverage.

The Valinor firmware was not changed in this release. Default Device D simulator results are documented separately and are not a Valinor hardware validation or a 1,000-device yield claim.

Final Valinor layout audit was clean and mock GUI smoke completed without exceptions. Parity checks cover every Max preset from 80 through 220 mmHg.

Unsigned installer: Windows may show Unknown publisher. On first install choose More info → Run anyway. Valinor is the small online installer; the GUI opens offline after dependencies are installed. NI-DAQmx is separately installed. Published bytes are verified by size and SHA-256.
