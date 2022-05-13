<p align="center">
  <img 
    src="https://github.com/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention/blob/main/Repo_Files/Face_Mask_Detection_with__n__Empirical_Attention.png?raw=true"
  >
</p>

<h4 align="center">Object detection of face masks worn by people during the COVID19 pandemic.</h4>

<p align="center">
  <img 
    src="https://github.com/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention/blob/main/Demo/ezgif.com-gif-maker.gif?raw=true"
  >
</p>

<h4 align="center">Raw Video: <a href="https://youtu.be/8J9iFWhZdsY" target="_blank">20 Types of People in Face Masks</a>.</h4>

<p align="center">
  <img 
    src="https://github.com/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention/blob/main/Demo/mapping.png?raw=true"
  >
</p>

---

# About the Project
![personal](https://img.shields.io/badge/project-chandlertimmdoloriel-red?style=for-the-badge&logo=appveyor)

Masks play a crucial role in protecting the health of individuals against respiratory diseases, as is one of the few precautions available for COVID-19 in the absence of immunization. In this project, I designed a Face Mask detector to identify humans that are wearing, not wearing, or improperly wearing masks during the COVID19 pandemic. During the pandemic, it is empirical to wear masks all the time when outside your house to minimize the transmission of the deadly disease. 

## Dataset
To realize our goal, I use the kaggle dataset [Face Mask Detection](https://www.kaggle.com/datasets/andrewmvd/face-mask-detection?datasetId=667889&sortBy=voteCount). With this dataset, it is possible to create a model to detect people wearing masks, not wearing them, or wearing masks improperly.

This dataset contains 853 images belonging to the 3 classes, as well as their bounding boxes in the PASCAL VOC format.
The classes are:

- With mask;
- Without mask;
- Mask worn incorrectly.

I plot the distribution of the dataset classes as shown below. I can see that class ```with mask``` dominate majority of the data. 

<p align="center">
  <img 
    src="https://github.com/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention/blob/main/Repo_Files/distri.jpg?raw=true"
  >
</p>

The data is skewed, so I investigated more and find if there are duplicate images. If I can find the duplicate images, I can atleast lessen the effect of overfitting or underfitting. 

Using the ```imagehash``` method, I found 23 duplicate images! an example is shown below. Basically, what ```imagehash``` do is calculate the hash values of an image and compare it (or find the similarity) with another hash value. There are several hash functions and I used all of them and combined the calculated hash values. 

The different hash functions are:

- average hashing (aHash)
- perception hashing (pHash)
- difference hashing (dHash)
- wavelet hashing (wHash) |

<br>

<p align="center">
  <img 
    src="https://user-images.githubusercontent.com/62779617/162480687-6e9d8f3d-3bd4-4726-b709-62552808fed8.png"
  >
</p>

I then splitted the data into train and test sets with split ratio of 0.2 for test. After that, I converted the dataset annotations to ```COCO format``` since this format is most commonly used on object detection libraries like ```MMDetection```.

```python
test_ratio = 0.20
train_img, test_img = np.split(np.array(imgfiles), [int(len(imgfiles)* (1 - test_ratio))])
```

## Model

For the implementation of the model, I used the object detection toolbox [MMDetection](https://github.com/open-mmlab/mmdetection) which is based on PyTorch. The toolbox fetures modular design which can decompose state-of-the-art frameworks into different componenents and can easily construct a customized object detection framework by combining different modules. 

The toolbox directly supports popular and contemporary detection frameworks, e.g. Faster RCNN, Mask RCNN, RetinaNet, etc.

<p align="center">
  <img
    src="https://user-images.githubusercontent.com/62779617/162557447-dd7a921b-9b6d-4baa-8912-68f47921cfe5.png"
  >
</p>

From the toolbox, I use the detector named [empirical attention](https://openaccess.thecvf.com/content_ICCV_2019/papers/Zhu_An_Empirical_Study_of_Spatial_Attention_Mechanisms_in_Deep_Networks_ICCV_2019_paper.pdf) based on this paper published in ICCV.

The paper presented an empirical study that ablates various spatial attention elements within a generalized attention formulation, encompassing the dominant Transformer attention as well as the prevalent deformable convolution and dynamic convolution modules. The study yields significant findings about spatial attention in deep networks, some of which run counter to conventional understanding. The baseline used for empirical attention is the Faster-RCNN with feature pyramid network.

The training pipeline is shown below. We flip the images randomly by 0.5 ratio and normalize it. The training is done in multi-scale, `img_scale=[(1333, 640), (1333, 800)]` and the `cfg.runner.max_epochs = 12` which is repeated 3x. `cfg.optimizer.lr = 0.02 / 8` and is divided by 10 after every `epoch=8` and `epoch=11` by the `lr_scheduler`.


```python
cfg.train_pipeline = [
        dict(type='LoadImageFromFile'),
        dict(type='LoadAnnotations', with_bbox=True),
        dict(
            type='Resize',
            img_scale=[(1333, 640), (1333, 800)],
            multiscale_mode='range',
            keep_ratio=True),
        dict(type='RandomFlip', flip_ratio=0.5),
        dict(type='Normalize', **cfg.img_norm_cfg),
        dict(type='Pad', size_divisor=32),
        dict(type='DefaultFormatBundle'),
        dict(type='Collect', keys=['img', 'gt_bboxes', 'gt_labels'])]
```

After setting-up the model architecture, I started the training process and did an inference with the output. The result is shown below. It obtained an `bbox mAP = 0.608`.

<p align="center">
  <img
    src="https://user-images.githubusercontent.com/62779617/162558031-267c5f4c-cff2-4dd9-8782-6088608cc3eb.png"
  >
</p>


# Usage

You don't need to install anything to run the project. The [notebook](https://www.kaggle.com/code/chandlertimm/face-mask-detection-with-empirical-attention?scriptVersionId=90852067) is hosted in `kaggle`. 

If you want to just read the code, click the [notebook](https://www.kaggle.com/code/chandlertimm/face-mask-detection-with-empirical-attention?scriptVersionId=90852067). If you want to try and run it, just click the `Copy & Edit` tab in the upper right corner of the notebook.

# License

This repo is licensed under [MIT License](https://github.com/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention/blob/main/LICENSE)
