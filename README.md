# VWCS Focal LightGBM reproducibility materials

This repository accompanies *Predicting Emergency Management Vulnerabilities in Social Welfare Institutions: A Vulnerability-Weighted Cost-Sensitive Focal Loss Approach*. It contains two archives: one for reproducing reported statistics from saved predictions, and a separate experiment comparing three models exposed to the same measurement noise. The benchmark endpoint is a facility-level regulatory penalty, not a patient outcome or an observed emergency.

| Archive | What it contains | What you can verify |
| --- | --- | --- |
| `VWCS_Reproducibility_Package_1.0.4.8.zip` | CMS source inventory and selected inputs, the analysis matrix and splits, archived facility-level scores, revision analyses, source snapshots, and a map to the manuscript tables | Recompute metrics from archived predictions. The reference replay covers 44 configurations in 8 panels, with 501 checks. |
| `VWCS_Shared_Noise_Control.zip` | Protocol, a no-added-noise reference, three shared-noise draws, fitting code, models, predictions and results | Recompute results for 12 model–scenario combinations from 17,640 prediction rows. This experiment is distinct from Supplementary Table S17. |

## Reproduce the checks

Download both ZIPs and `SHA256SUMS.txt` to the same directory. On Windows, compare the output of `Get-FileHash -Algorithm SHA256 -Path *.zip` with the two values in the checksum file. On Linux, run `sha256sum -c SHA256SUMS.txt`. Use a new, empty extraction directory for each archive; no package installation or model fitting is required for either verification script. Python 3.10 or later is recommended.

1. Extract the core ZIP. Enter the inner `VWCS_Reproducibility_Package_1.0.4.8` directory containing `replay_archived_metrics.py`. Run `python replay_archived_metrics.py --output-dir ../vwcs_replay_check`. The output includes `verification_summary.json`; the reference run passed all 501 checks.
2. Extract the shared-noise ZIP into a different directory. From the directory containing `verify_results.py`, run `python verify_results.py`. The script checks the local manifest and compares the recomputed metrics, confusion counts and fixed-budget results with the saved outputs.
3. For table-specific analyses, start with `table_map.csv` and `README.txt` in the core archive. Read the separate README in the shared-noise archive for the noise protocol.

The core replay verifies results conditional on saved predictions. It does not retrain the historical VWCS model: some historical loss settings and facility-specific weights were not retained. The source snapshots preserve original paths; do not run historical training or data-preparation scripts against the original data directory without checking their paths and outputs.

## Versions and attribution

The repository snapshot is designated **1.0.0**; the accompanying manuscript is **1.0.5.6**; the frozen core archive keeps its own version **1.0.4.8**. Use `CITATION.cff` for authorship and citation metadata. See `DATA_AND_RIGHTS.md` for CMS provenance, the distinction between observed and constructed variables, and the current licensing status.
