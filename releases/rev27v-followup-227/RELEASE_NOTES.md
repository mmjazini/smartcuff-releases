# Smart Cuff rev27v-followup-227

This host/installer release adds checksum-verified automatic updates, a Help
version selector for exact historical pins and verified downgrades, writable
per-user installation, automatic guarded firmware alignment, and recovery when
an older installation has no usable bundled Python runtime.

The GUI remains fully usable when Git, GitHub, internet access, Arduino, or
NI-DAQ hardware is unavailable. Update discovery runs after the window is
painted and every network failure is non-fatal. The user-facing application name
is **Smart Cuff**; the revision remains diagnostic metadata.

Firmware remains `rev27v-followup-223`; there is no control-path change.
