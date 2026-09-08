# Smart Cuff v3.1.231 (Default)

- Automatic updates now reopen the newly installed Smart Cuff after Setup
  completes successfully.
- One attached Arduino Due is aligned with the bundled Default firmware before
  the GUI opens. No Due still opens offline; ambiguous or failed alignment
  stops safely.
- A workstation-specific `.last_flash.json` is never shipped.
- The GUI, installer, shortcuts, and taskbar now use the supplied transparent
  Smart Cuff product artwork.

Firmware remains `rev27v-followup-223`; no control behavior changed.

## Windows SmartScreen / Unknown publisher

This official installer is currently unsigned. If Windows shows **Windows
protected your PC**, download only from this repository, verify the file is
`SmartCuff_Setup.exe`, select **More info**, then select **Run anyway**.
