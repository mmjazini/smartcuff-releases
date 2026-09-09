# Smart Cuff v3.1.238 (Default)

- Corrects host-side bookkeeping for current tagged CRC-16 `EXEC` commands.
  The GUI now removes the complete `*XHHHH` suffix before recording the parsed
  primitive ID, target, and rate used for start-event correlation.
- Does not change the transmitted command, retry behavior, firmware parser,
  pressure control, protocol timing, calibration, or GUI/standalone parity.
- Adds a regression test that sends the real tagged wire form and verifies the
  exact parsed `EXEC:6,200,10.00` arguments.
- Bundles firmware `rev27v-followup-227`. This host-only update needs no new
  controller flash or soak beyond the already verified v3.1.237 firmware run.
- Verification: 820 tests passed and 3 skipped; legacy, helper, parser, worker,
  analyzer, layout, and offscreen GUI smoke checks also passed.

## Windows SmartScreen / Unknown publisher

This official installer is currently unsigned. If Windows shows **Windows
protected your PC**, download only from this repository, verify the file is
`SmartCuff_Setup.exe`, select **More info**, then select **Run anyway**.
