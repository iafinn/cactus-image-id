# Columnar Cactus Identification with Machine Learning

In this project I tested 2 binary classification algorithms (logistic regression and a 5-layer convolutional neural network) to identify the presence or absence of columnar cactus in aerial photographs. I used a combination of Tensorflow, Keras, and Scikit-learn in Python. Model performace was evaluated with 10-fold cross validation and the F1 score (harmonic mean of precision and recall). 

The [dataset](https://www.kaggle.com/c/aerial-cactus-identification/overview) consists of 17500 32 x 32 RGB images with labels, and was posted on Kaggle by researchers from the [VIGIA project](https://jivg.org/research-projects/vigia/) in Mexico. The ultimate goal of this project is to develop machine learning algorithms that can assess changes in the protected natural areas in Mexico due to climate change, logging, mining, and agriculture. 

I ran the Jupyter notebooks on my local machine first and then an AWS t2.large Linux instance to get more familiar with AWS. I accessed the notebooks remotely with HTTPS. More details on this process are given in the docs. 

# Directory Overview

```
├───data
├───docs            <- info on AWS EC2 access
├───models          <- Jupyter NBs for LR and CNN
└───report          <- The final results of the two models
    └───figures
```

# Sample Results

Here is a random sample of the test set results for the CNN and Logistic regression:

## Convolutional Neural Network
![CNN](https://github.com/iafinn/cactus-image-id/blob/master/report/figures/CNN.png)

## Logistic Regression
![LR](https://github.com/iafinn/cactus-image-id/blob/master/report/figures/LR.png)

