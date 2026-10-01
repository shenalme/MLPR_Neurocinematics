# Predicting Shared Neural Responses to Cinematic Content

Code and results for a paper currently under review. This guide explains how to reproduce the
experiment: predicting, window by window, whether a viewer's EEG is highly or weakly synchronised
with the rest of the audience (inter-subject correlation, ISC) from features of the film itself.

## Contents of this repository

| Path | What it is |
|---|---|
| `notebooks/MrBean_ISC_CorrCA_Paper.ipynb` | The full analysis pipeline as a Google Colab notebook |
| `data/*.csv` | Results of the run reported in the paper, for comparison with your own run |
| `data/run_info.json` | Exact configuration and software versions of that run |
| `index.html`, `supplementary.html`, `assets/` | Project web page and supplementary figures |

## What you need

* A Google account with **Google Drive** and **Google Colab**. The notebook mounts Drive, reads the
  data from it, and writes all outputs and caches back to it.
* The **EEG data**: the Mr. Bean subset of the KU Leuven Video-EEG Encoding–Decoding dataset
  (Zenodo, [doi:10.5281/zenodo.10512414](https://doi.org/10.5281/zenodo.10512414)).
* The **stimulus video**: the first episode of *Mr. Bean*, as described in the dataset documentation.
  The video is not distributed with this repository; obtain it yourself.
* A GPU runtime is optional. It only speeds up the CLIP and DINOv2 embeddings; everything else runs on CPU.

## Step 1. Prepare the data on Google Drive

**EEG.** Download the Mr. Bean recordings from Zenodo and place them in one folder on Drive
(`EEG_DIR`). The notebook expects:

* one sub-folder per participant whose name ends in the participant number (`...01` to `...09`);
* an EEGLAB `.set` file in each sub-folder (if there are several, a file whose name contains `mr`
  is preferred), with its companion `.fdt` file if there is one;
* optionally, the dataset's README `.txt` file in each sub-folder. Bad channels are read from lines
  that mention *bad*, *noisy* or *interpolated*. If none are found, the fallback list in the
  configuration (`FALLBACK_BAD_CHANNELS`) is used.

**Video.** Place the video in `VIDEO_DIR` (`.mp4`, `.mkv`, `.avi`, `.mov` or `.webm`). If the folder
holds several videos, the largest file with *bean* in its name is used; set `VIDEO_FILE` to choose a
file explicitly. Two versions are accepted and detected automatically (`SYNC_MODE = "auto"`):

* the **full upload** (about 1,510 s long), in which the presented stimulus runs from **39.77 s** to
  1,495.00 s, as documented for the dataset;
* a file **already trimmed** so that it starts at stimulus onset.

## Step 2. Set the paths

Open the notebook in Colab (**Open in Colab** on the project page, or **File → Upload notebook**)
and edit the first code cell, **§0 Configuration**:

```python
"VIDEO_DIR": "/content/drive/MyDrive/<folder with the video>",
"EEG_DIR":   "/content/drive/MyDrive/<folder with the participant sub-folders>",
"OUT_DIR":   "/content/drive/MyDrive/<folder for outputs and caches>",
```

Leave every other setting unchanged to reproduce the reported run.

## Step 3. Run

Choose **Runtime → Run all** and allow Colab to access your Drive when asked.

* The setup cell installs missing packages (MNE-Python, XGBoost, SHAP, scikit-image; CLIP is
  installed from GitHub in §6c). If an import error follows the installation, choose
  **Runtime → Restart session** and run all again.
* Expensive steps are **cached in `OUT_DIR`**: preprocessed EEG, video and audio features,
  embeddings, and the results of every model configuration. If the session disconnects, run all
  again and the notebook continues from the cache. Delete `OUT_DIR` (or one of its sub-folders)
  to recompute from scratch.
* At the end, the notebook writes a summary report and downloads a ZIP with all results and figures.

### The pipeline, by notebook section

| § | Step |
|---|---|
| 0–1 | Configuration, package setup, Drive mount, figure style |
| 2 | Data inventory: video, one `.set` file per participant, bad channels |
| 3 | Video–EEG alignment and 363 non-overlapping 4-s windows |
| 4 | EEG preprocessing following the dataset authors: 10–20 channel names, bad-channel interpolation, average reference, 0.5-Hz high-pass, 50-Hz notch, resampling to 30 Hz, EOG regression |
| 5 | Channel-average ISC per participant and window |
| 6 | Video and audio features (visual, shot timing, temporal context, audio), CLIP and DINOv2 embeddings, feature visualisations |
| 7 | Shot detection and the leakage-controlled train / validation / test split |
| 8 | CorrCA ISC (filters fitted on training windows only) and the three prediction targets |
| 9 | 150 configurations (15 feature-block combinations × raw/PCA × 5 models), leave-one-subject-out evaluation, target comparison, stimulus–response lag |
| 10 | Diagnostics of the top configurations |
| 11 | Feature-group ablation |
| 12 | SHAP, permutation and block importance |
| 13 | Circular-shift null test, summary report and export |

## Outputs

All outputs go to `OUT_DIR`. Folder names carry the window length, so runs with a different
`WINDOW_S` never overwrite each other.

| Folder | Content |
|---|---|
| `eeg/` | Preprocessed EEG per participant (`P01_30Hz_raw.fif`, …) |
| `features/` | Cached video, audio, CLIP and DINOv2 features |
| `results_w4s_v3/` | All result tables (CSV), `results_tables.xlsx`, `summary_report.md`, `run_info.json` |
| `figures_w4s_v3/` | Every figure as a 600-dpi PNG and a vector PDF |
| `models_w4s_v3/` | Fitted models |
| `export/` | ZIP of results and figures |

## Checking your run against the paper

The CSV files in `data/` are the results of the reported run. They have the same names as the files
your run writes to `results_w4s_v3/`. Key values to compare:

| Quantity | Reported run | File |
|---|---|---|
| Windows per participant | 363 | `split_assignment.csv` |
| Windows in train / validation / test / buffer | 218 / 56 / 61 / 28 | `split_assignment.csv` |
| Split-half reliability of the audience ISC (median r), channel average / CorrCA | 0.653 / 0.621 | `isc_reliability_split_half.csv` |
| LOSO, unseen film, channel-average target: random forest, Visual, raw | 0.596 ± 0.046 balanced accuracy, 9/9 viewers above chance | `loso_summary.csv` |
| LOSO, unseen film, channel-average target: random forest, Visual + Audio, raw (primary) | 0.578 ± 0.058 balanced accuracy, 8/9 viewers above chance | `loso_summary.csv` |

The random seed is fixed at 42. Small numerical differences can still arise from different package
versions, from GPU arithmetic in the embedding models, and from t-SNE (which affects only a figure).
The reported run used:

| Package | Version |
|---|---|
| MNE-Python | 1.13.2 |
| scikit-learn | 1.6.1 |
| XGBoost | 3.4.1 |
| SHAP | 0.52.0 |
| OpenCV | 4.14.0 |
| CLIP | ViT-B/32, installed from github.com/openai/CLIP |
| DINOv2 | `facebook/dinov2-small` via Hugging Face Transformers |

To match these exactly, pin them in a cell before the setup cell, for example
`!pip install mne==1.13.2 scikit-learn==1.6.1 xgboost==3.4.1 shap==0.52.0`, then restart the session.

## Changing the analysis

All choices are in **§0 Configuration** and are saved with the results in `run_info.json`.
The settings most often changed:

| Setting | Default | Effect |
|---|---|---|
| `WINDOW_S` | 4.0 | Window length in seconds; outputs go to separate folders |
| `PRIMARY_TARGET` | `"audience"` | Target for §9–13: `audience`, `individual` or `individual_avg` |
| `CORRCA_K` | 3 | Number of correlated components used for ISC |
| `N_BLOCKS`, `SPLIT_FRACTIONS` | 10, [0.6, 0.2, 0.2] | Structure of the chronological split |
| `BUFFER_WINDOWS` | 1 | Windows dropped at every split boundary |
| `MODELS` | all five | Classifiers evaluated |
| `N_BOOT`, `N_NULL` | 1000, 30 | Bootstrap resamples and null-test shifts |
| `SEED` | 42 | Random seed |

## Data use

The EEG data are provided by the dataset authors under the terms stated on Zenodo. Please cite them
if you use the data:

Yao, Y., Stebner, A., Tuytelaars, T., et al.: 'Video-EEG encoding-decoding dataset KU Leuven'
(Zenodo, 2024), doi: 10.5281/zenodo.10512414

The stimulus video is copyrighted and is not included here.
