# Predicting Shared Neural Responses to Cinematic Content

Downloads and supplementary material for a paper currently under review.

## Folder layout

```
index.html          main page: Colab notebook, data files, technologies used
supplementary.html  supplementary figures S1–S17 and tables S1–S4
.nojekyll           makes GitHub Pages serve files as they are (keep it)
assets/fig/         images for the supplementary page
notebooks/          the Colab notebook (MrBean_ISC_CorrCA_Paper.ipynb)
data/               the 22 result CSVs and run_info.json
```

What you need: Google Drive and Colab, the Zenodo EEG data, and the Mr. Bean video, which the reader has to obtain themselves. A GPU is optional and only speeds up the CLIP and DINOv2 steps.
Preparing the data: the folder layout the notebook expects, how it picks up the EEG files and bad channels, and the two video versions it accepts. These are the full upload, where the stimulus starts at 39.77 s, or a copy already trimmed to that start.
Setting paths and running: the three Drive paths to edit in §0, "Run all", restarting the session after the first package install, and how caching lets a disconnected run carry on.
Pipeline: a section-by-section table of the notebook, §0–13.
Outputs: what the notebook writes to the output folder.
Checking a run against the paper: key numbers from your CSVs to compare against, plus the exact package versions from run_info.json, with a one-line pip command to pin them.
Changing the analysis: the main settings and their defaults, then a short data-use note and the dataset citation.

