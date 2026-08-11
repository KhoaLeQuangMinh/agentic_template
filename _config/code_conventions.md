# PyTorch & Modular Coding Conventions (Layer 3)

Guidelines for cloning external paper codebases, importing from `/repos/`, writing modular PyTorch code, and utilizing shared utilities.

---

## 1. External Repository Intake & Import Protocol (`/repos/`)

### Clean Cloning Protocol
When analyzing a paper with an official repository, shallow-clone it and strip the nested `.git` directory so the workspace remains clean:

```bash
git clone --depth 1 <paper_repository_url> repos/<repo_name>
rm -rf repos/<repo_name>/.git
```

### Strict Immutability Rule
Once cloned into `/repos/<repo_name>/`, **never edit, modify, or delete files inside `/repos/`**. It acts purely as a static reference codebase.

### Dynamic Import Protocol
When an experiment in `experiments/EXP_XX/models/` requires modules from `/repos/<repo_name>/`, add the repo root dynamically to `sys.path`:

```python
import sys
from pathlib import Path

# Add external repo root dynamically to sys.path
REPO_ROOT = Path(__file__).resolve().parents[3] / "repos" / "<repo_name>"
if str(REPO_ROOT) not in sys.path:
    sys.path.append(str(REPO_ROOT))

# Import clean reference modules from external repo
from external_module import CoreArchitecture
```

---

## 2. Shared Utilities (`/shared/`)

Import reusable data utilities, metrics, and logging handlers from `/shared/` instead of re-implementing boilerplate inside experiment folders:

```python
from shared.metrics import calculate_classification_metrics
from shared.logger import setup_experiment_logger
from shared.seed import set_seed
```

---

## 3. PyTorch Model Architecture Standards

1. **Class Hierarchy:** All model classes must inherit from `torch.nn.Module`.
2. **Decoupled Architecture:** Keep encoder/backbone feature extractors separate from task heads (e.g. classification or regression heads).
3. **Weight Initialization:** Include helper methods to load weights cleanly from saved checkpoints.
4. **Device Agnostic:** Ensure all tensor creations (`torch.zeros`, `torch.tensor`) explicitly use the passed `device` parameter.

```python
import torch
import torch.nn as nn

class CustomResearchModel(nn.Module):
    def __init__(self, backbone: nn.Module, num_classes: int = 2):
        super().__init__()
        self.backbone = backbone
        self.head = nn.Linear(backbone.output_dim, num_classes)
        
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        features = self.backbone(x)
        logits = self.head(features)
        return logits
```

---

## 4. Config-Driven Hyperparameters

- **Never hardcode parameters** (learning rate, batch size, weight decay, epoch count) inside `train.py`.
- Store all hyperparameters inside YAML files under `experiments/<EXP_ID>/configs/base.yaml`.
- Pass parameters via CLI argument parsers:

```python
import argparse
import yaml

parser = argparse.ArgumentParser()
parser.add_argument("--config", type=str, required=True, help="Path to config yaml")
args = parser.parse_args()

with open(args.config, "r") as f:
    config = yaml.safe_load(f)
```

---

## 5. Seed Setting & Reproducibility

Every training script must execute a seed initialization function before model creation:

```python
import random
import numpy as np
import torch

def set_seed(seed: int = 42):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    if torch.cuda.is_available():
        torch.cuda.manual_seed_all(seed)
```
