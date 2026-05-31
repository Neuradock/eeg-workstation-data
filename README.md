# NeuraDock EEG Dataset Description

This directory contains sample data files collected by the NeuraDock dry-electrode EEG device for tutorials and algorithm validation.

---

## Data File List

| File Name | Acquisition Method | Description | Usage |
| :--- | :--- | :--- | :--- |
| `example_data_bluetooth.txt` | Bluetooth | Raw `.txt` case data transmitted via Bluetooth. Each line contains 5 sample packets (8 fields per packet: 7 channels + 1 separator). | Example input for Tutorial 1 (Bluetooth offline data reading) |
| `example_data_usb.txt` | USB | Raw `.txt` case data transmitted via USB. Each line contains 1 sample packet (8 fields: 7 channels + 1 separator). | Example input for Tutorial 2 (USB offline data reading) |
| `open_closed_eye2.txt` | Bluetooth | **Eyes-Open/Eyes-Closed Paradigm Data**. 7-channel EEG data recorded while the subject alternated between eyes-open (EO) and eyes-closed (EC) states. | Tutorial 9 (Alpha Blocking / Signal quality assessment) |
| `rest_20251024160452_2m12s.txt` | Bluetooth | **Resting-State Paradigm Data**. Continuous EEG recording (~2 min 12 s) while the subject remained relaxed and task-free. | Tutorial 9 (Resting vs. Task state comparison / ERD analysis) |
| `task_20251024160748_2m33s.txt` | Bluetooth | **Task-State Paradigm Data**. Continuous EEG recording (~2 min 33 s) during high cognitive-load reading. | Tutorial 9 (Task-state Alpha desynchronization / ERD analysis) |

---

## Common Data Format

All `.txt` files follow the NeuraDock raw text export format:

- **Sampling Rate**: 250 Hz
- **Channels**: 7 (mapped to PO4, O2, T6, Oz, T5, O1, PO3 in the 10-20 system)
- **File Encoding**: UTF-8
- **Delimiter**: Comma `,`

### Bluetooth Format (`example_data_bluetooth.txt`)

Line structure:
```
Timestamp, Marker, C0_t0, C1_t0, ..., C6_t0, 0, C0_t1, ..., C6_t4, 0
```
Each line contains **5 sample packets**, with 8 fields per packet (7 channel values + 1 separator `0`).

### USB Format (`example_data_usb.txt`)

Line structure:
```
Timestamp, Marker, C0_t0, C1_t0, ..., C6_t0, 0
```
Each line contains **1 sample packet**, for a total of 10 fields.

---

## Paradigm Data Details

### Eyes-Open/Eyes-Closed Paradigm (`open_closed_eye2.txt`)

- **Experimental Design**: The subject alternated between eyes-open (EO) and eyes-closed (EC) states according to cues.
- **Expected Phenomenon**: During eyes-closed, the occipital region should show prominent **8–13 Hz Alpha wave energy enhancement**; after opening the eyes, Alpha energy is rapidly suppressed (Alpha Blocking).
- **Usage**: Validate the device's ability to capture basic rhythms and calculate Alpha-band SNR per channel.

### Resting State vs. Task State (`rest_*` / `task_*`)

- **Resting State**: The subject remained awake and relaxed with no visual or cognitive task. The brain is in "default mode," with high-energy Alpha synchronization (especially in the occipital region).
- **Task State**: The subject performed intensive text reading. The visual cortex and parietal regions become active, and Alpha waves exhibit **Event-Related Desynchronization (ERD)**.
- **Expected Phenomenon**: Compared to resting state, the occipital-parietal region shows significantly reduced Alpha energy during the task.
- **Usage**: Assess the device's sensitivity to cognitive load changes; used in Tutorial 9 for topographic mapping and ERD analysis.

---

## Quick Start

Load example data directly in the tutorial notebooks:

```python
# Tutorial 1 / 2: Bluetooth / USB data reading
from Neuradock_library import text2data_bluetooth  # or USB equivalent
eeg_data = text2data_bluetooth("example_data_bluetooth.txt")
print(eeg_data.shape)  # (7, N)

# Tutorial 9: Paradigm analysis
from Neuradock_library import analyze_alpha_and_plot_eeg_group
analyze_alpha_and_plot_eeg_group(eeg_data, fs=250, show_channel=0)
```

---

## File Naming Convention

Timestamp-named raw data files follow the pattern:
```
{state}_{YYYYMMDDHHMMSS}_{duration}.txt
```
Example: `rest_20251024160452_2m12s.txt`
- `rest`: Resting State
- `20251024160452`: Recording started at 2025-10-24 16:04:52
- `2m12s`: Total duration of 2 minutes 12 seconds

---

## Notes

1. **Data Volume**: The size difference between `example_data_usb.txt` and `example_data_bluetooth.txt` mainly comes from the transmission protocol (USB single-packet vs. Bluetooth 5-packet aggregation); the actual sample count and channel count are identical.
2. **Electrode Consistency**: All data were collected using the NeuraDock 7-channel dry-electrode system. The channel order is fixed: `PO4, O2, T6, Oz, T5, O1, PO3`.
3. **Ready to Use**: Paradigm data (eyes-open/closed, resting, task) can be fed directly into the quality-check and analysis pipelines in Tutorials 5, 6, and 9 without additional preprocessing.
