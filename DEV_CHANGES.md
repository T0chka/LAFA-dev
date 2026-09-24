# LAFA development changes

Changes relative to the released LAFA v1 (`lafa-container-v1`).

## PyBoost

### Configuration and training

- Set `term_neg_weight_alpha=2.0`.
- Simplified the sketch configuration: `sketch_size` is now passed as   `SketchBoost(sketch_outputs=...)`; the current default is `1`.
- Added fold-level resume support. Existing model and normalization files are   loaded instead of retraining the fold.
- Model and normalization files are now written immediately after training each fold, so an interrupted run can resume from completed folds.

### PyBoost ablation

Dec 2025 -> Mar 2026:

| Variant | Score | Delta vs baseline |
|---|---:|---:|
| Baseline PyBoost | 0.353577 | — |
| `term_neg_weight_alpha=0.5` | 0.354012 | +0.000435 |
| `term_neg_weight_alpha=1` | 0.354780 | +0.001203 |
| `term_neg_weight_alpha=2` | 0.355229 | +0.001652 |
| `use_hess=False` | 0.353104 | -0.000473 |
| projection sketch `k=5` | 0.351926 | -0.001651 |
| random sketch `k=1` | 0.352238 | -0.001339 |
| target budget | 0.341376 | -0.012201 |
| hard mask `d=1` | 0.251429 | -0.102148 |

`term_neg_weight_alpha=2` was numerically best, but its +0.001652 difference from baseline is too small to treat as evidence of a meaningful improvement.
The other ablation variants were not retained.