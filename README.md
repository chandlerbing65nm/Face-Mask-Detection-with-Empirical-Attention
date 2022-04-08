<p align="center">
  <img 
    src="https://github.com/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention/blob/main/Repo_Files/Face_Mask_Detection_with__n__Empirical_Attention.png?raw=true"
  >
</p>

<div align="center">

  <a href="">![last commit](https://img.shields.io/github/last-commit/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention)</a>
  <a href="">![repo size](https://img.shields.io/github/repo-size/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention)</a>
  <a href="">![watchers](https://img.shields.io/github/watchers/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention?style=social)</a>

</div>

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

<p align="left">
  <img 
    src="https://github.com/chandlerbing65nm/Face-Mask-Detection-with-Empirical-Attention/blob/main/Repo_Files/distri.jpg?raw=true"
  >
</p>

The data is skewed, so I investigated more and find if there are duplicate images. If I can find the duplicate images, I can atleast lessen the effect of overfitting or underfitting. 

Using the ```imagehash``` method, I found 23 duplicate images! an example is shown below. Basically, what ```imagehash``` do is calculate the hash values of an image and compare it (or find the similarity) with another hash value. There are several hash functions and I used all of them and combined the calculated hash values. 

The different hash functions are:

| average hashing (aHash) | perception hashing (pHash) | difference hashing (dHash) | wavelet hashing (wHash) |
| ----------------------- | -------------------------- | -------------------------- | ----------------------- |

<br>

<p align="left">
  <img 
    src="https://user-images.githubusercontent.com/62779617/162480687-6e9d8f3d-3bd4-4726-b709-62552808fed8.png"
  >
</p>

I then splitted the data into train and test sets with split ratio of 0.2 for test. After that, I converted the dataset annotations to ```COCO format``` since this format is most commonly used on object detection libraries like ```MMDetection```.

<p align="left">
  <img 
    src="https://user-images.githubusercontent.com/62779617/162481317-d6b032e2-6600-4f04-a22a-c89106b123e1.png"
  >
</p>


## Model

