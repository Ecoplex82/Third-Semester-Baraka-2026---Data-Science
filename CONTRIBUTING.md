# 🤝 Contributing & Student Submission Guidelines

Welcome **Baraka 2026 Cohort** students! This guide defines the repository conventions, Data Science & Machine Learning quality standards, and Git Pull Request workflow required when submitting live-session labs, assignments, or practical extensions to this repository.

---

## 1. Directory Isolation & File Naming Rules

> [!IMPORTANT]
> **Never place student files in the repository root or modify shared root files** (`README.md`, `requirements.txt`, `.gitignore`, root `.csv` datasets, or instructor `.ipynb` notebooks). Root-level edits create merge conflicts across the cohort and risk breaking shared dependencies.

Place **all** of your work inside a dedicated, lowercase `kebab-case` folder under `submissions/`:

```text
submissions/
└── <first-last>/                                   # e.g., submissions/jane-doe/
    ├── README.md                                   # Summary of your workflow, visuals & findings
    ├── session_01_ml_visualization.ipynb           # Clean snake_case notebook filenames
    ├── session_02_business_value_viz.ipynb         # No spaces or browser suffixes like "(1)"
    └── images/                                     # All exported PNG/JPEG figures referenced in your README
        └── 01_correlation_heatmap.png
```

### File Naming Checklist
- **Folder name:** `submissions/<first-last>/` (lowercase with hyphens, e.g., `submissions/eberechi-donalds/`).
- **Notebooks:** Use `snake_case.ipynb` (avoid spaces, ALL CAPS, or download counters such as `Project (1).ipynb`).
- **Custom Dependencies *(Optional)*:** Only add a `submissions/<first-last>/requirements.txt` if your notebook requires a package not already listed in the root [`requirements.txt`](requirements.txt).

---

## 2. Reproducible Data Science & ML Notebook Standards

Before committing any `.ipynb` file, verify all five reproducibility and methodological checks:

1. **Portable Relative Paths Only (Zero Machine-Specific Paths):**
   Never commit hardcoded local paths (`C:\Users\...` or `/Users/...`). Reference shared root datasets using relative paths from your `submissions/<first-last>/` directory (with an optional Google Colab fallback if needed):
   ```python
   import os
   import pandas as pd

   data_path = (
       "../../us_education_system_dataset.csv"
       if os.path.exists("../../us_education_system_dataset.csv")
       else "us_education_system_dataset.csv"
   )
   df = pd.read_csv(data_path)
   ```
2. **Clean-Room Execution (*Restart Kernel & Run All Cells*):**
   - Run **Restart Kernel and Run All Cells** right before committing so cell execution counts run sequentially (`[1]`, `[2]`, `[3]`, ...) with **zero errors**.
   - Delete blank scratch cells, commented-out dead code blocks, and duplicated setup cells.
   - Structure narrative sections using proper Markdown headings (`#`, `##`, `###`).
3. **Domain-Aware Missingness & Grouping Logic:**
   - Inspect *why* values are missing before imputing. Do not impute structurally inapplicable metrics across incompatible cohorts (e.g., SAT/ACT or AP/IB participation onto Elementary or Middle schools, or raw `999.9` / `9999.9` NOAA sensor sentinels without converting sentinels to `NaN` first).
   - Avoid geographic name collisions when aggregating (e.g., group U.S. counties by `["state_code", "county"]`, not `"county"` alone).
4. **Prevent Data Leakage in ML Pipelines:**
   - Exclude target-leaking or contemporaneous co-outcome variables unless explicitly auditing collinearity.
   - Fit preprocessing transformers (`SimpleImputer`, `StandardScaler`, `OneHotEncoder`, `PCA`) on **`X_train` only**, then call `.transform(X_test)`.
5. **Statistical & Visual Clarity:**
   - Separate observational associations ($r$, $r^2$) from causal claims in your executive interpretations.
   - Use diverging colormaps centered at zero (e.g., `cmap="coolwarm", vmin=-1, vmax=1`) for signed correlation matrices.
   - Commit every image referenced in your Markdown so zero broken image links (`404`) appear on GitHub.

---

## 3. Standard Git CLI Pull Request Workflow

Avoid uploading files via browser drag-and-drop (*"Add files via upload"*). Practice the standard industry Git CLI workflow:

```bash
# 1. Fork this repository on GitHub, then clone your fork locally
git clone https://github.com/<your-github-username>/Third-Semester-Baraka-2026---Data-Science.git
cd Third-Semester-Baraka-2026---Data-Science

# 2. Register the main cohort repository as 'upstream' and sync latest changes
git remote add upstream https://github.com/iPablo26/Third-Semester-Baraka-2026---Data-Science.git
git fetch upstream
git checkout main
git merge upstream/main

# 3. Create a descriptive feature branch for your submission
git checkout -b feat/<first-last>-session-03

# 4. Add your work inside submissions/<first-last>/ and inspect staged changes
git status
git add submissions/<first-last>/
git diff --cached --stat

# 5. Commit using a clear Conventional Commit message
git commit -m "feat(submissions): add <First Last> Live Session 3 ML workflow"

# 6. Push your branch to your fork and open a Pull Request against iPablo26:main
git push -u origin feat/<first-last>-session-03
```

> 💡 **Updating an Existing Pull Request:** If changes are requested during review, **do not open a new PR**. Simply make the fixes locally on the *same* branch, commit, and `git push`—your open PR updates automatically.

---

## 4. Pre-Submission Self-Review Checklist

Immediately after opening your Pull Request on GitHub, click the **Files changed** tab and confirm:

- [ ] All added/modified files live strictly inside `submissions/<first-last>/`.
- [ ] Root `requirements.txt`, root `README.md`, and instructor notebooks/datasets are **untouched**.
- [ ] No hardcoded local paths (`C:\Users\...` or `/Users/...`), `.DS_Store`, or `.ipynb_checkpoints` appear in the diff.
- [ ] All notebooks were run via **Restart Kernel & Run All** with sequential `[1..N]` execution counts and zero errors.
- [ ] Every image linked in `submissions/<first-last>/README.md` is committed and renders properly on GitHub.
