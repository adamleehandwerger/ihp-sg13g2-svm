# IHP__SVM5740 — 5-Class SVM Cardiac Arrhythmia Classifier

**IHP SG13G2 130 nm BiCMOS — Open-Silicon MPW submission**
**Author:** Adam Handwerger, Portland State University (ECE410)
**Contact:** handwerg@pdx.edu

---

## Overview

A 5-class one-vs-rest RBF-SVM ASIC that classifies cardiac arrhythmias from 12-lead ECG features in real time. Designed for ambulatory monitoring with a coin-cell battery.

| Class | Arrhythmia |
|-------|-----------|
| 0 | Normal sinus rhythm |
| 1 | PVC (Premature ventricular contraction) |
| 2 | AFib (Atrial fibrillation) |
| 3 | VT (Ventricular tachycardia) |
| 4 | SVT (Supraventricular tachycardia) |

**Accuracy:** 98.33% (295/300, PhysioNet Challenge test set)
**Algorithm:** OVR RBF-SVM, 500 support vectors, 256-dimensional Q6.10 feature space

---

## Key Results

| Metric | Value |
|--------|-------|
| Die area | 2400 × 2400 µm (5.76 mm²) |
| Technology | IHP SG13G2 130 nm |
| Clock | 40 MHz (25 ns period) |
| Supply | 1.2 V |
| Standard cells | 157,991 |
| Setup WNS (typ 1.2V, 25°C) | +12.93 ns |
| Hold WNS (typ 1.2V, 25°C) | +0.35 ns |
| Active power | 55.25 mW |
| Avg power @ 80 bpm | 0.869 mW |
| Magic DRC violations | 0 |
| KLayout DRC violations | 0 |
| ASIC accuracy (Q6.10) | **98.33%** |

---

## Repository Structure

```
IHP__SVM5740/
├── doc/
│   ├── Datasheet.md            — Full electrical and physical datasheet
│   ├── info.json               — Machine-readable design metadata
│   ├── Specification.md        — Architecture and I/O specification
│   └── TRL-Digital-Hard-IP.md — TRL5 quality assessment checklist
├── SVM5740-main/
│   ├── PlaceAndRoute/
│   │   ├── gds/                — svm_top_ihp.gds.gz, svm_compute_core.gds.gz
│   │   ├── lef/                — Abstract LEF files
│   │   └── netlist/            — Gate-level netlist (svm_top_ihp.v)
│   ├── rtl/                    — SystemVerilog RTL source
│   └── verification/
│       ├── drc/                — Magic + KLayout DRC reports
│       └── sta/                — Post-PnR STA (typ corner)
├── release/
│   └── v.1.0.0/                — Release package (GDS, netlist, doc)
└── measurements/               — Post-silicon (pending tape-out)
```

---

## Flow

- **Synthesis:** Yosys via LibreLane 3.0.4
- **PnR:** OpenROAD (global routing + detailed routing, IHP SG13G2)
- **Signoff:** Magic DRC, KLayout DRC, OpenROAD STA (typ/fast/slow corners), PSM IR drop
- **ECE410 source repo:** https://github.com/adamleehandwerger/ECE410 (`project/m6/`)

---

## License

Apache-2.0
