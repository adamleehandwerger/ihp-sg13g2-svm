# IHP__SVM5740 — Specification

## Overview

5-class one-vs-rest (OVR) radial basis function support vector machine (RBF-SVM) ASIC for real-time cardiac arrhythmia classification. Targets ambulatory ECG monitoring on a coin-cell battery.

## Classification

| Class | Label | Description |
|-------|-------|-------------|
| 0 | Normal | Normal sinus rhythm |
| 1 | PVC | Premature ventricular contraction |
| 2 | AFib | Atrial fibrillation |
| 3 | VT | Ventricular tachycardia |
| 4 | SVT | Supraventricular tachycardia |

**Algorithm:** One-vs-rest RBF-SVM; 5 binary classifiers vote for final class.  
**SVs per class:** [95, 95, 95, 120, 95] = 500 total support vectors.  
**Feature dimension:** 256 (128 single-beat + 64 10-beat morphology + 64 RR-interval).  
**Accuracy:** 98.33% (295/300 samples, PhysioNet Challenge test set).

## Architecture

```
SPI slave (nRF52840 MCU)
       |
  [interface.sv]
       |
  [compute_core.sv]
  - Feature RAM controller (off-chip 1 MB async SRAM, IS62WV51216)
  - SV ROM (on-chip, Q6.10 fixed-point, 500 SVs × 256 features)
  - RBF kernel unit: squared-distance accumulator → exp LUT → alpha scale
  - 5 decision accumulators (Q6.10 format)
  - Winner-take-all classifier
```

## I/O Description

| Port | Dir | Width | Description |
|------|-----|-------|-------------|
| clk | in | 1 | 40 MHz system clock |
| rst_n | in | 1 | Active-low synchronous reset |
| spi_csn | in | 1 | SPI chip select (active low) |
| spi_clk | in | 1 | SPI clock |
| spi_mosi | in | 1 | SPI data in |
| spi_miso | out | 1 | SPI data out |
| sram_addr | out | 20 | Off-chip SRAM address |
| sram_data | inout | 16 | Off-chip SRAM data bus |
| sram_ce_n | out | 1 | SRAM chip enable |
| sram_oe_n | out | 1 | SRAM output enable |
| sram_we_n | out | 1 | SRAM write enable |
| result | out | 3 | Classification result (0–4) |
| result_valid | out | 1 | Pulses high when result is ready |

## Timing

| Parameter | Value |
|-----------|-------|
| Clock period | 25 ns (40 MHz) |
| Setup WNS (typ 1.20V, 25°C) | +12.93 ns |
| Hold WNS (typ 1.20V, 25°C) | +0.35 ns |
| Latency per classification | ~500 k cycles (at 40 MHz ≈ 12.5 ms) |

## Power

| Metric | Value |
|--------|-------|
| Active power | 55.25 mW |
| Average power at 80 bpm | 0.869 mW |
| Estimated battery life (coin cell, 235 mAh) | ~18.9 days |

## Physical

| Parameter | Value |
|-----------|-------|
| Technology | IHP SG13G2 130 nm BiCMOS |
| Die area | 2400 × 2400 µm (5.76 mm²) |
| Core area | 2388 × 2366 µm |
| Placement density | 15.0% |
| Standard cells | 157,991 |
| Supply voltage | 1.2 V nominal |

## Dependencies

- Off-chip SRAM: IS62WV51216 (1 MB, 10 ns access, 2.5–3.6 V)
- Host MCU: nRF52840 (SPI master, handles feature extraction and result display)
- PDK: IHP SG13G2 (sg13g2_stdcell standard cell library)
