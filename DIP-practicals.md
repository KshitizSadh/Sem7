# DU Digital Image Processing (DSE7a) — Practical Record

**Practicals covered**

1. Basic image operations (read, resize, gray-scale, B&W, profile, RGB planes, write 2-D data)
2. Point processing (negative, flip, thresholding, contrast stretching)
3. Image arithmetic (addition, subtraction, mean, brightness)
4. Image logical operations (AND, OR, intersection, XOR watermarking, NOT)
5. Histogram calculation and equalization (inbuilt and manual)
6. Geometric transformations (translation, scaling, rotation, shrinking, zooming)
7. Noise models and restoration (salt & pepper, Gaussian, median, Wiener)
8. Spatial filtering (1-D/2-D convolution, 3×3 low-pass and high-pass masks)
9. Frequency domain filtering (FFT, ideal/Butterworth LPF & HPF, IFFT)
10. Edge detection with different masks
11. Morphological operations (erosion, dilation, opening, closing)

> **How to use this file:** Each practical has an aim, theory, a complete
> Python program, and the expected output. Every program runs on its own.
> If a file named `input.jpg` (or `input2.jpg` where two images are needed)
> is in the same folder, the program uses it; otherwise it falls back to a
> built-in sample image from scikit-image, so it always runs.

---

## Setup

```bash
pip install opencv-python scikit-image scipy matplotlib numpy
```

| Library | Used for |
|---|---|
| `opencv-python` (`cv2`) | Reading/writing images, colour conversion, filtering, transforms |
| `scikit-image` (`skimage`) | Sample images, noise models, restoration |
| `numpy` | Images are NumPy arrays; all manual (non-inbuilt) operations |
| `scipy` | Convolution, Wiener filter |
| `matplotlib` | Displaying images, plots and histograms |

**Note:** OpenCV loads colour images in **BGR** order, while matplotlib
expects **RGB**. The programs convert with `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)`
before displaying.

---

# Practical 1 — Basic Image Operations

## Aim

To perform the following:

- a. Read and display an image.
- b. Resize a given image.
- c. Convert a given colour image into a corresponding gray-scale image.
- d. Convert a given colour/gray-scale image into a black & white image.
- e. Draw the image profile.
- f. Separate a given colour image into three R, G and B planes.
- g. Create a colour image using separate R, G and B planes.
- h. Write given 2-D data in an image file.

## Theory

A digital image is a 2-D function $`f(x, y)`$ sampled on a grid and
quantized to discrete intensity levels. In Python it is a NumPy array:

- **Gray-scale image:** shape `(M, N)`, each pixel in `[0, 255]` for 8-bit images.
- **Colour image:** shape `(M, N, 3)`, one plane per channel (R, G, B).

**Gray-scale conversion** uses the weighted luminance formula (ITU-R BT.601):

```math
Y = 0.299R + 0.587G + 0.114B
```

**Black & white (binary) conversion** applies a threshold $`T`$:

```math
g(x,y) = \begin{cases} 255 & f(x,y) \ge T \\ 0 & f(x,y) < T \end{cases}
```

**Image profile** is a plot of pixel intensities along a line (e.g. one
row) of the image. It shows how brightness changes across the image.

**Resizing** changes the number of pixels using interpolation
(nearest-neighbour, bilinear, bicubic).

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

# a. Read and display an image
if os.path.exists("input.jpg"):
    img = cv2.imread("input.jpg")                      # BGR
else:
    img = cv2.cvtColor(data.astronaut(), cv2.COLOR_RGB2BGR)
rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
print("Original shape:", img.shape, "dtype:", img.dtype)

# b. Resize the image (to half size)
resized = cv2.resize(img, None, fx=0.5, fy=0.5, interpolation=cv2.INTER_AREA)
print("Resized shape:", resized.shape)

# c. Colour -> gray-scale
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# d. Gray-scale -> black & white (threshold 127)
_, bw = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)

# e. Image profile along the middle row
row = gray.shape[0] // 2
profile = gray[row, :]

# f. Separate R, G, B planes
B, G, R = cv2.split(img)

# g. Recombine planes into a colour image
merged = cv2.merge([B, G, R])
print("Merged equals original:", np.array_equal(merged, img))

# h. Write 2-D data into an image file
data2d = np.tile(np.linspace(0, 255, 256, dtype=np.uint8), (100, 1))  # gradient
cv2.imwrite("gradient_2d.png", data2d)
cv2.imwrite("gray_output.png", gray)
print("Saved gradient_2d.png and gray_output.png")

# Display
fig, ax = plt.subplots(3, 3, figsize=(12, 11))
ax[0, 0].imshow(rgb);                       ax[0, 0].set_title("a. Original")
ax[0, 1].imshow(cv2.cvtColor(resized, cv2.COLOR_BGR2RGB)); ax[0, 1].set_title(f"b. Resized {resized.shape[1]}x{resized.shape[0]}")
ax[0, 2].imshow(gray, cmap="gray");         ax[0, 2].set_title("c. Gray-scale")
ax[1, 0].imshow(bw, cmap="gray");           ax[1, 0].set_title("d. Black & White")
ax[1, 1].plot(profile);                     ax[1, 1].set_title(f"e. Profile of row {row}")
ax[1, 1].set_xlabel("Column"); ax[1, 1].set_ylabel("Intensity")
ax[1, 2].imshow(R, cmap="Reds_r");          ax[1, 2].set_title("f. Red plane")
ax[2, 0].imshow(G, cmap="Greens_r");        ax[2, 0].set_title("f. Green plane")
ax[2, 1].imshow(B, cmap="Blues_r");         ax[2, 1].set_title("f. Blue plane")
ax[2, 2].imshow(cv2.cvtColor(merged, cv2.COLOR_BGR2RGB)); ax[2, 2].set_title("g. Merged R+G+B")
for a in ax.flat:
    if a is not ax[1, 1]:
        a.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- The original, resized, gray-scale and binary images are displayed.
- The profile plot shows intensity (0–255) against column number for the middle row; peaks are bright regions, dips are dark regions.
- The three planes appear as separate single-channel images; the merged image is identical to the original (`Merged equals original: True`).
- `gradient_2d.png` (a left-to-right black-to-white ramp) and `gray_output.png` are saved to disk.

## Result

Basic image I/O, resizing, colour conversions, profiling and channel
operations were performed successfully.

---

# Practical 2 — Point Processing

## Aim

To write and execute image processing programs using the point
processing method:

- a. Obtain negative image
- b. Obtain flip image
- c. Thresholding
- d. Contrast stretching

## Theory

In **point processing**, each output pixel depends only on the input
pixel at the same location: $`s = T(r)`$, where $`r`$ is the input
intensity and $`s`$ the output intensity.

