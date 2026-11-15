# Anticipating Hypoglycemia with CGM Forecasters

Code for the paper **"Anticipating Hypoglycemia with CGM Forecasters: Alarm Thresholds, Hypoglycemia-Weighted Training, and What RMSE Hides — A Multi-Study Evaluation on DiaData"** (Md Rasheduzzaman, [supervisor]; Shanghai University of Engineering Science).

The study asks why CGM glucose forecasters with good RMSE rarely anticipate hypoglycemic episodes at the default 70 mg/dL alarm threshold, and how much a validation-tuned alarm threshold, a hypoglycemia-weighted loss, and a simple trend rule each contribute. It is a retrospective algorithmic evaluation, not a clinically validated alert system.

## Data (not included)

The experiments use **DiaData v3**, raw (non-imputed) 5-min main database (`MDB_5min_sampling_raw.csv`), CC BY-NC 4.0:
Zenodo doi: [10.5281/zenodo.17285631](https://doi.org/10.5281/zenodo.17285631).
Download it yourself and set the path in the configuration cell (see below). The 200 sampled patients are drawn with `RANDOM_STATE = 42` after excluding the two 15-min studies (HUPA-UCM, ShanghaiT1D); `patient_study.csv` lists the sampled patients and their studies.

## Contents

| File | Purpose |
|---|---|
| `GlucoseNet_experiments.ipynb` | The whole pipeline, with outputs from the reported run |
| `results/` | Small CSV summaries behind the tables and figures (see below) |
| `requirements.txt` | Python packages |

## How to run

1. Install packages: `pip install -r requirements.txt` (a CUDA GPU is strongly recommended).
2. In the **CONFIGURATION** cell (Cell 2) set `DIADATA_PATH` and `RESULTS_DIR` to your own folders (the notebook ships with the author's Windows paths).
3. Optional smoke test: set `QUICK_TEST = True` (about 5 minutes), then back to `False`.
4. Run all cells in order. Finished training runs are cached in `RESULTS_DIR` and reloaded rather than retrained, so an interrupted run can be resumed.

Seeds: neural models use seeds 0, 1, 2; patient sampling uses `RANDOM_STATE = 42`.

## Notebook map

| Steps | Content | Paper |
|---|---|---|
| Setup, A | Data loading, gap-aware preprocessing, windows, benchmark (persistence, ridge, CNN, LSTM, CNN–BiLSTM–attention) | Sec. II–III, Table I |
| B, C, F | Choice of weight W (pre-defined rule), ablation, W sweep | Sec. III-B, Table I |
| D | 15- and 60-min horizons | Table IV |
| E | Summary tables, Wilcoxon tests, Clarke grids | Table I, Fig. 1 |
| G | Event-based detection, false alarms, warning time, bootstrap CIs | Sec. III-D |
| H | Leave-one-study-out | Table VI |
| I-1 to I-4 | Dense validation windows, rule baselines, weighted ridge, **validation-tuned thresholds** | Tables II–III (primary protocol) |
| I-5, I-9 | Strict false-alarm definition; level 2 (<54 mg/dL) | Sec. IV-C, Table III |
| I-6 | W sweep mean ± SD; LOSO with 3 seeds | Table I, Table VI |
| I-7, I-10 | Detection–false-alarm figure | Fig. 2 |
| I-8, I-11 | Floored/damped trend rule and refined grid | Tables II–III |
| I-12 | 15- and 60-min horizons vs. trend rule | Table IV |
| I-13 | Time in alarm | Sec. IV-C |
| I-14 | Required warning time | Table V |

## Results files

CSV outputs written to `RESULTS_DIR` (for example `val_tuned_threshold_results_v2.csv`, `paired_differences_fixed_FA.csv`, `level2_paired_differences.csv`, `loso_30min_seeds_named.csv`, `sweep_W_30min_mean_sd.csv`, `alarm_time_*`, `warning_time_*`) contain the numbers reported in the paper.

## Notes

- Thresholds for the primary 30-min comparison are tuned on validation data; the 15/60-min, time-in-alarm and warning-time analyses use thresholds matched on the test set for all models alike (see the paper's limitations).
- Run times and exact numbers can differ slightly with GPU, driver and library versions.

## Citation

If you use this code, please cite the paper (arXiv link to be added) and DiaData (Cinar & Maleshkova, 2025/2026).

## License

Code: MIT (see `LICENSE`). DiaData is distributed separately under CC BY-NC 4.0; do not redistribute it with this repository.
