# ECG Arrhythmia Pattern Detection (C++)

**Author:** Syeda Lubaba Zainab

A beginner-level C++ program that reads real ECG data and detects
irregular heartbeats — written using only basic C++ (vectors, loops,
functions, file streams).

## What it does

1. Reads ECG voltage readings from `data/ecg_data.csv`
2. Smooths the signal to reduce noise (simple moving average)
3. Detects heartbeats by finding tall spikes (R-peaks)
4. Calculates the time gap between each heartbeat (RR interval)
5. Flags any beat that comes unusually early compared to recent beats —
   a simple, explainable sign of an irregular heartbeat
6. Prints a summary and saves detailed results to `output/results.csv`
## Dataset

`data/ecg_data.csv` contains the first 20 seconds (7,200 samples at
360 Hz) of ECG lead MLII from record **100** of the **MIT-BIH Arrhythmia
Database** on [PhysioNet](https://physionet.org/content/mitdb/) — a
real clinical ECG recording with cardiologist-verified beat labels.

## How to compile and run

```bash
g++ -std=c++11 -o ecg_arrhythmia ecg_arrhythmia.cpp
./ecg_arrhythmia
```

(On Windows with MinGW: `g++ -std=c++11 -o ecg_arrhythmia.exe ecg_arrhythmia.cpp`
then run `ecg_arrhythmia.exe`)
## Sample output

```
Loaded 7200 ECG samples from data/ecg_data.csv
Signal smoothed to reduce noise.
Detected 25 heartbeats.
Average heart rate: 73.75 beats per minute
Flagged 1 beat(s) as possibly premature.

Details of flagged beats:
  - Beat #7 at t = 5.68s (RR interval = 0.65s)
```

This flagged beat matches a real atrial premature beat independently
labeled by a cardiologist in the original dataset at the same location
— confirming the detection logic works correctly.## How the detection works (plain-language)

- **Smoothing**: averages each point with its close neighbors so small
  noise wiggles don't get mistaken for heartbeats.
- **Peak detection**: a point counts as a heartbeat if it's taller than
  a threshold, taller than the points beside it, and far enough away
  (0.3s) from the last detected beat (since a real heart can't beat
  faster than ~200 bpm).
- **Premature beat flagging**: compares each gap between beats (RR
  interval) to the average of the last 5 gaps. If a beat comes much
  sooner than expected (less than 85% of the recent average gap), it's
  flagged as possibly premature.

## Limitations (be ready to mention these if asked)

- Only tested on a 20-second segment of one recording — not validated
  across the full dataset or noisier recordings.
- The peak threshold (`0.4`) is fixed in the code rather than
  automatically adaptive to different recordings.
- Only flags *that* a beat is premature, not what specific type of
  arrhythmia it might be.
  ## Files

```
ecg-cpp-project/
├── ecg_arrhythmia.cpp   # the program
├── data/
│   └── ecg_data.csv     # ECG signal (PhysioNet MIT-BIH record 100)
├── output/
│   └── results.csv      # generated after running the program
└── README.md
```
