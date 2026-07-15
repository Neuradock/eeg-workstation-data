# Homepage Figure Provenance

The homepage figures are derived examples, not additional labelled data.

| Figure | Source data | Display method |
|---|---|---|
| `01-time-domain-alpha-envelope.png` | `cohort_3subj_rest_task/S01/rest_S01_1.txt` and `task_S01_1.txt` | Representative quality-gated raw EEG segment plus posterior 8-13 Hz Alpha envelope |
| `02-rest-task-time-frequency.png` | The same paired S01 session | 6-16 Hz Rest-to-task time-frequency strip plus median `Task - Rest` spectral contrast |
| `03-longitudinal-alpha-channel-map.png` | The three `longitudinal_state/raw_txt/` recordings | Thirty-second 8-13 Hz Alpha power for all seven channels, normalized as dB change from each channel's early baseline |

The longitudinal marker times come from
`mini_dataset_v20260622/longitudinal_state/analysis/automatic_state_markers.csv`.
They identify candidate spectral changes and are not verified sleep,
meditation, or cognitive-load labels.
An asterisk and hatch pattern in the alpha-map channel label identify a channel
flagged bad by the official quality screen. Gray gaps indicate windows with low
retained quality. The black trend trace uses usable posterior channels when
available.

`longitudinal_alpha_channel_features_30s.csv` contains the plotted channel-level
Alpha values, sample retention, channel quality retention, and bad-channel flag
for each 30 s window.

`task_rest_pair_screening.csv` records the quality-gated screening of all six
paired Rest/Task sessions. The homepage example is selected from that screen by
the largest median four-second posterior Alpha-power decrease while retaining at
least two shared posterior channels and 70% data retention in both files.

The parsing and quality-gating steps use
`eeg-workstation-python/examples/Neuradock_library.py`:
`text2data_bluetooth`, `eeg_quality_check`, and `clean_eeg_data`.
