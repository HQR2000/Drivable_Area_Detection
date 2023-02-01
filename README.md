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

### Lane Line Detection of Baseline Model

For the lane line detection part of baseline model, it's not suprising that it also cannot handle complex lane line situations. Mostly it can only detect one of the several lane lines in the testing images of BDD100K.

![drawback3](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/drawback3.png)
![drawback4](https://github.com/HQR2000/Drivable_Area_Detection/blob/main/Public/drawback4.png)

We also compare the Interaction of Union(IOU) for baseline model and YOLOP model, it's clear that the deep learning model is much more better than the baseline model.

| Model            | IOU(%)|
| ---------------- | ----- |
| `YOLOP`          |  26.2 |
| `Baseline Model` |  0.61 |

Other evalution results of YOLOP can be found in the `'README.md` of [YOLOP](https://github.com/hustvl/YOLOP).

### Anaysis&Conclusion

#### YOLOP

YOLOP achieves the latest state-of-the-art performance, but we found that it has difficulties in handling extreme lighting and reflection conditions, when the reflection on the window is large and the sunlight is very strong, the detection will be greatly affected. We think this is because the training set does not contain such extreme conditions, and it may be improved by applying more data augmentation methods. 

#### Baseline Model

For the baseline model, it surprised us that both the accuracy and IoU of the lane line segmentation part of the baseline model are close to 0. We think that there are three main reasons for this problem. 

Firstly, the lane line segmentation part of the baseline model has no learning process, which means it is just an image processing task, the model captures almost all the remaining pixels after processing. 

Secondly, the baseline model only choose the lower half of the image as the region of interest, which means any lane lines that belong to the upper half of the image will not be detected.

Thirdly, the baseline model can only detect the two lane lines directly in front of the vehicle, but BDD100K dataset contains annotations of all the lane lines. 

## Appendix

Here are some of the errors we met while running YOLOP, we list them out here for your reference.

* `IndexError: boolean index did not match indexed array along dimension 0`

Solution: Add the following code to tools/demo.py line 130 and delete line 60 in `./lib/utils/plot.py`.

`img_det=cv2.resize(img_det,(da_seg_mask.shape[1],da_seg_mask.shape[0]) ,interpolation=cv2.INTER_AREA)`

Then add the following code to “demo.py” line 148 and run “demo.py” to check the result.

`cv2.namedWindow("test", cv2.WINDOW_NORMAL)
cv2.resizeWindow("test", int(img_det.shape[1] * 0.5), int(img_det.shape[0] * 0.5))
cv2.imshow("test", img_det)
cv2.waitKey(30)`

* `TypeError: can’t pickle generator objects，EOFError: Ran out of input`

Solution: Modify `./lib/config/default.py` in line 9 to `_C.WORKERS=0`.

* `RuntimeError: CUDA out of memory`

Solution: Modify `./lib/config/default.py` in line 19 to `_C.CUDNN.BENCHMARK = False`.

* `AttributeError: ‘list’ object has no attribute ‘seek’. You can only torch.load from a file that is seekable. Please pre-load the data into a buffer like io.BytesIO and try to load from it`

Solution: Delete `nargs=’+’`in line 39 of `./test.py`.


