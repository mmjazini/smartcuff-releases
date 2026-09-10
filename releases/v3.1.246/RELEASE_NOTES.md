# Smart Cuff v3.1.246 - USB receive scheduling

Default bundle with firmware `rev27v-followup-233`.

The Due Programming Port's UART can now interrupt the pressure sampler.
Previously both interrupts had equal priority, allowing the 64-conversion
sample to block reception for several serial byte times. This fixes the
measured receive-starvation fault without reducing ADC averaging, changing the
50 Hz safety sampling period, or weakening CRC/retries. Pressure control,
standalone/GUI parity, each branch's menu, and OLED recovery are unchanged.

Device C verification, seed 213:

- Before: 301/400 first-attempt read-only replies; 92 CRC rejections, 7 absent.
- After: 1000/1000 replies, zero CRC failures/timeouts/buffer overflows, and
  9152/9152 telemetry frames. The safety sampler measured 706-735 us.
- Protected flash/restore: all settings acknowledged on attempt 1; saved
  calibration and SysID verified unchanged.
- Ten-minute soak: 6/6 cycles, zero flags, 29359/29359 emitted frames received.

| Cycle | Preset | Deflation verdict | Valve reversals/s | Delivery |
|---|---|---|---:|---:|
| 1 | Smart Const | PASS | 0.000 | 100.00% |
| 2 | Arm CVP | PASS | 0.073 | 100.00% |
| 3 | Smart Const | PASS | 0.000 | 100.00% |
| 4 | Wrist CVP | PASS | 0.049 | 100.00% |
| 5 | Regular | PASS | 0.021 | 100.00% |
| 6 | High Max | PASS | 0.019 | 100.00% |

OLED transfer counters advanced throughout, with final draw ages 1-107 ms.
Offline tests: 918 passed / 3 skipped; compile, layout audit, and GUI smoke pass.
The offline preflight now executes pytest assertions and retains legacy checks.
`GET_SERIAL_STATS` and a read-only command probe make future transport faults
measurable. This is not a guarantee against defective cables or disconnected
hardware; CRC warnings remain visible and corrupt commands remain rejected.

Latest users receive the verified bundle automatically at the existing safe
idle gate; the installed launcher aligns attached firmware through protected
snapshot/flash/restore. Offline use and Help's same-branch version pin remain.

This official installer is unsigned. For **Windows protected your PC**, choose
**More info**, verify `SmartCuff_Setup.exe` came from
`mmjazini/smartcuff-releases`, then **Run anyway**. First setup opens Smart Cuff
automatically; subsequent updates install and relaunch at the safe idle gate.
