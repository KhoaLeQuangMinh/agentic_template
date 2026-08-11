# Diff & Comparative Results: [SUB_ID]

---

## 1. Parameter & Config Differences vs Parent EXP

- **Config Overrides:** `configs/sub_config.yaml` vs parent `configs/base.yaml`
  - Changed `learning_rate`: `1e-3` ➡️ `1e-4`
  - Changed `weight_decay`: `1e-4` ➡️ `1e-2`
- **Code Modifications (if any):** Minor diff description (e.g. added Focal Loss scaling factor).

---

## 2. Benchmark Comparison Matrix

| Metric | Parent Experiment (`EXP_XX`) | Sub-Experiment (`[SUB_ID]`) | Delta / Improvement |
| :--- | :--- | :--- | :--- |
| **Validation Loss** | X.XXXX | Y.YYYY | -0.XXXX (Lower is better) |
| **Validation Accuracy** | XX.X% | YY.Y% | +Z.Z% |
| **F1 Score / AUC** | 0.XXXX | 0.YYYY | +0.ZZZZ |

---

## 3. Takeaway & Next Steps
- **Key Insight:** Did the parameter shift validate the micro-hypothesis?
- **Recommendation:** Should this change be merged into the primary experiment baseline or discarded?
