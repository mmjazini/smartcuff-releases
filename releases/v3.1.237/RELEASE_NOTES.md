# Smart Cuff v3.1.237 (Default)

- Treats the bounded post-zero deflation window as normal completion: 15
  seconds for non-CVP and 30 seconds for CVP, with no error or forced dump.
  Non-CVP presets continue sealed into the existing hold+5 inflation.
- Uses tagged CRC-16 command frames, bounded writes, retry/readback checks, and
  parsed EXEC argument verification to harden the intermittent USB path while
  retaining compatibility with earlier firmware frames.
- Shows `MAP DETECTED / <value>` after a valid Smart Const envelope and
  `MAP NOT FOUND / HOLD <fallback>` when the envelope is rejected.
- Bundles firmware `rev27v-followup-227`. The protected Device A flash and
  restore passed, followed by a fixed-seed ten-minute soak: 6/6 cycles,
  100.00% telemetry delivery in every cycle, all deflation analyzers passed,
  and the OLED remained fresh and advancing.

## Windows SmartScreen / Unknown publisher

This official installer is currently unsigned. If Windows shows **Windows
protected your PC**, download only from this repository, verify the file is
`SmartCuff_Setup.exe`, select **More info**, then select **Run anyway**.