**Negative** (for $`L`$ gray levels, $`L = 256`$):

```math
s = (L - 1) - r
```

**Flip** reverses pixel order along an axis (horizontal: columns reversed;
vertical: rows reversed). Strictly it is a geometric remap, but it is
computed pixel-by-pixel without neighbourhood information.

**Thresholding:**

```math
s = \begin{cases} L-1 & r \ge T \\ 0 & r < T \end{cases}
```

**Contrast stretching** (min–max normalisation) maps the input range
$`[r_{min}, r_{max}]`$ to the full range $`[0, L-1]`$:

```math
s = \frac{r - r_{min}}{r_{max} - r_{min}} (L - 1)
```

A piecewise-linear version uses control points $`(r_1, s_1)`$ and $`(r_2, s_2)`$.

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

if os.path.exists("input.jpg"):
    gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE)
else:
    gray = data.camera()

# Make a low-contrast version to demonstrate stretching
low_contrast = (gray * 0.4 + 70).astype(np.uint8)   # range approx. 70..172

# a. Negative
negative = 255 - gray

# b. Flip
flip_h = gray[:, ::-1]          # horizontal (left-right)
flip_v = gray[::-1, :]          # vertical (up-down)
# Equivalent OpenCV: cv2.flip(gray, 1), cv2.flip(gray, 0)

# c. Thresholding (manual)
T = 128
thresh = np.where(gray >= T, 255, 0).astype(np.uint8)

# d. Contrast stretching (min-max)
r_min, r_max = low_contrast.min(), low_contrast.max()
stretched = ((low_contrast.astype(np.float64) - r_min) / (r_max - r_min) * 255).astype(np.uint8)
print(f"Low-contrast range: {r_min}-{r_max} -> stretched range: {stretched.min()}-{stretched.max()}")

# d (alt). Piecewise-linear contrast stretching
def piecewise_stretch(img, r1, s1, r2, s2):
    img = img.astype(np.float64)
    out = np.zeros_like(img)
    m1 = img < r1
    m2 = (img >= r1) & (img <= r2)
    m3 = img > r2
    out[m1] = s1 / r1 * img[m1]
    out[m2] = (s2 - s1) / (r2 - r1) * (img[m2] - r1) + s1
    out[m3] = (255 - s2) / (255 - r2) * (img[m3] - r2) + s2
    return np.clip(out, 0, 255).astype(np.uint8)

piecewise = piecewise_stretch(low_contrast, 70, 20, 172, 235)

images = [gray, negative, flip_h, flip_v, thresh, low_contrast, stretched, piecewise]
titles = ["Original", "a. Negative", "b. Horizontal flip", "b. Vertical flip",
          f"c. Threshold (T={T})", "Low-contrast input", "d. Min-max stretched", "d. Piecewise stretched"]
fig, ax = plt.subplots(2, 4, figsize=(16, 8))
for a, im, t in zip(ax.flat, images, titles):
    a.imshow(im, cmap="gray", vmin=0, vmax=255); a.set_title(t); a.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- The negative shows dark areas as bright and vice versa.
- Horizontal and vertical flips are mirror images of the original.
- The thresholded image contains only black (0) and white (255) pixels.
- The low-contrast input looks washed out; after stretching its range expands to 0–255 and details become clearer.

## Result

Negative, flip, thresholding and contrast stretching were implemented
using point processing.

---

# Practical 3 — Image Arithmetic Operations

## Aim

To write and execute programs for image arithmetic operations:

- a. Addition of two images
- b. Subtract one image from another image
- c. Calculate the mean value of an image
- d. Different brightness by changing the mean value

## Theory

Arithmetic operations are applied pixel-by-pixel between two images of
the **same size**:

```math
g(x,y) = f_1(x,y) \pm f_2(x,y)
```

With 8-bit images, results must be kept in `[0, 255]`:

- **Saturation arithmetic** (`cv2.add`, `cv2.subtract`) clips values, e.g. 250 + 10 = 255.
- **NumPy `uint8` arithmetic** wraps around (modulo 256), e.g. 250 + 10 = 4 — usually not wanted.

**Mean** intensity of an $`M \times N`$ image:

```math
\mu = \frac{1}{MN} \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} f(x,y)
```

**Brightness** is changed by adding a constant $`c`$ to every pixel,
which shifts the mean by $`c`$ (until clipping occurs).
Subtraction is used for change detection: identical regions become 0.

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

def load_gray(path, fallback):
    return cv2.imread(path, cv2.IMREAD_GRAYSCALE) if os.path.exists(path) else fallback

img1 = load_gray("input.jpg", data.camera())
img2 = load_gray("input2.jpg", cv2.resize(data.moon(), (img1.shape[1], img1.shape[0])))
img2 = cv2.resize(img2, (img1.shape[1], img1.shape[0]))     # sizes must match

# a. Addition
add_sat = cv2.add(img1, img2)                       # saturated (clipped)
add_wrap = img1 + img2                              # uint8 wrap-around
blend = cv2.addWeighted(img1, 0.5, img2, 0.5, 0)    # weighted average

# b. Subtraction
sub = cv2.subtract(img1, img2)
absdiff = cv2.absdiff(img1, img2)

# c. Mean value
mean_inbuilt = np.mean(img1)
mean_manual = img1.astype(np.float64).sum() / img1.size
print(f"Mean (np.mean): {mean_inbuilt:.2f}, Mean (manual): {mean_manual:.2f}")

# d. Brightness by changing mean
brighter = cv2.add(img1, 60)
darker = cv2.subtract(img1, 60)
print(f"Mean original: {img1.mean():.2f}, brighter: {brighter.mean():.2f}, darker: {darker.mean():.2f}")

# Set image to an exact target mean
target = 180
shifted = np.clip(img1.astype(np.float64) + (target - img1.mean()), 0, 255).astype(np.uint8)
print(f"Target mean {target} -> achieved {shifted.mean():.2f} (lower if clipping occurs)")

images = [img1, img2, add_sat, add_wrap, blend, sub, absdiff, brighter, darker]
titles = ["Image 1", "Image 2", "a. cv2.add (saturated)", "a. NumPy + (wrap-around)",
          "a. Weighted blend", "b. cv2.subtract", "b. Absolute difference",
          f"d. Brighter (mean {brighter.mean():.0f})", f"d. Darker (mean {darker.mean():.0f})"]
fig, ax = plt.subplots(3, 3, figsize=(12, 12))
for a, im, t in zip(ax.flat, images, titles):
    a.imshow(im, cmap="gray", vmin=0, vmax=255); a.set_title(t); a.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- `cv2.add` gives a bright, clipped sum; NumPy `+` shows artefacts where values wrapped past 255.
