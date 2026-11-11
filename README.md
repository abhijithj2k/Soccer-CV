# Football Vision Analytics Studio

A computer-vision system that tracks football players in match footage, identifies their teams, maps their
positions onto a pitch and presents the results in an interactive dashboard. Built as my final industry
project for the Master of Artificial Intelligence at the University of Technology Sydney (2026). I designed
and built the system; my project partner wrote the report.

## What it does

1. **Detection and tracking:** YOLO11 detects players and a multi-object tracker follows them across frames.
2. **Team identification:** a random-forest classifier assigns each track to a team.
3. **Pitch calibration:** automatic calibration (PnLCalib with motion propagation between frames) estimates a
   homography that maps image positions to pitch coordinates.
4. **Movement analysis:** trajectories, heatmaps and per-player movement statistics.
5. **Dashboard:** a Gradio app with frame-by-frame playback, team and player filters, a tactical pitch view,
   player comparison, quality checks and CSV/JSON export.

## Notebooks

| Notebook | Stage |
|---|---|
| `notebooks/01_Baseline_Detection_Tracking.ipynb` | Baseline detection and tracking |
| `notebooks/02_SoccerNet_Data_Preparation.ipynb` | Data preparation and sequence selection |
| `notebooks/03B_Automatic_Pitch_Calibration_v2.ipynb` | Automatic pitch calibration, version 2 |
| `notebooks/05_Football_Vision_Analytics_Studio.ipynb` | Gradio dashboard |

Two further notebooks are too large to host on GitHub, so they open in Google Colab:

| Notebook | Stage |
|---|---|
| [03_Pitch_Calibration_and_Homography](https://colab.research.google.com/drive/1rjyKBC7qWatQjtE8a_Xn6uwDNQvO9G-0) | Pitch calibration and homography |
| [04_Player_Tracking_and_Movement_Analysis](https://colab.research.google.com/drive/19ky0CKL3VpzwD3h7oxvuCVpL-d0yW7Gv) | Tracker comparison, team identification and movement analysis |

## Results

Evaluated on SoccerNet tracking sequence SNMOT-066, frames 350-450 (101 frames, 2,433 ground-truth
observations, 25 identities, IoU threshold 0.5).

| Tracker | MOTA | IDF1 | ID switches |
|---|---|---|---|
| ByteTrack | 0.676 | 0.757 | 33 |
| BoT-SORT | 0.694 | 0.776 | 33 |
| BoT-SORT + ReID | 0.688 | 0.796 | 35 |

Team identification: 93.8% track-level and 92.2% sample-level accuracy (leave-one-group-out
cross-validation by track).

## Limitations

- Tracking is evaluated on one sequence and 101 frames.
- Tracking IDs are not verified player identities, and identity switches remain.
- Pitch calibration is usable for frames 350-379 only; distances and speeds are pitch-reference estimates,
  not validated metres.
- Team identification has not been validated on unseen matches.

## Data and models

The SoccerNet dataset and model weights are not included. SoccerNet data must be requested from
[soccer-net.org](https://www.soccer-net.org/) under its own terms.

## Roadmap

- Extend pitch calibration to the full sequence and to more matches.
- Improve the dashboard interface and add further analytics.
- Deploy the dashboard as a hosted app.

## Tools

Python, Ultralytics YOLO11, ByteTrack, BoT-SORT, PnLCalib, OpenCV, scikit-learn, Gradio, Google Colab.
