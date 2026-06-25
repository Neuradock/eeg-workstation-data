# Mini Dataset For Visual Cognitive Load

Dataset version: `20260622`

This folder contains a small public tutorial dataset for the NeuraDock Visual
Cognitive Load Agent. It is designed to let users experience:

- EEG preprocessing and quality-control gating
- Alpha Dynamics on eyes/rest/task recordings
- Within-subject Rest/Task visual cognitive-load comparison
- Quality-risk interpretation for mixed-eye-state or noisier files

No generated analysis results are included. Users should run the Agent locally
to reproduce figures and reports.

## Folder Structure

```text
mini_dataset_v20260622/
|-- cohort_2subj_ljw_xzy/
|   |-- ljw/
|   |   |-- 01_rest.txt
|   |   |-- 02_chat.txt
|   |   `-- 03_game.txt
|   `-- xzy/
|       |-- 01_rest_eye_half.txt
|       |-- 02_music_eye_half.txt
|       `-- 03_game.txt
`-- cohort_3subj_rest_task/
    |-- S01/
    |-- S02/
    `-- S03/
```

## Channel And Format

- Sampling rate: 250 Hz
- Channel order: `0=CP5, 1=CP6, 2=PO3, 3=PO4, 4=O1, 5=Oz, 6=O2`
- Amplitude unit: microvolts (`uV`)
- Format: NeuraDock raw text export, parsed to `7 x samples`

## File Manifest

See:

```text
manifest.csv
manifest.json
checksums_sha256.txt
```

## Recommended Usage

Use each subject's own rest/baseline file. Do not compare across subjects.

### Cohort 1: Task Variants

`cohort_2subj_ljw_xzy/ljw/01_rest.txt` is the baseline for `ljw`.
Compare it with:

- `02_chat.txt`: task, expected medium cognitive load
- `03_game.txt`: task, expected higher visual cognitive load

`cohort_2subj_ljw_xzy/xzy/01_rest_eye_half.txt` is the baseline for `xzy`.
This subject has mixed eye-state caveats. Compare it with:

- `02_music_eye_half.txt`: music listening, mixed-eye-state caveat
- `03_game.txt`: game task, cleaner task file for this subject

### Cohort 2: Rest/Task Sessions

For `S01`, `S02`, and `S03`, compare rest and task files within the same
subject and session:

```text
rest_S01_1.txt vs task_S01_1.txt
rest_S01_2.txt vs task_S01_2.txt
...
```

## Example Commands

Assume the data repository and agent repository are next to each other:

```text
parent/
|-- eeg-workstation-data/
`-- neuradock-agent/
```

Install the Agent:

```powershell
cd neuradock-agent
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -e ".[dev]"
```

Alpha Dynamics example:

```powershell
.\.venv\Scripts\neuradock-agent.exe analyze `
  ..\eeg-workstation-data\visual_cognitive_load\mini_dataset_v20260622\cohort_2subj_ljw_xzy\ljw\01_rest.txt `
  --workflow alpha dynamics
```

Within-subject Rest/Task comparison example:

```powershell
.\.venv\Scripts\neuradock-agent.exe analyze `
  ..\eeg-workstation-data\visual_cognitive_load\mini_dataset_v20260622\cohort_3subj_rest_task\S01\rest_S01_1.txt `
  ..\eeg-workstation-data\visual_cognitive_load\mini_dataset_v20260622\cohort_3subj_rest_task\S01\task_S01_1.txt `
  --workflow visual cognition comparison `
  --condition-labels Rest Task
```

Task-variant comparison example:

```powershell
.\.venv\Scripts\neuradock-agent.exe analyze `
  ..\eeg-workstation-data\visual_cognitive_load\mini_dataset_v20260622\cohort_2subj_ljw_xzy\ljw\01_rest.txt `
  ..\eeg-workstation-data\visual_cognitive_load\mini_dataset_v20260622\cohort_2subj_ljw_xzy\ljw\03_game.txt `
  --workflow visual cognition comparison `
  --condition-labels Rest Game
```

## Interpretation Notes

- Alpha suppression is a relative within-subject signal.
- Mixed eye-state files can change posterior Alpha independent of workload.
- Quality warnings should be reported, not hidden.
- This dataset is for tutorial and engineering validation, not diagnosis.