- The weighted blend shows both images superimposed.
- The difference images are dark where the images are similar.
- The printed means match between the inbuilt and manual methods; the brighter and darker images have means approximately 60 above and below the original.

## Result

Image addition, subtraction, mean calculation and brightness adjustment
were performed.

---

# Practical 4 — Image Logical Operations

## Aim

To write and execute programs for image logical operations:

- a. AND operation between two images
- b. OR operation between two images
- c. Calculate the intersection of two images
- d. Watermarking using XOR operation
- e. NOT operation (negative image)

## Theory

Logical operations work **bit-by-bit** on pixel values (for binary
images, 0 = false and 255 = true).

| Operation | Rule | Typical use |
|---|---|---|
| AND | 1 only if both bits are 1 | Masking / region of interest; intersection |
| OR | 1 if either bit is 1 | Union of regions |
| XOR | 1 if bits differ | Watermarking; change detection |
| NOT | Inverts every bit | Negative: $`255 - r`$ for 8-bit images |

**Intersection** of two binary images is the set of pixels that are
white in both, i.e. $`A \cap B = A \text{ AND } B`$.

**XOR watermarking** relies on $`(I \oplus W) \oplus W = I`$: XOR-ing the
image with a watermark embeds it, and XOR-ing again with the same
watermark recovers the original exactly. This makes it a simple
reversible (but not secure) scheme.

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE) if os.path.exists("input.jpg") else data.camera()
h, w = gray.shape

