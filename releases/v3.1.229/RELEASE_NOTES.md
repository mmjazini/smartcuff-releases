# Smart Cuff v3.1.229 (Default)

- Fixes update downloads that could fail with Windows error 5 or 32 when a
  previous installer file was still locked. Downloads now use collision-free
  filenames and bounded promotion retries.
- Keeps exact release URL, byte-size, SHA-256, and live pneumatic safety checks.
- This installer is intentionally unsigned. Windows displays **Unknown
  publisher**, which the operator has accepted until a trusted institutional
  Authenticode certificate is available.
- No control-path or firmware changes. Bundled firmware remains
  `rev27v-followup-223`.
