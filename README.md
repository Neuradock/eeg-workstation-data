# NeuraDock EEG Workstation Data

This repository contains public sample EEG recordings collected with the
NeuraDock 7-channel dry-electrode EEG workstation. The files are intended for
tutorials, algorithm validation, and reproducible examples for the NeuraDock
Python workstation and NeuraDock Visual Cognitive Load Agent.

The repository is data-only. Analysis code lives in the relevant software
repositories.

## Repository Layout

```text
.
|-- example_data_bluetooth.txt
|-- example_data_usb.txt
|-- open_closed_eye2.txt
|-- rest_20251024160452_2m12s.txt
|-- task_20251024160748_2m33s.txt
`-- visual_cognitive_load/
    `-- mini_dataset_v20260622/
```

## Current Public Channel Profile

The current NeuraDock Agent profile uses this zero-based channel order:

```text
0=CP5, 1=CP6, 2=PO3, 3=PO4, 4=O1, 5=Oz, 6=O2
```

Other older tutorials or notes may show legacy mappings. For current
NeuraDock Agent workflows, use the profile above.

Common recording facts:

- Sampling rate: 250 Hz
- Channel count: 7
- Amplitude unit: microvolts (`uV`)
- File encoding: UTF-8 text
- Delimiter: comma

## Text Data Format

Bluetooth text exports aggregate five sample groups after one timestamp and
marker pair:

```text
timestamp, marker, C0_t0, C1_t0, ..., C6_t0, reserved,
C0_t1, ..., C6_t1, reserved, ..., C0_t4, ..., C6_t4, reserved
```

USB text exports contain one sample packet per line:

```text
timestamp, marker, C0, C1, C2, C3, C4, C5, C6, reserved
```

Parsers should expand both layouts to a `7 x samples` matrix in the public
channel order above.

## Root Example Files

| File | Acquisition | Description | Typical use |
|---|---|---|---|
| `example_data_bluetooth.txt` | Bluetooth | Small raw Bluetooth text export | Parser tutorial |
| `example_data_usb.txt` | USB | Small raw USB text export | Parser tutorial |
| `open_closed_eye2.txt` | Bluetooth | Eyes-open/eyes-closed Alpha modulation recording | Alpha dynamics and quality-control demo |
| `rest_20251024160452_2m12s.txt` | Bluetooth | Resting-state recording | Rest/task Alpha comparison demo |
| `task_20251024160748_2m33s.txt` | Bluetooth | Task-state recording | Rest/task Alpha comparison demo |

## Visual Cognitive Load Data

The `visual_cognitive_load/mini_dataset_v20260622/` folder contains a small
multi-subject tutorial dataset for Alpha dynamics and within-subject
Rest/Task visual cognitive-load comparison.

Start here:

```text
visual_cognitive_load/mini_dataset_v20260622/README.md
```

## Example With NeuraDock Agent

```powershell
git clone https://github.com/Neuradock/eeg-workstation-data.git
git clone https://github.com/Neuradock/neuradock-agent.git
cd neuradock-agent
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -e ".[dev]"

.\.venv\Scripts\neuradock-agent.exe analyze `
  ..\eeg-workstation-data\open_closed_eye2.txt `
  --workflow alpha dynamics
```

## Scientific Boundary

These recordings are tutorial and engineering-validation data. They should not
be used for medical, clinical, attention, fatigue, or performance diagnosis.
For cognitive-load analysis, compare conditions within the same subject unless
a validated cross-subject calibration protocol is used.
