# Indian Sign Language (ISL) Alphabet Recognition

## Project Overview
This project recognizes Indian Sign Language (ISL) alphabet hand gestures from images using **Transfer Learning with MobileNetV2** and TensorFlow/Keras.

The model classifies images into 24 alphabet classes (A–Y, excluding J and Z).

## What is MobileNetV2?

MobileNetV2 is a lightweight Convolutional Neural Network (CNN) architecture developed by Google, designed for efficient image classification, especially on devices with limited computational resources.

Unlike traditional CNNs, MobileNetV2 reduces computational cost and model size using **Depthwise Separable Convolutions** and **Inverted Residual Blocks**.

### Key Concepts

- **Depthwise Separable Convolution:** Splits a standard convolution into two operations:
  - Depthwise Convolution: Applies a separate filter to each input channel.
  - Pointwise Convolution: Uses a 1×1 convolution to combine information across channels.
- **Inverted Residual Blocks:** Expand the number of channels, apply depthwise convolution, and then project the features back to a smaller number of channels.
- **Linear Bottleneck:** Uses a linear projection layer to preserve useful information and reduce information loss.
- **Skip Connections:** Connect the input directly to the output when dimensions and stride allow, helping information and gradients flow through the network.

## MobileNetV2 Architecture

A simplified representation of MobileNetV2:

```text
Input Image (160 × 160 × 3)
          |
          v
Initial Convolution
          |
          v
Inverted Residual Blocks
          |
          |-- 1×1 Expansion Convolution
          |-- 3×3 Depthwise Convolution
          |-- 1×1 Linear Projection
          |-- Skip Connection (when applicable)
          |
          v
Final Convolution
          |
          v
Global Average Pooling
          |
          v
Dropout (0.3)
          |
          v
Fully Connected Layer
          |
          v
Softmax (24 Classes)
```

## How We Used MobileNetV2 in This Project

We used **Transfer Learning** instead of training a CNN from scratch.

1. **Pretrained Model:** Loaded MobileNetV2 with ImageNet weights using `include_top=False`.
2. **Input Processing:** Resized images to 160 × 160 pixels and applied MobileNetV2 preprocessing to scale pixel values to the range [-1, 1].
3. **Feature Extraction:** Froze the pretrained MobileNetV2 base so its learned ImageNet features were not updated during initial training.
4. **Custom Classification Head:** Added:
   - Global Average Pooling to convert feature maps into a feature vector.
   - Dropout (0.3) to help reduce overfitting.
   - Dense layer with 24 neurons and Softmax activation for alphabet classification.
5. **Training:** Trained the newly added classification layers using our ISL image dataset.

### Model Configuration

| Component | Configuration |
|---|---|
| Architecture | MobileNetV2 |
| Pretrained Weights | ImageNet |
| Input Shape | 160 × 160 × 3 |
| Base Model | Frozen |
| Pooling | Global Average Pooling |
| Dropout | 0.3 |
| Output Layer | Dense (24 neurons) |
| Activation | Softmax |
| Framework | TensorFlow / Keras |

## Results

- **Total Images:** 4,972
- **Number of Classes:** 24
- **Training Images:** 3,978
- **Validation Images:** 994
- **Validation Accuracy:** 99.8% in the reported training run

*Note: This is validation accuracy, not independently verified test accuracy. Further testing on independent images is recommended to evaluate real-world generalization.*

## Technologies Used

Python, TensorFlow, Keras, MobileNetV2, NumPy, Matplotlib, Scikit-learn, Jupyter Notebook.

## Project Scope

This project recognizes individual ISL alphabet gestures from static images. It does not currently support continuous sign-language recognition, sentence formation, or real-time sign-language translation.