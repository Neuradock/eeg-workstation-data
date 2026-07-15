# Longitudinal State Cohort

This cohort adds three complete long NeuraDock recordings to the visual
cognitive-load mini dataset. It is intended for longitudinal EEG engineering:
within-person Alpha dynamics, quality-aware time series, and candidate
state-transition markers.

## Why These Files Matter

Short Rest/Task files show a contrast. These continuous recordings expose what
a deployed system must handle: slow changes in arousal, prolonged eyes-closed
or meditation-like periods, variable signal quality, and transitions that do
not occur at known trial boundaries. They are therefore useful for testing
time-resolved features and quality gates, not only classifiers.

They also demonstrate a practical device-level question: can a lightweight
7-channel workstation preserve interpretable posterior Alpha dynamics during
long naturalistic recording, rather than only during short controlled trials?
The included analysis answers this with quality-aware Alpha maps, retained
window coverage, and explicit bad-channel reporting.

## Current Candidate Patterns

| Recording | Parsed duration | Usable posterior channels | Automatic candidate pattern |
|---|---:|---|---|
| `long_01_sample14` | 60.96 min | PO3, PO4 | Progressive slow-Alpha shift; sleep-onset-like candidate from 45 min |
| `long_02_sample15` | 53.74 min | PO3, PO4, O1 | Fluctuating slow-Alpha episodes; mixed-state candidate from 26 min |
| `long_03_0125` | 26.42 min | PO4 | No sustained slow-Alpha shift detected; indeterminate |

These patterns are intentionally conservative. They can help users design
`state confound`, `quality fail`, and `recalibrate` rules for cognitive-load
systems. They should not be treated as verified meditation, sleep, or
wakefulness labels without external session notes.

## Contents

```text
longitudinal_state/
|-- raw_txt/
|   |-- long_01_sample14_20260114223404_1h57s_NeuraDock99170.txt
|   |-- long_02_sample15_20260115224803_53m44s_NeuraDock99170.txt
|   `-- long_03_0125_20260125232531_46m36s_NeuraDock99170.txt
`-- analysis/
    |-- STATE_DYNAMICS_REPORT.md
    |-- alpha_state_markers.png
    |-- slow_alpha_dynamics.png
    |-- quality_coverage.png
    |-- automatic_state_markers.csv
    |-- one_minute_spectral_features.csv
    `-- state_pattern_summary.csv
```

The GitHub homepage also includes a higher-resolution 30 s channel-level Alpha
state map and the plotted feature table in:

```text
visual_cognitive_load/figures/
|-- 03-longitudinal-alpha-channel-map.png
`-- longitudinal_alpha_channel_features_30s.csv
```

The raw files are complete exports and are deliberately not split. The text
files are protected from Git line-ending conversion by the local
`.gitattributes` file.

## Automatic Marker Policy

The analysis uses the offline preprocessing approach in
`examples/5.offline_data_preprocess.ipynb`: 1-100 Hz zero-phase filtering,
one-second quality metrics, bad-channel exclusion, and segment rejection.
Only one-minute windows with at least 50% retained one-second segments are
analysed. Posterior Alpha is `8-13 Hz / 1-30 Hz`; theta/Alpha and delta/Alpha
are complementary slow-wave ratios.

Markers are not source annotations. They are automatic research candidates:

- `relative_alpha_peak`: highest local posterior Alpha relative power.
- `sustained_slow_alpha_shift`: at least three of four valid one-minute
  windows have Alpha at or below 90% of the early baseline, with theta/Alpha
  and delta/Alpha each at or above 125%.
- `brief_slow_alpha_maximum`: strongest isolated slow/Alpha point when no
  sustained rule is met.

See `analysis/STATE_DYNAMICS_REPORT.md` for the result interpretation,
confidence limits, and the three candidate patterns.

## Scientific Boundary

These features can identify candidate changes in arousal-related EEG dynamics.
They cannot distinguish sleep, wakefulness, meditation, eye closure, or a
specific sleep stage without synchronized ground truth such as a session log,
video, EOG/EMG, or scored polysomnography. Do not use these files for clinical
or diagnostic decisions.
