# IHP__SVM5740 Datasheet
## 5-Class OVR RBF-SVM Cardiac Arrhythmia Classifier

**Revision:** 1.0.0  
**Date:** June 2026  
**Author:** Adam Handwerger, Portland State University (ECE410)  
**Technology:** IHP SG13G2 130 nm BiCMOS  

---

## 1. Features

- Classifies 5 cardiac rhythm types: Normal, PVC, AFib, VT, SVT
- 98.33% accuracy on PhysioNet Challenge test set (295/300 samples)
- One-vs-rest RBF-SVM; 500 support vectors, 256-dimensional feature space
- Q6.10 fixed-point arithmetic throughout
- Off-chip SRAM interface (IS62WV51216 1 MB async SRAM)
- SPI slave interface (CPOL=0, CPHA=0) for host MCU integration
- 40 MHz clock, 1.2 V supply
- Active power: 55.25 mW; average power at 80 bpm: 0.869 mW
- IHP SG13G2 tape-out ready (Magic DRC: 0, KLayout DRC: 0)

---

## 2. Block Diagram

```
              ┌─────────────────────────────────────────────┐
              │               svm_top_ihp                   │
              │  ┌──────────────────────────────────────┐   │
  SPI ───────►│  │           interface.sv               │   │
              │  │  (SPI slave, config regs, sequencer) │   │
              │  └──────────────┬───────────────────────┘   │
              │                 │                           │
              │  ┌──────────────▼───────────────────────┐   │
              │  │          compute_core.sv              │   │
              │  │  ┌─────────────────────────────────┐ │   │
              │  │  │ RAM FSM → off-chip SRAM (20-bit │ │   │
              │  │  │ addr, 16-bit data, 10ns access) │ │   │
              │  │  ├─────────────────────────────────┤ │   │
 SRAM ───────►│  │  │ SV ROM (on-chip 500×256 Q6.10) │ │   │
              │  │  ├─────────────────────────────────┤ │   │
              │  │  │ RBF kernel: ‖x-sv‖² → exp LUT  │ │   │
              │  │  │ → alpha_i × K(x, sv_i) sum      │ │   │
              │  │  ├─────────────────────────────────┤ │   │
              │  │  │ 5 decision accumulators + WTA   │ │   │
              │  │  └─────────────────────────────────┘ │   │
              │  └──────────────────────────────────────┘   │
              │                                result[2:0] ─►│
              └─────────────────────────────────────────────┘
```

---

## 3. Pin Description

| Pin | Type | Description |
|-----|------|-------------|
| clk | Input | 40 MHz system clock |
| rst_n | Input | Active-low synchronous reset |
| spi_csn | Input | SPI chip select (active low) |
| spi_clk | Input | SPI serial clock |
| spi_mosi | Input | SPI master-out slave-in |
| spi_miso | Output | SPI master-in slave-out |
| sram_addr[19:0] | Output | Off-chip SRAM 20-bit address |
| sram_data[15:0] | Bidir | Off-chip SRAM 16-bit data bus |
| sram_ce_n | Output | SRAM chip enable (active low) |
| sram_oe_n | Output | SRAM output enable (active low) |
| sram_we_n | Output | SRAM write enable (active low) |
| result[2:0] | Output | Classification result (0=Normal, 1=PVC, 2=AFib, 3=VT, 4=SVT) |
| result_valid | Output | Pulses high for 1 clock when result is ready |

---

## 4. Electrical Characteristics

| Parameter | Min | Typ | Max | Unit |
|-----------|-----|-----|-----|------|
| Supply voltage (VDD) | 1.08 | 1.20 | 1.32 | V |
| Clock frequency | — | 40 | 80 | MHz |
| Setup slack (typ corner) | — | +12.93 | — | ns |
| Hold slack (typ corner) | — | +0.35 | — | ns |
| Active power | — | 55.25 | — | mW |
| Average power (80 bpm) | — | 0.869 | — | mW |
| IR drop (VGND, typ) | — | 0.57% | 5% | % of VDD |

---

## 5. Physical Characteristics

| Parameter | Value |
|-----------|-------|
| Die area | 2400 × 2400 µm |
| Core area | 2388 × 2366 µm |
| Technology | IHP SG13G2 130 nm |
| Standard cells | 157,991 |
| Utilization | 15.0% |
| Layers used | M1–TopMetal2 (IHP SG13G2 stack) |
| DRC violations (Magic) | 0 |
| DRC violations (KLayout) | 0 |

---

## 6. SPI Protocol

The host MCU (nRF52840) loads feature vectors via SPI at startup. Feature format: 256 × 16-bit Q6.10 words (512 bytes total per classification). The core signals `result_valid` when classification completes (~12.5 ms at 40 MHz).

---

## 7. Integration Notes

- Off-chip SRAM (IS62WV51216 or equivalent 16-bit async SRAM) must be co-located on the PCB. Address and data lines are driven directly from the ASIC.
- No on-chip ADC; feature extraction is done by the host MCU from a 12-lead ECG front-end.
- The SV ROM is hardened inside `svm_compute_core` (core macro) and instantiated within `svm_top_ihp` (top wrapper).

---

## 8. Files

| File | Description |
|------|-------------|
| `SVM5740-main/PlaceAndRoute/gds/svm_top_ihp.gds.gz` | Final GDS (gzip compressed) |
| `SVM5740-main/PlaceAndRoute/lef/svm_top_ihp.lef` | Abstract LEF |
| `SVM5740-main/PlaceAndRoute/netlist/svm_top_ihp.v` | Gate-level netlist |
| `SVM5740-main/rtl/compute_core.sv` | RTL — SVM compute engine |
| `SVM5740-main/rtl/interface.sv` | RTL — SPI interface and sequencer |
| `SVM5740-main/verification/drc/drc.magic.rpt` | Magic DRC report (0 violations) |
| `SVM5740-main/verification/drc/drc.klayout.lyrdb` | KLayout DRC report (0 violations) |
| `SVM5740-main/verification/sta/sta_postpnr_typ.log` | Post-PnR STA (typ corner) |
