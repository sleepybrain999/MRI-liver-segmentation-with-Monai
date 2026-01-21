# MRI-liver-segmentation-with-Monai (CHAOS Dataset)

This project implements a robust deep learning pipeline using **MONAI** and **PyTorch** to segment the liver from abdominal MRI scans. It specifically utilizes the T1-Dual (In-Phase and Out-of-Phase) sequences to improve boundary detection.

## 🚀 Key Features

* **Multi-Channel Input:** Combines T1-Inphase and T1-Outphase images to leverage the "India Ink" effect for sharper liver boundaries.
* **Subject-Level Splitting:** Prevents data leakage by ensuring slices from the same patient are never shared between training and validation sets.
* **Residual U-Net:** Utilizes MONAI's `UNet` with `num_res_units=2` to ensure stable training and better feature extraction.
* **Intensity Standardization:** Scales varied MRI signal intensities to a normalized  range.
* **Adaptive Resizing:** Handles heterogeneous DICOM sizes (e.g.,  and ) by standardizing all slices to a fixed resolution.
* **MRI Image Inspection:** Show the MRI image and the ground truth level with matplotlib
---

## 🛠 Project Structure

### 1. Data Preprocessing

The data is loaded using MONAI's `LoadImaged` and `EnsureChannelFirstd`. Because the CHAOS labels are multi-organ, we apply a specific threshold to isolate the liver:

* **Liver Value:**  (Thresholded between  and ).
* **Background/Other:** Set to .

### 2. The Model: Residual U-Net

We use a 2D U-Net architecture with skip connections and residual blocks.

* **Input Channels:** 2 (T1-In + T1-Out)
* **Output Channels:** 1 (Binary Mask)
* **Activation:** Sigmoid (applied via Dice Loss during training)

### 3. Training Strategy

* **Loss Function:** Dice Loss (ideal for class imbalance where the liver is a small part of the image).
* **Optimizer:** Adam ().
* **Batch Size:** 16.
* **Metric:** Mean Dice Coefficient.

---

## 💻 Getting Started

### Prerequisites

```bash
pip install monai torch numpy matplotlib nibabel pydicom

```

### Usage

1. **Load & Scale:** Run the `load_data` function to read DICOM and PNG files.
2. **Split:** Use the subject-level indices to create `train_sub` and `val_sub`.
3. **Slice:** Convert 3D volumes into 2D slices using the `create_2d_list_multichannel` function to ensure consistent shapes ().
4. **Train:** Execute the training loop. The best model will be saved as `best_liver_unet.pth`.

---

## 📊  Performance

The model on the CHAOS dataset achieves a **Dice Score between of 0.8743**. 
---

## 📝 License

This project uses the **CHAOS Challenge Dataset** (Combined Healthy Abdominal Organ Segmentation) and **Monai** U-Net. Please cite the original authors if using this for research.

