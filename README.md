# Vehicle Counting and Speed Estimation from Video

> A computer vision system that automatically detects, tracks, and estimates the speed of vehicles in road traffic video sequences, with sub-frame precision.

---

## Context

Developed for the **Image Processing and Vision (PIV)** course in the Computer Engineering degree at **ISEL — Instituto Superior de Engenharia de Lisboa**, Winter 2025/26.

Speed monitoring is traditionally solved with radar or physical sensors — expensive hardware requiring complex installation. This project explores an alternative: automatically analysing existing surveillance video to extract speed information, with no additional hardware.

The goal was to demonstrate that classical image processing techniques — background subtraction, morphological operations, and centroid-based tracking — are sufficient to produce speed estimates with acceptable accuracy for traffic analysis.

---

## Features

- **Static background estimation** from multiple evenly-spaced frames, removing ambient noise before any detection
- **Moving object detection** via frame-to-background difference followed by adaptive thresholding (Otsu)
- **Morphological mask cleanup** to reduce false positives and consolidate regions belonging to individual vehicles
- **Multi-vehicle tracking** with persistent identity assignment across frames, using Euclidean centroid distance with tolerance for temporary absences
- **Sub-frame speed interpolation** — instead of counting whole frames, the system computes the exact fractional interval at which a vehicle crossed each detection line, yielding higher-resolution temporal estimates
- **Final statistical report** with mean, min, max, and standard deviation of detected vehicle speeds
- **Annotated video export** with overlaid detection lines, bounding boxes, vehicle IDs, and estimated speeds

---

## Technologies

- **Python 3**
- **OpenCV** — image processing, video capture and annotated output
- **NumPy** — matrix operations and statistical calculations
- **Matplotlib** — interactive frame and histogram visualisation during development
- **Jupyter Notebook** — iterative development and presentation environment

---

## Technical Approach

Speed measurement is based on two **virtual horizontal lines** defined in the scene. When a tracked vehicle crosses the first line (entry) and then the second (exit), the system records the exact timestamps — with sub-frame precision via linear interpolation — and computes speed from the known real-world distance between the lines.

---

## Results

Tested on the provided video (`AutoEstrada.wmv`): **12 vehicles detected**, **11 completed the full measurement pass**.

| Metric | Value |
|---|---|
| Mean Speed | **73.55 km/h** |
| Min Speed | 54.57 km/h |
| Max Speed | 99.03 km/h |
| Std Deviation | 15.23 km/h |
| Mean Travel Time | 0.3565 s |

---

## What I Learned

- **Measurement precision reasoning** — sub-frame interpolation was a deliberate engineering decision: at 24 fps, each frame represents ~41 ms of uncertainty. Ignoring that granularity introduces systematic errors in speed estimates.
- **Full image processing pipeline** — every stage from acquisition to final report required empirical trade-off decisions (threshold values, morphological kernels, tracking distance threshold) based on observed system behaviour.
- **Communicating results** — producing an annotated video and a structured statistical report reflects that a technical system is only complete when its outputs are interpretable by others.

This project also serves as a proof of concept that low-cost traffic monitoring solutions are viable using open-source tools and conventional camera footage.

---

## Authors

| Name | Student ID |
|---|---|
| **Ruben Zhang** | A51388 |
| **Rodrigo Neves** | A51452 |

**Class 51D · Lecturer: Pedro Jorge**
ISEL — Image Processing and Vision, Winter 2025/26
