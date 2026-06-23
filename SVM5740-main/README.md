# SVM5740-main

Place-and-route outputs, RTL, and verification reports for `svm_top_ihp`.

## Directory Structure

```
SVM5740-main/
├── PlaceAndRoute/
│   ├── gds/          — svm_top_ihp.gds.gz, svm_compute_core.gds.gz
│   ├── lef/          — svm_top_ihp.lef, svm_compute_core.lef (abstract LEF)
│   └── netlist/      — svm_top_ihp.v (gate-level netlist, IHP SG13G2 cells)
├── rtl/
│   ├── compute_core.sv  — SVM compute engine (RBF kernel, decision logic)
│   └── interface.sv     — SPI slave interface and classification sequencer
└── verification/
    ├── drc/          — Magic + KLayout DRC reports (0 violations each)
    ├── sta/          — Post-PnR static timing analysis (typ corner)
    └── lvs/          — (LVS pending post-silicon)
```

## Flow

Hardened with **LibreLane 3.0.4** on **IHP SG13G2 130 nm BiCMOS**.

- Synthesis: Yosys (AREA 0 strategy)
- Floorplan: 2400 × 2400 µm, 15% utilization
- PnR: OpenROAD (GRT + DRT)
- Signoff: Magic DRC, KLayout DRC, OpenROAD STA, PSM IR drop

## Key Results

| Metric | Value |
|--------|-------|
| Accuracy (ASIC Q6.10) | 98.33% |
| Die area | 2400 × 2400 µm |
| Setup WNS (typ) | +12.93 ns |
| Hold WNS (typ) | +0.35 ns |
| Active power | 55.25 mW |
| DRC violations | 0 (Magic) / 0 (KLayout) |
