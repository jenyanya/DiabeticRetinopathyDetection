# DiabeticRetinopathyDetection
## Introduction
Hey! This is my bachelor's project: diabetic retinopathy detection using convolutional neural networks. To perform this task I have trained 3 networks: [ResNet-34](https://arxiv.org/abs/1512.03385), [Inception v2](https://arxiv.org/abs/1409.4842) and a very small CNN of my own design!
The 2 former were trained with the use of **transfer learning**. 
## Data
The data were obtrained at [Kaggle](https://www.kaggle.com/competitions/diabetic-retinopathy-detection) and [MESSIDOR](https://www.adcis.net/en/third-party/messidor/), MESSIDOR was later used as a test set.
The examples in the Kaggle dataset were classified into 5 categories according to [ETDRS](https://www.researchgate.net/figure/International-Clinical-Diabetic-Retinopathy-Scale-Compared-with-Early-Treatment-Diabetic_tbl1_340168321).
Based on the literature the data were re-labeled: original labels 0 and 1 became 'No diabetic retinopathy', since it's hard to distinguish the two, and data with labels 3, 4, 5 were showing apperent signs of DR so they were labeled 'Diabetic Retinopathy'.
### Preprocessing
The data required heavy preprocessing. The pictures were presented in varying sizes, there was a need to get rid of all the unnecessary information like the size of the border around the circular retina image, as well as the retinal color, which could potentially be the feature neural net can overfit on.
First of all, there was a need to identify the center of the retina, so the Hough circle detection algorithm was used. Visualising it for the report was fun, so here is the result when it finds the circle center (a fractoin of the border points are used for clarity).
After that everything but the square with the center at the same point was cropped away. Then the median filtered version of an image was created and subtracted from the original, removing almost all color, but preserving the necessary features. The black mask outside of the retina was reapplied.
### Data augmentation
The data in resulting 2 classes were unbalanced, so the data augmentation technique was performed. Both classes were flipped horizontally, and the minority class was oversampled with random rotation from -30° to 30° around the center. 
