# eeg-pilepsy
# EEG dataset for epilepsy research


The dataset contains 70 adult scalp EEG recordings (Cadwell Arc, 250 Hz, 19 channels, 10–20 system, about 36 h in total) from University Hospital, Riyadh. Each recording is a band-pass filtered (0.5–40 Hz) MNE-Python FIF file with expert annotations of four classes: **A** normal, **B** slowing, **C** epileptiform discharges, **D** electrographic seizure. The recordings are also provided as 1 s labelled windows.


## Contents

| File | Purpose |
|---|---|
| `read_dataset.ipynb` | Reads the FIF recordings, the annotation workbook and the 1 s windows; plots examples; makes a split by recording |
| `requirements.txt` | Python packages |

Tested with MNE-Python 1.13.2. Suggested `requirements.txt`:

```
mne>=1.13
numpy
pandas
openpyxl
matplotlib
scikit-learn
jupyter
```

## Quick start

1. Download the dataset and note the folder path.
2. Open `read_dataset.ipynb` and set `ROOT` to that folder.
3. Run the notebook.

Minimal example:

```python
import mne

raw = mne.io.read_raw_fif("EEG_data/SZ1_filtered_eeg.fif", preload=True)
print(raw.annotations)          # onset (s), duration (s), description (e.g. "C5,C7", "D1")
```

Signals are in volts. Annotation times are in the time base of the FIF file.

## Dataset layout

```
[dataset root]/
├── EEG_data/<RecordingID>_filtered_eeg.fif    continuous recordings with embedded annotations
├── annotations/EEG_annotations.xlsx           annotation workbook
└── windows/
    ├── eeg_dataset.npz                        1 s windows
    └── recording_index.tsv                    recording_id 
```

`eeg_dataset.npz` holds `X` (windows × 19 × 250, volts), `y` (class A–D), `recording_id`, `start_s` and `win_mode` (`train` or `test`).

## Annotation codes

Codes combine a class letter and a number, for example `C5` or `D1`. A trailing number of 1 means generalised, and even and odd numbers within classes B–D mean right and left. An event covering several regions has several codes separated by commas (`C5,C7,C9`). The full list is in the `Codes` sheet of the workbook.


## Using the data

- Split training and test data by **recording**, never by window. Windows from the same recording are highly correlated.
- Time without an annotation is unlabelled, not normal.
- Class frequencies are not clinical prevalence; recordings were selected.
- The `train` windows include augmented class C windows. Evaluate on `test` windows only.


## Ethics

The study was approved by the Institutional Review Board of King Abdullah bin Abdulaziz University Hospital (HA-01-R-104, Log 26-0043). Recordings were de-identified before release.

## Contact

[Name, e-mail]. Please open an issue for questions about the code or the files.
