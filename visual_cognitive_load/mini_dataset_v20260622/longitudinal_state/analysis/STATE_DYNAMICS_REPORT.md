# Longitudinal State Dynamics Report

## Result In Brief

The three recordings show separable longitudinal spectral patterns, but not
verified behavioural labels.

| recording | parsed duration | usable posterior channels | candidate pattern | confidence |
|---|---:|---|---|---|
| `long_01_sample14` | 60.96 min | PO3, PO4 | Progressive slow-Alpha shift; sleep-onset-like candidate from 45 min | moderate |
| `long_02_sample15` | 53.74 min | PO3, PO4, O1 | Fluctuating slow-Alpha episodes; mixed-state candidate from 26 min | moderate |
| `long_03_0125` | 26.42 min | PO4 | No sustained slow-Alpha shift detected; indeterminate | low |

`long_01_sample14` has the clearest late transition: compared with its first
ten valid one-minute windows, late posterior Alpha is 31.2% lower, theta/Alpha
is 96.9% higher, and delta/Alpha is 41.9% higher. This is compatible with a
sleep-onset or reduced-arousal progression.

`long_02_sample15` has a 26-minute slow-Alpha episode but later alternates
between slower and more Alpha-dominant windows. That pattern is compatible with
the described half-awake/half-asleep state, but it is not proof of it.

`long_03_0125` does not meet the sustained-shift rule. This is consistent with
a session that did not enter a stable drowsy pattern, but only PO4 remains
after quality control, so wakefulness or meditation cannot be determined from
this file alone.

## Figures

- `alpha_state_markers.png`: posterior Alpha relative to the early baseline,
  with automatic markers drawn on the time axis.
- `slow_alpha_dynamics.png`: Alpha and the `(theta + delta) / Alpha` balance.
- `quality_coverage.png`: valid one-second data coverage per analysed minute.

## Method

1. Parse the NeuraDock Bluetooth 47-column export and run the offline
   quality-gated preprocessing described in
   `examples/5.offline_data_preprocess.ipynb`.
2. Exclude channels marked bad by the one-second quality screen; calculate
   spectral metrics from the remaining posterior electrodes.
3. Retain one-minute windows only when at least 30 of 60 one-second segments
   pass quality gating. Estimate the PSD from each retained one-second segment
   and average before integrating bands.
4. Define the early baseline as the first ten valid one-minute windows within
   a recording. Mark a sustained slow-Alpha shift when at least three of four
   valid windows satisfy all of: Alpha <= 90% baseline, theta/Alpha >= 125%,
   and delta/Alpha >= 125%.

The precise marker times, raw metrics, and coverage are available in the CSV
files beside this report. The parsed duration is calculated from the 250 Hz
sample stream; filename duration strings are not treated as verified timing.

## Interpretation Limits

The marker is a reproducible feature rule, not sleep scoring. Alpha may change
with eye state, relaxation, meditation, attention, sensor contact, and
movement. Reliable separation of awake, meditation, drowsiness, and sleep
requires at least one synchronized behavioural or physiological ground-truth
channel. If session notes identify which recording was awake, meditating, or
asleep, add them as external annotations rather than changing the automatic
marker labels.
