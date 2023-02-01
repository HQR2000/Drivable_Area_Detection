# Drivable_Area_Detection

![Drivable_Area_Detection](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/Drivable_Area_Detection.jpg)

## Introduction

In this project, me and my teammate read quite a lot papers related to traditional and state-of-art object detection methods. And we finally choose to implement the traditional method which use Historgram of Gradient(HOG) for feature extraction and SVM(Support Vector Machine) for vehicle detection and statistical method for lane line segementation. Besides, we also use YOLOP for the same task and them compare the performance of YOLOP and the baseline model.

The implementation of traditional object detection can be found in `./code/Final_Notebook.ipynb` and the implementation of YOLOP can be found in [YOLOP](https://github.com/hustvl/YOLOP)

## Dataset

The dataset used for the training of traditional object detection method can be found in `./data`. Since the baseline model is based on statistical method, the training dataset will be seperated to images with vehicles and images that are not vehicles. You can find images with vehicle in `./data/vehicles` and non-vehicles images in `./data/non-vehicles`. The training dataset for YOLOP and testing dataset for YOLOP and baseline model are all from [BDD100k](https://www.bdd100k.com), one of the best dataset for heterogeneous multitask learning in autonomous driving.

## Result Analysis
As shown in the figures below, it's clear that the robustness of the baseline model is very low, it will be easily affected by lighting, environment, reflection and other factors, resulting in the failure to successfully locate the vehicles.
[drawback1]()[drawback2]()
