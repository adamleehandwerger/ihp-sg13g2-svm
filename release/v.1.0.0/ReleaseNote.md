# Release Note — v.1.0.0

**Date:** June 2026  
**Design:** IHP__SVM5740 (`svm_top_ihp`)  
**Technology:** IHP SG13G2 130 nm BiCMOS  
**Flow:** LibreLane 3.0.4  

## Summary

First tape-out ready release of the 5-class OVR RBF-SVM arrhythmia classifier.

## What's Included

- `gds/svm_top_ihp.gds.gz` — Final hardened GDS
- `netlist/svm_top_ihp.v` — Gate-level netlist
- `doc/` — Datasheet, Specification, TRL checklist

## Signoff Status

| Check | Result |
|-------|--------|
| Magic DRC | 0 violations |
| KLayout DRC | 0 violations |
| Setup timing (typ) | PASS (+12.93 ns WNS) |
| Hold timing (typ) | PASS (+0.35 ns WNS) |
| IR drop (VGND) | PASS (0.57%) |
| Accuracy (ASIC) | 98.33% (295/300) |

## Known Limitations

- LVS not run (Magic DRC only for this release)
- Fast-corner (1.65V) hold timing verified separately via post-PnR STA
- Off-chip SRAM required (IS62WV51216 or equivalent)
