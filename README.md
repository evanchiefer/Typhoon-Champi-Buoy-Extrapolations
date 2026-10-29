# Typhoon Champi (29W) — central pressure from a drifting-buoy eyewall pass

Estimate of Champi's minimum sea-level pressure at **17:33 UTC, 20 October 2026**. Drifting buoy 2802285 [B7] passed through the inner part of the
south-west eyewall. It read **909.4 hPa about 26 km from the centre at 17:04Z**, near the radius of maximum wind, and 913.3 hPa at 18:04Z.

## Result (version 12.1; headline as in version 12)

| | Central pressure (hPa) | 68% range | 95% range |
|---|---|---|---|
| **Combined estimate: three profile models weighted by fit to the buoy and SAR together** | **883** | 874–892 | 866–897 |
| Ring model: SAR eye shape + secondary ring (weight 0.95) | 883 | 873–891 | 866–897 |
| SAR eye shape, no secondary ring (weight 0.05) | 890 | 884–896 | 876–901 |
| Two-ring Holland profile (weight 0.00) | 889 | 882–895 | 875–900 |
| Sensitivity: no constraint on the wind ratio at 2× the max-wind radius | 885 | 877–892 | 868–897 |
| Sensitivity: conversion at 2× radius flat 0.77–0.91 | 884 | 874–892 | 866–898 |
| Sensitivity: conversion at 2× radius from the log-layer model, 0.769 ± 0.029 | 882 | 873–890 | 865–897 |
| Sensitivity: buoy only (no SAR outer-structure terms) | 885 | 878–894 | 871–899 |
| Check: SAR winds integrated inward from the buoy (no profile model) | 881 | 873–888 | 864–894 |
| Weighting: buoy leave-one-out only (v11 headline method) | 888 | 881–895 | 871–900 |
| Weighting: BIC with effective parameters | 886 | 877–894 | 868–899 |
| Weighting: equal | 887 | 879–894 | 870–900 |
| Previous version (v11) | 889 | 880–894 | 872–899 |
| Previous version (v10) | 884 | 876–892 | 867–899 |
| JTWC working best track, interpolated to 17:33Z (operational value; final best track not yet released) [B5] | 895 | | |

- Half-width of the 68% range ±9.1 hPa (sd 8.3). 3000 headline trials (ring model 1000, no-ring model 1000, two-ring Holland 1000); 40310 trials in all cases.
- **95% of the combined distribution lies below JTWC's 895 hPa**, which sits between the 68% and 95% upper limits.
- Monte Carlo precision (bootstrap standard error of the 2.5 / 16 / 50 / 84 / 97.5 percentiles): 0.59 / 0.40 / 0.33 / 0.39 / 0.44 hPa.
- Uncertainty budget (1σ, hPa; single-source runs of each profile model, weighted over the three, plus the spread between the models):
  - radius of maximum wind (incl. tilt) 5.7
  - peak surface wind (incl. SAR calibration ±8%) 4.4
  - reading scatter 3.3
  - centre position (track, drift, wander) 3.1
  - inner-eye wind shape 2.8
  - secondary wind maximum 2.7
  - 10-m to gradient conversion at the RMW 2.0
  - profile-shape choice (3 models) 1.6
  - conversion at 2× radius (α20) 1.5
  - oval eyewall 1.3
  - wind ratios at 1.4× and 2× radius 0.7
  - barometer calibration 0.4
  - air density 0.2
  - environmental pressure 0.2

**Reading the result.** The change from v11 comes from treating the SAR wind ratios properly. One 10-m-to-gradient conversion profile now feeds
the wind ratios at 1.4× and 2× the max-wind radius and the secondary maximum, with the value at 2× the radius fitted (prior 0.80 ± 0.05). The
ring model can match those ratios and the buoy together; the no-ring and Holland profiles cannot (they miss the 1.4× and 2× ratios by 2σ or more), so a
weighting that counts the SAR measurements puts 0.95 on the ring model. The v11 headline (889) leaned on the Holland profile through a
buoy-only score that could not see this. The answer still depends on the weighting: 883 with the measurement-aware AIC,
886 with BIC, 888 with the buoy-only leave-one-out score. The fork in the physics is the conversion at 2× the radius:
882 with the log-layer value, 883 with the 0.80 prior, 884 with a flat 0.77–0.91 range, 885 with no constraint.
The 68% range is wider than v11's (±9.1 against ±6.8 hPa). v11's narrower range came from the Holland profile, whose fixed shape
cannot follow the SAR structure; the ring model carries the buoy fit's full trade-off between the radius of maximum wind and the peak wind
(the two largest items in the budget).

## What changed in version 12

Best fits (Pc free, unperturbed inputs), one change at a time on top of the previous (`ladder12.py`):

| Step | Ring | No ring | Holland |
|---|---|---|---|
| v11 (log-layer conversion, ring radius at the surface, drift fixed 1 km) | 878.9 | 889.3 | 888.7 |
| + one conversion profile, alpha20 ~ N(0.80, 0.05) fitted | 879.1 | 889.7 | 886.5 |
| + secondary-ring radius at gradient level (tilted) | 881.5 | 889.7 | 886.5 |
| + IR-surface drift from its posterior (median track) = v12 headline configuration | 881.2 | 889.4 | 886.1 |

1. **One conversion profile, α20 fitted** (v12 spec H1–H3). α(r) = α_rm inside the RMW and varies linearly in ln(r/RMW) to α20 at 2 RMW.
   α_rm = 0.885 ± 0.030 from the log-layer model (drawn per trial); α20 is a fitted parameter with prior N(0.80, 0.05). The SAR terms are surface-ratio
   residuals, (model gradient ratio × α(x)/α_rm − measured surface ratio)/σ with σ = 0.03 (0.035 at the secondary maximum) combined with the spread
   from the 21:15Z interpolation weight. In v11 the 1.4× and 2× ratios each carried their own share of the log-layer uncertainty, which counted it twice.
   Posterior α20 (16–84%): ring model 0.791 (0.739–0.839); no-ring model 0.851 (0.810–0.889); two-ring Holland 0.831 (0.798–0.869).
2. **Secondary-ring radius at gradient level** (found by the v11 review [B9]). v11 used the surface, untilted radius (43.2 km at 21:15Z) while
   everything else was tilted to gradient level. v12 uses x2 × RMW at gradient level (x2 from the converted SAR shape).
3. **IR-to-surface drift drawn from its posterior** (spec H4, adapted). The track model already estimates the wander from the fixes; the one loosely
   bound hyperparameter is the amplitude of the slow drift of the IR-to-surface offset (fixed at 1 km in v11; only four surface fixes constrain it).
   v12 refits the track on a 0.1-km grid of that amplitude (0–15 km, `track_hier.py`) and each trial draws it from the restricted-likelihood
   posterior with a flat prior: median 0.83 km (16–84% 0.17–2.21), P(> 3 km) 0.09; the mass beyond the grid is below 10⁻⁴.
   Centre sd at 17:04Z on the median track ±0.92 km per component (v11 ±0.99).
4. **Weights that count the SAR measurements** (spec H7): AIC on the buoy readings plus the SAR terms shared by all three models plus one
   secondary-maximum term scored the same way for all, with effective parameter counts (hat-matrix trace). See "Model weighting".
