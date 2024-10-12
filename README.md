# Diabetic Retinopathy Detection
## Introduction
Hey! This is my bachelor's project: diabetic retinopathy detection using convolutional neural networks. To perform this task I have trained 3 networks: [ResNet-50](https://arxiv.org/abs/1512.03385) and a very small CNN of my own design!
The 2 former were trained with the use of **transfer learning**. 
## Data
The data were obtained at [Kaggle](https://www.kaggle.com/competitions/diabetic-retinopathy-detection).
The examples in the Kaggle dataset were classified into 5 categories according to [ETDRS](https://www.researchgate.net/figure/International-Clinical-Diabetic-Retinopathy-Scale-Compared-with-Early-Treatment-Diabetic_tbl1_340168321).
Based on the literature the data were re-labeled: original labels 0 and 1 became 'No diabetic retinopathy', since it's hard to distinguish the two, and data with labels 3, 4, 5 were showing apperent signs of DR so they were labeled 'Diabetic Retinopathy'.
### Preprocessing
The data required heavy preprocessing. The pictures were presented in varying sizes, there was a need to get rid of all the unnecessary information like the size of the border around the circular retina image, as well as the retinal color, which could potentially be the feature neural net can overfit on.
First of all, there was a need to identify the center of the retina, so the Hough circle detection algorithm was used. Visualising it for the report was fun, so here is the result when it finds the center of the retina in the image (a fraction of the border points are used for clarity).
![exact_radius](https://github.com/user-attachments/assets/01e52f69-09a3-4f64-9949-0c3c048cf693)
After that everything but the square with the center at the same point was cropped away. Then the median filtered version of an image was created and subtracted from the original, removing almost all color, but preserving the necessary features. The black mask outside of the retina was reapplied.
### Data augmentation
The data in resulting 2 classes were unbalanced, so the data augmentation technique was performed. Both classes were flipped horizontally, and the minority class was oversampled with random rotation from -30° to 30° around the center. 
## Own model architecture
The architecture was motivated by the need of the model to take into the consideration different features of different sizes, like detecting small micro-aneurysms, and medium and large haematomas.
It has 3 paths for image to flow through, then the inputs are flattened and concatenated, feeding into classification layers.
In those 3 paths the first layers consist of 55x55 with stride 9, 31x31 with stride 7, and 11x11 with stride 3 convolutions respectively. In all paths they are followed by 3x3 stride 1 convolutions.
## Training
ResNet was trained using Adam with primary learning rate being 0.001, 10 epochs warm up from 0.0005 followed by cosine annealing with minimum learning rate reached at 100 epochs. During training test error was decreasing and started to increase around 27th epoch so **early stopping** was used. 
Due to compute limitations ResNet was trained on image size 299x299, while the own network had an input size of 512x512.