# Two binary shape images for AND / OR / intersection
A = np.zeros((h, w), np.uint8); cv2.circle(A, (w // 2 - 60, h // 2), 150, 255, -1)
B = np.zeros((h, w), np.uint8); cv2.rectangle(B, (w // 2 - 40, h // 2 - 120), (w // 2 + 200, h // 2 + 120), 255, -1)

# a. AND
and_img = cv2.bitwise_and(A, B)
masked = cv2.bitwise_and(gray, gray, mask=A)     # AND used as a mask on a real image

# b. OR
or_img = cv2.bitwise_or(A, B)

# c. Intersection (pixels white in both)
intersection = np.logical_and(A > 0, B > 0).astype(np.uint8) * 255
print("Intersection pixels:", int(np.count_nonzero(intersection)),
      "| equals AND:", np.array_equal(intersection, and_img))

# d. Watermarking using XOR
wm = np.zeros((h, w), np.uint8)
cv2.putText(wm, "DU-DIP", (w // 8, h // 2), cv2.FONT_HERSHEY_SIMPLEX, 4, 60, 12)
watermarked = cv2.bitwise_xor(gray, wm)
recovered = cv2.bitwise_xor(watermarked, wm)
print("Original recovered exactly:", np.array_equal(recovered, gray))

# e. NOT (negative)
not_img = cv2.bitwise_not(gray)
print("NOT equals 255 - image:", np.array_equal(not_img, 255 - gray))

images = [A, B, and_img, or_img, intersection, masked, wm, watermarked, recovered, not_img]
titles = ["Image A", "Image B", "a. A AND B", "b. A OR B", "c. Intersection",
          "a. AND as mask", "d. Watermark", "d. Image XOR watermark", "d. Recovered", "e. NOT"]
fig, ax = plt.subplots(2, 5, figsize=(18, 8))
for a, im, t in zip(ax.flat, images, titles):
    a.imshow(im, cmap="gray", vmin=0, vmax=255); a.set_title(t); a.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- AND shows only the overlap of the circle and rectangle; OR shows their combined area.
- The intersection is identical to the AND result.
- The watermark text is visible in the XOR image, and the second XOR recovers the original exactly (`True`).
- NOT produces the negative image, identical to `255 - image`.

## Result

AND, OR, intersection, XOR watermarking and NOT operations were
implemented.

---

# Practical 5 — Histogram Calculation and Equalization

## Aim

To write and execute a program for histogram calculation and equalization:

- a. Using inbuilt functions
- b. Without using inbuilt functions

## Theory

The **histogram** of an image with $`L`$ gray levels is
$`h(r_k) = n_k`$, the number of pixels with intensity $`r_k`$.
The normalised histogram (probability) is:

```math
p(r_k) = \frac{n_k}{MN}, \quad k = 0, 1, \dots, L-1
```

**Histogram equalization** spreads intensities so the output histogram
is approximately uniform, improving contrast. It maps each level using
the cumulative distribution function (CDF):

```math
s_k = T(r_k) = (L-1) \sum_{j=0}^{k} p(r_j)
```

and rounds $`s_k`$ to the nearest integer.

**Procedure (manual):**

1. Count pixels at each level (histogram).
2. Divide by $`MN`$ to get the PDF.
3. Compute the cumulative sum (CDF).
4. Multiply by $`L-1`$ and round to get the mapping.
5. Replace each pixel $`r_k`$ by $`s_k`$.

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE) if os.path.exists("input.jpg") else data.moon()

# ---------- a. Using inbuilt functions ----------
hist_cv = cv2.calcHist([gray], [0], None, [256], [0, 256]).ravel()
eq_cv = cv2.equalizeHist(gray)

# ---------- b. Without inbuilt functions ----------
def histogram(img):
    h = np.zeros(256, dtype=np.int64)
    for value in img.ravel():          # explicit count of every pixel
        h[value] += 1
    return h

def equalize(img):
    h = histogram(img)
    pdf = h / img.size
    cdf = np.zeros(256)
    running = 0.0
    for k in range(256):               # cumulative sum
        running += pdf[k]
        cdf[k] = running
    mapping = np.round(255 * cdf).astype(np.uint8)
    out = np.zeros_like(img)
    rows, cols = img.shape
    for i in range(rows):
        for j in range(cols):
            out[i, j] = mapping[img[i, j]]
    return out

hist_manual = histogram(gray)
eq_manual = equalize(gray)
print("Manual histogram matches cv2.calcHist:", np.array_equal(hist_manual, hist_cv.astype(np.int64)))
print("Max difference between manual and cv2 equalization:",
      int(np.abs(eq_manual.astype(int) - eq_cv.astype(int)).max()), "gray level(s)")

fig, ax = plt.subplots(3, 3, figsize=(15, 12))
for col, (im, t) in enumerate([(gray, "Original"), (eq_cv, "a. cv2.equalizeHist"), (eq_manual, "b. Manual equalization")]):
    ax[0, col].imshow(im, cmap="gray", vmin=0, vmax=255); ax[0, col].set_title(t); ax[0, col].axis("off")
    h = histogram(im)
    ax[1, col].bar(range(256), h, width=1); ax[1, col].set_title(f"Histogram — {t}")
    ax[2, col].plot(np.cumsum(h) / im.size); ax[2, col].set_title(f"CDF — {t}")
    ax[2, col].set_xlabel("Gray level")
plt.tight_layout()
plt.show()
```

## Output

- The original histogram is concentrated in a narrow band; after equalization it spreads across 0–255.
- The CDF of the equalized image is close to a straight line (uniform distribution).
- The manual histogram matches `cv2.calcHist` exactly. Manual and OpenCV equalization agree to within a gray level or two (OpenCV uses a slightly different rounding/normalisation of the CDF).

## Result

Histogram calculation and equalization were implemented with and
without inbuilt functions, and contrast was improved.

---

# Practical 6 — Geometric Transformations

## Aim

To write and execute a program performing the following geometric
transformations on an image:

- a. Translation
- b. Scaling
- c. Rotation
- d. Shrinking
- e. Zooming

## Theory

Geometric transformations change the **spatial position** of pixels.
In homogeneous coordinates, a point $`(x, y)`$ is mapped by a matrix:

**Translation** by $`(t_x, t_y)`$:

```math
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} =
\begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix}
\begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
```

**Scaling** by $`(s_x, s_y)`$:

```math
\begin{bmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{bmatrix}
```

**Rotation** by angle $`\theta`$ (about the origin):

```math
\begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}
```

**Shrinking** reduces image size (scale factor < 1, e.g. by keeping every
k-th pixel — decimation). **Zooming** enlarges it (scale factor > 1);
new pixel values are found by **interpolation**:

- **Nearest neighbour** — copies the closest pixel (blocky).
- **Bilinear** — weighted average of 4 neighbours (smoother).
- **Bicubic** — uses 16 neighbours (smoothest).

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE) if os.path.exists("input.jpg") else data.camera()
h, w = gray.shape

# a. Translation by (tx, ty)
tx, ty = 80, 50
M_t = np.float32([[1, 0, tx], [0, 1, ty]])
translated = cv2.warpAffine(gray, M_t, (w, h))

# b. Scaling (non-uniform: 1.5x wide, 0.75x tall)
scaled = cv2.resize(gray, None, fx=1.5, fy=0.75, interpolation=cv2.INTER_LINEAR)

# c. Rotation by 45 degrees about the centre
M_r = cv2.getRotationMatrix2D((w / 2, h / 2), 45, 1.0)
rotated = cv2.warpAffine(gray, M_r, (w, h))

# d. Shrinking by factor 4 (manual decimation and inbuilt)
shrunk_manual = gray[::4, ::4]
shrunk_cv = cv2.resize(gray, (w // 4, h // 4), interpolation=cv2.INTER_AREA)

# e. Zooming a 64x64 crop by factor 4
crop = gray[100:164, 200:264]
zoom_manual = np.repeat(np.repeat(crop, 4, axis=0), 4, axis=1)          # pixel replication
zoom_nearest = cv2.resize(crop, None, fx=4, fy=4, interpolation=cv2.INTER_NEAREST)
zoom_bilinear = cv2.resize(crop, None, fx=4, fy=4, interpolation=cv2.INTER_LINEAR)
zoom_bicubic = cv2.resize(crop, None, fx=4, fy=4, interpolation=cv2.INTER_CUBIC)

for name, im in [("Original", gray), ("Scaled", scaled), ("Shrunk", shrunk_cv), ("Zoomed", zoom_bicubic)]:
    print(f"{name:9s} shape: {im.shape}")

images = [gray, translated, scaled, rotated, shrunk_manual, shrunk_cv,
          crop, zoom_manual, zoom_bilinear, zoom_bicubic]
titles = ["Original", f"a. Translated ({tx},{ty})", "b. Scaled 1.5x / 0.75x", "c. Rotated 45°",
          "d. Shrunk (decimation)", "d. Shrunk (INTER_AREA)", "Crop 64x64",
          "e. Zoom — replication", "e. Zoom — bilinear", "e. Zoom — bicubic"]
fig, ax = plt.subplots(2, 5, figsize=(20, 8))
for a, im, t in zip(ax.flat, images, titles):
    a.imshow(im, cmap="gray", vmin=0, vmax=255); a.set_title(t); a.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- The translated image is shifted right and down, with black filling the uncovered area.
- The scaled image is wider and shorter; the rotated image is turned 45° with black corners.
- Shrunk images are a quarter of the size; decimation shows more aliasing than `INTER_AREA`.
- Zoomed crops show the effect of interpolation: replication/nearest is blocky, bilinear is smoother, bicubic is smoothest.

## Result

Translation, scaling, rotation, shrinking and zooming were performed.

---

# Practical 7 — Noise Models and Image Restoration

## Aim

To understand various image noise models and to write programs for:

- a. Image restoration
- b. Removing salt and pepper noise
- c. Minimizing Gaussian noise
- d. Median filter and Wiener filter

## Theory

The degradation model is:

```math
g(x,y) = h(x,y) \star f(x,y) + \eta(x,y)
```

where $`f`$ is the original image, $`h`$ the degradation function (e.g.
blur), $`\star`$ convolution, and $`\eta`$ additive noise.

**Common noise models**

| Noise | PDF / description | Best filter |
|---|---|---|
| Gaussian | $`p(z) = \frac{1}{\sqrt{2\pi}\sigma} e^{-(z-\mu)^2/2\sigma^2}`$ | Mean, Gaussian, Wiener |
| Salt & pepper (impulse) | Random pixels set to 0 or 255 | Median |
| Uniform | Constant probability over $`[a, b]`$ | Mean |
| Rayleigh, Gamma (Erlang), Exponential | Skewed PDFs (range imaging, laser) | Mean / adaptive |

**Median filter** replaces each pixel by the median of its neighbourhood.
Impulse values are extremes, so they are never chosen as the median and
edges are preserved.

**Wiener filter** minimises the mean-square error between the estimate
and the original. In the frequency domain:

```math
\hat{F}(u,v) = \left[ \frac{H^*(u,v)}{|H(u,v)|^2 + K} \right] G(u,v)
```

where $`K`$ approximates the noise-to-signal power ratio. With no blur
($`H = 1`$), it acts as an adaptive local smoothing filter
(`scipy.signal.wiener`).

**PSNR** (peak signal-to-noise ratio, in dB) measures quality — higher is better:

```math
\text{PSNR} = 10 \log_{10} \frac{255^2}{\text{MSE}}
```

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import wiener
from skimage import data, util, restoration
from skimage.metrics import peak_signal_noise_ratio as psnr

gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE) if os.path.exists("input.jpg") else data.camera()
f = util.img_as_float(gray)                        # work in [0, 1]
rng = np.random.default_rng(0)

# ----- Noise models -----
sp_noisy = util.random_noise(f, mode="s&p", amount=0.08, rng=0)
gauss_noisy = util.random_noise(f, mode="gaussian", var=0.01, rng=0)
uniform_noisy = np.clip(f + rng.uniform(-0.2, 0.2, f.shape), 0, 1)
rayleigh_noisy = np.clip(f + rng.rayleigh(0.08, f.shape) - 0.08 * np.sqrt(np.pi / 2), 0, 1)

# ----- b. Remove salt & pepper noise: median vs mean -----
sp_u8 = util.img_as_ubyte(sp_noisy)
sp_median = cv2.medianBlur(sp_u8, 3)
sp_mean = cv2.blur(sp_u8, (3, 3))

# ----- c. Minimize Gaussian noise: Gaussian blur and Wiener -----
g_u8 = util.img_as_ubyte(gauss_noisy)
g_gauss = cv2.GaussianBlur(g_u8, (5, 5), 1.0)
g_wiener = np.clip(wiener(gauss_noisy, (5, 5)), 0, 1)

# ----- a. Image restoration: blur + noise, then Wiener deconvolution -----
psf = np.ones((5, 5)) / 25                                         # known blur (degradation function h)
blurred = cv2.filter2D(f, -1, psf, borderType=cv2.BORDER_REFLECT)
degraded = np.clip(blurred + rng.normal(0, 0.01, f.shape), 0, 1)
restored = np.clip(restoration.wiener(degraded, psf, balance=0.05), 0, 1)

def score(img):
    return psnr(f, util.img_as_float(img), data_range=1.0)

print("PSNR (dB) — higher is better")
print(f"  S&P noisy        : {score(sp_noisy):6.2f}")
print(f"  S&P -> mean 3x3  : {score(sp_mean):6.2f}")
print(f"  S&P -> median 3x3: {score(sp_median):6.2f}")
print(f"  Gaussian noisy   : {score(gauss_noisy):6.2f}")
print(f"  Gauss -> Gaussian: {score(g_gauss):6.2f}")
print(f"  Gauss -> Wiener  : {score(g_wiener):6.2f}")
print(f"  Blur+noise       : {score(degraded):6.2f}")
print(f"  Wiener restored  : {score(restored):6.2f}")

images = [f, sp_noisy, gauss_noisy, uniform_noisy, rayleigh_noisy,
          sp_mean, sp_median, g_gauss, g_wiener, f,
          degraded, restored]
titles = ["Original", "Salt & pepper", "Gaussian", "Uniform", "Rayleigh",
          f"b. S&P mean ({score(sp_mean):.1f} dB)", f"b./d. S&P median ({score(sp_median):.1f} dB)",
          f"c. Gaussian blur ({score(g_gauss):.1f} dB)", f"c./d. Wiener ({score(g_wiener):.1f} dB)", "Original",
          f"a. Blurred + noise ({score(degraded):.1f} dB)", f"a. Wiener restored ({score(restored):.1f} dB)"]
fig, ax = plt.subplots(3, 5, figsize=(20, 12))
for a in ax.flat:
    a.axis("off")
for a, im, t in zip(ax.flat, images, titles):
    a.imshow(im, cmap="gray", vmin=0, vmax=1); a.set_title(t, fontsize=10)
plt.tight_layout()
plt.show()
```

## Output

- The first row shows the four noise models: impulse dots (salt & pepper), fine grain (Gaussian, uniform) and a brightened, skewed grain (Rayleigh).
- For salt & pepper noise, the median filter gives a much higher PSNR than the mean filter, which only smears the dots.
- For Gaussian noise, both the Gaussian blur and the Wiener filter raise the PSNR; Wiener preserves edges better because it adapts to local variance.
- Wiener deconvolution of the blurred, noisy image recovers sharper detail and a higher PSNR than the degraded input.

## Result

Different noise models were generated, and median, Gaussian and Wiener
filters were used to remove noise and restore a degraded image.

---

# Practical 8 — Spatial Filtering

## Aim

To write and execute programs to remove noise from images using spatial
filtering:

- a. Understand 1-D and 2-D convolution
- b. Use a 3×3 mask for a low-pass filter and a high-pass filter

## Theory

**Convolution** slides a mask (kernel) $`w`$ over the image; the output
at each position is the weighted sum of the neighbourhood.

1-D convolution:

```math
(f * w)(x) = \sum_{s=-a}^{a} w(s)\, f(x - s)
```

2-D convolution with an $`m \times n`$ mask ($`m = 2a+1`$, $`n = 2b+1`$):

```math
g(x,y) = \sum_{s=-a}^{a} \sum_{t=-b}^{b} w(s,t)\, f(x-s, y-t)
```

**Correlation** is the same without flipping the kernel. For symmetric
kernels the two are identical; `cv2.filter2D` actually computes correlation.

**Low-pass (smoothing) 3×3 mask** — averages neighbours, removes noise, blurs edges:

```math
\frac{1}{9}\begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix}
```

**High-pass (sharpening) 3×3 mask** — coefficients sum to 0, keeps edges and detail:

```math
\begin{bmatrix} -1 & -1 & -1 \\ -1 & 8 & -1 \\ -1 & -1 & -1 \end{bmatrix}
```

Adding the high-pass output back to the original gives a sharpened
image (high-boost style), e.g. with the mask
`[[0,-1,0],[-1,5,-1],[0,-1,0]]`.

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import convolve2d
from skimage import data, util

# ---------- a. 1-D convolution ----------
def conv1d(f, w):
    n, k = len(f), len(w)
    out = np.zeros(n + k - 1)
    for i in range(len(out)):
        for j in range(k):
            if 0 <= i - j < n:
                out[i] += w[j] * f[i - j]
    return out

f1 = np.array([0, 0, 1, 0, 0, 2, 2, 2, 0, 0], dtype=float)
w1 = np.array([1, 2, 1], dtype=float) / 4
print("1-D signal       :", f1)
print("Manual 1-D conv  :", np.round(conv1d(f1, w1), 3))
print("np.convolve      :", np.round(np.convolve(f1, w1), 3))

# ---------- a. 2-D convolution (manual, zero padding, 'same' size) ----------
def conv2d(img, kernel):
    k = np.flipud(np.fliplr(kernel))           # flip for true convolution
    kh, kw = k.shape
    ph, pw = kh // 2, kw // 2
    padded = np.pad(img.astype(np.float64), ((ph, ph), (pw, pw)))
    out = np.zeros(img.shape, dtype=np.float64)
    for i in range(img.shape[0]):
        for j in range(img.shape[1]):
            out[i, j] = np.sum(padded[i:i + kh, j:j + kw] * k)
    return out

small = np.array([[10, 10, 10, 10, 10],
                  [10, 50, 50, 50, 10],
                  [10, 50, 90, 50, 10],
                  [10, 50, 50, 50, 10],
                  [10, 10, 10, 10, 10]], dtype=float)
box = np.ones((3, 3)) / 9
print("\nManual 2-D conv (5x5 example):\n", np.round(conv2d(small, box), 1))
print("Matches scipy convolve2d:", np.allclose(conv2d(small, box), convolve2d(small, box, mode="same")))

# ---------- b. 3x3 low-pass and high-pass on a noisy image ----------
gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE) if os.path.exists("input.jpg") else data.camera()
noisy = util.img_as_ubyte(util.random_noise(gray, mode="gaussian", var=0.005, rng=1))

lpf_mask = np.ones((3, 3), np.float32) / 9
hpf_mask = np.array([[-1, -1, -1], [-1, 8, -1], [-1, -1, -1]], np.float32)
sharpen_mask = np.array([[0, -1, 0], [-1, 5, -1], [0, -1, 0]], np.float32)

lpf = cv2.filter2D(noisy, -1, lpf_mask)
lpf_manual = np.clip(conv2d(noisy, lpf_mask), 0, 255).astype(np.uint8)
hpf = cv2.filter2D(gray.astype(np.float32), -1, hpf_mask)
hpf_display = cv2.convertScaleAbs(hpf)                       # |response| for display
sharpened = cv2.filter2D(gray, -1, sharpen_mask)
print("\nManual vs cv2 low-pass max diff (interior):",
      int(np.abs(lpf_manual[1:-1, 1:-1].astype(int) - lpf[1:-1, 1:-1].astype(int)).max()))

fig = plt.figure(figsize=(18, 9))
ax = fig.add_subplot(2, 4, 1)
ax.stem(f1); ax.set_title("1-D input signal")
ax = fig.add_subplot(2, 4, 2)
ax.stem(conv1d(f1, w1)); ax.set_title("1-D conv with [1,2,1]/4")
for idx, (im, t) in enumerate([(small, "2-D input 5x5"), (conv2d(small, box), "2-D conv with 3x3 box")], start=3):
    ax = fig.add_subplot(2, 4, idx); ax.imshow(im, cmap="gray"); ax.set_title(t); ax.axis("off")
for idx, (im, t) in enumerate([(noisy, "Noisy image"), (lpf, "b. 3x3 low-pass (mean)"),
                               (hpf_display, "b. 3x3 high-pass"), (sharpened, "Sharpened (original + HPF)")], start=5):
    ax = fig.add_subplot(2, 4, idx); ax.imshow(im, cmap="gray", vmin=0, vmax=255); ax.set_title(t); ax.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- The 1-D example shows the impulse and step smoothed by the `[1, 2, 1]/4` kernel; the manual result equals `np.convolve`.
- The 5×5 example shows the bright centre spread to its neighbours; the manual 2-D convolution matches `scipy.signal.convolve2d`.
- The 3×3 low-pass filter reduces the noise but blurs edges.
- The high-pass output is dark except at edges and fine texture; the sharpened image has crisper edges.

## Result

1-D and 2-D convolution were implemented, and 3×3 low-pass and
high-pass masks were applied for noise removal and sharpening.

---

# Practical 9 — Frequency Domain Filtering

## Aim

To write and execute programs for image frequency domain filtering:

- a. Apply FFT to a given image
- b. Perform low-pass and high-pass filtering in the frequency domain
- c. Apply IFFT to reconstruct the image

## Theory

The 2-D **Discrete Fourier Transform** of an $`M \times N`$ image:

```math
F(u,v) = \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} f(x,y)\, e^{-j2\pi(ux/M + vy/N)}
```

and its inverse:

```math
f(x,y) = \frac{1}{MN} \sum_{u=0}^{M-1} \sum_{v=0}^{N-1} F(u,v)\, e^{j2\pi(ux/M + vy/N)}
```

The **FFT** computes the DFT efficiently. `fftshift` moves the zero-frequency
(DC) component to the centre. The magnitude spectrum is displayed as
$`\log(1 + |F(u,v)|)`$ because its dynamic range is very large.

**Filtering steps:** FFT → shift → multiply by filter $`H(u,v)`$ →
inverse shift → IFFT → take real part.

Let $`D(u,v)`$ be the distance from the centre and $`D_0`$ the cutoff.

| Filter | Low-pass $`H(u,v)`$ | High-pass $`H(u,v)`$ |
|---|---|---|
| Ideal | 1 if $`D \le D_0`$, else 0 | 0 if $`D \le D_0`$, else 1 |
| Butterworth (order $`n`$) | $`\dfrac{1}{1 + (D/D_0)^{2n}}`$ | $`\dfrac{1}{1 + (D_0/D)^{2n}}`$ |
| Gaussian | $`e^{-D^2/2D_0^2}`$ | $`1 - e^{-D^2/2D_0^2}`$ |

The ideal filter's sharp cutoff causes **ringing**; Butterworth and
Gaussian filters have smooth transitions and little or no ringing.

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE) if os.path.exists("input.jpg") else data.camera()
f = gray.astype(np.float64)
M, N = f.shape

# a. FFT
F = np.fft.fft2(f)
F_shift = np.fft.fftshift(F)
magnitude = np.log1p(np.abs(F_shift))

# Distance grid D(u, v) from the centre
u = np.arange(M) - M / 2
v = np.arange(N) - N / 2
V, U = np.meshgrid(v, u)
D = np.sqrt(U ** 2 + V ** 2)

D0, n = 40, 2
filters = {
    "Ideal LPF":        (D <= D0).astype(float),
    "Butterworth LPF":  1 / (1 + (D / D0) ** (2 * n)),
    "Gaussian LPF":     np.exp(-(D ** 2) / (2 * D0 ** 2)),
    "Ideal HPF":        (D > D0).astype(float),
    "Butterworth HPF":  1 / (1 + (D0 / np.maximum(D, 1e-6)) ** (2 * n)),
    "Gaussian HPF":     1 - np.exp(-(D ** 2) / (2 * D0 ** 2)),
}

# b + c. Filter in frequency domain and reconstruct with IFFT
def apply(H):
    G_shift = F_shift * H
    g = np.fft.ifft2(np.fft.ifftshift(G_shift))
    return np.real(g)

results = {name: apply(H) for name, H in filters.items()}

# c. IFFT without filtering reconstructs the original
reconstructed = np.real(np.fft.ifft2(F))
print("Max reconstruction error after IFFT:", float(np.abs(reconstructed - f).max()))

fig, ax = plt.subplots(3, 6, figsize=(24, 12))
ax[0, 0].imshow(gray, cmap="gray"); ax[0, 0].set_title("Original")
ax[0, 1].imshow(magnitude, cmap="gray"); ax[0, 1].set_title("a. Log magnitude spectrum")
ax[0, 2].imshow(np.angle(F_shift), cmap="gray"); ax[0, 2].set_title("a. Phase spectrum")
ax[0, 3].imshow(reconstructed, cmap="gray"); ax[0, 3].set_title("c. IFFT reconstruction")
for a in ax[0, 4:]:
    a.set_visible(False)
for col, (name, H) in enumerate(filters.items()):
    ax[1, col].imshow(H, cmap="gray"); ax[1, col].set_title(f"{name} mask (D0={D0})")
    out = results[name]
    if "HPF" in name:
        out = np.abs(out)
    ax[2, col].imshow(out, cmap="gray"); ax[2, col].set_title(f"b./c. {name} result")
for a in ax.flat:
    a.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- The magnitude spectrum has a bright centre (DC and low frequencies) with energy falling off outward; strong lines correspond to dominant edges in the image.
- IFFT of the unfiltered spectrum reconstructs the original (error is about $`10^{-12}`$, floating-point rounding).
- Low-pass results are blurred: the ideal LPF shows visible ringing near edges, Butterworth less, Gaussian none.
- High-pass results keep only edges and fine detail; again, the ideal filter shows the most ringing.

## Result

The FFT of an image was computed, ideal, Butterworth and Gaussian
low-pass and high-pass filters were applied in the frequency domain, and
images were reconstructed using the IFFT.

---

# Practical 10 — Edge Detection Using Different Masks

## Aim

To write and execute a program for edge detection using different edge
detection masks.

## Theory

An **edge** is a location of sharp intensity change. Edges are found
using derivatives.

**First derivative (gradient):** the gradient magnitude is

```math
|\nabla f| = \sqrt{G_x^2 + G_y^2} \approx |G_x| + |G_y|
```

and its direction is $`\theta = \tan^{-1}(G_y / G_x)`$.

| Operator | $`G_x`$ mask | $`G_y`$ mask |
|---|---|---|
| Roberts (2×2) | `[[1, 0], [0, -1]]` | `[[0, 1], [-1, 0]]` |
| Prewitt | `[[-1,0,1],[-1,0,1],[-1,0,1]]` | `[[-1,-1,-1],[0,0,0],[1,1,1]]` |
| Sobel | `[[-1,0,1],[-2,0,2],[-1,0,1]]` | `[[-1,-2,-1],[0,0,0],[1,2,1]]` |

Sobel weights the centre row/column more, giving some smoothing and
better noise resistance than Prewitt.

**Second derivative (Laplacian):**

```math
\nabla^2 f = \frac{\partial^2 f}{\partial x^2} + \frac{\partial^2 f}{\partial y^2}
```

with mask `[[0,1,0],[1,-4,1],[0,1,0]]`. It is isotropic but very
sensitive to noise, so it is usually applied after Gaussian smoothing
(**LoG** — Laplacian of Gaussian).

**Canny edge detector** — a multi-stage algorithm:

1. Gaussian smoothing.
2. Gradient magnitude and direction (Sobel).
3. Non-maximum suppression (thin edges to 1 pixel).
4. Double thresholding and edge tracking by hysteresis.

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE) if os.path.exists("input.jpg") else data.camera()
f = gray.astype(np.float32)