5. **Paired random streams.** Each uncertain input has its own random stream (seed, source), so trial k of every case and every model shares
   every draw except the one the case changes. Every trial is written in full (`results12/draws_<case>.csv`).

## Response to the v12 specification [B9]

| Item | v12 | Note |
|---|---|---|
| H1–H3 α20 prior in the fit, one α(r) | Done | As specified; α_rm drawn from the log-layer ensemble, independent of α20. |
| H4 hierarchical wander | Adapted | Applied to the IR-to-surface drift amplitude (the v12 track model already estimates the wander itself). Posterior median 0.83 km here vs their 1.55 km. Their Jeffreys prior is improper at 0 for this likelihood; S8 uses log-uniform from 0.1 km (median 0.22 km). |
| H5 paired streams | Done | Per-source streams, `rng([seed, source, 4242])`. |
| H6, H8, S29, S30 (their M1/M3 scoring) | Not applicable | The Holland model here is scored on the eye integral, which sets the pressure drop inside the closest approach; its eye shape is wrong but that costs ~1 hPa there. |
| H7 measurement-aware weights | Done | measAIC headline; measAIC_all, AIC/BIC nominal, BIC_eff, buoy and buoy+SAR leave-one-out, equal reported. |
| H9 item #12 | Done, larger than rated | Every headline ring-model trial was refitted with 10 starts: 100 of 1000 move by more than 1 hPa (largest 15.8). The headline uses the better fit of the two for every ring-model trial; ring-model percentiles (2.5/16/50/84/97.5) 865.6, 873.5, 882.8, 891.3, 897.0 vs 4 starts only 865.6, 873.5, 882.6, 891.1, 896.6. The sensitivity cases use 4 starts and are compared with the 4-start headline trials. |
| H10 sample sizes | Adapted | 1000 trials per model for the headline and the three other α20 cases; 250 per model, paired, for the other cases (Evan's choice). |
| Sensitivity cases S1–S31 | Done where applicable | Flag mapping in `results12/flag_map.json`; S11, S16–S18 not applicable (see the file). S23 uses the STAR storm centre in the Sentinel-1D 09:09Z pass summary; S23b adds the one in the RCM-3 21:15Z pass summary; S23c uses the STAR centres for all three 20 Oct passes, adding RCM-3 09:20Z (all in `data_source/star_pass_summaries.txt`). The v12 README called the 09:09Z centre the only STAR centre available; that was wrong (the same 21:15Z centre is printed on STAR's wind-profile plots for that pass, used since v3). The STAR "MERGED_FIX" files carry scene centres, not storm centres. |
| Literature (§7) | Partly | Powell et al. 2009 [A8] reports 0.84 ± 0.09 as a slant reduction at the maximum wind (2–4 km flight level), with no eyewall/outer split: it does not support a value at 2 RMW. Franklin [A6b] is a 10-m/700-hPa climatology from other storms. The dPc/dVm literature values in [B9] were not re-derived here. |
| Pre-check shifts for T/U/A | Differ | Their pre-check moved the v12 ring model −8 hPa under the prior; with the shared profile and the ring-radius fix the ring model's best fit is 881.2 under the prior vs 884.5 free (-3.4). |

## Follow-up to the other group's v12.1 changes [B11] (version 12.1)

| Their v12.1 change | Here | Detail |
|---|---|---|
| Reading weights re-iterated to convergence ("IRLS") | Checked; headline unchanged | Their fix is for weights frozen at each fit's starting vector. Here the position term of every reading's σ uses one set of reference gradients (`grad_ref.json`) in every fit, so all fits and all Pc share one fixed-σ likelihood. The reference comes from a v11 fit at 886 hPa; the v12 gradients near the closest approach are up to 11% steeper. Refreshing the reference from the v12 best fit and iterating moves the unperturbed ring-model best fit from 881.1 to 881.7 hPa; recomputing the gradients inside each fit gives 880.2 without and 882.7 with the log-det term (`irls_check.py`, `irls_check.json`). The refresh moves the best fit by less than 1 hPa, about the Monte Carlo noise of a 250-trial case, so the headline keeps the v11 reference. |
| Centre-offset bound 15 → 40 km | Already ±40 km | Fitted constant offsets from the IR-fix track reach 22.9 km in S24 (139 of 250 trials beyond 15 km on one axis) and 16.6 km with JTWC positions; a 15-km bound would have clipped them. The headline has no free offset (the centre comes from the track model). |
| Wander sd capped by the IR-fix scatter | Already tighter | The track model's wander prior is 0.65–0.97 km per axis at the window readings, estimated from the fixes. Fitted wander in the headline ring-model trials: median 0.64 km, 95% below 2.3 km, no trial within 0.5 km of the ±12 km bound (`wander_offset_check.py`). Their capped 97.5% limit is 3.05 km. |
| Log-det term off | Same | With fixed σ the term is a constant. |
| More Holland-family variants | Not added | The two-ring Holland profile carries weight 0.005 here and about 0 in [B11]; the shortfall is the SAR eye fit, which the variants share. |
| Pooled-fix and IR-vs-STAR centre priors | S23b, S23c added | STAR storm centres from the pass summaries in place of the SAR ring-fit centres of the same passes. 09:09Z only (S23): paired shift +2.3 hPa; 09:09Z and 21:15Z (S23b; the same coordinates as in [B10]'s table): +5.6; all three 20 Oct passes (S23c): +7.7 (ring model +7.50 ± 0.19). Their STAR case moves +6.5 hPa. Each STAR centre lies south-west of the ring fit for the same image (09:09Z -3.3 E, -3.2 N km; 09:20Z -2.7 E, -3.2 N km; 21:15Z -2.7 E, -1.8 N km), so the STAR cases move the whole surface track the same way rather than adding scatter. The IR-vs-STAR random-effects case is not repeated; it overlaps S23c. |

Their v12.1 headline, 884.8 hPa (68% 876.3–892.8, 95% 867.9–900.3), is +1.5 hPa from this one. The two analyses share most of their inputs,
so the agreement is a check on method, not independent data.

## The conversion at 2× the max-wind radius (α20)

Buoy-conservation diagnostics (best fits; Δ relative to the free case; spec §3). Conflict criterion: |Δχ²_buoy| > 1. "Rescaled" divides Δχ² by the
buoy χ²/dof of that fit. Implied α20 = α_rm × s20 / g20 (shape-flexibility test).

| Case | Model | Pc best | χ² buoy | Δχ² vs free | χ²/dof | Δχ² rescaled | conflict | 18:04Z z | ΔLOO vs free | implied α20 | Monte Carlo median (68%) |
|---|---|---|---|---|---|---|---|---|---|---|---|
| free | ring model | 884.5 | 15.72 | +0.00 | 0.59 | +0.00 | no | -2.22 | +0.00 | 1.001 | 885.1 (877.4–891.8) |
| free | no-ring model | 887.2 | 16.84 | +0.00 | 0.61 | +0.00 | no | -1.98 | +0.00 | 0.976 | 887.0 (881.1–892.6) |
| free | two-ring Holland | 886.1 | 13.28 | +0.00 | 0.49 | +0.00 | no | -1.21 | +0.00 | 0.998 | 885.3 (879.5–892.1) |
| prior | ring model | 881.2 | 17.54 | +1.82 | 0.64 | +2.82 | yes | -3.10 | -0.04 | 0.803 | 882.8 (873.5–891.3) |
| prior | no-ring model | 889.4 | 19.45 | +2.61 | 0.70 | +3.72 | yes | -2.94 | -0.32 | 0.923 | 889.8 (883.9–895.6) |
| prior | two-ring Holland | 886.9 | 17.24 | +3.96 | 0.62 | +6.33 | yes | -2.29 | +1.19 | 0.921 | 888.5 (882.3–895.1) |
| flat | ring model | 880.4 | 17.17 | +1.45 | 0.63 | +2.30 | yes | -2.89 | -0.31 | 0.788 | 882.8 (873.6–891.4) |
| flat | no-ring model | 889.2 | 18.30 | +1.46 | 0.66 | +2.22 | yes | -2.50 | -0.32 | 0.969 | 889.7 (884.0–895.1) |
| flat | two-ring Holland | 886.8 | 15.51 | +2.23 | 0.57 | +3.92 | yes | -1.94 | +2.20 | 0.941 | 887.1 (881.7–894.8) |
| loglayer | ring model | 881.4 | 17.58 | +1.86 | 0.65 | +2.88 | yes | -2.95 | -0.36 | 0.785 | 881.8 (873.4–890.2) |
| loglayer | no-ring model | 889.7 | 20.84 | +4.00 | 0.75 | +5.33 | yes | -3.32 | -1.09 | 0.884 | 890.0 (883.8–896.0) |
| loglayer | two-ring Holland | 889.8 | 18.45 | +5.17 | 0.67 | +7.75 | yes | -2.40 | +0.61 | 0.897 | 889.1 (881.9–895.7) |
| buoyonly | ring model | 883.7 | 11.41 | -4.31 | 0.43 | -10.05 | — | -0.46 | +3.27 | 0.978 | 884.8 (878.3–893.7) |
| buoyonly | no-ring model | 887.3 | 18.05 | +1.21 | 0.66 | +1.84 | — | -2.35 | -0.25 | 0.939 | 887.2 (881.2–892.7) |
| buoyonly | two-ring Holland | 885.8 | 13.25 | -0.03 | 0.49 | -0.06 | — | -1.29 | +1.69 | 0.936 | 886.8 (880.3–892.5) |

- Every model's buoy fit pays for the 2× constraint (Δχ² ring model +1.8, no-ring model +2.6, two-ring Holland +4.0 under the prior): a mild conflict (1–2σ), in all
  three models. Freed of it, all three fits settle on a gradient-wind ratio at 2× the radius of 0.63–0.66, which would need α20 ≈ 0.9–1.0 - more
  than "no radial change". That value is partly an artefact of the fitted peak wind (pulled about 1.5σ above the measured value when freed) and of
  the profile shapes between 1 and 2 RMW; the profile-free check below gives 0.81–0.82 at 1.8–2.8 RMW.
- The direction of the shift differs by model: the ring model moves down under the constraint, the no-ring and Holland models barely move or move up.
- The buoy-only rows remove the SAR outer terms rather than add one, so the conflict test does not apply to them.
- Holland's best fit has several near-equal minima (about 886–888 hPa); each table shows the best found in its own run.

## Model weighting

Best fits under the headline case (`weights12.json`); χ²_meas = buoy + SAR terms (RMW, eye integral, 1.4×, peak wind, 2×) + one ring term:

| Model | Pc best | χ² buoy | χ² SAR (5 terms) | ring z | χ²_meas | p_eff | measAIC | α20 fitted |
|---|---|---|---|---|---|---|---|---|
| ring model | 881.2 | 17.54 | 3.32 | +0.21 | 20.90 | 16.5 | 53.8 | 0.786 |
| no-ring model | 889.4 | 19.45 | 9.57 | -1.87 | 32.50 | 13.6 | 59.7 | 0.846 |
| two-ring Holland | 886.9 | 17.24 | 17.53 | -1.55 | 37.18 | 13.7 | 64.5 | 0.829 |

SAR terms (χ² each: RMW, eye integral, 1.4×, peak wind, 2×):

| Model | RMW | eye | 1.4× | peak wind | 2× |
|---|---|---|---|---|---|
| ring model | 1.02 | 0.00 | 0.50 | 1.53 | 0.26 |
| no-ring model | 0.48 | 0.00 | 4.51 | 0.74 | 3.83 |
| two-ring Holland | 0.05 | 0.01 | 11.04 | 0.63 | 5.81 |

| Weighting (headline trials) | Ring / no ring / Holland | Combined median (68%) |
|---|---|---|
| **Measurement-aware AIC, effective parameters (headline)** | 0.95 / 0.05 / 0.00 | 883.3 (873.6–891.7) |
| … adding the SAR ellipse terms | 0.95 / 0.05 / 0.00 | 883.3 (873.6–891.7) |
| AIC, nominal parameter counts | 0.94 / 0.06 / 0.01 | 883.4 (873.7–891.8) |
| BIC, nominal | 0.51 / 0.45 / 0.04 | 886.8 (877.6–894.0) |
| BIC, effective parameters | 0.58 / 0.38 / 0.03 | 886.3 (876.9–893.5) |
| Leave-one-out on buoy readings, pseudo-BMA+ (v11 headline method) | 0.20 / 0.25 / 0.55 | 888.0 (880.9–894.8) |
| … stacking | 0.23 / 0.00 / 0.77 | 887.3 (880.3–894.5) |
| Leave-one-out on buoy readings + SAR measurements, pseudo-BMA+ | 0.29 / 0.64 / 0.07 | 888.4 (880.2–894.8) |
| Equal | 0.33 / 0.33 / 0.33 | 887.5 (879.2–894.5) |
| Ring model only | 1.00 / 0.00 / 0.00 | 882.8 (873.5–891.3) |

- The buoy-only leave-one-out score still prefers Holland, as in v11; once the SAR measurements count, the ring model leads. Leave-one-out on
  buoy + SAR measurements prefers the no-ring model: the ring model honours the SAR ratios in the fit but mispredicts several of them when each
  is held out (peak wind z +3.1, RMW z -2.3), the same pattern as in v11.

## Sensitivity cases (paired seeds; headline weights unless the case has its own)

"Shift vs headline" compares each case with the headline trials that have the same seeds (1000 for the four α20 cases, the first 250
otherwise); the ring model's mean paired shift has its standard error. Sampling noise between a 250-trial case and the full headline is
about ±0.6 hPa on the median, so read the shift columns, not the raw medians.

| Case | Combined median | 68% | 95% | shift vs headline, same seeds | ring model: mean paired shift | ring / no ring / Holland medians |
|---|---|---|---|---|---|---|
| Headline | 883.3 | 873.6–891.7 | 865.6–897.4 | — | — | 882.8 / 889.8 / 888.5 |
| S1 no 2× constraint (own weights) | 885.1 | 877.4–891.8 | 867.6–897.3 | +2.5 | +2.13 ± 0.09 | 885.1 / 887.0 / 885.3 |
| S2 α20 flat 0.77–0.91 (own weights) | 883.9 | 874.1–892.4 | 866.1–897.5 | +0.1 | +0.13 ± 0.08 | 882.8 / 889.7 / 887.1 |
| S3 α20 log-layer (own weights) | 881.8 | 873.4–890.4 | 865.1–896.7 | -0.8 | -0.57 ± 0.05 | 881.8 / 890.0 / 889.1 |
| S4 α20 for the 2× ratio only (1.4× and ring keep the log-layer α; v11 form) | 883.6 | 874.9–891.0 | 868.7–896.4 | -0.1 | +0.42 ± 0.11 | 883.1 / 888.4 / 887.8 |
| S5 IR-surface drift fixed at 3 km | 885.5 | 875.0–893.1 | 868.4–898.4 | +1.8 | +1.49 ± 0.14 | 885.3 / 891.7 / 890.0 |
| S6 drift fixed at 1.5 km | 884.4 | 873.9–891.7 | 866.8–897.6 | +0.7 | +0.41 ± 0.10 | 883.9 / 890.6 / 888.8 |
| S7 no centre wander | 882.3 | 873.3–890.0 | 866.6–897.2 | -1.4 | -0.93 ± 0.25 | 881.8 / 889.3 / 887.4 |
| S8 drift posterior with a log-uniform prior | 882.4 | 873.5–890.4 | 866.2–895.9 | -1.3 | -0.63 ± 0.08 | 882.1 / 889.5 / 888.3 |
| S9 v11 track (drift 1 km) | 883.2 | 873.6–891.1 | 866.2–897.0 | -0.4 | -0.14 ± 0.11 | 882.9 / 889.9 / 888.1 |
| S12 RMW–α20 correlation −0.5 | 883.8 | 873.5–891.4 | 867.0–896.5 | +0.1 | -0.02 ± 0.09 | 883.1 / 890.0 / 888.6 |
| S12 … −0.3 | 883.5 | 873.3–891.5 | 866.9–896.4 | -0.2 | -0.06 ± 0.06 | 883.1 / 889.9 / 888.5 |
| S12 … +0.3 | 883.6 | 873.5–891.9 | 866.6–896.5 | -0.1 | +0.01 ± 0.06 | 883.1 / 889.9 / 888.3 |
| S12 … +0.5 | 883.7 | 873.3–891.8 | 867.1–896.5 | +0.1 | +0.07 ± 0.11 | 883.1 / 889.9 / 887.9 |
| S13 tilt–α20 correlation −0.5 | 884.2 | 873.5–891.2 | 866.8–896.3 | +0.5 | +0.11 ± 0.12 | 883.9 / 889.9 / 888.4 |
| S13 … +0.5 | 883.2 | 873.4–891.4 | 866.4–896.6 | -0.5 | -0.18 ± 0.09 | 882.8 / 890.0 / 888.4 |
| S14 21:15Z weight sd 0.05 | 883.5 | 873.5–891.3 | 866.7–896.4 | -0.2 | -0.04 ± 0.03 | 883.1 / 889.9 / 888.7 |
| S14 … 0.02 | 883.5 | 873.5–891.3 | 866.7–896.4 | -0.2 | -0.04 ± 0.05 | 883.1 / 890.0 / 888.6 |
| S15 SAR 1.4×/2× error correlation 0.3 | 883.7 | 873.4–891.5 | 866.7–896.4 | -0.0 | +0.03 ± 0.04 | 883.2 / 889.7 / 888.6 |
| S15 … 0.5 | 883.7 | 873.3–891.6 | 866.6–896.9 | +0.0 | -0.00 ± 0.05 | 882.9 / 889.6 / 888.5 |
| S15 … 0.7 | 883.7 | 873.5–891.7 | 866.6–896.8 | +0.0 | +0.03 ± 0.06 | 883.0 / 889.4 / 888.3 |
| S19 position term 0.5 km | 880.8 | 870.8–890.0 | 864.0–895.8 | -2.8 | -2.74 ± 0.19 | 880.0 / 889.3 / 888.1 |
| S19 position term 1.5 km | 885.3 | 876.7–893.0 | 870.2–899.6 | +1.6 | +2.11 ± 0.12 | 885.1 / 892.3 / 889.7 |
| S20 reading σ rescaled to buoy χ²/dof = 1 | 882.6 | 872.4–891.1 | 865.1–896.2 | -1.0 | -0.86 ± 0.09 | 882.0 / 889.7 / 888.1 |
| S23 STAR storm centre at 09:09Z | 886.0 | 876.9–893.5 | 868.6–898.4 | +2.3 | +2.35 ± 0.15 | 885.8 / 891.6 / 890.1 |
| S23b STAR storm centres at 09:09Z and 21:15Z | 889.2 | 880.0–895.2 | 872.6–899.7 | +5.6 | +5.21 ± 0.15 | 889.1 / 893.5 / 893.3 |
| S23c STAR storm centres for all three 20 Oct SAR passes | 891.3 | 883.2–896.9 | 875.2–901.1 | +7.7 | +7.50 ± 0.19 | 891.2 / 895.0 / 895.6 |
| S24 flat ±4 km centre error, v10 track | 895.7 | 880.7–900.9 | 864.1–905.1 | +12.0 | +9.16 ± 0.51 | 895.5 / 898.9 / 896.7 |
| S25 21:15Z structure only | 880.4 | 870.6–888.3 | 864.3–894.8 | -3.3 | -3.05 ± 0.09 | 879.7 / 888.1 / 885.2 |
| S26 09Z structure only | 889.8 | 879.9–896.6 | 872.2–901.6 | +6.1 | +6.17 ± 0.16 | 889.6 / 894.3 / 894.5 |
| S27 buoy only (no SAR outer-structure terms) | 885.0 | 878.3–893.7 | 871.0–898.9 | +1.3 | +2.77 ± 0.32 | 884.8 / 887.2 / 886.8 |
| v11 check: trochoidal wobble ≤ 3 km | 884.0 | 873.9–891.4 | 865.5–896.7 | +0.4 | -0.00 ± 0.11 | 883.2 / 889.8 / 888.3 |
| v11 check: v10 geometry | 889.3 | 878.0–896.8 | 869.8–903.1 | +5.7 | +5.09 ± 0.32 | 888.7 / 895.2 / 891.2 |
| v11 check: JTWC best-track positions | 888.4 | 875.7–894.6 | 856.7–902.7 | +4.7 | +2.84 ± 0.53 | 887.9 / 895.0 / 890.8 |
| v11 check: log-ramp density | 883.7 | 873.8–891.3 | 866.8–896.4 | +0.0 | +0.11 ± 0.05 | 883.4 / 889.8 / 888.4 |
| v11 check: Gaussian reading errors | 879.2 | 871.6–889.5 | 864.6–895.6 | -4.5 | -2.98 ± 0.26 | 878.5 / 888.6 / 887.3 |
| v11 check: Student-t scaled to sd σ | 882.2 | 871.6–891.1 | 865.2–896.0 | -1.5 | -1.34 ± 0.13 | 881.7 / 889.5 / 888.1 |
| v11 check: structure only (no wind input) | 869.9 | 857.5–885.5 | 848.9–894.8 | -13.7 | -12.26 ± 0.55 | 869.3 / 885.6 / 885.5 |
| v11 check: power-law eye (ring model) | 882.7 | 873.1–889.7 | 866.7–895.1 | — | — | 882.7 |

## Derivatives (headline trials; regression slope ± standard error)

| Model | dPc/dV (hPa per kt, surface peak, with RMW held) | dPc/dRMW total (hPa/km) | dPc/dRMW with peak held | dPc/dα_rm (hPa per unit) | dPc/dα20 (fitted) | dPc/d(effective distance at 17:04Z) (hPa/km) |
|---|---|---|---|---|---|---|
| ring model | -0.62 ± 0.02 | 3.48 ± 0.10 | 2.69 ± 0.07 | 45.09 ± 9.04 | 54.44 ± 4.93 | -3.92 ± 0.21 |
| no-ring model | -0.66 ± 0.02 | 1.64 ± 0.13 | 1.80 ± 0.08 | 48.63 ± 6.61 | -0.35 ± 4.87 | -2.29 ± 0.16 |
| two-ring Holland | -0.68 ± 0.02 | 2.74 ± 0.12 | 1.72 ± 0.09 | 52.54 ± 7.08 | 14.68 ± 5.78 | -2.63 ± 0.19 |

- [B9] quotes −0.97 to −1.03 hPa/kt from Knaff & Zehr [A4] and Chavas et al. [A28] for the wind-pressure sensitivity without a buoy anchor
  (not re-derived here). With the buoy reading fixing the pressure near the RMW, the sensitivity to the peak wind is smaller.

## Centre position at 17:33Z (`centre12.py`, `centre_map.py`, `centre_compare.py`, `centretrials12.py`; Figure 3)

![Centre at 17:33Z](figures/fig3_centre.png)

Figure 3: shading is the ring model's best-fit central pressure with the 17:33Z centre held at each point of a 1.5-km grid (the whole centre
track shifted by a constant offset; no centre prior, no wander); white contours are the buoy + SAR misfit above its minimum (Δχ² 2.3 / 6.2 / 11.8,
68 / 95 / 99.7% for two parameters). Markers are the fitted 17:33Z centre in the Monte Carlo trials of each case (mean ± 1σ), the centre fixes at
their own times and the separately run analysis [B10]. Positions are km from the smoothed IR eye track at 17:33Z.

- **Data only.** With the centre fixes removed (the whole track shifted by a free constant offset, no wander), the buoy readings and SAR terms pull the
  centre south-west along a long, weakly bound valley: best fit 12.9 km west and 32.6 km south of the fix-based track, Pc 902.9; the
  fix-based centre is Δχ² 11.4 worse (about 3σ for two parameters). The no-ring and Holland models do the same (Pc 905 and 908 at their own optima).
  Near the fix-based centre, Pc rises about 2.4 hPa per km as the centre slides south-west (Figure 3).
- **Not independent evidence of position.** A symmetric profile can absorb the 18:04Z misfit and any wavenumber-1 asymmetry only by moving
  the centre. The fixes nearest the pass that neither this analysis nor [B10] fits agree with the fix-based track, measured at their own times:

| Time | Fix | East (km) | North (km) | Distance (km) | Stated uncertainty |
|---|---|---|---|---|---|
| 17:01Z | ATMS microwave eye centre (ATMS (NOAA-20)) | +0.1 | -1.6 | 1.6 | n/a (epoch scatter 0.74 km, 3 epochs) |
| 17:02Z | ARCHER (Geo-ref) (ATMS-N20 183GHz) | -25.8 | +5.5 | 26.4 | 10 (50% certainty radius) |
| 17:02Z | ARCHER (raw) (ATMS-N20 183GHz) | -0.4 | -2.3 | 2.3 | 10 (50% certainty radius) |
| 17:30Z | ARCHER (Geo-ref) (HIMAWARI9 SWIR) | -11.1 | -0.9 | 11.2 | 46 (50% certainty radius) |
| 17:30Z | ARCHER (Geo-ref) (HIMAWARI9 IR) | -11.1 | -0.9 | 11.2 | 8 (50% certainty radius) |
| 17:30Z | ARCHER (raw) (HIMAWARI9 SWIR) | +1.6 | -4.2 | 4.5 | 46 (50% certainty radius) |
| 17:30Z | ARCHER (raw) (HIMAWARI9 IR) | +1.6 | -4.2 | 4.5 | 8 (50% certainty radius) |
| 18:00Z | JTWC 6-hourly position (0.1 deg) (JTWC) | -7.9 | -2.7 | 8.3 | ~5.6 (0.05 deg rounding) |

  None points 10 km or more south-west. The ARCHER geo-referenced positions lie 11–26 km west and disagree with ARCHER's own raw positions for the same images.
- **Separate centre analysis.** A separately run centre-relocation analysis [B10] (IR-fix track plus an offset prior times a buoy + SAR likelihood;
  different code, mostly the same fixes) puts the 17:33Z centre 1.4 km from the v12 fitted centre (+0.9 km east, -1.1 km north), within both
  analyses' 1σ (theirs 1-sigma along 0.86 / cross 1.43; v12 ±0.9 km per component). Their IR-vs-STAR sensitivity centre lies 5.5 km away, to the south (-1.0 E, -5.4 N).
  The two analyses share most of their inputs, so this is a check on method, not independent data.
- In the headline the centre is carried by the track model (±0.9 km); wider centre assumptions are sensitivity cases: flat ±4 km
  +12, v10 geometry +6, JTWC positions +5, STAR centre at 09:09Z +2, STAR centres at 09:09Z and 21:15Z +6, STAR centres for all three passes +8 hPa (paired shifts).

Fitted 17:33Z centre in the ring-model trials (km from the smoothed IR eye track; mean ± 1σ):

| Case | Trials | East | North | Case Pc (hPa) |
|---|---|---|---|---|
| headline | 1000 | +7.0 ± 0.9 | -9.5 ± 1.4 | 883.3 |
| no centre wander | 250 | +7.2 ± 0.3 | -8.9 ± 0.3 | 882.3 |
| IR-surface drift fixed at 3 km | 250 | +7.2 ± 1.2 | -10.6 ± 1.8 | 885.5 |
| STAR 09:09Z centre | 250 | +6.3 ± 0.8 | -9.7 ± 0.9 | 886.0 |
| STAR 09:09Z and 21:15Z centres | 250 | +5.5 ± 0.9 | -10.9 ± 1.0 | 889.2 |
| STAR centres, all three SAR passes | 250 | +4.9 ± 0.8 | -11.3 ± 0.9 | 891.3 |
| JTWC best-track positions | 250 | +10.8 ± 2.2 | -12.8 ± 3.7 | 888.4 |
| flat ±4 km centre error (v10 track) | 250 | +5.9 ± 1.8 | -14.2 ± 4.2 | 895.7 |

## The 18:04Z reading

Residuals of the ring model held at the combined median (883.3 hPa):

| Reading | P (hPa) | obs − model | σ | z | Student-t weight |
|---|---|---|---|---|---|
| 15:00Z | 969.5 | -0.5 | 0.6 | -0.73 | 1.10 |
| 16:04Z | 949.7 | +0.6 | 1.5 | +0.37 | 1.21 |
| 17:04Z | 909.4 | -0.4 | 3.0 | -0.13 | 1.24 |
| 18:04Z | 913.3 | -7.3 | 2.1 | -3.47 | 0.31 |
| 19:04Z | 949.9 | +1.1 | 1.4 | +0.73 | 1.10 |
| 20:04Z | 965.6 | +0.7 | 0.7 | +1.01 | 1.00 |

- Kept in every run. With the outer constraint the ring model's z at 18:04Z is -3.10 (free -2.22).

## How far the data can be pushed (17:04Z reading + measured SAR winds, no profile model; `bounds.py`)

17:04Z reading at 26.0 km effective distance (posterior-median track).

| Assumptions | Pressure drop to the centre | Central pressure at most |
|---|---|---|
| 21:15Z structure, log-layer conversion | 28.7 hPa | 880.7 |
| 09Z structure (wider, weaker eyewall) | 17.6 hPa | 891.8 |
| ...and surface winds treated as gradient winds (friction ignored) | 13.7 hPa | 895.7 |
| ...and centre 2 km closer to the buoy | 10.4 hPa | 899.0 |
| ...and the Sentinel-1D wind level (0.83), eyewall 10% wider | 5.4 hPa | 904.0 |

## Buoy vs SAR pressure rise (`gapcheck.py`)

Measured rise ÷ rise predicted from the 17:33Z SAR winds in gradient balance (no profile model):

| Pair | Radii (km) | Flat factor 0.85 | Log-layer α(r) | Two-point, α20 = 0.80 |
|---|---|---|---|---|
| 17:04 -> 16:04 | 26 → 47 | 1.22 | 1.17 | 1.23 |
| 18:04 -> 19:04 | 30 → 47 | 1.62 | 1.48 | 1.58 |
| 17:04 -> 15:00 | 26 → 75 | 1.19 | 1.07 | 1.13 |
| 18:04 -> 20:04 | 30 → 69 | 1.42 | 1.24 | 1.32 |
| 16:04 -> 15:00 | 47 → 75 | 1.13 | 0.91 | 0.97 |
| 19:04 -> 20:04 | 47 → 69 | 1.10 | 0.89 | 0.96 |

The two outer pairs (1.8–2.8 RMW) are matched with a constant factor of about 0.81–0.82 (±0.065 for the ±8% SAR wind level), between the
dropsonde climatology (0.78 at 700 hPa [A6b]) and no radial change (0.885). The inner pairs (1–1.8 RMW) need a lower factor (0.77; 0.67 with
18:04Z), where the centre position, the RMW and the 18:04Z reading are all uncertain.

## SAR passes (NOAA/STAR level-2, 500 m [B1]; retrieval [A26], cross-pol GMFs [A14, A27]; `sar/`)

| Pass | RMW (km) | Az.-mean max (kt) | Eye integral I47 | v(1.4 RMW)/max | Secondary max | Incidence at eyewall |
|---|---|---|---|---|---|---|
| Sentinel-1D 20 Oct 09:09Z | 28.25 | 112 | 0.363 | 0.78 | 43.2 km, 0.83 | 28–32° |
| RCM-3 20 Oct 09:20Z | 27.25 | 135 | 0.317 | 0.72 | 45.8 km, 0.81 | 48–52° |
| RCM-3 20 Oct 21:15Z | 25.75 | 146 | 0.375 | 0.72 | 43.2 km, 0.77 | 27–33° |
| RCM-3 21 Oct 09:29Z | 27.25 | 134 | 0.391 | 0.83 | none | 28–33° |
| RCM-3 21 Oct 21:23Z | 30.25 | 99 | 0.507 | 0.90 | none | — |

## Files

- `model/`: scripts, inputs and results side by side (all paths relative to the script folder). v12 additions: `track_hier.py` (track models over the
  drift amplitude, `track_variants.npz`, `track_hier.json`), `mc12.py` (Monte Carlo), `weights12.py` (best fits, diagnostics, weights),
  `analyze12.py` (combination), `ladder12.py`, `scan12.py`, `cfits12.py`, `centre12.py` / `centre_map.py` / `centre_compare.py` / `centretrials12.py` (centre checks) and `fig3_centre.py` (Figure 3).
  v12.1 additions: `irls_check.py` (reading-error gradients), `wander_offset_check.py` (wander and offset bounds), the both-STAR track in `track_hier.py`.
- `model/results12/`: `summary_<case>.json` (quantiles, sd, weights, per-model results, bootstrap SE), `conservation_<case>.json` (spec §3),
  `derivatives_<case>.json` (spec §7), `draws_<case>.csv` (every trial: drawn inputs, fitted parameters, χ² by component, residual, z and effective
  distance of every reading, mixture weight), `flag_map.json` (spec flag → v12 setting).
- `model/mc12.json`: every trial record. `archive/`: versions 3–12.
- The per-trial records (`model/mc12.json`, `model/results12/draws_*.csv`) ship as a second zip, `champi_buoy_pressure_v12.1_trials.zip`; unzip both into the
  same place. Everything else, including every summary, diagnostic and figure, is in the main zip.

## Running

`./run_all.sh` reproduces everything from the shipped inputs (Python 3 with numpy, scipy, matplotlib). The track models (`model/track_variants.npz`, 31 MB)
are not shipped: `python3 track_hier.py` in `model/` rebuilds the identical file in about 3 minutes (run_all.sh does this first); run it before
using any script on its own. The Monte Carlo (40310 trials) takes about
17 hours on two cores and is resumable: rerun the same command after an interruption.

## Version history

1–7. Earlier hand fits and single-profile models (see `archive/`).
8. Level-2 SAR at 09:20Z, time interpolation, Sentinel-1D cross-calibration, leave-one-out: 878 (870–887).
9–10. SAR-measured eye, secondary ring, Student-t errors, external review: 884 (876–892).
11. Track model with wander, oval eyewall, three profile models weighted by buoy-only leave-one-out: 889 (880–894).
12. One conversion profile with α20 fitted, ring-radius fix, IR-surface drift from its posterior, weights that count the SAR measurements:
    883 (874–892).
12.1. Follow-up to the other group's v12.1 changes: STAR-centre cases S23b and S23c, reference-gradient and bound checks; headline unchanged:
    **883 (874–892)**.

## References and data sources

Citations were checked against publisher, Crossref or agency pages on 25–26 Oct 2026. Items marked † were matched to standard bibliographic
records but not re-opened. Items marked ‡ could not be confirmed (see notes).

### A. Literature
- [A1] Holland, G. J., 1980: An analytic model of the wind and pressure profiles in hurricanes. *Mon. Wea. Rev.*, 108, 1212–1218. https://doi.org/10.1175/1520-0493(1980)108<1212:AAMOTW>2.0.CO;2 (two-ring Holland profile)
- [A2] Willoughby, H. E., R. W. R. Darling, and M. E. Rahn, 2006: Parametric representation of the primary hurricane vortex. Part II: A new family of sectionally continuous profiles. *Mon. Wea. Rev.*, 134, 1102–1120. https://doi.org/10.1175/MWR3106.1 (power-law eye, dual-exponential decay)
- [A3] Atkinson, G. D., and C. R. Holliday, 1977: Tropical cyclone minimum sea level pressure/maximum sustained wind relationship for the western North Pacific. *Mon. Wea. Rev.*, 105(4), 421–427. https://doi.org/10.1175/1520-0493(1977)105<0421:TCMSLP>2.0.CO;2 (context curve in Fig. 2b)
- [A4] Knaff, J. A., and R. M. Zehr, 2007: Reexamination of tropical cyclone wind–pressure relationships. *Wea. Forecasting*, 22, 71–88. https://doi.org/10.1175/WAF965.1 (context)
- [A5] Courtney, J., and J. A. Knaff, 2009: Adapting the Knaff and Zehr wind–pressure relationship for operational use in Tropical Cyclone Warning Centres. *Aust. Meteor. Oceanogr. J.*, 58(3), 167–179. https://doi.org/10.22499/2.5803.002 (context)
- [A6] Franklin, J. L., M. L. Black, and K. Valde, 2003: GPS dropwindsonde wind profiles in hurricanes and their operational implications. *Wea. Forecasting*, 18, 32–44. https://doi.org/10.1175/1520-0434(2003)018<0032:GDWPIH>2.0.CO;2
- [A6b] Franklin, J. L., M. L. Black, and K. Valde, 2000: *Eyewall Wind Profiles in Hurricanes Determined By GPS Dropwindsondes*. NOAA/NHC web report. https://www.nhc.noaa.gov/aboutwindprofile.shtml (357 eyewall dropsonde profiles from NOAA and Air Force Reserve aircraft in 17 Atlantic and eastern-Pacific hurricanes, 1997–1999; 10-m wind / 700-hPa flight-level wind: 0.91 eyewall, 0.88 at the RMW (a lower bound), 0.78 outer vortex. Not Champi data, and the reference level is 700 hPa (~3 km), not the top of the boundary layer: used as a consistency check on the shape of α(r) only)
- [A7] Powell, M. D., P. J. Vickery, and T. A. Reinhold, 2003: Reduced drag coefficient for high wind speeds in tropical cyclones. *Nature*, 422, 279–283. https://doi.org/10.1038/nature01481 (roughness range in the log-layer factor)
- [A8] Powell, M. D., E. W. Uhlhorn, and J. D. Kepert, 2009: Estimating maximum surface winds from hurricane reconnaissance measurements. *Wea. Forecasting*, 24(3), 868–883. https://doi.org/10.1175/2008WAF2007087.1 (RMW tilt; mean slant reduction factor, maximum surface wind / maximum 2–4 km flight-level wind on the same leg, 0.84 ± 0.09 (SFMR, 15 hurricanes) - an eyewall value with no eyewall/outer split, so it does not constrain α at 2 RMW)
- [A9] Stern, D. P., and D. S. Nolan, 2009: Reexamining the vertical structure of tangential winds in tropical cyclones: Observations and theory. *J. Atmos. Sci.*, 66(12), 3579–3600. https://doi.org/10.1175/2009JAS2916.1 (RMW tilt)
- [A10] Kepert, J., 2001: The dynamics of boundary layer jets within the tropical cyclone core. Part I: Linear theory. *J. Atmos. Sci.*, 58(17), 2469–2484. https://doi.org/10.1175/1520-0469(2001)058<2469:TDOBLJ>2.0.CO;2 ; Kepert, J., and Y. Wang, 2001: Part II: Nonlinear enhancement. *J. Atmos. Sci.*, 58(17), 2485–2501. https://doi.org/10.1175/1520-0469(2001)058<2485:TDOBLJ>2.0.CO;2 (supergradient boundary-layer wind in the log-layer factor)
- [A11] Muramatsu, T., 1986: Trochoidal motion of the eye of Typhoon 8019. *J. Meteor. Soc. Japan*, 64, 259–272. https://doi.org/10.2151/jmsj1965.64.2_259
- [A12] Nolan, D. S., M. T. Montgomery, and L. D. Grasso, 2001: The wavenumber-one instability and trochoidal motion of hurricane-like vortices. *J. Atmos. Sci.*, 58(21), 3243–3270. https://doi.org/10.1175/1520-0469(2001)058<3243:TWOIAT>2.0.CO;2
- [A13] Kuo, H.-C., R. T. Williams, and J.-H. Chen, 1999: A possible mechanism for the eye rotation of Typhoon Herb. *J. Atmos. Sci.*, 56(11), 1659–1673. https://doi.org/10.1175/1520-0469(1999)056<1659:APMFTE>2.0.CO;2
- [A14] Mouche, A., B. Chapron, J. Knaff, Y. Zhao, B. Zhang, and C. Combot, 2019: Copolarized and cross-polarized SAR measurements for high-resolution description of major hurricane wind structures: Application to Irma category 5 hurricane. *J. Geophys. Res. Oceans*, 124, 3905–3922. https://doi.org/10.1029/2019JC015056
- [A15] Combot, C., A. Mouche, J. Knaff, Y. Zhao, Y. Zhao, L. Vinour, Y. Quilfen, and B. Chapron, 2020: Extensive high-resolution synthetic aperture radar (SAR) data analysis of tropical cyclones: Comparisons with SFMR flights and best track. *Mon. Wea. Rev.*, 148(11), 4545–4563. https://doi.org/10.1175/MWR-D-20-0005.1
- [A16] Velden, C. S., and D. Herndon, 2020: A consensus approach for estimating tropical cyclone intensity from meteorological satellites: SATCON. *Wea. Forecasting*, 35(4), 1645–1662. https://doi.org/10.1175/WAF-D-20-0015.1
- [A17] Olander, T. L., and C. S. Velden, 2019: The Advanced Dvorak Technique (ADT) for estimating tropical cyclone intensity: Update and new capabilities. *Wea. Forecasting*, 34(4), 905–922. https://doi.org/10.1175/WAF-D-19-0007.1
- [A18] Wimmers, A. J., and C. S. Velden, 2016: Advancements in objective multisatellite tropical cyclone center fixing. *J. Appl. Meteor. Climatol.*, 55(1), 197–212. https://doi.org/10.1175/JAMC-D-15-0098.1 (ARCHER)
- [A19] Griffin, S. M., A. Wimmers, and C. S. Velden, 2024: Predicting short-term intensity change in tropical cyclones using a convolutional neural network. *Wea. Forecasting*, 39(1), 177–202. https://doi.org/10.1175/WAF-D-23-0085.1 (D-MINT and D-PRINT; the products have also been called DeepMicroIRNet, DeepMultiNet and OPEN-AIIR)
- [A19b] Olander, T., A. Wimmers, C. Velden, and J. P. Kossin, 2021: Investigation of machine learning using satellite-based advanced Dvorak technique analysis parameters to estimate tropical cyclone intensity. *Wea. Forecasting*, 36(6), 2161–2186. https://doi.org/10.1175/WAF-D-20-0234.1 (AiDT)
- [A20] Lange, K. L., R. J. A. Little, and J. M. G. Taylor, 1989: Robust statistical modeling using the t distribution. *J. Amer. Stat. Assoc.*, 84(408), 881–896. https://doi.org/10.1080/01621459.1989.10478852 †
- [A21] Yao, Y., A. Vehtari, D. Simpson, and A. Gelman, 2018: Using stacking to average Bayesian predictive distributions (with discussion). *Bayesian Analysis*, 13(3), 917–1007. https://doi.org/10.1214/17-BA1091 † (pseudo-BMA+ and stacking weights)
- [A22] Akaike, H., 1974: A new look at the statistical model identification. *IEEE Trans. Autom. Control*, 19(6), 716–723. https://doi.org/10.1109/TAC.1974.1100705 † ; Schwarz, G., 1978: Estimating the dimension of a model. *Ann. Statist.*, 6(2), 461–464. https://doi.org/10.1214/aos/1176344136 † (AIC, BIC)
- [A23] Rubin, D. B., 1981: The Bayesian bootstrap. *Ann. Statist.*, 9(1), 130–134. https://doi.org/10.1214/aos/1176345338 †
- [A24] Rasmussen, C. E., and C. K. I. Williams, 2006: *Gaussian Processes for Machine Learning*. MIT Press. http://www.gaussianprocess.org/gpml/ † (Matérn covariance)
- [A24b] Patterson, H. D., and R. Thompson, 1971: Recovery of inter-block information when block sizes are unequal. *Biometrika*, 58(3), 545–554. https://doi.org/10.1093/biomet/58.3.545 † (restricted maximum likelihood)
- [A25] Alet, F., et al., 2026: Operational tropical cyclone forecasting with AI. *Nature*, 657, 680–688. https://doi.org/10.1038/s41586-026-10953-2 (WeatherNext-Cyclones; its GenCast baseline converts pressure to wind with v_max = 6.70·max(0, 1010 − p_min)^0.644, constants refitted to ERA5 and IBTrACS in the Atkinson–Holliday form; discussed, not used in the fit)
- [A26] Monaldo, F. M., 2005: *ANSWRS: APL/NOAA SAR Wind Retrieval System Software Documentation*. JHU/APL Report SRO-05-13. Listed at https://www.star.nesdis.noaa.gov/socd/mecb/sar/publications.php ; Monaldo, F., C. Jackson, W. Pichel, and X. Li, 2015: Early validation of operational SAR wind retrievals from Sentinel-1A. *IGARSS 2015*, 1223–1226. https://doi.org/10.1109/IGARSS.2015.7325993 ; Monaldo, F., C. Jackson, and W. Pichel, 2013: Seasat to RADARSAT-2: Research to operations. *Oceanography*, 26(2). https://doi.org/10.5670/oceanog.2013.29
- [A27] NOAA/NESDIS/STAR: *Tropical SAR Winds Technical Document* (cross-polarisation GMFs for Sentinel-1, RADARSAT-2 and RCM, July 2023). https://www.star.nesdis.noaa.gov/socd/mecb/sar/tropical_gmf_tech_doc.php ‡ The GMF labels in the file metadata ("S1A_V2", "RCM_V1") are not documented publicly; STAR documents its cross-pol GMFs as C-10 (Sentinel-1) and C-12 (RCM), matching the "C-10"/"C-12" in the file names.
- [A28] Chavas, D. R., J. A. Knaff, and P. Klotzbach, 2025: A simple model for predicting tropical cyclone minimum central pressure from intensity and size. *Wea. Forecasting*, 40(2), 333–346. https://doi.org/10.1175/WAF-D-24-0031.1 † (title, volume and pages checked at https://purr.purdue.edu/projects/chavasetal2024 ; DOI as given in [B9]; context for the wind-pressure sensitivity)

### B. Data and products
- [B1] NOAA/NESDIS/STAR SAR tropical-cyclone winds, Champi (WP292026): level-2 500-m NetCDF and 3-km "MERGED_FIX" files for the six passes in `sar/sar_files.json` (names, acquisition times, sha256). Storm centres from STAR's pass summaries, supplied by Evan (`data_source/star_pass_summaries.txt`): Sentinel-1D 09:09Z 133.719 E / 16.670 N, RCM-3 09:20Z 133.684 E / 16.687 N, RCM-3 21:15Z 130.818 E / 17.707 N ("Storm Center Longitude/Latitude"; ATCF fix records 16.67N 133.72E, 16.69N 133.68E, 17.71N 130.82E); the RCM-3 21 Oct 09:29Z summary (128.349 E / 18.931 N) is included for completeness and not used. https://www.star.nesdis.noaa.gov/socd/mecb/sar/sarwinds_tropical.php . Credit: "Synthetic Aperture Radar (SAR) based tropical wind products were generated and provided through the STAR Water Surface Conditions Science Team's tropical cyclone winds program." STAR products are for experimental use. RADARSAT Constellation Mission imagery © Government of Canada 2026; RADARSAT is an official mark of the Canadian Space Agency. Contains modified Copernicus Sentinel data (2026).
- [B2] NOAA/NCEP Global Forecast System 0.25° (wind directions used by ANSWRS; file metadata "gfs025"). https://nomads.ncep.noaa.gov/ † The cross-polarisation retrievals used here are close to direction-independent.
- [B3] Japan Meteorological Agency, Himawari-9 infrared imagery (hourly eye fixes, `ir_eye_fixes.json`). https://www.data.jma.go.jp/mscweb/en/himawari89/ † ; imagery viewer RAMMB/CIRA SLIDER https://rammb-slider.cira.colostate.edu/ †
- [B4] ATMS 165/183-GHz imagery (NOAA/NASA JPSS), eye fix at 17:01Z. https://www.nesdis.noaa.gov/our-satellites/currently-flying/joint-polar-satellite-system † (the satellite and the imagery page used are not recorded here)
- [B5] Joint Typhoon Warning Center, WP29 2026 (ATCF working best track; positions and 895 hPa). https://www.metoc.navy.mil/jtwc/jtwc.html ‡ JTWC releases final best tracks after the season, so 895 hPa is provisional. RSMC Tokyo (JMA) lists 900 hPa (not checked here).
- [B6] CIMSS Tropical Cyclones, University of Wisconsin–Madison SSEC (ADT, AiDT, SATCON, D-MINT, D-PRINT, ARCHER values in Fig. 2). https://tropic.ssec.wisc.edu/
- [B7] Drifting buoy WMO 2802285, hourly sea-level pressure and position (spreadsheet `data_source/champi_revised.xlsx`, supplied by Evan; NOAA source page shared by Evan). ‡ The originating platform record was not found in public GDP/OSMC/NDBC listings from here. Drifting-buoy data of this kind are distributed via the GTS (WMO buoy identifiers: https://community.wmo.int/en/buoy-wmo-identification-numbers), the NOAA Global Drifter Program (https://www.aoml.noaa.gov/phod/gdp/) and NOAA NDBC.
- [B8] Independent review of v9/v10 by a separately run analysis ("champi_v11guidance_for_claude": `AUDIT.md`, `REVIEW_V10.md`, `WRITEUP.md`), supplied by Evan, 24–25 Oct 2026. Unpublished.
- [B9] The same group's review of v11 (`review_claude_v11.md`) and v12 specification (`v12_spec_for_claude.md`), supplied by Evan, 25–26 Oct 2026. Unpublished. Literature values quoted from [B9] and not re-derived here are marked as such.
- [B10] The same group's centre-relocation and Holland-shape study ("v12-CR"), with the centre-fix table `data_source/centre_fixes_17Z.csv`
  (Himawari-9 IR fixes, SAR ring-fit and STAR centres, ATMS, CIMSS ARCHER and JTWC positions), supplied by Evan, 26 Oct 2026. Unpublished.
- [B11] The same group's v12.1 changes (addendum to `v12_spec_for_claude.md`, v12.1 section of their results notes), supplied by Evan, 28 Oct 2026. Unpublished.

### C. Software
- Harris, C. R., et al., 2020: Array programming with NumPy. *Nature*, 585, 357–362. https://doi.org/10.1038/s41586-020-2649-2 †
- Virtanen, P., et al., 2020: SciPy 1.0: fundamental algorithms for scientific computing in Python. *Nature Methods*, 17, 261–272. https://doi.org/10.1038/s41592-019-0686-2 †
- Hunter, J. D., 2007: Matplotlib: A 2D graphics environment. *Computing in Science & Engineering*, 9(3), 90–95. https://doi.org/10.1109/MCSE.2007.55 †
