# Smart Cuff v3.2.42 (Valinor)

This release removes an ambiguous firmware contract while preserving Valinor's
Max 80 through Max 220 pressure-control behavior.

- The six broad knob zones used to detect operator takeover now have their own
  named count and a hard table bound, including at exact full-scale travel.
- The 15 exact Max targets remain 80 through 220 mmHg in 10-mmHg steps and
  continue to use the dedicated pressure reader.
- No target, PID, feedforward value, valve limit, timing, calibration equation,
  endpoint, or safety threshold changed.
- Firmware `rev27v-gauge-30` is bundled and installed through the protected
  snapshot/flash/restore path when one Arduino Due is attached.
- The tagged CRC-16 USB protections from v3.2.41 remain in place.

Validation: 707 tests passed (7 skipped), firmware compiled successfully, and a
fixed-seed ten-minute Device A soak completed 8/8 Max cycles with
17,294/17,294 frames (100.00% delivery), no sequence gaps, a live OLED, and a
control hard-gate PASS. Six corrupted Programming-Port frames were detected,
rejected, and recovered without losing a control or telemetry frame.

This installer is intentionally unsigned. Windows may show **Windows protected
your PC** and **Unknown publisher**. If and only if you downloaded the official
`SmartCuff_Setup.exe` from this GitHub repository, select **More info**, verify
the file is named `SmartCuff_Setup.exe`, then select **Run anyway**. Do not
continue with a differently named file or an installer from another source.
