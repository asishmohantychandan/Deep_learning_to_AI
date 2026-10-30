# Indian Sign Language (ISL) Recognition

## 1. Problem Statement

The goal of this project is to build an image-classification model that
recognizes Indian Sign Language alphabet hand signs. The system takes an
image of a hand sign as input and predicts the corresponding alphabet.

## 2. Dataset

-   **Dataset:** PRAICP-1000-IndiSignLang
-   **Classes:** 24 alphabet classes (A--I and K--Y; J and Z are not
    included)
-   **Total images:** 4,972
-   **Training images:** 3,978 (80%)
-   **Validation images:** 994 (20%)

## 3. Approach

1.  Inspected the class folders, image counts, and sample images.
2.  Resized images to **160 × 160** pixels and prepared batches of 32
    images.
3.  Applied MobileNetV2 preprocessing to scale pixel values to the range
    **\[-1, 1\]**.
4.  Used **MobileNetV2 with ImageNet pretrained weights** as a frozen
    feature extractor.
5.  Added Global Average Pooling, Dropout, and a 24-unit Softmax output
    layer.
6.  Trained the classification head for up to five epochs and evaluated
    predictions using a classification report and confusion matrix.

## 4. Model Training

The initial training run reported the following results after five
epochs:

-   **Training accuracy:** 99.75%
-   **Validation accuracy:** 99.80%
-   **Training loss:** 0.0327
-   **Validation loss:** 0.0233

These are the metrics from the training run. The final corrected
evaluation metrics should be taken from the corrected classification
report, as the validation prediction and label ordering issue was fixed
separately.

## 5. Problems Faced and Solutions

  -----------------------------------------------------------------------
  Problem                             Solution
  ----------------------------------- -----------------------------------
  The dataset contained images        Used TensorFlow's
  organized in class-specific         `image_dataset_from_directory()` to
  folders.                            load images and assign labels
                                      automatically.

  Images needed a consistent input    Resized images to 160 × 160 and
  size and pixel range.               applied MobileNetV2 preprocessing.

  GPU was not available in the        Used a lightweight
  environment.                        transfer-learning setup with the
                                      MobileNetV2 base layers frozen.

  The first evaluation report showed  Recreated the validation dataset
  unexpectedly low accuracy because   with `shuffle=False` and generated
  labels and predictions could be     labels and predictions in the same
  misaligned between separate         fixed order.
  iterations of a shuffled validation 
  dataset.                            

  The first prediction visualization  Randomly sampled images from the
  displayed images from only one      validation dataset for
  class.                              visualization, without changing the
                                      evaluation dataset.
  -----------------------------------------------------------------------

## 6. Conclusion

This project demonstrates how transfer learning with MobileNetV2 can be
used to classify Indian Sign Language alphabet images. The model was
trained on 24 classes using a relatively small image dataset, and
individual image predictions were successfully demonstrated.

The project is an **image-based alphabet classifier**, not a complete
real-time sign-language translation system. It does not yet interpret
continuous gestures, facial expressions, or sentence-level sign
language. Further testing on unseen images and different users,
backgrounds, lighting conditions, and hand orientations is needed before
real-world deployment.

## 7. Future Improvements

-   Test the model on an independent test set and images from different
    users.
-   Check for duplicate or near-duplicate images across dataset splits.
-   Improve robustness using appropriate data augmentation.
-   Build a webcam-based application for live sign recognition.
-   Extend the system to recognize dynamic signs and sequences.
