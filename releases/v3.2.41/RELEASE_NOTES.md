# Smart Cuff v3.2.41 (Valinor)

This release strengthens communication with the Arduino Due while preserving
Valinor's existing Max 80 through Max 220 control behavior.

- Commands now use tagged CRC-16 protection so damaged or truncated frames are
  rejected instead of being interpreted as valid settings.
- The controller reports the exact parsed arguments for every accepted protocol
  command, and SysID stops safely if they do not match what the GUI sent.
- Serial writes and retries remain bounded so communication faults cannot freeze
  the display or GUI.
- Firmware `rev27v-gauge-29` is bundled and installed through the protected
  snapshot/flash/restore path when one Arduino Due is attached.

Validation: 705 tests passed (7 skipped), firmware compiled successfully, and a
fixed-seed ten-minute Device A soak completed 8/8 Max cycles with 100.00%
telemetry delivery and live OLED refresh. The control hard gate passed. The
physical Programming Port remains noisy; four corrupted frames were detected,
rejected, and recovered without unsafe command execution.

This installer is unsigned. Windows may show **Unknown publisher**. When using
the official download from this repository, select **More info**, verify the
file is named `SmartCuff_Setup.exe`, then select **Run anyway**.
