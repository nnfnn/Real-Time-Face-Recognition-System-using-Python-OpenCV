# Real-Time Face Recognition System

A real-time face detection and recognition system built with Python and OpenCV. The system identifies registered individuals from a live webcam feed using machine learning-based face encodings, displays bounding boxes with name labels and confidence scores, and includes a dedicated tool for registering new faces.

---

## Overview

The system combines OpenCV for video capture and image processing with the face_recognition library (built on dlib's deep learning models) to perform accurate face identification in real time. Performance is optimized through frame skipping, frame scaling before detection, and caching of face encodings to disk so registered faces do not need to be reprocessed on every run.

---

## Features

- Real-time face detection and recognition via webcam
- ML-based face encoding using dlib's 128-dimensional face descriptor
- Support for multiple registered identities
- Confidence score displayed per recognized face
- Frame skipping and resolution scaling for performance optimization
- Encoding caching to disk (pickle) to avoid reprocessing on restart
- Dedicated face registration tool with live webcam capture
- HUD overlay showing FPS, face count, known identities, and timestamp
- Screenshot capture with keystroke
- Color-coded bounding boxes: green for known, red for unknown

---

## Tech Stack

| Component | Technology |
|---|---|
| Video Capture and Image Processing | OpenCV (cv2) |
| Face Detection | HOG-based detector via face_recognition |
| Face Recognition | dlib 128-d face encoding + cosine distance |
| Encoding Storage | Python pickle |
| Language | Python 3.10+ |

---

## Project Structure

```
Real-Time-Face-Recognition/
├── main.py                  # Real-time recognition loop
├── register_face.py         # Webcam-based face registration tool
├── requirements.txt         # Dependencies
├── README.md                # Documentation
├── known_faces/             # Auto-created — store face images here
│   └── Person_Name/
│       ├── photo_1.jpg
│       └── photo_2.jpg
├── encodings.pkl            # Auto-generated — cached face encodings
└── screenshots/             # Auto-created — saved captures
```

---

## Setup and Installation

**Step 1 — Clone the repository**
```bash
git clone https://github.com/nnfnn/Real-Time-Face-Recognition.git
cd Real-Time-Face-Recognition
```

**Step 2 — Install dependencies**

Note: dlib requires CMake and a C++ compiler. On most systems:
```bash
pip install cmake
pip install dlib
pip install face-recognition opencv-python numpy
```

Or install all at once:
```bash
pip install -r requirements.txt
```

**Step 3 — Register faces**
```bash
python register_face.py
```
Enter a name when prompted, look at the camera, press SPACE, and the system captures 5 photos automatically.

**Step 4 — Run the recognition system**
```bash
python main.py
```

---

## How It Works

```
Webcam frame captured by OpenCV
        |
        v
Frame scaled to 50% for faster processing
        |
        v
HOG-based face detector locates face regions
        |
        v
dlib computes 128-dimensional face encoding per detected face
        |
        v
Encoding compared against all registered face encodings
using Euclidean distance with a 0.5 tolerance threshold
        |
        v
Best match returned with confidence score
        |
        v
Bounding box and label drawn on original resolution frame
        |
        v
HUD overlay added (FPS, count, timestamp)
        |
        v
Frame displayed in real-time window
```

---

## Performance Optimization

Three techniques are applied to keep the system running smoothly on standard hardware:

**Frame skipping** — face detection only runs every 2nd frame. The previous frame's results are reused on skipped frames, cutting processing load by 50% with minimal visual impact.

**Frame scaling** — frames are resized to 50% before face detection. Smaller frames process significantly faster while still detecting faces accurately. Bounding box coordinates are then scaled back up for display on the full resolution frame.

**Encoding caching** — face encodings are computed once from the registered images and saved to `encodings.pkl`. On all subsequent runs, encodings are loaded directly from disk rather than reprocessed from images, cutting startup time significantly when many identities are registered.

---

## Controls

| Key | Action |
|---|---|
| Q | Quit the application |
| S | Save a screenshot to screenshots/ |
| SPACE | Start face capture during registration |

---

## Configuration

Key parameters at the top of `main.py` can be adjusted:

| Parameter | Default | Description |
|---|---|---|
| TOLERANCE | 0.5 | Recognition threshold. Lower = stricter (0.4 to 0.6 recommended) |
| FRAME_SKIP | 2 | Process every Nth frame |
| SCALE_FACTOR | 0.5 | Frame resize ratio before detection |

---

## Requirements

```
opencv-python==4.9.0.80
face-recognition==1.3.0
numpy==1.26.4
dlib==19.24.2
```

---

## Usage Notes

- A webcam is required. The system defaults to camera index 0.
- For best recognition accuracy, register multiple photos per person under different lighting conditions.
- dlib installation may require CMake and Visual Studio Build Tools on Windows.
- The system runs on CPU by default. GPU acceleration is available via dlib's CUDA build for significantly higher FPS.

---

## Author

**Muhammed Nifan**
MSc Data Science and Analytics — Jain University, Bengaluru
GitHub: github.com/nnfnn
LinkedIn: linkedin.com/in/nifan-mk-/
