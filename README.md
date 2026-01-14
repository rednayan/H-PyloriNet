# **HPyloriNet: H. Pylori Detection in Gastric Biopsy Slides**

**HPyloriNet** is a deep learning framework designed to automate the detection of *Helicobacter pylori* (H. pylori) in Histopathology Whole Slide Images (WSIs). Given that manual examination is time-consuming and labor-intensive, this project aims to aid pathologists by highlighting areas of bacterial presence using Computer Vision techniques.  
The system implements and evaluates two distinct approaches: **Classification** and **Object Detection**.

## **Code Availability & Privacy Notice**

**Please Note:** Due to privacy regulations and agreements with our clinical partner [LABOKLIN](https://laboklin.com/en/), the **training code and the full dataset cannot be made publicly available**.  
This repository contains only the **inference pipeline**, designed to run predictions on new or sample histology slides. It demonstrates the region extraction, segmentation, and detection logic described in our research.

## **Project Overview**

* **Goal:** Improve the efficiency of Histopathology examination for *H. pylori*.  
* **Data Source:** 22 H\&E-stained veterinary gastric biopsy slides provided by LABOKLIN.  
* **Key Architectures:** ResNet-101 (Classification), YOLO11x (Detection), NuClick (Segmentation).

## **Methodologies**

This repository contains inference code for the two detection strategies described in the associated paper.

### **1\. Classification Approach**

This method focuses on extracting candidate regions (gastric glands) and classifying them as positive or negative.

* **Region Extraction:** Uses template matching with gastric gland templates to extract $256 \\times 256$ candidate patches.  
* **Backbone:** Utilizes pre-trained models (specifically ResNet-101) as feature extractors.  
* **Classifier:** A Multilayer Perceptron (MLP) trained with Focal Loss to handle class imbalance.  
* **Output:** Binary classification of patches.

### **2\. Object Detection Approach**

This method utilizes a "Sliding Window Patch Inference" algorithm to locate specific instances of bacteria.

* **Inference:** Splits WSIs into $640 \\times 640$ patches.  
* **Segmentation (ROI Refinement):** Uses a custom-trained **NuClick** model (from MONAI) to segment gastric glands and filter irrelevant tissue.  
* **Detector:** **YOLO11x** is used for the final detection of *H. pylori* instances.  
* **Metric:** Optimized using Distance IoU (DIoU).

## **Dataset & Preprocessing**

The research was conducted on 22 WSIs at 40x magnification. The following preprocessing logic is embedded in the inference pipeline:

1. **Annotation:** Ground truth regions provided by experts, refined via an active learning loop to catch missed positives.  
2. **Patch Extraction:**  
   * *Classification:* Template matching thresholds used to identify gastric glands.  
   * *Detection:* Sliding window extraction.  
3. **Normalization:** Patches normalized using ImageNet statistics.

## **Results**

Performance was evaluated on a stratified test split of 7 slides.

| Method | Recall | Precision | Metric Notes |
| :---- | :---- | :---- | :---- |
| **Classification** | **56%** | **45%** | Evaluated on extracted candidate regions. |
| **Object Detection** | **52%** | **58%** | Based on DIoU threshold of 0.15. |


## **Acknowledgments**

We sincerely thank **LABOKLIN** for providing the dataset and ground truth labeling instrumental to this research.

## **Citation**

If you use this code or methodology, please cite:  
```bibtex
@article{HPyloriNet2024,
  title={A Computer-Aided Detection System for Helicobacter Pylori in Gastric Biopsy Slides},
  author={Nayan Sharma, Omer Ahmed, Zeyi Lu},
  year={2024}
}
