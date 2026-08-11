# 🤖 AI Research Workspace — 5-Layer Context Hierarchy

Welcome to your AI Research Workspace! This environment translates academic research papers into reproducible PyTorch experiments while keeping reference materials pristine and experiment logs completely structured.

---

## 📁 Workspace Layout

```text
Folder_Agentic_AI/
├── GEMINI.md                                  # Layer 0: Global Identity & Rules
├── CONTEXT.md                                 # Layer 1: Workspace Master Router
├── README.md                                  # Workspace Guide & User Manual
├── _config/                                   # Layer 3: System Execution & Coding Rules
│   ├── execution_commands.md                  # Local (macOS MPS) & Kaggle (CUDA) Commands
│   ├── environment_setup.md                   # Path Fallbacks & Kaggle Dataset Registry
│   └── code_conventions.md                    # PyTorch Standards & Clean Repo Cloning
├── papers/                                    # Literature Knowledge Base
│   ├── inbox/                                 # 📥 DROP-ZONE for new paper PDFs
│   └── CONTEXT.md                             # Layer 2: Paper Intake Stage Contract
├── repos/                                     # Layer 3: Immutable Cloned Repositories
├── shared/                                    # Layer 3: Common Metrics, Loggers & Data Utilities
├── data/                                      # Local raw datasets (ignored by git)
├── checkpoints/                               # Local model weights (ignored by git)
└── experiments/                               # Layer 4: Experiment Sandbox
    └── CONTEXT.md                             # Layer 2: Experiment Generation Stage Contract
```

---

## 🗣️ How to Command the AI Agent

Simply instruct the AI agent using natural language prompts. Here are standard commands for each workflow stage:

### 1. Ingesting & Summarizing a Research Paper
* **Step 1:** Drop a PDF or text file into `papers/inbox/paper.pdf` (or paste an arXiv link / DOI).
* **Command to AI:**
  > *"Read the paper in inbox, create a folder for it, extract all math and parameters, and check if it has a code repo to clone."*
* **What happens:** The AI creates `papers/<concise_paper_name>/`, moves the PDF inside, generates `summary.md`, and shallow-clones the repo into `/repos/<repo_name>/` (removing `.git`).

### 2. Creating a New Primary Experiment (`EXP_XX`)
* **Command to AI:**
  > *"Create a new primary experiment EXP_01 based on `papers/2024_mamba/summary.md` to test the core attention layer."*
* **What happens:** The AI auto-scaffolds `experiments/EXP_01_<name>/`, builds custom PyTorch models in `models/` by dynamically importing clean reference code from `/repos/`, sets up `configs/base.yaml`, `scripts/train.py`, `EXP_MANIFEST.md`, and `Run_Commands.md`.

### 3. Running a Micro-Iteration / Sub-Experiment (`SUB_YY`)
* **Command to AI:**
  > *"Run a sub-experiment on EXP_01 testing learning rate 1e-4 vs 1e-3 and Focal Loss instead of BCE."*
* **What happens:** The AI scaffolds `experiments/EXP_01_<name>/sub_experiments/SUB_01_<variant_name>/`, writes parameter diffs, and logs comparative metric tables in `diff_and_notes.md`.

---

## 📊 Where to Find Results & Metrics

| What You Are Looking For | Directory / File Location | Description |
| :--- | :--- | :--- |
| **Extracted Paper Summaries** | `papers/<paper_name>/summary.md` | Lossless LaTeX math, loss formulas, tensor dimensions & claims |
| **Experiment Hypotheses & Run Logs** | `experiments/EXP_XX/EXP_MANIFEST.md` | Core objective, model structure, and history table of all runs |
| **Execution Manual (Local & Kaggle)** | `experiments/EXP_XX/Run_Commands.md` | Copy-pasteable shell commands for macOS MPS and Kaggle CUDA |
| **Experiment Metrics & Plots** | `experiments/EXP_XX/results/` | Validation loss plots, classification reports, CSV metric logs |
| **Model Weight Checkpoints** | `./checkpoints/EXP_XX/best_model.pt` | Saved model weights (ignored by git to keep repo clean) |
| **Sub-Experiment Comparison Diffs** | `.../sub_experiments/SUB_YY/diff_and_notes.md` | Side-by-side benchmark comparison matrix vs parent experiment |

---

## 💻 Running Code Locally vs. Kaggle

### Local Development (macOS — Apple Silicon)
Target PyTorch device automatically resolves to `mps` or `cpu`:
```bash
python experiments/EXP_01_<name>/scripts/train.py --config experiments/EXP_01_<name>/configs/base.yaml
```

### Kaggle Notebook Session (Linux — CUDA GPU)
Clone your repository directly into Kaggle's working directory:
```bash
cd /kaggle/working
git clone <your_repository_url>
cd Folder_Agentic_AI

# Single GPU Run:
CUDA_VISIBLE_DEVICES=0 python experiments/EXP_01_<name>/scripts/train.py --config experiments/EXP_01_<name>/configs/base.yaml

# Dual T4 Multi-GPU Run (~1.8x speedup):
torchrun --nproc_per_node=2 experiments/EXP_01_<name>/scripts/train.py --config experiments/EXP_01_<name>/configs/base.yaml
```
