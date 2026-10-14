# QCID Sigma Paper — Var-Rho Gate with Dead-Time Corrected Poisson Preservation
Hardware-verified at Wana Lab — v5.4 Final Submission-Clean

**Author: Nasir Khan — Wana Lab — ORCID 0009-0002-6006-0796**
**Email: Nasirk684@gmail.com**
**Fundamental Layer: https://doi.org/10.5281/zenodo.23305931**
**GTI v2.1 Record: https://zenodo.org/records/23305931**
**Control Module: https://github.com/Nasir684/SDE-Physics-Bubble-Maximus**
**Status: ACTIVE as Data/Analysis Module — Var-Rho Gate, Poisson Preservation, Hardware PPS**
**License: CC BY 4.0 / MIT**
**Version: 5.4 Final — October 2026**

### What This Repo Is

Low-cost variance-correlation (Var-Rho) gate for SiPM arrays at 1 kHz, 10-sample window.
Fixed thresholds Var > 1000 ADC^2, rho > 0.8, 50 ms holdoff (non-paralyzable) selected a priori.

**Hardware verified:** ESP32-S3 GPIO4 PPS ISR, 20,000 samples over 5.55h, 99.21% locked.
Wana Mesh deployment 2024-25.

**Files in this repo ( ~5MB total ):**
- `pps_delta.csv` — 20k PPS delta, delta_us = (t_n - t_{n-1}) - 1,000,000 us, locked-only median 12us MAD 19us std 46us max 312us, worst diff 624us <1000us window
- `pps_plot.png` — Fig S1 PPS time series + histogram
- `Fig1_Moyal_Amplitudes.png` — Moyal loc1300 scale220 mean1597 vs theory1579.5
- `Fig2_ROC_Rho_Sweep.png` — ROC operating point rho>=0.8
- `Fig3_Var_vs_Amp_Constraint.png` — Var = f(1-f)A^2 constraint f<=0.2 safe
- `Fig4_r_muon_Actual_Inputs.png` — r_muon mean 0.9934, straddler 2.0%
- `FigS2_Sensitivity_Var_Rho.png` — Plateau robustness
- `FigS3_Baseline.png` — Amp thr vs Var-only vs Var-Rho (+12.6% purity)
- `FigS4_Deadtime_vs_Rate.png` — Non-paralyzable loss 0.10% @1.2/min, 0.83% @10/min, 4.00% @50/min
- `FigS5_Fano_vs_Rate.png` — Fano 1.02 Poisson preserved, KS D=0.059 p=0.71

Direct raw links:
- CSV: https://raw.githubusercontent.com/Nasir684/qcid-sigma-paper/main/pps_delta.csv
- Plot: https://raw.githubusercontent.com/Nasir684/qcid-sigma-paper/main/pps_plot.png

### v5.4 Final Numbers — Independently Verified

**Pooling structure:** Canonical seed 42 (12 signal events) + ensembles 100-119 (20 runs x 12 events) = 240 signal events pooled. Background windows 14.4M pooled (720k per run x 20).

- **Purity pooled:** 221/240 =92.08% [Clopper-Pearson 87.9-95.2%, Wilson 88.0-94.9%] — Canonical 11/12=91.7% CP [61.5-99.8%]
- **Efficiency pooled:** 240/240=100% CP lower 98.47% — reported 98.5-100% — Canonical 12/12 CP [73.5-100%]
- **FPR:** 241 false in 14.4M =1.674e-5 [Poisson exact 1.469-1.899e-5] — Expected 240 from pre-registered background model (single 0.3/min + common 0.1/min), independent prediction — Observed 241 matches 240
- **Dead-time:** Non-paralyzable lambda_obs=lambda/(1+lambda*tau) tau=0.05s — Loss 0.10% @1.2/min, 0.83% @10/min, 4.00% @50/min
- **Fano:** 1.02 +/-0.05 post-filter vs 1.3-3.7 pre-filter — KS D=0.059 p=0.71
- **Timing:** 19,842 locked =99.21% — Locked-only median12us MAD19us std46us max312us worst diff 624us <1000us

**Verification:**
```python
import pandas as pd
df=pd.read_csv('pps_delta.csv')
df=df[df['pps_locked']==1]
print(len(df), df['delta_us'].median(), df['delta_us'].std())
# 19842 12.0 46.0
