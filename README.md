# GVLiD: Robust Grapevine Disease Recognition (Project P01): code

Advanced Topics in Information Technology, Hanoi University. Midterm baselines and Final RQ1 (IID versus metadata-aware OOD split).

## 1. Install

Google Colab (free tier, T4 GPU) already has everything. Locally: Python 3.11+ and `pip install -r requirements.txt` (a CUDA GPU is recommended).

## 2. Data

GVLiD dataset, Mendeley Data, DOI 10.17632/wkymf8bhcg.5 (https://data.mendeley.com/datasets/wkymf8bhcg/5); descriptor paper DOI 10.1038/s41597-026-07431-9. Licence: CC BY 4.0 (as stated on the Mendeley Data page, version 5, published 6 April 2026). The raw images are **not** included in this submission.
Download the images, keep the class folders (`healthy`, `black rot`, `esca`, `leaf blight`) and `metadata.json` together, and set `DATA_ROOT` in the first code cell of each notebook (the Colab default is `/content/data/A`).

## 3. Order of the notebooks

| Order | Notebook | Reproduces |
|---|---|---|
| 1 | `notebooks/01_midterm_baselines_dedup.ipynb` | Dataset analysis, near-duplicate audit, the IID split (`splits/iid_split_folder_seed42.csv`), the four baselines (training set de-duplicated, class-weighted loss), IID confusion matrices, shortcut probes. Report: Table 2 (split statistics), Table 6 (baselines), Figure 1 (class distribution), Figure 2 (Midterm confusion matrices). Midterm tables: `results/tables/midterm/`. |
| 2 | `notebooks/02_final_rq1_ood.ipynb` | RQ1: EXIF camera-model domain split, IID control, the four models x 3 seeds, delta F1, rankings, per-class degradation, confusion matrices, shortcut analysis, failure audit, prediction file. Report: Table 2 (domain rows), Table 3 (`domain_summary.csv`, `domain_lighting.csv`), Table 5 (`split_summary_rq1.csv`), Table 7 (`rq1_main.csv`), Table 8 (`rq1_per_class_delta.csv`), Table 9 (`shortcut_border_vs_delta.csv`), Figure 3 (`fig_rq1_f1_and_delta.png`), Figure 4 (`fig_rq1_confusion_iid_ood.png`), Figure 5 (`fig_ood_failure_gallery.png`). Table 4 is `configs/rq1_config.json`. |

The notebook cells that set up Colab (unzipping the dataset) use the authors' Google Drive paths; edit `DATA_ROOT`, `REPO` and `WORK` in the first code cells. The executed copy in this folder keeps its outputs. Its last cell was edited after the run (the results zip skips Google Drive shortcut files); the rest is as executed.

Notebook 2 only needs `splits/iid_split_folder_seed42.csv` and `configs/rq1_config.json` from this folder. Set `REPO` to the `code/` folder (for example on Google Drive). It resumes automatically if the Colab session stops.

## 4. Splits (`splits/`)

* `iid_split_folder_seed42.csv`: Midterm IID split (train / val / test, stratified group split by duplicate cluster, seed 42). Kept unchanged as the IID control baseline.
* `rq1_domain_assignment.csv`: label, duplicate cluster, EXIF camera model and metadata for the RQ1 pool.
* `rq1_<iid|ood_A2B|ood_B2A>_seed<42|43|44>.csv`: sample ids of train / val / test for every RQ1 protocol and seed.

## 5. Configuration

All hyper-parameters, seeds and split settings are in `configs/rq1_config.json`. Midterm hyper-parameters are stored in `notebooks/01_...` (cell "Models and training protocol") and in the Midterm column of the report (Table 4.2).

## 6. Results (`results/`)

* `tables/`: CSV tables used in the report (`rq1_main.csv`, `rq1_per_class_delta.csv`, `rq1_runs.csv`, `domain_summary.csv`, `shortcut_*.csv`, `failure_audit_filled.csv`, `midterm/`).
* `figures/`: figures used in the report.
* `predictions/rq1_predictions.csv`: one row per sample, model, seed and protocol. Required columns first: `sample_id, true_label, pred_label, prob_healthy, prob_black_rot, prob_esca, prob_leaf_blight, domain_device, domain_lighting`; then `domain_weather, filename, model, seed, protocol, split`. RQ1 is a 2-class task (Black Rot vs Leaf Blight), so `prob_healthy` and `prob_esca` are 0 by construction. `domain_device` is the EXIF camera model. Probabilities are the raw softmax outputs, uncalibrated.
* `environment.md`: GPU, library versions, seeds (written by notebook 2).

## 7. Reproducibility notes

* Seeds 42, 43, 44; all splits persisted; the test set or test domain is never used for model selection (early stopping on validation macro-F1).
* Pretrained checkpoints: torchvision `IMAGENET1K_V1` weights (ResNet-18, MobileNetV3-Small, ConvNeXt-Tiny).
* The failure audit (`results/tables/failure_audit_filled.csv`, `results/figures/fig_ood_failure_gallery.png`) lists 30 cases of the OOD test domain with the cause the group read from each image (`manual_cause`, `manual_notes`); 22 of them are copies of one scene.
* GitHub repository: [TO BE ADDED BY THE GROUP]
