# m7 — Reharden at Lower Placement Density

## Status
In progress — rehardening `svm_compute_core` with `PL_TARGET_DENSITY_PCT=35` (down from 45) to resolve Metal2 local density violations.

## v.1.0.0 Baseline (m6 fill attempt)

The `release/v.1.0.0` chip_top GDS had Metal2 density violations after assembly.
A full chip-level fill campaign was run with progressively smaller fill cells:

| Fill pass | M2 global | Local M2Fil.h violations |
|-----------|-----------|--------------------------|
| No fill   | ~33.4%    | 6 |
| Default (1.5 µm spacing) | 34.10% ❌ | 6 |
| Dense (0.42 µm spacing)  | 35.37% ✅ | 5 |
| Tiny cells (0.6 µm, 0.42 µm spacing) | 35.72% ✅ | 4 |
| Window-targeted pass     | 35.72% ✅ | 4 |

Global M2.j (≥35%) passes. Four local 800×800 µm window violations (M2Fil.h)
remain in the core routing area — routing tracks are at physical density limit,
no more fill can be placed.

Fill artifacts: `fill_artifacts/window_density.lyrdb`, `window_density.log`

## m7 Plan

Reharden `svm_compute_core` with:
- `PL_TARGET_DENSITY_PCT: 35` (was 45)
- Same die area: 1400×1400 µm
- Same RTL: NUM_SV=500, FEATURE_DIM=256

Lower placement density spreads standard cells, creating physical room for Metal2
fill in the previously violating windows.

New artifacts will be placed here after the reharden completes.
