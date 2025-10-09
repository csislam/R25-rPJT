# Hierarchical Spatial Algorithms for High-Resolution Image Quantization and Feature Extraction

<img width="813" height="366" alt="image" src="https://github.com/user-attachments/assets/0787c765-765f-4928-8268-06c22da641e6" />


---

## Abstract
This study introduces a modular framework for spatial image processing, integrating grayscale quantization, color and brightness enhancement, image sharpening, bidirectional transformation pipelines, and geometric feature extraction. A stepwise intensity transformation quantizes grayscale images into eight discrete levels, producing a posterization effect that simplifies representation while preserving structural detail. Color enhancement is achieved via histogram equalization in both RGB and YCrCb color spaces, with the latter improving contrast while maintaining chrominance fidelity. Brightness adjustment is implemented through HSV value-channel manipulation, and image sharpening is performed using a $3 \times 3$ convolution kernel to enhance high-frequency details. A bidirectional transformation pipeline that integrates unsharp masking, gamma correction, and noise amplification achieved accuracy levels of 76.10\% and 74.80\% for the forward and reverse processes, respectively. Geometric feature extraction employed Canny edge detection, Hough-based line estimation (e.g., 51.50° for billiard cue alignment), Harris corner detection, and morphological window localization. Cue isolation further yielded 81.87\% similarity against ground truth images. Experimental evaluation across diverse datasets demonstrates robust and deterministic performance, highlighting its potential for real-time image analysis and computer vision.

---

## Key Contributions

* **Reliable Edge and Line Detection:** The framework employs `cv2.Canny` and `cv2.HoughLines` to robustly identify cue boundaries, even under challenging lighting and cluttered backgrounds.
* **Adaptive Ball and Background Suppression:** Billiard balls are detected via `cv2.HoughCircles` and masked, while histogram-based intensity thresholding removes background pixels, isolating the cue for further processing.
* **Precise Geometric Alignment:** Cue edges are used to compute the mean orientation angle, which is converted to degrees, complemented, and applied via `ndimage.rotate`, achieving sub-degree rotational accuracy.
* **Modular and Extensible Design:** Each stage is independent and adaptable, enabling future extensions for other sports equipment, real-time video tracking, or integration with AI-based analytics.
* **Experimental Validation:** The pipeline was tested across multiple cue orientations and positions, demonstrating consistent alignment and reproducibility.

<img width="1434" height="944" alt="image" src="https://github.com/user-attachments/assets/586d9085-f5f8-4db3-9ef8-b51123207c3f" />

---

## Methodology

### A. Cue Line Detection and Orientation

1. **Preprocessing:** Images are loaded in **BGR** and converted to **RGB** for improved edge detection.
2. **Edge Detection:** `cv2.Canny` is applied with optimized thresholds (100 and 200) to extract the linear boundaries of the cue.
3. **Line Extraction:** `cv2.HoughLines` identifies straight lines corresponding to cue edges. Optimal parameters include a line width of 1 pixel and an accumulator threshold of 200. Using RGB images instead of grayscale significantly improves line detection performance.
4. **Angle Computation:** Detected lines are represented by **rho** (distance from origin) and **theta** (angle in radians). The mean theta is computed, converted to degrees, and complemented to yield the cue’s orientation.
5. **Rotation:** The cue is rotated using `ndimage.rotate`. As the function rotates clockwise by default, the rotation angle is multiplied by -1 to align with the reference image, resulting in a final rotation of **-51.50°**.

### B. Cue Isolation

1. **Ball Detection and Masking:** The grayscale image undergoes `cv2.HoughCircles()` detection to locate billiard balls, particularly those near the cue, which are filled with black.
2. **Background Suppression:** Pixel intensity histograms identify the tablecloth intensity (`max_intensity_index`). All pixels below this threshold are blackened.
3. **Edge Integration:** The image is reloaded in **RGB**, edges are detected, and zero-intensity pixels from grayscale preprocessing are set to 1.
4. **Cue Line Reinforcement:** `cv2.HoughLines` detects lines above and below the cue; corresponding pixels in the grayscale image are set to 0, leaving only cue edges.
5. **Pixel Range Masking:** Row-wise parsing ensures only pixels between the detected lines retain their original intensity; all others are blackened.
6. **Final Transformation:** Rotation and minor adjustments produce the final **rotated grayscale image**, with the cue fully isolated and aligned.

---

## Applications and Impact

This framework provides a **lightweight, interpretable, and reproducible method** for sports image analysis. Potential applications include:

* Automated cue alignment for **player training systems**
* Preprocessing for **AI-driven analytics in snooker or billiard sports**
* Adaptation for **other linear sports equipment** or structured object detection tasks

By combining classical image processing techniques in a systematic pipeline, this work demonstrates that **high-precision alignment and isolation** can be achieved without relying on deep learning models, making it accessible for research, prototyping, and educational purposes.

---

## Usage

The code is designed for **Google Colab** or **Kaggle** environments. Libraries must be loaded first, and cells executed sequentially.

```python
# Example: Load and rotate cue image
import cv2
from scipy import ndimage

img = cv2.imread('image32.png')
# Apply edge detection, line extraction, and rotation
```

---

## License

This repository is intended for **educational and research purposes** as part of coursework at Istanbul Technical University.
<img width="950" height="482" alt="image" src="https://github.com/user-attachments/assets/8f0efa0e-613f-4cc2-8885-a32fa1da576a" />
