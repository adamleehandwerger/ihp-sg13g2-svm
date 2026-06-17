# IHP SG13G2 SVM Arrhythmia Classifier

5-class (Normal / PVC / AFib / VT / SVT) OVR RBF-SVM ASIC — IHP SG13G2 130 nm BiCMOS.

## Design Summary
| Parameter | Value |
|-----------|-------|
| Die area | 2400 × 2400 µm |
| PDK | IHP SG13G2 130 nm |
| Clock | 40 MHz |
| Config | SPI slave (nRF52840); off-chip 1 MB async SRAM |
| Support vectors | 500 total — [95,95,95,120,95] / class |
| Accuracy | **98.33%** Q6.10 fixed-point (295/300 PhysioNet) |
| Magic DRC | **0 violations** |
| KLayout DRC | **0 violations** |
| Setup WNS | +12.93 ns (typ 1.20 V, 25 °C) |
| Hold WNS | +0.35 ns (typ 1.20 V, 25 °C) |

## Files
| Path | Description |
|------|-------------|
| `gds/svm_top_ihp.gds.gz` | Top-level GDS — gunzip before use |
| `gds/svm_compute_core.gds.gz` | Core macro GDS |
| `lef/svm_top_ihp.lef` | Top-level abstract LEF |
| `lef/svm_compute_core.lef` | Core macro abstract LEF |
| `netlist/svm_top_ihp.v` | Post-PnR gate-level netlist |
| `reports/drc/` | Magic + KLayout DRC (0 violations) |
| `reports/timing/` | Post-PnR STA (typ corner) |

## RTL Source
https://github.com/adamleehandwerger/ECE410/tree/main/project/m6

## ECE410 — Portland State University, 2026
