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

Everything is already in place, including the notebook.

## Put it on GitHub (web browser, no command line)

1. Unzip this folder on your computer.
2. On github.com, click **New repository**, make it **Public**, and click **Create repository**.
3. Click **uploading an existing file**. Drag in everything inside the unzipped folder:
   `index.html`, `supplementary.html`, `README.md`, `.nojekyll`, and the `assets`, `notebooks`
   and `data` folders. Click **Commit changes**.
   `.nojekyll` is hidden: press Cmd+Shift+. in Finder on a Mac, or turn on View → Hidden items on Windows.
   If the browser struggles with one large upload, upload the folders in two or three batches.
4. Go to **Settings → Pages**, set **Source** to **Deploy from a branch**, branch **main**,
   folder **/ (root)**, and click **Save**.
5. The site goes live at `https://<username>.github.io/<repository>/` within a couple of minutes.

## Changing the notebook

If you replace the notebook with a file of a different name, update its path in `index.html`
(search for `MrBean_ISC_CorrCA_Paper.ipynb`; it appears in the Colab button and the Download link).
The Colab link is built automatically from the GitHub Pages address.

## Keeping it anonymous during review

* Your GitHub username appears in the site address and in the Colab links. Create the repository
  under an anonymous GitHub account, or share it through https://anonymous.4open.science.
* Check that the notebook and CSVs contain no names, file paths or metadata that identify you
  (for example `/Users/<name>/...` paths printed in notebook outputs, or author fields).

## Limits to keep in mind

* GitHub's web uploader takes files up to 25 MB each. The largest file here, the notebook, is about 5 MB.
* Changes take a minute or two to appear on the live site. Refresh with Ctrl+Shift+R
  (Cmd+Shift+R on a Mac) if you still see the old version.
