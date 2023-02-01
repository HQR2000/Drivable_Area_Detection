# Drivable Area Detection

![Drivable_Area_Detection](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/Drivable_Area_Detection.jpg)

## Introduction

In this project, me and my teammate read quite a lot papers related to traditional and state-of-art object detection methods. And we finally choose to implement the traditional method which use Historgram of Gradient(HOG) for feature extraction and SVM(Support Vector Machine) for vehicle detection and statistical method for lane line segementation. Besides, we also use YOLOP for the same task and them compare the performance of YOLOP and the baseline model.

The implementation of traditional object detection can be found in `./code/Final_Notebook.ipynb` and the implementation of YOLOP can be found in [YOLOP](https://github.com/hustvl/YOLOP)

## Dataset

The dataset used for the training of traditional object detection method can be found in `./data`. Since the baseline model is based on statistical method, the training dataset will be seperated to images with vehicles and images that are not vehicles. You can find images with vehicle in `./data/vehicles` and non-vehicles images in `./data/non-vehicles`. The training dataset for YOLOP and testing dataset for YOLOP and baseline model are all from [BDD100k](https://www.bdd100k.com), one of the best dataset for heterogeneous multitask learning in autonomous driving.

## Result Analysis

### Vehicle Detection of Baseline Model

While testing the vehicle detection part for baseline model, we noticed that it can only work well on the images without complex lighting situations.

For example:

![good1](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/good1.jpg)
![good2](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/good2.jpg)

As shown in the figures below, it's clear that the robustness of the baseline model is very low, it will be easily affected by lighting, environment, reflection and other factors, resulting in the failure to successfully locate the vehicles.

![drawback1](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/drawback1.jpg)
![drawback2](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/drawback2.jpg)

###Lane Line Detection of Baseline Model

For the lane line detection part of baseline model, it's not suprising that it also cannot handle complex lane line situations. Mostly it can only detect one of the several lane lines in the testing images of BDD100K.

[drawback3](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/drawback3.png)
[drawback4](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/drawback4.png)

We also compare the Interaction of Union(IOU) for baseline model and YOLOP model, it's clear that the deep learning model is much more better than the baseline model.

| Model            | IOU(%)|
| ---------------- | ----- |
| `YOLOP`          |  26.2 |
| `Baseline Model` |  0.61 |

