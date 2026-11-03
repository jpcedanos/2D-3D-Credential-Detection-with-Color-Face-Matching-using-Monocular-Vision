# 2D–3D Credential Detection with Color & Face Matching

A single-camera system that finds a student ID card in a live video feed, checks that it has a face on it, tells which university it belongs to by color, and estimates how far away it is.

Team project for TE3002B (Intelligent Robotics Implementation), Tecnológico de Monterrey, 2024. The full write-up is in [`ID Recognition with Computer Vision Report.pdf`](ID%20Recognition%20with%20Computer%20Vision%20Report.pdf) (in Spanish).

![Undistorted calibration result](calibresult.png)

## How it works

1. **Camera calibration from 2D–3D points.** You click the corners of ID cards in a reference image. The script maps those clicks to known card dimensions (3.3 × 2.0 in) and runs `cv2.calibrateCamera` to get the intrinsics and distortion coefficients. It then saves an undistorted image (`calibresult.png`).
2. **Color selection.** In the live feed, double-click the card to sample its HSV color.
3. **Detection.** Each frame goes through these steps:
   - an HSV mask isolates that color
   - contours are filtered by area and by a four-corner polygon check, leaving card-shaped regions
   - a Haar cascade looks for a face inside each candidate card
4. **Classification and distance.** If there's a face, the sampled color is matched to the closest known ID: blue for Tec, yellow for UDEM. Distance comes from the card's apparent size and the focal length.

## Requirements

Python 3 with `opencv-python` and `numpy`.

## Running

```bash
python "Ultimo Intento.py"
```

Before running, update the paths in the script:
- the reference image (`img_path`)
- the Haar cascade file (`haarcascade_frontalface_default.xml`, available through `cv2.data.haarcascades`)

Then:
1. Click the card corners in the reference image and press any key to calibrate.
2. In the live window, double-click the card to pick its color and press **Space**.
3. Press **Q** to quit.

## Team

Emilio Rizo, José Pablo Cedano, Luis Antonio Zermeño, Omar Flores, Luis Mario Lozoya, Jorge Axel Castruita, Rodrigo Escandón
