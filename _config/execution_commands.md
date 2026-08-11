# Execution Commands & Environment Cheat-Sheet (Layer 3)

Master reference of standard CLI commands for running experiments locally (macOS MPS/CPU) and on Kaggle (Linux CUDA/GPU).

---

## 1. Setup & Environment Activation

### Local Machine (macOS)
```bash
conda activate ai_exp
# Target PyTorch Device: 'mps' (Metal Performance Shaders) or 'cpu'
```

### Kaggle Notebook Setup (Linux Session)
```bash
cd /kaggle/working
git clone <your_repository_url>
cd Folder_Agentic_AI
```

---

## 2. Standard Execution Commands

### Local Run (macOS — MPS / CPU)
```bash
python experiments/<EXP_ID>_<name>/scripts/train.py --config experiments/<EXP_ID>_<name>/configs/base.yaml
```

### Kaggle Run (Single CUDA GPU)
```bash
CUDA_VISIBLE_DEVICES=0 python experiments/<EXP_ID>_<name>/scripts/train.py --config experiments/<EXP_ID>_<name>/configs/base.yaml
```

### Kaggle Multi-GPU Run (PyTorch DDP — Dual T4 GPUs)
```bash
torchrun --nproc_per_node=2 experiments/<EXP_ID>_<name>/scripts/train.py --config experiments/<EXP_ID>_<name>/configs/base.yaml
```

### Evaluation Run (Local / Kaggle)
```bash
python experiments/<EXP_ID>_<name>/scripts/eval.py --config experiments/<EXP_ID>_<name>/configs/base.yaml --checkpoint checkpoints/<EXP_ID>/best_model.pt
```

---

## 3. Path & Hardware Resolution Reference

| Environment | OS / Hardware | Target PyTorch Device | Data Location | Checkpoint Location |
| :--- | :--- | :--- | :--- | :--- |
| **Local Machine** | macOS (Apple Silicon) | `mps` or `cpu` | `./data/<dataset>/` | `./checkpoints/<EXP_ID>/` |
| **Kaggle Session** | Linux (NVIDIA GPU) | `cuda` (`cuda:0`, `cuda:1`) | `/kaggle/input/<dataset>/` | `./checkpoints/<EXP_ID>/` |