masks = {
    "Roberts": (np.array([[1, 0], [0, -1]], np.float32),
                np.array([[0, 1], [-1, 0]], np.float32)),
    "Prewitt": (np.array([[-1, 0, 1], [-1, 0, 1], [-1, 0, 1]], np.float32),
                np.array([[-1, -1, -1], [0, 0, 0], [1, 1, 1]], np.float32)),
    "Sobel":   (np.array([[-1, 0, 1], [-2, 0, 2], [-1, 0, 1]], np.float32),
                np.array([[-1, -2, -1], [0, 0, 0], [1, 2, 1]], np.float32)),
}

def gradient_edges(img, kx, ky):
    gx = cv2.filter2D(img, -1, kx)
    gy = cv2.filter2D(img, -1, ky)
    mag = np.sqrt(gx ** 2 + gy ** 2)
    return gx, gy, np.uint8(np.clip(mag / mag.max() * 255, 0, 255))

results = {}
for name, (kx, ky) in masks.items():
    gx, gy, mag = gradient_edges(f, kx, ky)
    results[name] = mag
    if name == "Sobel":
        sobel_gx, sobel_gy = cv2.convertScaleAbs(gx), cv2.convertScaleAbs(gy)

# Laplacian (direct) and LoG (Gaussian smoothing first)
lap_mask = np.array([[0, 1, 0], [1, -4, 1], [0, 1, 0]], np.float32)
results["Laplacian"] = cv2.convertScaleAbs(cv2.filter2D(f, -1, lap_mask))
results["LoG (σ=1.5)"] = cv2.convertScaleAbs(cv2.Laplacian(cv2.GaussianBlur(gray, (0, 0), 1.5), cv2.CV_32F, ksize=3), alpha=3)

