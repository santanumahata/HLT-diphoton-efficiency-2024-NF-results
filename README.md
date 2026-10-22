# HLT diphoton trigger efficiency — 2024 results with the NF η–R9 map

Tag-and-probe measurements of the CMS diphoton HLT leg efficiencies on 2024 data, with the η–R9
reweighting map `2024Lowmass_R9_vs_eta_ratio_presel.root`. Companion to
[HLT-diphoton-efficiency-2024-results](https://github.com/santanumahata/HLT-diphoton-efficiency-2024-results).

Slides comparing with and without NF: [`Lmass_hlt_eff_w&wo_NF.pdf`](Lmass_hlt_eff_w&wo_NF.pdf)

## Provenance

| Set | Source |
|---|---|
| with NF, normal | fitter output `HLT_2024_withPresel_SS/` (fits run on lxplus 2026-10-01/02) |
| with NF, fullPath | fitter output `HLT_2024_withPresel_fullPath_SS/` |
| without NF (comparison only, not in `csvs/`) | [HLT-diphoton-efficiency-2024-results](https://github.com/santanumahata/HLT-diphoton-efficiency-2024-results) @ `d422813`, `etaR9rw_withPresel[_fullPath]`, map `2024_R9_vs_eta_HggOverDY_presel_vf.root` |

Built by `notebooks/2026-10-21_build_nf_csv_jsons_plots.ipynb`.

## Layout

- `csvs/hlt_2024_NF_trigger_efficiency_asym_errors.csv` — efficiencies + asymmetric uncertainties (normal selection), 105 rows.
- `efficiency_plots/` — efficiency vs probe pT, one plot per leg.
- `fit_plots/{seeded,unseeded}/<category>/sig_bwDscb_bkg_{RooCMSShape,Exponential}/` — tag-and-probe mass-fit plots. [`fit_plots/README.md`](fit_plots/README.md) lists every efficiency linked to its fit plot ([`fullPath/fit_plots/README.md`](fullPath/fit_plots/README.md) for fullPath).
- `jsons/2024/Lowmass_{seeded,unseeded}_leg_efficinecy.json` — HiggsDNA `TriggerSF_LM` correctionlib JSONs.
- `fullPath/` — the same four subtrees for the fullPath selection (`probe_passFullPath == 1`).
- `notebooks/` — the build notebook.
- `etaR9_map_study/` — the with/without-NF comparison study (notebooks, data, plots, slide source).

**Legs:** `seed` = seeded (lead) leg; `unseed` = unseeded (sublead) leg.
**Categories:** EB R9 0.50–0.55 / 0.55–0.85 / > 0.85; EE R9 0.90–0.93 / > 0.93.
**Fit model:** `bwDscbCms` (nominal, RooCMSShape background) and `bwDscbExp` (alternate, Exponential background).

## CSV columns

Same schema as the 2023 repo, plus `err_*_png`.

- `eff_cms`, `eff_exp` — efficiency printed on the fit PNG (4 decimals).
- `err_hi_*`, `err_lo_*` — fit errors from `egammaEffi.txt`, rounded to 4 decimals, `err_hi` capped at 1 − eff. The egm_tnp fitter floors this error at 0.001 (`libPython/rootUtils.py:95` in `cms-egamma/egm_tnp_analysis` @ `3d3b8da`).
- `err_hi_*_png`, `err_lo_*_png` — fit errors printed on the fit PNG, not floored.
- `status_pass_*`, `status_fail_*` — fit status of the pass and fail fits.
- `bkg_syst` = |eff_cms − eff_exp|.
- `total_unc_hi` = err_hi_cms ⊕ bkg_syst, capped at 1 − eff_cms; `total_unc_lo` = err_lo_cms ⊕ bkg_syst.
- `fit_plot_cms`, `fit_plot_exp` — GitHub URL of the fit plot each efficiency was read from.

## JSON mapping

Inputs `(SCEta = |ScEta|, r9, pt)`, `flow: clamp`; systematic `nominal` / `up` / `down`.

- SCEta edges `[0, 1.4442, 1.566, 2.5, 999]`; r9 edges `[0, 0.50, 0.55, 0.85, 0.90, 0.93, 999]`; pt edges = the measured bins of each leg.
- EB: r9 < 0.50 → 1.0; 0.50–0.55 → `EB_R9_0p50to0p55`; 0.55–0.85 → `EB_R9_0p55to0p85`; > 0.85 → `EB_R9gt0p85`.
- EE: r9 < 0.90 → 1.0; 0.90–0.93 → `EE_R9_0p90to0p93`; > 0.93 → `EE_R9gt0p93`.
- 1.4442–1.566 and > 2.5 → 1.0.
- nominal = `eff_cms`; up = min(1, eff_cms + `total_unc_hi`); down = max(0, eff_cms − `total_unc_lo`).

## Validation (run in the build notebook)

- 420 fit PNGs read by OCR, 0 unparsed.
- `egammaEffi.txt` efficiency vs PNG: 0 failures at tolerance 5.5e-5; errors vs max(PNG error, 0.001): 0 failures.
- Fit status vs stored fit results: 420 / 420 match.
- CSV efficiencies equal the PNG values: 0 / 105 mismatches per set.
- correctionlib evaluation of every JSON cell against the CSV: 0 failures in all 4 files.
- `fit_plot_*` paths and `fit_plots/README.md` links: every target exists and shows the linked efficiency (210 links per set).
- Each gate is tested against a deliberately broken input.
