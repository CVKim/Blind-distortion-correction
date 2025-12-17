<img width="800" height="420" alt="image" src="https://github.com/user-attachments/assets/b1c053a8-f399-4b3d-8cf2-29151a389515" />

<img width="818" height="770" alt="4_easy_230914-23-1" src="https://github.com/user-attachments/assets/bf7cc32c-3852-47de-ace9-d41e5b8434a0" />
<img width="808" height="581" alt="image" src="https://github.com/user-attachments/assets/7414d5d7-636d-4c4d-ac64-aeb07d53978f" />


# Blind Distortion Correction & Metric Measurement

This repository provides a **calibration-free** solution for image distortion correction and metric distance measurement. By leveraging **YOLO instance segmentation results** and **EXIF metadata** (specifically from mobile phone cameras), this tool performs automatic perspective warping and precise object analysis without requiring a traditional checkerboard calibration.

## 🚀 Key Features

* **Calibration-Free Unwarping:** Uses image EXIF data (Focal Length) and a reference object to calculate the homography matrix and correct perspective distortion.
* **YOLO Integration:** Parses JSON outputs from YOLO instance segmentation to identify reference objects (`TIMBER`) and targets (`SCREW`).
* **Metric Distance Measurement:** Calculates real-world distances (in pixels/mm) between detected objects after perspective correction.
* **Stability Verification:** Includes a perturbation algorithm to verify the stability and accuracy of the distortion correction by simulating point shifts.
* **Automated Logging:** Generates detailed logs of the transformation process, including homography matrices and measurement errors.

## 🛠️ Prerequisites

* Python 3.8+
* Images containing EXIF data (Focal Length & 35mm Equivalent).

## 📦 Installation

1.  Clone the repository:
    ```bash
    git clone [https://github.com/your-username/blind-distortion-correction.git](https://github.com/your-username/blind-distortion-correction.git)
    cd blind-distortion-correction
    ```

2.  Install the required dependencies:
    ```bash
    pip install opencv-python numpy ExifRead Pillow
    ```
    *Note: `tkinter` is usually included with standard Python installations.*

## 📂 Data Preparation

The script expects pairs of **Image files** (`.jpg`, `.png`) and **JSON files** (YOLO segmentation results) with the same filename in the source directory.

### 1. JSON Structure
The JSON file must follow a standard label format (e.g., LabelMe or YOLO-to-JSON) containing the following classes:

* **`TIMBER` (Reference Object):**
    * Used to calculate the perspective transform.
    * Must contain at least 4 points (polygon).
* **`SCREW` (Target Object):**
    * The objects to be measured and connected.
    * Must contain `points` or a `bbox`.

### 2. EXIF Data
Ensure your source images contain the following EXIF tags (standard in most mobile phones):
* `EXIF FocalLength`
* `EXIF FocalLengthIn35mmFilm`

## ⚙️ Configuration

Key parameters are currently defined within `main.py`. You may need to adjust the following variable based on your physical reference object size:

In `warp_image` function:
```python
real_height_mm = 56.0  # The actual physical height of your reference object (TIMBER) in mm.