# Canny
results["Canny (50, 150)"] = cv2.Canny(gray, 50, 150)

# Binary edge maps from the Sobel magnitude
_, sobel_bin = cv2.threshold(results["Sobel"], 50, 255, cv2.THRESH_BINARY)

for name, im in results.items():
    print(f"{name:16s} edge pixels (>50): {int(np.count_nonzero(im > 50))}")

images = [gray, sobel_gx, sobel_gy, *results.values(), sobel_bin]
titles = ["Original", "Sobel Gx (vertical edges)", "Sobel Gy (horizontal edges)",
          *results.keys(), "Sobel thresholded"]
fig, ax = plt.subplots(2, 5, figsize=(20, 8))
for a, im, t in zip(ax.flat, images, titles):
    a.imshow(im, cmap="gray"); a.set_title(t); a.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- $`G_x`$ highlights vertical edges and $`G_y`$ horizontal edges.
- Roberts gives thin but noisy edges; Prewitt and Sobel give stronger, smoother edges (Sobel slightly cleaner).
- The plain Laplacian responds strongly to noise and texture; LoG is cleaner because of the Gaussian pre-smoothing.
- Canny gives thin, continuous, 1-pixel-wide edges. It also marks fine texture (e.g. grass) at thresholds (50, 150); raising them (e.g. 100, 200) keeps only the strong object boundaries.

