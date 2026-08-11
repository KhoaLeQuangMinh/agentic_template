# Environment Setup & Hardware Specs (Layer 3)

This reference documents environment dependencies, PyTorch device targeting, dynamic path resolution, and Kaggle dataset path registries.

---

## 1. Environment Specifications

### Local Development Environment
- **OS:** macOS (Apple Silicon)
- **Primary Accelerator:** Metal Performance Shaders (`mps`) / CPU
- **Python Environment:** Conda (`conda activate ai_exp`) or `.venv`
- **PyTorch Device Initialization:**
  ```python
  import torch

  def get_device() -> torch.device:
      if torch.cuda.is_available():
          return torch.device("cuda")
      elif torch.backends.mps.is_available():
          return torch.device("mps")
      return torch.device("cpu")
  ```

### Kaggle Execution Environment
- **OS:** Linux (Ubuntu base)
- **Primary Accelerator:** NVIDIA GPU (`cuda:0`, `cuda:1`)
- **Repository Root:** `/kaggle/working/Folder_Agentic_AI`

---

## 2. Dynamic Path Resolution Standard

To ensure scripts run seamlessly without path breakage across macOS and Kaggle:

```python
import os
from pathlib import Path

def get_data_dir(dataset_name: str, kaggle_slug: str = None) -> Path:
    """Dynamically resolves dataset path across Kaggle and local environments."""
    if kaggle_slug:
        kaggle_dir = Path(f"/kaggle/input/{kaggle_slug}")
        if kaggle_dir.exists():
            return kaggle_dir
            
    local_dir = Path(f"./data/{dataset_name}")
    return local_dir

def get_checkpoint_dir(exp_id: str) -> Path:
    """Resolves model checkpoint directory."""
    chkpt_dir = Path(f"./checkpoints/{exp_id}")
    chkpt_dir.mkdir(parents=True, exist_ok=True)
    return chkpt_dir
```

---

## 3. Kaggle Dataset & Directory Mapping Registry

Update this table with your Kaggle dataset slugs so the AI agent knows where external inputs live on Kaggle:

| Resource / Dataset Name | Local Path | Kaggle Input Path | Used By Experiments | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `alzheimers_mri` | `./data/alzheimers_mri/` | `/kaggle/input/alzheimers-mri-dataset` | `EXP_01`, `EXP_02` | Brain MRI scans |
| `ucsf_tabular` | `./data/ucsf_tabular/` | `/kaggle/input/ucsf-tabular-data` | `EXP_01`, `EXP_03` | Tabular clinical records |
| `<resource_name>` | `./data/<resource>/` | `/kaggle/input/<kaggle-dataset-slug>` | `EXP_XX` | Description / Notes |

---

## 4. Dependency Guidelines

- Maintain top-level `requirements.txt` for standard dependencies (`torch`, `torchvision`, `numpy`, `pandas`, `pyyaml`, `scikit-learn`, `tqdm`).
- Keep experiment-specific requirements under `experiments/<EXP_ID>/requirements.txt` if a paper requires unique packages.
