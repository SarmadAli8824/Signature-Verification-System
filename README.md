# ✍️ Signature Verification System

An automated handwritten signature verification system built with **Digital Image Processing** and **Neural Networks**. The system distinguishes between genuine and forged signatures using a multi-stage image processing pipeline combined with a Multilayer Perceptron (MLP) classifier.

> 🎓 Final Project — Digital Image Processing (DIP)  
> **Team:** Saim Ahmad · Ayesha Binte Imran · Sarmad Ali · Abyaz Israr  
> **Section:** DS-B

---

## 📌 What It Does

Given a signature image and a person's ID, the system outputs:
- ✅ **"Genuine Image"** — the signature matches the enrolled person
- ❌ **"Forged Image"** — the signature is likely a forgery

---

## 🧠 How It Works

### 1. Image Preprocessing Pipeline
Raw signature images go through a 5-step pipeline:

| Step | Technique | Purpose |
|------|-----------|---------|
| Noise Reduction | Non-Local Means Denoising (`cv2.fastNlMeansDenoising`) | Remove scan/photo noise |
| Grayscale Conversion | Custom RGB averaging | Reduce dimensionality |
| Sharpening | Laplacian kernel via `cv2.filter2D` | Enhance signature edges |
| Binarization | Otsu's Thresholding | Separate signature from background |
| Region Extraction | Bounding box crop | Normalize signature size |

### 2. Feature Extraction
9 discriminative features are extracted from the preprocessed binary image:

- **Size Ratio** — density of signature pixels
- **Centroid (x, y)** — center of mass of the signature
- **Eccentricity** — elongation of the signature shape
- **Solidity** — ratio of signature area to convex hull area
- **Skewness (x, y)** — asymmetry of pixel distribution
- **Kurtosis (x, y)** — sharpness/flatness of pixel distribution

### 3. Classification (Neural Network)
- Architecture: **Multilayer Perceptron (MLP)**
- Input: 9-feature vector
- Output: Binary (Genuine = 1, Forged = 0)
- Optimizer: **Adam**
- Training: Up to 1000 epochs with early stopping (cost < 0.0001)

---

## 📁 Project Structure

```
signature-verification-system/
│
├── Code.ipynb                   # Main Jupyter Notebook (full pipeline)
├── Report.docx                  # Detailed project report
├── Dataset Google Drive Link.txt  # Link to signature dataset
└── README.md
```

---

## 📊 Dataset

The dataset consists of genuine and forged signatures from **9 individuals**.

| Split | Genuine | Forged | Total |
|-------|---------|--------|-------|
| Training | 27 (3/person) | 27 (3/person) | 54 |
| Testing | 18 (2/person) | 18 (2/person) | 36 |
| **Total** | **45** | **45** | **90** |

**Naming Convention:**
- Genuine: `PERSONID_PERSONID_00N.PNG` (e.g., `001001_000.PNG`)
- Forged: `999PERSONID_00N.PNG` (e.g., `999001_000.PNG`)

📂 [Download Dataset from Google Drive](https://drive.google.com/drive/folders/1F0oLwX494p7HMgdn5QQjvNq2ErQ36wFw?usp=drive_link)

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install numpy opencv-python scikit-image scipy matplotlib pandas tensorflow
```

> ⚠️ This notebook was built and tested on **Google Colab**. It mounts Google Drive for dataset access.

### Running the Notebook

1. Upload `Code.ipynb` to [Google Colab](https://colab.research.google.com/)
2. Download the dataset from the Google Drive link above and place it in your Google Drive at:
   ```
   MyDrive/Signature/
   ├── train/
   │   ├── genuine/
   │   └── forged/
   └── test/
   ```
3. Run all cells top to bottom
4. When prompted, enter:
   - **Person ID** (e.g., `001`)
   - **Path to the signature image** to verify

### Example Usage

```python
train_person_id = "001"
test_image_path = "/content/drive/MyDrive/Signature/test/001001_003.PNG"
# Output: "Genuine Image"

test_image_path = "/content/drive/MyDrive/Signature/test/999001_003.PNG"
# Output: "Forged Image"
```

---

## 📈 Results

The system was evaluated on 36 test signatures (18 genuine, 18 forged):

- ✅ High accuracy in genuine vs. forged classification
- 📉 Low False Acceptance Rate (forged accepted as genuine)
- 📉 Low False Rejection Rate (genuine rejected as forged)

Performance metrics reported: **Accuracy, Misclassification Rate, Precision, Recall, Specificity**

---

## ⚠️ Limitations

- Small dataset (9 individuals, 10 signatures each)
- Fixed hand-crafted features (no deep feature learning)
- No GUI — runs via command-line input in notebook
- Not adapted for different signature styles per individual

---

## 🔭 Future Work

- Implement **CNN/RNN** architectures for automatic feature learning
- Add support for **skilled forgery** vs. **random forgery** classification
- Build a **graphical user interface** for real-time verification
- Expand dataset with more individuals and forgery types
- Support **digital/touch-based** signature input

---

## 📚 References

1. Otsu, N. (1979). *A threshold selection method from gray-level histograms.* IEEE Trans. SMC, 9(1), 62–66.
2. Bulacu, M., & Schomaker, L. (2007). *Text-independent writer identification.* IEEE TPAMI, 29(4), 701–717.
3. Plamondon, R., & Srihari, S. N. (2000). *Online and off-line handwriting recognition: a survey.* IEEE TPAMI, 22(1), 63–84.
4. [scikit-image](https://scikit-image.org/) — Image processing in Python
5. [Keras](https://keras.io/) — The Python Deep Learning API

---

## 📄 License

This project was developed as an academic assignment. All rights reserved by the authors.