## Result

Edges were detected using Roberts, Prewitt, Sobel, Laplacian, LoG and
Canny operators, and their outputs were compared.

---

# Practical 11 — Morphological Operations: Erosion and Dilation

## Aim

To write and execute a program for the image morphological operations
erosion and dilation (and the derived operations opening, closing and
boundary extraction).

## Theory

Morphology processes shapes in (usually binary) images using a small
**structuring element** (SE) $`B`$, such as a 3×3 square, a cross or a disk.

**Erosion** — the SE must fit entirely inside the foreground:

```math
A \ominus B = \{ z \mid (B)_z \subseteq A \}
```

It shrinks objects, removes small specks and breaks thin connections.

**Dilation** — the reflected SE must overlap the foreground:

```math
A \oplus B = \{ z \mid (\hat{B})_z \cap A \neq \emptyset \}
```

It grows objects, fills small holes and joins nearby parts.

**Derived operations**

| Operation | Definition | Effect |
|---|---|---|
| Opening | $`A \circ B = (A \ominus B) \oplus B`$ | Removes small objects/noise, smooths contours |
| Closing | $`A \bullet B = (A \oplus B) \ominus B`$ | Fills small holes and gaps |
| Boundary extraction | $`\beta(A) = A - (A \ominus B)`$ | Object outlines |
| Morphological gradient | $`(A \oplus B) - (A \ominus B)`$ | Thick outlines |

