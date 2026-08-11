# Experiment Manifest: [EXP_ID] — [Experiment Name]

---

## 1. Research Context & Core Hypothesis
- **Paper Reference:** [Link to `papers/<paper_name>/summary.md`](../../papers/<paper_name>/summary.md)
- **Repo Reference:** [Link to `/repos/<repo_name>/`](../../repos/<repo_name>/)
- **Primary Objective:** What core research concept or paper architecture are we testing?
- **Hypothesis:** What performance, loss, or capability outcome do we expect?

---

## 2. Architecture & Codebase Design
- **Custom Models (`models/`):** Summary of architecture implemented in `models/model.py`.
- **Imported Reference Modules:** Modules imported directly from `/repos/<repo_name>/`.
- **Loss Formulation:** Mathematical loss function used for training.
- **Shared Utilities Used:** List modules imported from `/shared/`.

---

## 3. Configuration & Parameters
- **Active Config File:** [`configs/base.yaml`](configs/base.yaml)
- **Dataset Used:** Dataset location (local `./data/` vs Kaggle `/kaggle/input/`).

---

## 4. Run History Log

| Run ID | Date | Device / Hardware | Config File | Key Results / Metrics | Notes / Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `Run_01` | YYYY-MM-DD | Local (macOS MPS) / Kaggle (CUDA) | `configs/base.yaml` | Val Loss: X.XX, Acc: XX.X% | Initial baseline run |
| `Run_02` | YYYY-MM-DD | Kaggle (Dual T4 DDP) | `configs/base.yaml` | Val Loss: Y.YY, Acc: YY.Y% | Full epoch training |
