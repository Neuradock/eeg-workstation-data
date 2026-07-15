# Dataset Card: NeuraDock Visual Cognitive Load

## Purpose

This dataset supports development and validation of a practical visual
cognitive-load workflow for the NeuraDock 7-channel EEG workstation. The
intended workflow is individual calibration, quality gating, posterior
Alpha-based task comparison, and longitudinal confound detection.

## Dataset Components

| Component | Records | Primary role |
|---|---:|---|
| Paired Rest/Task cohort | 12 raw recordings across 3 subjects and 2 sessions | Reproduce within-subject visual-load comparisons |
| Task-variant cohort | 6 raw recordings across 2 subjects | Explore task, chat, music, game, and eye-state variation |
| Longitudinal-state cohort | 3 complete raw recordings | Stress-test quality and state confounds over 26-61 minutes |

All data use the NeuraDock Bluetooth text-export layout with nominal 250 Hz
sampling and seven channels: `CP5, CP6, PO3, PO4, O1, Oz, O2`.

## Intended Uses

- Implement and test Bluetooth text parsing.
- Reproduce 1-100 Hz preprocessing and quality gating.
- Build within-subject posterior-Alpha features for Rest/Task experiments.
- Evaluate quality-aware rejection, recalibration, and abstention rules.
- Test whether a visual-load model produces false positives during long-term
  state changes.

## Non-Uses

- Clinical or consumer sleep staging.
- Meditation-state detection.
- Medical, diagnostic, or performance claims.
- Cross-subject ranking of absolute cognitive-load score.
- Treating automatic long-recording markers as behavioural ground truth.

## Evaluation Principle

Use each participant as their own baseline. A valid visual-load conclusion
requires both a within-subject comparison and acceptable signal quality. The
longitudinal cohort should be used as a robustness set, not as high/low-load
supervision.

## Quality And Device Evidence

This dataset is not a formal device-certification report, but it gives public
engineering evidence for how the NeuraDock workstation behaves in practical EEG
workflows:

- Raw Bluetooth text exports can be parsed into seven-channel EEG arrays with
  nominal 250 Hz sampling.
- Complete long recordings are included without splitting, preserving natural
  state drift over 26-61 minutes.
- Posterior Alpha dynamics are visible after preprocessing and quality gating.
- Every long-recording interpretation is paired with retained-window coverage
  and usable posterior-channel information.
- SHA-256 checksums, manifests, figure provenance, and derived CSV features are
  included so users can audit the data pipeline.

## Longitudinal Marker Policy

The long recordings contain automatic candidate markers derived from posterior
Alpha, theta/Alpha, delta/Alpha, and quality-retained one-minute windows. They
are transparent engineering labels that identify possible confounds. They are
not verified labels for sleep, wakefulness, meditation, or any sleep stage.

Current automatic candidate patterns:

| Recording | Candidate pattern | Confidence boundary |
|---|---|---|
| `long_01_sample14` | Progressive slow-Alpha shift; sleep-onset-like candidate | Moderate confidence, not clinical sleep staging |
| `long_02_sample15` | Fluctuating slow-Alpha episodes; mixed-state candidate | Moderate confidence, could reflect meditation-like quiet rest, drowsiness, or half-awake state |
| `long_03_0125` | No sustained slow-Alpha shift detected; indeterminate | Low confidence because only one posterior channel remains after quality control |

## Known Limitations

- Small convenience sample; results are not population estimates.
- Short task labels represent session context, not continuous ground truth.
- Eye state and non-task state changes can alter Alpha.
- Long-recording quality retention varies by recording and electrode.
- One long recording retains only one usable posterior channel after quality
  control.

## Publication And Redistribution

Before external redistribution or publication, the maintainer should confirm
the appropriate participant-consent, anonymisation, and licensing terms. Cite
the dataset version, raw-file paths, preprocessing parameters, and any derived
marker rules used in analysis.
