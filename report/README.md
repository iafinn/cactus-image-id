# Results

I developed two models for identifying columnar cactus in aerial photographs. In the first model, I used logistic regression, a relatively simple machine learning algorithm. For the second model, I used a 5-layer convolutional neural network. This is a more modern model, with design guidelines that I learned in Andrew Ng's Deep Learning Specialization from Coursera. I evaluated the models with a 10-fold cross validation. This entails splitting the data into 10 parts and doing 10 separate 90/10 train/test splits to evaluate their performance. The mean accuracy, precision, recall, and F1 scores for the models are given below with the standard deviation for the 10 sets for the last digit in parentheses. The CNN had superior performance for all evaluation metrics. Most notable is the F1 score of 0.993(8) for the CNN and 0.933(4) for logistic regression.

| Method              | Accuracy | Precision | Recall | F1   |
|---------------------|----------|-----------|--------|------|
| LR (Scikit-learn)   | 0.900(6)     | 0.940(7)      | 0.927(6)   | 0.933(4) |
| 5-layer CNN (TensorFlow) | 0.98(1) | 0.99(1)     | 0.99(2)   | 0.993(8) |

Here are some sample results for the two models. It is clear that the CNN has higher certainty in its predictions as compared to logistic regression.

## Convolutional Neural Network
![CNN](https://github.com/iafinn/cactus-image-id/blob/master/report/figures/CNN.png)


## Logistic Regression
![LR](https://github.com/iafinn/cactus-image-id/blob/master/report/figures/LR.png)


# Model Design

Here are schematic representations of the two models with some explanation of model design.


## Logistic Regression

For the logistic regression model there are 3073 parameters in the fit. The input images are 
```
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
input_1 (InputLayer)         (None, 32, 32, 3)         0         
_________________________________________________________________
flatten_1 (Flatten)          (None, 3072)              0         
_________________________________________________________________
fc_2 (Dense)                 (None, 1)                 3073      
=================================================================
Total params: 3,073
Trainable params: 3,073
Non-trainable params: 0
_________________________________________________________________
```



