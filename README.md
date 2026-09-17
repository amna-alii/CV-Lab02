# CV-Lab02
# Lab 02 — Effect of Image Filtering on Skin-Lesion Classification

## Objective

Investigate how five spatial-domain image-processing filters affect the classification
performance of the **three best pretrained models identified in Lab Activity 1 (Task 01)**.

## Models Used (carried over from Task 01 results)

| Rank | Model | Task 01 Accuracy |
|---|---|---|
| 1 | EfficientNet-B0 | 96.0% |
| 2 | ResNet18 | 90.0% |
| 3 | ResNet50 | 89.0% |

##  Dataset

Same 5-class HAM10000-derived subset used in Task 01:
Melanoma, Melanocytic nevus, Benign keratosis, Dermatofibroma, Vascular lesion.
(Source: [Skin Disease Classification Image Dataset, Kaggle](https://www.kaggle.com/datasets/riyaelizashaju/skin-disease-classification-image-dataset))

##  Filters Compared Against the No-Filter Baseline

- Average / Mean filter
- Gaussian filter
- Median filter
- Sharpening filter
- Sobel edge filter

##  Experimental Design

- **18 total experiments**: 3 models × (1 baseline + 5 filters)
- **Identical settings across all runs**: same dataset split, image size, batch size,
  optimizer, learning rate, and number of epochs — only the filter changes.
- Metrics collected per run: Accuracy, Precision, Recall, F1-score, Macro-F1, Balanced
  Accuracy, AUC (macro, one-vs-rest).


##  How to Run

1. Open `Task2_Image_Filtering_Lab.ipynb` in [Google Colab](https://colab.research.google.com/).
2. Set **Runtime → Change runtime type → GPU (T4)**.
3. Run all cells top to bottom:
   - Upload the same dataset zip used in Task 01 when prompted.
   - The notebook filters it down to the same 5 classes automatically.
   - All 18 model/filter combinations are trained, evaluated, and visualized.
4. Download the generated CSVs and figures (final cell does this automatically) and place
   them into `results/` and `figures/` in this repo.
5. Copy the comparison table values into `Task_02.docx`.
6. Answer the "Questions to Answer" section in the report using the real results — these are
   intentionally left blank in the notebook and the Word doc until the experiments are done.

##  Final Results

| Model | Filter | Accuracy | Precision | Recall | F1-score | Macro-F1 | AUC | Balanced Acc. |
|---|---|---|---|---|---|---|---|---|
| EfficientNet-B0 | No Filter | 91.0 | 91.55 | 91.0 | 90.97 | 90.97 | 99.41 | 91.0 |
| EfficientNet-B0 | Average | 81.0 | 81.17 | 81.0 | 80.35 | 80.35 | 97.79 | 81.0 |
| EfficientNet-B0 | Gaussian | 88.0 | 88.72 | 88.0 | 88.01 | 88.01 | 98.46 | 88.0 |
| EfficientNet-B0 | Median | 84.0 | 83.82 | 84.0 | 83.72 | 83.72 | 98.28 | 84.0 |
| EfficientNet-B0 | Sharpening | 91.0 | 91.81 | 91.0 | 90.86 | 90.86 | 99.56 | 91.0 |
| EfficientNet-B0 | Sobel | 80.0 | 80.79 | 80.0 | 79.81 | 79.81 | 95.95 | 80.0 |
| ResNet18 | No Filter | 83.0 | 84.12 | 83.0 | 83.25 | 83.25 | 96.91 | 83.0 |
| ResNet18 | Average | 89.0 | 89.71 | 89.0 | 89.11 | 89.11 | 97.48 | 89.0 |
| ResNet18 | Gaussian | 89.0 | 88.95 | 89.0 | 88.70 | 88.70 | 96.60 | 89.0 |
| ResNet18 | Median | 90.0 | 90.71 | 90.0 | 89.88 | 89.88 | 97.70 | 90.0 |
| ResNet18 | Sharpening | 96.0 | 96.07 | 96.0 | 95.94 | 95.94 | 99.70 | 96.0 |
| ResNet18 | Sobel | 78.0 | 80.31 | 78.0 | 78.19 | 78.19 | 95.65 | 78.0 |
| ResNet50 | No Filter | 91.0 | 91.50 | 91.0 | 90.92 | 90.92 | 98.48 | 91.0 |
| ResNet50 | Average | 86.0 | 88.03 | 86.0 | 84.64 | 84.64 | 95.90 | 86.0 |
| ResNet50 | Gaussian | 86.0 | 87.48 | 86.0 | 86.08 | 86.08 | 97.84 | 86.0 |
| ResNet50 | Median | 87.0 | 87.93 | 87.0 | 87.34 | 87.34 | 98.30 | 87.0 |
| ResNet50 | Sharpening | 92.0 | 92.19 | 92.0 | 92.07 | 92.07 | 99.04 | 92.0 |
| ResNet50 | Sobel | 80.0 | 83.23 | 80.0 | 80.62 | 80.62 | 95.78 | 80.0 |

**Average absolute change in accuracy vs. each model's own no-filter baseline:**

| Filter | Avg. \|Δ Accuracy\| across models |
|---|---|
| Sobel | 9.00 |
| Average | 7.00 |
| Median | 6.00 |
| Gaussian | 4.67 |
| Sharpening | 4.67 |

##  Questions to Answer

**1. Which three pretrained models performed best in Lab Activity 1?**
EfficientNet-B0 (96.0%), ResNet18 (90.0%), and ResNet50 (89.0%) validation accuracy.

**2. How does filtering affect each of the three models?**
The effect is model-dependent rather than uniform. For **EfficientNet-B0** and **ResNet50**
(both had strong ~91% no-filter baselines), every smoothing filter (Average, Gaussian, Median)
*reduced* accuracy, and only Sharpening matched or slightly beat the baseline. For **ResNet18**
(a weaker ~83% baseline), Average, Gaussian, Median, and especially Sharpening all *improved*
accuracy substantially — Sharpening pushed it all the way to 96.0%, the best result of the
whole experiment.

**3. Which filter produces the greatest change compared with the unfiltered baseline?**
**Sobel**, by a clear margin — it lowered accuracy for all three models (−11, −5, and −11
points respectively), for the largest average absolute change (9.00 points) of any filter.

**4. Does the effect of a filter remain consistent across all three models?**
No, not for the smoothing filters. Average/Gaussian/Median hurt EfficientNet-B0 and ResNet50
but helped ResNet18. **Sobel is the one exception that was consistently negative** across all
three models (only the size of the drop varied). **Sharpening was consistently non-negative**
across all three models (flat for EfficientNet-B0, and a clear gain for ResNet18 and ResNet50).

**5. Does filtering improve or decrease macro-F1 and balanced accuracy?**
Macro-F1 and balanced accuracy move in lockstep with accuracy in these results (the validation
set is class-balanced, so this is expected). Sobel decreases both metrics for every model;
smoothing filters decrease them for the two stronger models but increase them for ResNet18;
Sharpening increases or maintains both metrics for every model.

**6. Which lesion classes are most affected by filtering?**
*This needs the per-class breakdown from `task2_per_class_metrics.csv` / the confusion-matrix
figures generated by the notebook, which weren't included in the results shared here.* In
general, classes that are distinguished mainly by fine texture or subtle color gradation
(e.g. **Melanoma vs. Melanocytic nevus**) are typically the most sensitive to smoothing and
edge filters, while classes with strong, large-scale color/shape cues (e.g. **Vascular
lesion**) tend to be more robust. Once you export the per-class CSV, drop it here and this
answer can be made precise instead of general.

**7. Why might smoothing remove useful lesion texture or morphological information?**
Average, Gaussian, and Median filters are low-pass operations — they attenuate high-spatial-
frequency content. In dermoscopic images, exactly that high-frequency content (irregular
borders, pigment network, fine granularity) carries much of the diagnostic signal, particularly
for melanoma-type lesions. Blurring it away removes cues the network relied on, which is
consistent with the accuracy drops seen for EfficientNet-B0 and ResNet50.

**8. Why might sharpening or edge detection help or hurt classification?**
Sharpening is a high-boost filter: it keeps all the original color/intensity information and
*adds back* emphasis on edges and fine detail, which can make lesion borders and texture more
salient to the network — consistent with it being the only filter that never hurt performance
here. Sobel, in contrast, **throws away color and absolute intensity entirely**, keeping only
gradient magnitude. Since color (e.g. blue-white veils, pigmentation patterns) is a primary
diagnostic feature in dermoscopy, this information loss explains why Sobel was the most harmful
filter for all three models.

**9. What is the difference between convolution and correlation?**
Mathematically, convolution flips the kernel 180° (both axes) before sliding it across the
image, i.e. `output(x,y) = Σ kernel(i,j)·input(x−i, y−j)`, whereas cross-correlation slides the
kernel without flipping it: `output(x,y) = Σ kernel(i,j)·input(x+i, y+j)`. For symmetric kernels
(like Gaussian or mean blur) the two operations are identical; for asymmetric kernels (like a
Sobel kernel) they differ. In practice, the "convolution" layers in CNNs (including every model
used in this lab) actually implement cross-correlation — the kernel isn't flipped — because the
weights are learned from data anyway, so the flip is an unnecessary computational step.

**10. Based on your results, explain the relationship between classical image processing and
deep-learning-based feature extraction.**
Classical filters are hand-designed, fixed linear operators for extracting specific properties
(edges via Sobel, smoothed intensity via Gaussian/Average/Median, local contrast via
sharpening). CNNs learn an analogous — but data-driven and much larger — bank of convolutional
filters automatically during training, and early CNN layers are well known to converge toward
edge- and blob-detector-like kernels similar to classical operators. This experiment shows the
practical consequence of that overlap: pre-applying a classical filter that reinforces
information the network would already learn to extract (Sharpening, which preserves and boosts
detail) tends to be harmless or mildly helpful, while pre-applying one that destroys information
the network needs but hasn't learned to reconstruct (Sobel discarding color; heavy smoothing
discarding texture) actively hurts performance. In short, classical image processing and deep
feature extraction aren't competing approaches — the classical filters here act as a manual,
irreversible preprocessing step on top of whatever the CNN would otherwise learn for itself.

##  Requirements

Installed automatically inside the notebook:

```
torch
torchvision
opencv-python-headless
scikit-learn
pandas
numpy
matplotlib
thop
```

##  License

Academic use only, as part of a Computer Vision lab assignment. Dataset used under its
original Kaggle license.
