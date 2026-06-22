# Visual Cognitive Load Data

This folder contains NeuraDock EEG datasets intended for visual cognitive-load
examples, especially posterior Alpha dynamics and within-subject Rest/Task
comparison.

## Datasets

| Folder | Description |
|---|---|
| `mini_dataset_v20260622/` | Small tutorial dataset with two cognitive-load cohorts: subject-specific task variants and three-subject Rest/Task sessions. |

## Analysis Policy

Use each subject as their own baseline. Do not compare cognitive-load indices
across subjects unless a separate calibration protocol is introduced.

The NeuraDock Agent workflows parse raw `.txt` files, run preprocessing and
quality control, and then compute Alpha or visual cognitive-load metrics from
the cleaned signal.
