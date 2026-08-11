# Stage Contract: Experiment Generation & Sub-Experiment Execution (Layer 2)

Defines the decision criteria, directory creation standards, and workflow steps for scaffolding primary experiments and sub-experiment micro-iterations.

---

## 1. Decision Matrix: Primary Experiment vs. Sub-Experiment

The AI agent must evaluate the user's intent against this matrix to determine the correct level:

| Decision Factor | Create Primary Experiment (`EXP_XX`) | Create Sub-Experiment (`SUB_YY`) |
| :--- | :--- | :--- |
| **Core Objective** | Testing a new research paper, novel architecture, or major pipeline | Testing hyperparameter tuning, loss function swaps, or single ablations |
| **Codebase Scope** | Requires building new custom model classes or runner scripts | Reuses parent `models/` and `scripts/`, overriding only configs or minor params |
| **Model Architecture** | Structural changes to backbone, encoder, or multi-modal fusion | Tuning dropout rates, layer norm choices, or learning rate schedules |
| **Directory Location** | `experiments/EXP_XX_<name>/` | `experiments/EXP_XX_<name>/sub_experiments/SUB_YY_<name>/` |

---

## 2. Primary Experiment Creation Workflow (`EXP_XX`)

### Inputs
- **Layer 3 References:** `_config/code_conventions.md`, `_config/environment_setup.md`, `_config/execution_commands.md`
- **Layer 4 Working Artifact:** `papers/<paper_name>/summary.md`
- **Layer 3 Immutable Reference:** `/repos/<repo_name>/`

### Workflow Steps

1. **Auto-Increment Experiment ID:**
   - Inspect existing directories under `experiments/`.
   - Find the highest `EXP_XX` index and auto-increment (e.g. `EXP_01`, `EXP_02`).

2. **Scaffold Experiment Directory Structure:**
   ```text
   experiments/EXP_XX_<short_name>/
   ├── EXP_MANIFEST.md               # Core hypothesis, architecture, run logs
   ├── Run_Commands.md               # Local (macOS) & Kaggle CLI run manual
   ├── models/                       # Custom PyTorch models (importing from /repos/)
   ├── configs/                      # Yaml hyperparameters (base.yaml)
   ├── scripts/                      # Runner scripts (train.py, eval.py)
   ├── results/                      # Metrics, plots, log outputs
   └── sub_experiments/              # Micro-iterations folder
   ```

3. **Model & Codebase Setup:**
   - Implement model inside `experiments/EXP_XX_<name>/models/`.
   - Dynamically import clean reference modules from `/repos/<repo_name>/` using `sys.path.append()`. Do not edit `/repos/`.

4. **Config & Documentation Parity:**
   - Create `configs/base.yaml`, `scripts/train.py`, and `scripts/eval.py`.
   - Create and populate `EXP_MANIFEST.md` and `Run_Commands.md`.

---

## 3. Sub-Experiment Micro-Iteration Workflow (`SUB_YY`)

### Workflow Steps

1. **Auto-Increment Sub-ID:**
   - Inspect `experiments/EXP_XX_<name>/sub_experiments/`.
   - Auto-increment naming: `SUB_01_<variant_name>`, `SUB_02_<variant_name>`.

2. **Scaffold Sub-Experiment Folder:**
   ```text
   sub_experiments/SUB_YY_<variant_name>/
   ├── SUB_MANIFEST.md               # Micro-experiment objective & hypothesis
   └── diff_and_notes.md             # Parameter diffs & comparison results vs parent EXP
   ```

3. **Promotion Policy:**
   - If a sub-experiment's scope expands into major architectural changes, promote it to a new primary `EXP_XX` directory.
