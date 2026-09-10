# Smart Cuff v3.2.48 - USB receive scheduling

Valinor bundle with firmware `rev27v-gauge-34`.

The Due Programming Port UART can now preempt the complete pressure sampler.
Equal interrupt priorities previously allowed the ADC sample to block UART
reception for several byte times. The full 50 Hz safety sample, all 64 ADC
conversions, raw-voltage checks, CRC/retries, calibration, and pressure-control
settings are preserved. Valinor retains Max 80-220 and standalone/GUI parity.

Device C protected flash/restore acknowledged every saved setting on attempt 1
and verified unchanged calibration/SysID. A seed-213 read-only test returned
1000/1000 first attempts, zero CRC failures/timeouts/buffer overflow, and
9217/9217 telemetry frames (100%). The full sampler's maximum was 757 us.

Ten-minute seed-213 soak: 8/8 cycles, no checksum/retry warnings,
28941/28941 emitted frames received (100.00%), no gaps above 0.16 s,
and scorer HARD-GATE PASS. Worst pressure rebound was 0.03 mmHg.

| Cycle | Max preset | Deflation mmHg/s | Valve reversals/s | Delivery |
|---|---|---:|---:|---:|
| 1 | 140 | 2.26 | 0.087 | 100.00% aggregate |
| 2 | 200 | 2.45 | 0.030 | 100.00% aggregate |
| 3 | 140 | 2.26 | 0.087 | 100.00% aggregate |
| 4 | 180 | 2.39 | 0.067 | 100.00% aggregate |
| 5 | 120 | 2.25 | 0.051 | 100.00% aggregate |
| 6 | 160 | 2.31 | 0.075 | 100.00% aggregate |
| 7 | 200 | 2.47 | 0.061 | 100.00% aggregate |
| 8 | 220 | 2.53 | 0.028 | 100.00% aggregate |

OLED counters advanced from 1051 to 589605, with a final draw age of 19 ms.
Offline tests: 795 passed / 7 skipped; compile, layout audit, and GUI smoke pass.
Offline preflight now executes pytest assertions and retains legacy checks.
GET_SERIAL_STATS and a read-only command probe preserve future fault evidence.
This fixes the measured interrupt-starvation fault, not every possible cable
or driver failure. CRC warnings remain visible; corrupt commands are rejected.

Latest users receive verified updates automatically at the safe idle gate;
the installed launcher aligns attached firmware via snapshot/flash/restore.
Offline use and Help's same-branch version pin remain supported.

This official installer is unsigned. For **Windows protected your PC**, choose
**More info**, verify `SmartCuff_Setup.exe` came from
`mmjazini/smartcuff-releases`, then **Run anyway**. Valinor remains the small
online installer: first setup needs internet, then opens Smart Cuff.
