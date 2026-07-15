# Mini Dataset: Visual Cognitive Load With Longitudinal Robustness

Dataset version: `20260622`

This folder is the downloadable raw-data bundle for the NeuraDock Visual
Cognitive Load Dataset. It supports one complete workflow:

- calibrate posterior Alpha from a person's Rest recording
- compare short visual-task recordings against that individual baseline
- inspect mixed-eye-state and task-variant caveats
- test a visual-load pipeline against long-duration state and quality confounds

The short Rest/Task cohorts are raw tutorial recordings. The
`longitudinal_state/` cohort includes quality-aware derived CSVs and figures so
that automatic candidate-state markers can be inspected transparently. Those
markers are not cognitive-load labels and must not be used as supervision.

Use the short files to build a within-subject Alpha feature. Use the long files
to test whether that feature remains trustworthy during eyes-closed rest,
meditation-like quiet state, drowsiness, near-sleep transition, and signal
quality changes.

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
`-- longitudinal_state/
    |-- raw_txt/
    `-- analysis/
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

Use each subject's own Rest recording as the baseline. Do not compare absolute
Alpha or cognitive-load scores across subjects. Before interpreting a task
effect, inspect data quality and the long-recording confound examples.

Recommended first pass:

1. Parse `cohort_3subj_rest_task/S01/rest_S01_1.txt` and
   `task_S01_1.txt`.
2. Run the official preprocessing and quality gate.
3. Compute posterior 8-13 Hz Alpha from usable posterior channels.
4. Reproduce the homepage Rest/Task Alpha decrease.
5. Inspect `longitudinal_state/` before turning the feature into a continuous
   cognitive-load score.

### Longitudinal State Cohort

`longitudinal_state/` contains three complete 26-61 minute raw recordings for
time-resolved Alpha and state-transition engineering. They are not labelled
sleep, awake, or meditation recordings. The included automatic markers identify
candidate changes in Alpha and slow-wave balance only; see
`longitudinal_state/README.md` and
`longitudinal_state/analysis/STATE_DYNAMICS_REPORT.md`.

The current automatic patterns are:

- `long_01_sample14`: progressive slow-Alpha shift; sleep-onset-like candidate.
- `long_02_sample15`: fluctuating slow-Alpha episodes; mixed-state candidate.
- `long_03_0125`: no sustained slow-Alpha shift detected; indeterminate.

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
