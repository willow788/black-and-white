# Black-and-White Image Colorization (OpenCV DNN)

This repository contains a Jupyter notebook (`main.ipynb`) that colorizes a black-and-white (grayscale) image using OpenCV’s DNN module and a pre-trained Caffe colorization model.

The notebook loads the model files, runs inference to predict the **a/b chrominance channels** in **LAB** color space, then converts the result back to **BGR** and displays the original image next to the colorized output.

---

## What’s inside

- `main.ipynb` — end-to-end colorization workflow:
  - loads a Caffe model (`.prototxt` + `.caffemodel`)
  - loads cluster centers (`pts_in_hull.npy`)
  - preprocesses an image into LAB space
  - predicts the `ab` channels and reconstructs a full-color image
  - displays a side-by-side comparison using `cv2.imshow`

---

## Requirements

- Python 3.x
- `opencv-python` (needs `cv2.dnn`)
- `numpy`

Example install:

```bash
pip install opencv-python numpy
```

---

## Model files expected

The notebook expects these files to exist:

- `Model/colorization_deploy_v2.prototxt`
- `Model/colorization_release_v2.caffemodel`
- `Model/pts_in_hull.npy`

> Note: In the notebook, two paths use Windows-style backslashes:
> - `Model\colorization_deploy_v2.prototxt`
> - `Model\colorization_release_v2.caffemodel`
>
> If you are on macOS/Linux, change them to forward slashes (`Model/...`) or use `os.path.join(...)`.

---

## Input image

By default, the notebook reads:

- `paris.jpg`

Place `paris.jpg` in the repository root (same folder as `main.ipynb`), or update the path in the notebook:

```python
img = cv2.imread('paris.jpg')
```

---

## How to run

1. Ensure the model files are in the `Model/` directory (see above).
2. Add your input image (default: `paris.jpg`) to the repo root.
3. Open and run the notebook:

```bash
jupyter notebook main.ipynb
```

4. Run all cells. A window titled **"Original and Colorised"** should appear showing the original image and the colorized version side-by-side.

---

## Output

The notebook displays the result via an OpenCV GUI window:

- left: original image
- right: colorized image

If you want to save the output instead of (or in addition to) displaying it, you can add something like:

```python
cv2.imwrite("colorized.png", colorised)
```

---

## Notes / Troubleshooting

- `cv2.imshow` requires a GUI environment. If you run this in a headless environment (some servers, some notebook environments), the window may not open.
- The notebook resizes both images to `640x640` before stacking/displaying.


This approach uses the widely shared OpenCV DNN colorization model architecture (Caffe-based) that predicts `ab` channels in LAB space from the `L` channel.
