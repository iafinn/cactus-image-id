# Results

I developed two models for identifying columnar cactus in aerial photographs. In the first model, I used logistic regression, a relatively simple machine learning algorithm. For the second model, I used a 5-layer convolutional neural network. This is a more modern model, with design guidelines that I learned in Andrew Ng's Deep Learning Specialization from Coursera. I evaluated the models with a 10-fold cross validation. Data were shuffled and split into 10 parts for 10 separate 90/10 train/test splits to evaluate their performance. The mean accuracy, precision, recall, and F1 scores for the models are given below with the standard deviation of the last digit in parentheses. The CNN had superior performance for all evaluation metrics. Most notable is the F1 score of 0.993(8) for the CNN and 0.933(4) for logistic regression.

| Method              | Accuracy | Precision | Recall | F1   |
|---------------------|----------|-----------|--------|------|
| LR (Scikit-learn)   | 0.900(6)     | 0.940(7)      | 0.927(6)   | 0.933(4) |
| 5-layer CNN (TensorFlow) | 0.98(1) | 0.99(1)     | 0.99(2)   | 0.993(8) |

Here are some sample results for the two models in the test set. It is clear that the CNN has higher certainty in its predictions as compared to logistic regression. The cactus label is given as "Cactus = 1" for an image containing cactus and "Cactus = 0" for an image not containing cactus. The output of the models are given as "Prediction = ". For the final prediction, these values are rounded to the nearest 0 or 1 (cutoff=0.5). This is an ok default choice for situations that don't favor the precision or recall.

## Convolutional Neural Network
![CNN](https://github.com/iafinn/cactus-image-id/blob/master/report/figures/CNN.png)

## Logistic Regression
![LR](https://github.com/iafinn/cactus-image-id/blob/master/report/figures/LR.png)

# Model Design

Here are schematic representations of the two models with some explanation of model design.


## Logistic Regression

For the logistic regression model there are 3073 parameters in the fit. The input images are first flattened from 32 x 32 x 3 tensors into 1 x 3072 vectors. They are input to a sigmoid activation with 3072 + 1 parameters, where the extra parameter comes from the constant bias term. This is the same as a single layer fully-connected neural network with sigmoid activation. The None in the output shape in the schematic indicates the dimension m for the size of the training set.
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

## Convolutional Neural Network

The convolutional neural network has many more parameters to fit -- 14,465 in total. First the images are passed to a set of 8 3 x 3 x 3 (for RGB) convolutional filters with same padding (1 in this case). These filters move across the image to generate a tensor of the same size in x and y, but with 8 instead of 3 filters. The output of the filters are normalized with batch normalization and then passed to a RELU activation followed by a max pooling layer. This is a fairly standard design for image recognition.

This is followed by two more convolutional layers of the same general structure. Each layer, however, reduces the x and y sizes of the tensor, while increasing the number of filters resulting in a final dimension of 2 x 2 x 32. This is flattened into a 1 x 128 vector, and input to two fully connected (dense) layers. The first of these layers has a standard RELU activation, while the last layer has a sigmoid activation, so that the output is bounded by [0,1]. This overall structure of several convolutional layers with increasing filter number, followed by fully connected layers for the output has been shown to be an effective hyperparamter design for image recognition.

```
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
input_1 (InputLayer)         (None, 32, 32, 3)         0         
_________________________________________________________________
conv_1 (Conv2D)              (None, 32, 32, 8)         224       
_________________________________________________________________
bn_1 (BatchNormalization)    (None, 32, 32, 8)         32        
_________________________________________________________________
activation_1 (Activation)    (None, 32, 32, 8)         0         
_________________________________________________________________
max_pool_1 (MaxPooling2D)    (None, 16, 16, 8)         0         
_________________________________________________________________
conv_2 (Conv2D)              (None, 16, 16, 16)        1168      
_________________________________________________________________
bn_2 (BatchNormalization)    (None, 16, 16, 16)        64        
_________________________________________________________________
relu_1 (Activation)          (None, 16, 16, 16)        0         
_________________________________________________________________
max_pool_2 (MaxPooling2D)    (None, 8, 8, 16)          0         
_________________________________________________________________
conv_3 (Conv2D)              (None, 4, 4, 32)          4640      
_________________________________________________________________
bn_3 (BatchNormalization)    (None, 4, 4, 32)          128       
_________________________________________________________________
activation_2 (Activation)    (None, 4, 4, 32)          0         
_________________________________________________________________
max_pool_3 (MaxPooling2D)    (None, 2, 2, 32)          0         
_________________________________________________________________
flatten_1 (Flatten)          (None, 128)               0         
_________________________________________________________________
fc_1 (Dense)                 (None, 64)                8256      
_________________________________________________________________
fc_2 (Dense)                 (None, 1)                 65        
=================================================================
Total params: 14,577
Trainable params: 14,465
Non-trainable params: 112
_________________________________________________________________
```
