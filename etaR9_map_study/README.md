# HLT diphoton efficiency 2024 — new eta-R9 map

Comparison of the 2024 HLT diphoton leg efficiencies obtained with two eta-R9 weight maps, against 2023 postBPix, plus a study of the two maps.

Slides: [`slides/Lmass_hlt_eff_w&wo_NF.pdf`](slides/Lmass_hlt_eff_w&wo_NF.pdf)

## Efficiency sets (nominal fit `bwDscbCms`)

| Label | Source | eta-R9 weight |
|---|---|---|
| 2023 (postBPix) | [HLT-diphoton-efficiency-2023-results](https://github.com/santanumahata/HLT-diphoton-efficiency-2023-results) @ `9450e11` | — |
| 2024 repo | [HLT-diphoton-efficiency-2024-results](https://github.com/santanumahata/HLT-diphoton-efficiency-2024-results) @ `d422813`, `etaR9rw_withPresel[_fullPath]` | `2024_R9_vs_eta_HggOverDY_presel_vf.root` (old map) |
| 2024 local | fits run on lxplus 2026-10-01/02 | `2024Lowmass_R9_vs_eta_ratio_presel.root` (new map) |

Error bars are stat ⊕ syst: fit error ⊕ |eff(bwDscbCms) − eff(bwDscbExp)|, upper error capped at 1 − eff (the `total_unc` definition of the 2023 CSV), for all three sets.

Both sets of 2024 input trees differ only in the `r9etaWeight_with_presel` and `weight_with_presel` branches (`data/inputs/presel_weight_validation.json`).

## Layout

- `notebooks/01_scan_trees_per_map_cell.ipynb` — reads the 2024 fitter trees (not in this repo, ~29 GB each) and counts selected probes per fitter bin × pass/fail × map cell → `data/cell_counts.npz`.
- `notebooks/02_map_and_efficiency_study.ipynb` — all plots and numbers in the slides → `plots/`, `data/efficiency_three_way.csv`, `data/counting_vs_fit.csv`, `data/slide_numbers.json`.
- `data/inputs/` — efficiency CSVs from both repos, the 2024 local `egammaEffi.txt` files, both map files.
- `plots/eff/` efficiency comparisons, `plots/diff/` difference heatmaps, `plots/maps/` map comparisons, `plots/why/` counting-efficiency study and fit-plot pairs.
- `slides/` — Beamer source and PDF.

## Reproducing

`02` runs from the files in this repo. `01` needs the fitter trees at the paths set in its config cell.