## Program

```python
import os
import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data

# Binary image: threshold the input (or the built-in "coins" image) with Otsu
gray = cv2.imread("input.jpg", cv2.IMREAD_GRAYSCALE) if os.path.exists("input.jpg") else data.coins()
_, binary = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)

# Add salt noise and small holes to show the effect of opening/closing
rng = np.random.default_rng(0)
noisy = binary.copy()
noisy[rng.random(binary.shape) < 0.02] = 255      # white specks in the background
noisy[rng.random(binary.shape) < 0.02] = 0        # black holes in the objects

se = cv2.getStructuringElement(cv2.MORPH_RECT, (3, 3))

# ---------- Manual erosion and dilation (binary, 3x3 square SE) ----------
def erode_manual(img, k=3):
    p = k // 2
    padded = np.pad(img > 0, p, constant_values=False)
    out = np.ones(img.shape, dtype=bool)
    for di in range(k):
        for dj in range(k):
            out &= padded[di:di + img.shape[0], dj:dj + img.shape[1]]
    return out.astype(np.uint8) * 255

def dilate_manual(img, k=3):
    p = k // 2
    padded = np.pad(img > 0, p, constant_values=False)
    out = np.zeros(img.shape, dtype=bool)
    for di in range(k):
        for dj in range(k):
            out |= padded[di:di + img.shape[0], dj:dj + img.shape[1]]
    return out.astype(np.uint8) * 255

# ---------- Inbuilt ----------
eroded = cv2.erode(binary, se, iterations=1)
dilated = cv2.dilate(binary, se, iterations=1)
eroded_m = erode_manual(binary)
dilated_m = dilate_manual(binary)
print("Manual erosion matches cv2 (interior):", np.array_equal(eroded_m[1:-1, 1:-1], eroded[1:-1, 1:-1]))
print("Manual dilation matches cv2 (interior):", np.array_equal(dilated_m[1:-1, 1:-1], dilated[1:-1, 1:-1]))

opened = cv2.morphologyEx(noisy, cv2.MORPH_OPEN, se)
closed = cv2.morphologyEx(noisy, cv2.MORPH_CLOSE, se)
open_close = cv2.morphologyEx(opened, cv2.MORPH_CLOSE, se)
boundary = cv2.subtract(binary, eroded)
gradient = cv2.morphologyEx(binary, cv2.MORPH_GRADIENT, se)

for name, im in [("Original", binary), ("Eroded", eroded), ("Dilated", dilated)]:
    print(f"{name:8s} foreground pixels: {int(np.count_nonzero(im))}")

# Effect of SE size on erosion/dilation
eroded_big = cv2.erode(binary, cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (9, 9)))
dilated_big = cv2.dilate(binary, cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (9, 9)))

images = [gray, binary, eroded, dilated, eroded_big, dilated_big,
          noisy, opened, closed, open_close, boundary, gradient]
titles = ["Gray input", "Binary (Otsu)", "Erosion 3x3", "Dilation 3x3", "Erosion 9x9 disk", "Dilation 9x9 disk",
          "Noisy binary", "Opening", "Closing", "Opening then closing", "Boundary A - (A ⊖ B)", "Morph. gradient"]
fig, ax = plt.subplots(2, 6, figsize=(24, 8))
for a, im, t in zip(ax.flat, images, titles):
    a.imshow(im, cmap="gray"); a.set_title(t); a.axis("off")
plt.tight_layout()
plt.show()
```

## Output

- Erosion shrinks the objects (fewer foreground pixels), and dilation enlarges them (more foreground pixels); a larger SE gives a stronger effect.
- The manual erosion and dilation match OpenCV's output.
- Opening removes the white specks from the background; closing fills the black holes inside objects; opening followed by closing removes both.
- Boundary extraction and the morphological gradient give the outlines of the objects.

## Result

Erosion and dilation were implemented manually and with OpenCV, and the
opening, closing and boundary-extraction operations were demonstrated.

---

## Viva questions (quick revision)

1. **Why does OpenCV display colours wrongly in matplotlib?** OpenCV uses BGR order; matplotlib expects RGB.
2. **Difference between `cv2.add` and NumPy `+`?** `cv2.add` saturates at 255; NumPy `uint8` addition wraps around modulo 256.
3. **Why is the median filter best for salt & pepper noise?** Impulse values are extremes and are never chosen as the median; edges are preserved.
4. **What does histogram equalization do?** It maps intensities through the CDF to spread them over the full range, increasing global contrast.
5. **Why is `fftshift` used?** To move the zero-frequency component to the centre of the spectrum for display and easier filter design.
6. **Why does the ideal low-pass filter cause ringing?** Its sharp cutoff in frequency corresponds to a sinc function in the spatial domain, whose side lobes create ripples.
7. **Sobel vs Prewitt?** Sobel gives more weight to the centre pixels, which adds smoothing and makes it less sensitive to noise.
8. **Why use LoG instead of the plain Laplacian?** The Laplacian amplifies noise; Gaussian smoothing first suppresses it.
9. **Opening vs closing?** Opening (erosion then dilation) removes small objects; closing (dilation then erosion) fills small holes.
10. **Convolution vs correlation?** Convolution flips the kernel by 180° before sliding it; for symmetric kernels the results are identical.
