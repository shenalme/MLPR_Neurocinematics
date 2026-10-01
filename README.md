# Predicting Shared Neural Responses to Cinematic Content

Downloads and supplementary material for a paper currently under review.

## Folder layout


```
index.html          main page: downloads, dataset, technologies used
supplementary.html  supplementary figures S1–S17 and tables S1–S4
.nojekyll           makes GitHub Pages serve files as they are (keep it)
assets/fig/         supplementary figures S1–S17 (already included)
notebooks/          put your .ipynb file(s) here
data/               put your .csv files here
```

## Put it on GitHub (web browser, no command line)

1. Unzip this folder on your computer.
2. On github.com, click **New repository**. Give it a name (for example `shared-neural-responses`),
   set it to **Public**, and click **Create repository**.
3. On the empty repository page, click **uploading an existing file**.
4. Drag in **everything inside** the unzipped folder: `index.html`, `supplementary.html`, `README.md`, `.nojekyll`
   and the `assets`, `notebooks` and `data` folders. Drag the folders themselves so the structure is kept.
   `.nojekyll` is a hidden file; on a Mac press Cmd+Shift+. in Finder to see it, on Windows turn on
   View → Hidden items.
5. Click **Commit changes**.
6. Upload your files into the right folders: open the `notebooks` folder on GitHub, click
   **Add file → Upload files**, drop in your notebook, commit. Do the same for your CSVs in `data`.
7. Go to **Settings → Pages**. Under **Build and deployment**, set **Source** to
   **Deploy from a branch**, branch **main**, folder **/ (root)**, and click **Save**.
8. After a minute or two the page is live at `https://<your-username>.github.io/<repository-name>/`.

## Match the download list to your file names

The page lists files from a settings block in `index.html`. To edit it on GitHub, open `index.html`,
click the pencil icon, and search (Ctrl+F / Cmd+F) for `SITE CONFIGURATION`.

* Each entry has a `title`, a `path` and a `description`. The `path` must match the file's
  location exactly, including capital letters, for example `data/isc_windows.csv`.
* Add an entry by copying an existing `{ ... },` line; remove one by deleting its line.
* The **Open in Colab** button is created automatically from your GitHub Pages address.
  Only fill in `github.user` and `github.repo` if you use a custom domain, and change
  `branch` if your branch is not called `main`.
* Files not uploaded yet are marked "Not uploaded yet" and their buttons are disabled.

Default paths the page expects:

| Path | Content |
|---|---|
| `notebooks/predicting_shared_neural_responses.ipynb` | full analysis pipeline |
| `data/isc_windows.csv` | window-wise ISC per participant and HIGH/LOW labels |
| `data/features_lowlevel.csv` | 170 low-level features per window |
| `data/shots.csv` | detected cuts and shots |
| `data/split.csv` | train / validation / test / buffer assignment |
| `data/results_loso.csv` | LOSO results (P1, P2) |
| `data/results_config_search.csv` | 150-configuration search (P4) |

## Keeping it anonymous during review

* Your GitHub username appears in the site address and in the Colab links. Create the repository
  under an anonymous GitHub account, or share it through https://anonymous.4open.science.
* Check that the notebook and CSVs contain no names, file paths or metadata that identify you
  (for example `/Users/<name>/...` paths printed in notebook outputs, or author fields).

## Limits to keep in mind

* GitHub's web uploader takes files up to 25 MB each; files over 100 MB are refused entirely.
  Put very large data on Zenodo or similar and add it to the `datasets` list in the settings block
  as a link instead.
* Changes take a minute or two to appear on the live site. Refresh with Ctrl+Shift+R
  (Cmd+Shift+R on a Mac) if you still see the old version.
