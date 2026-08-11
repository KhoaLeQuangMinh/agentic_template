# Master Workspace Router (Layer 1)

## Purpose & Navigation Protocol
When starting a task, inspect the user's intent below and follow the designated stage contract or reference guide:

---

### 1. Intake & Extract a Research Paper
* **Target Stage:** Literature Processing
* **Stage Contract:** Read `papers/CONTEXT.md`
* **Action:**
  1. **Paper Sourcing & Inbox Drop-Zone:**
     - **Inbox Drop:** If a PDF/text file is placed in `papers/inbox/` (or provided directly), open and read the paper.
     - **Online / arXiv ID / DOI:** If given an arXiv ID or web link, download the PDF into memory/temp storage.
  2. **Auto-Naming & Folder Setup:**
     - Analyze the paper's title/model name to generate a concise, standardized folder name (e.g., `papers/2024_mamba_ssm/` or `papers/swin_transformer/`).
     - Create `papers/<concise_paper_name>/` and save/move `paper.pdf` into it.
  3. **Lossless Technical Extraction:**
     - Extract math formulas, loss structures, tensor dimensions, hyperparameters, and architectural claims using `papers/templates/extraction_prompt.md`.
     - Save structured notes to `papers/<concise_paper_name>/summary.md`.

---

### 2. Create or Execute a Main Experiment
* **Target Stage:** Experiment Generation & Execution
* **Stage Contract:** Read `experiments/CONTEXT.md`
* **Action:**
  1. Determine next ID (`EXP_01`, `EXP_02`, etc.) and scaffold `experiments/EXP_XX_<short_name>/`.
  2. Build custom model architectures inside `experiments/EXP_XX_<short_name>/models/` by dynamically importing clean reference code from `/repos/` without modifying `/repos/`.
  3. Write experiment-specific configs (`experiments/EXP_XX_<short_name>/configs/base.yaml`) and runner scripts (`experiments/EXP_XX_<short_name>/scripts/train.py`, `eval.py`).
  4. Create and populate `experiments/EXP_XX_<short_name>/EXP_MANIFEST.md` and `Run_Commands.md`.

---

### 3. Run a Micro-Iteration / Parameter Shift (Sub-Experiment)
* **Target Stage:** Sub-Experiment Iteration
* **Stage Contract:** Read `experiments/CONTEXT.md` (Sub-Experiment Section)
* **Action:**
  1. Scaffold `experiments/EXP_XX_<name>/sub_experiments/SUB_YY_<variant_name>/`.
  2. Modify hyperparameter configs or apply minor code adjustments.
  3. Log parameter differences and metrics comparison against parent run in `diff_and_notes.md`.

---

### 4. Lookup CLI Commands, Environment Specs, or Code Standards
* **Target Stage:** Reference Specs Lookup
* **Action:**
  - Shell/Kaggle CLI flags: Read `_config/execution_commands.md`
  - Environment specs & GPU paths: Read `_config/environment_setup.md`
  - PyTorch coding & import rules: Read `_config/code_conventions.md`

---

### 5. Access Shared Infrastructure & Common Utilities
* **Target Stage:** Shared Infrastructure
* **Action:**
  - Common data splitters, metric calculations, and logger wrappers: Read `/shared/`
