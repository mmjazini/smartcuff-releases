# Smart Cuff rev27v-gauge-34

Valinor now uses its own `version-valinor.json` feed and rejects Default
releases. The release adds checksum-verified automatic updates, exact historical
pins and verified downgrades from Help, guarded automatic installation and
firmware alignment, writable per-user installation, and the user-facing name
**Smart Cuff**.

The GUI remains fully usable when Git, GitHub, internet access, Arduino, or
NI-DAQ hardware is unavailable. Update discovery is asynchronous and every
network failure is non-fatal.

Firmware remains `rev27v-gauge-27`; the fifteen Max presets and control path are
unchanged.
