# EEG Seizure Detection — Neural Time-Series Analysis

End-to-end pipeline for detecting seizure events in multi-channel scalp EEG recordings using time-series decomposition, spectral analysis, and statistical anomaly detection.

**Dataset:** PhysioNet CHB-MIT Scalp EEG Database (23 subjects, 916 hours of continuous recordings)  
**Status:** In progress — pipeline complete, running on synthetic data pending dataset download

---

## Methods

| Step | Technique | Clinical Relevance |
|---|---|---|
| Preprocessing | Notch filter (60Hz) + bandpass (0.5–80Hz) | Remove powerline noise, isolate neural signal |
| Band isolation | Butterworth filter per EEG band | Separate delta/theta/alpha/beta/gamma contributions |
| Spectral analysis | Welch PSD per epoch | Quantify frequency-band power shifts at seizure onset |
| Segmentation | 4s epochs, 50% overlap | Temporal resolution for event localization |
| Detection | Rolling Z-score on gamma band power | Flag epochs deviating from rolling baseline |
| Characterization | Band power: baseline vs pre-ictal vs ictal | Identify spectral signatures before clinical onset |

## Satellite → Neural Signal Transfer

This pipeline directly adapts anomaly detection methods from satellite telemetry analysis:

| Satellite Telemetry | EEG Neural Signal |
|---|---|
| Multi-channel RF sensor streams | Multi-channel EEG electrode recordings |
| Anomaly = system degradation signal | Anomaly = ictal event onset |
| Band-pass filter for signal isolation | Band-pass filter for frequency band isolation |
| Z-score threshold for event detection | Z-score threshold for seizure detection |
| Rolling baseline characterization | Pre-ictal baseline characterization |

## Setup

```bash
pip install numpy pandas scipy matplotlib scikit-learn mne wfdb
```

**Download the CHB-MIT dataset:**
```bash
python -c "import wfdb; wfdb.dl_database('chbmit', './data/chbmit', records=['chb01/chb01_03'])"
```
Or download directly: https://physionet.org/content/chbmit/1.0.0/

The notebook runs on **synthetic EEG data** automatically if the dataset isn't present — all figures and analysis are fully reproducible without downloading the dataset first.

## Figures

- `figures/01_preprocessing.png` — Raw vs filtered signal comparison
- `figures/02_psd_comparison.png` — PSD: baseline vs ictal period
- `figures/03_seizure_detection.png` — Full detection pipeline output
- `figures/04_preictal_characterization.png` — Band power across recording periods

## Run

```bash
jupyter notebook eeg_seizure_detection.ipynb
```
