# Execution Manual: [EXP_ID] — [Experiment Name]

This manual provides copy-pasteable CLI execution commands for local (macOS) and cloud (Kaggle) runs of `[EXP_ID]`.

---

## 1. Setup & Environment Activation

### Local Development (macOS)
```bash
conda activate ai_exp
```

### Kaggle Notebook Session (Linux)
```bash
cd /kaggle/working
git clone <your_repository_url>
cd Folder_Agentic_AI
```

---

## 2. Model Training Execution Commands

### Local macOS Execution (MPS / CPU)
Run from the workspace root directory:
```bash
python experiments/[EXP_ID]_[name]/scripts/train.py --config experiments/[EXP_ID]_[name]/configs/base.yaml
```
*What it does:* Loads datasets from `./data/`, initializes model architecture from `models/`, executes training loop, and logs checkpoints/metrics to `results/`.

### Kaggle Single-GPU Execution (CUDA)
```bash
CUDA_VISIBLE_DEVICES=0 python experiments/[EXP_ID]_[name]/scripts/train.py --config experiments/[EXP_ID]_[name]/configs/base.yaml
```
*What it does:* Runs single NVIDIA GPU acceleration inside Kaggle session.

### Kaggle Multi-GPU Execution (PyTorch DDP - Dual T4 GPUs)
```bash
torchrun --nproc_per_node=2 experiments/[EXP_ID]_[name]/scripts/train.py --config experiments/[EXP_ID]_[name]/configs/base.yaml
```
*What it does:* Executes Distributed Data Parallel training across 2 Kaggle GPUs (~1.8x - 1.9x throughput speedup).

---

## 3. Evaluation & Validation Commands

```bash
python experiments/[EXP_ID]_[name]/scripts/eval.py --config experiments/[EXP_ID]_[name]/configs/base.yaml --checkpoint checkpoints/[EXP_ID]/best_model.pt
```
*What it does:* Loads best checkpoint weights and evaluates model metrics (Loss, Accuracy, F1, AUC) on the test split.
