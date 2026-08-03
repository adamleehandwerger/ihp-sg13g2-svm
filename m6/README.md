# SVM5740 — m6 Submission

**Status:** Shuttle submission candidate  
**Chip:** SVM5740 — 5-class SVM inference accelerator, IHP SG13G2 130 nm  
**Core:** svm\_compute\_core — 500 support vectors, 16-bit fixed-point, 1400 × 1400 µm  
**Flow:** LibreLane 3.0.4, IHP PDK rev. c4b8b4e5

---

## Contents

| File / Folder | Description |
|---|---|
| `gds/SVM5740_m6_filled.gds.gz` | Chip-top GDS with Metal fill applied (submission GDS) |
| `drc/density_drc.lyrdb` | KLayout density DRC report — 5 local-window violations |
| `drc/density_drc.log` | Full fill + DRC run log |
| `ihp_density_waiver_request.pdf` | IHP foundry waiver request for remaining violations |

Original unhardened GDS and LEF are in `SVM5740-main/PlaceAndRoute/`.  
Original unfilled chip-top is in `release/v.1.0.0/`.

---

## Fill Summary

Metal fill was applied using minimum-size IHP filler cells to maximise coverage:

| Parameter | Value |
|---|---|
| Filler cell size | 0.6 × 0.6 µm (MFil.b minimum) |
| Filler-to-signal spacing | 0.42 µm (MFil.c minimum) |
| Fill script | `sg13g2_filler_ActGatP` + custom `Metal_tiny` + `sg13g2_filler_TopMetal` |

### Global density after fill (all pass)

| Layer | Coverage | Rule min | Rule max |
|---|---|---|---|
| Metal1 | 48.5% | 35% | 60% |
| Metal2 | 35.7% | 35% | 60% ✓ |
| Metal3 | 45.2% | 35% | 60% |
| Metal4 | 47.1% | 35% | 60% |
| Metal5 | 48.8% | 35% | 60% |
| TopMetal1 | 47.0% | 25% | 70% |
| TopMetal2 | 42.5% | 25% | 70% |

---

## Remaining DRC Violations (5 total — waiver requested)

All violations are local sliding-window rules (M2Fil.h / M3Fil.h, 800 × 800 µm windows, minimum 25%).  
Global Metal2 density **passes** at 35.7% (M2.j ≥ 35%).

| # | Rule | Window (µm) |
|---|---|---|
| 1 | M2Fil.h | (400, 400) – (1200, 1200) |
| 2 | M2Fil.h | (400, 800) – (1200, 1600) |
| 3 | M2Fil.h | (800, 400) – (1600, 1200) |
| 4 | M2Fil.h | (800, 800) – (1600, 1600) |
| 5 | M3Fil.h | (400, 400) – (1200, 1200) |

All five windows fall within the compute core (400–1600 µm). The signal routing track pitch
in these windows is below the minimum gap (1.44 µm) needed for any legal fill cell.
A targeted fill pass restricted to these windows placed zero additional cells, confirming
the physical limit. Three re-hardening attempts at 40%, 42%, and 45% placement density
produced no improvement. See `ihp_density_waiver_request.pdf` for full justification.

---

## m7 Reharden Attempt

A separate re-harden of `svm_compute_core` at reduced placement density (40% and 42%)
was attempted to create routing room for fill. It did not reduce the violation count —
see [`m7/`](../m7/) for artifacts and analysis.
