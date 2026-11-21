# GitBook setup — use Git Sync (recommended)

The ZIP file-import flow can import Markdown files as individual pages, but it does not reliably apply this book's `SUMMARY.md` navigation in the way this project requires. The previous import may therefore show a flat list of pages and `SUMMARY.md` as a normal page.

For the intended navigation, connect this folder to GitBook through **Git Sync**. The `.gitbook.yaml` file explicitly tells GitBook to use `README.md` as the first page and `SUMMARY.md` as the table of contents.

## 1. Create a dedicated GitHub repository

Create a repository such as `xauusd-h1-research-book`. Keep the repository separate from the EA source repository. The repository should contain the contents of this folder at its root:

- `.gitbook.yaml`
- `README.md`
- `SUMMARY.md`
- `chapters/`
- `assets/`
- `notebooks/`
- `outputs/`

Do not upload the outer ZIP file as a file inside the GitHub repository. Extract it first and push the files/folders inside it.

## 2. Push the files to GitHub

From Terminal, inside the extracted `XAUUSD_H1_Research_GitBook` folder:

```bash
git init
git branch -M main
git add .
git commit -m "Add XAUUSD H1 research documentation"
git remote add origin https://github.com/YOUR_USERNAME/xauusd-h1-research-book.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username. Create the GitHub repository first, preferably empty.

## 3. Connect GitBook using Git Sync

1. Open the GitBook Docs site.
2. Create a new space for this book, or select the target space you want to synchronize.
3. Open **Git Sync** / **Set up Git Sync** in the space or site sidebar.
4. Connect GitHub and authorize the GitBook app for this repository.
5. Select `xauusd-h1-research-book`, branch `main`, and repository root `/`.
6. Set the initial sync direction to **Repository → GitBook**.
7. Start the sync.

GitBook's current configuration supports `.gitbook.yaml` for the first page and navigation file, and `SUMMARY.md` for the space table of contents.

## 4. Verify the navigation

The left navigation should show these groups in this order:

1. Welcome
2. Research Foundations
   - 01 — Research scope and data
   - 02 — N-Method rule definitions
   - 03 — Baseline EMA/ADX regime classifier
3. Notebook Experiments
   - 04 — Notebook 01: N-Method backtest
   - 05 — Notebook 02: Regime performance
   - 06 — Notebook 03: HMM regime detection
   - 07 — Notebook 04: Swing-window sensitivity
   - 08 — Notebook 05: Position capacity and account simulation
4. Research Synthesis
   - 09 — Results, decisions, and caveats
   - 10 — Validation and next steps
5. Reference
   - 11 — Glossary
   - 12 — Reproducing the experiments

The `chapters/` directory is a source-file folder, not a navigation group label. The navigation groups are defined in `SUMMARY.md`.

## 5. Replace the earlier ZIP import only after verifying Git Sync

Keep the earlier file-imported pages until the Git-synced space is visible and correct. After verifying the new synchronized space, remove or archive the earlier flat import to avoid duplicate content.

## 6. Preview and publish

Check the charts, relative image paths, notebook links, and CSV links in Preview. Publish only after verifying that files exposed publicly are licensed for redistribution.

## Important data note

The raw XAUUSD OHLCV CSV is intentionally excluded because its source vendor and redistribution permission have not been established. Do not add the raw dataset to a public repository unless its licence permits redistribution.
