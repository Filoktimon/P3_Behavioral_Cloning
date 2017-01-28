
###############################

  Behavioral Cloning Project

###############################

## Overview

A description of the python code, the data preprocessing and the deep learning model, used to teach the car driving itself on the first track of the simulator for the behavioral cloning of the Udacity, Self Drivng Car Engineer Nanodegree.
After learning the behavior of the driver, the model can drive the car havinf as inputs the images from the cameras and directly controling the steering angle.

## preprocess.py
This python script imports the raw image data and resizes them.

I resized the image because image contains unnecessary background content such as sky, river, and trees.

I decided to remove them and reduced the size of the image by 25%, then I used only one channel from each image. From the nanodegree communication channels, reading the comments of more experienced students, I found that the image data do not have to have a the whole pixel content when training the model. Reducing the size by 25% and using just one channel was more efficient in terms of time and space.

I saved them as features and saved the data of steering angels as labels.

Then, I splitted the data into train and validation, and saved them as frontcamera.pickle file.

## model.py
The main purpose of this script is to train the model using the data saved from the preprocessing python script.

First, it imports the pickle file from the local directory and trains the data using the designed model.

The description of the model can be found in the script model.py and bellow.

When the training completes, the model and weights are saved as model.json and model.h5.

## drive.py
This is the python script that receives the running data from the simulator, i.e., driving state and images, predicts the new steering angle using the deep learning model, and send the throttle and the predicted angles back to the simulator.

Since the images were reshaped and normalized during training, the image from the simulator is reshaped and normalized just as in preprocess.py and model.py in order match with what the model has used as training input.

######################

## Training

Below is the summary of the model I designed to be trained using the data.

        ___________________________________________________________________________________________________
        Layer (type)                     Output Shape          Param #     Connected to                     
        ====================================================================================================
        convolution2d_1 (Convolution2D)  (None, 16, 78, 16)    160         convolution2d_input_1[0][0]      
        ____________________________________________________________________________________________________
        activation_1 (Activation)        (None, 16, 78, 16)    0           convolution2d_1[0][0]            
        ____________________________________________________________________________________________________
        convolution2d_2 (Convolution2D)  (None, 14, 76, 8)     1160        activation_1[0][0]               
        ____________________________________________________________________________________________________
        activation_2 (Activation)        (None, 14, 76, 8)     0           convolution2d_2[0][0]            
        ____________________________________________________________________________________________________
        convolution2d_3 (Convolution2D)  (None, 12, 74, 4)     292         activation_2[0][0]               
        ____________________________________________________________________________________________________
        activation_3 (Activation)        (None, 12, 74, 4)     0           convolution2d_3[0][0]            
        ____________________________________________________________________________________________________
        convolution2d_4 (Convolution2D)  (None, 10, 72, 2)     74          activation_3[0][0]               
        ____________________________________________________________________________________________________
        activation_4 (Activation)        (None, 10, 72, 2)     0           convolution2d_4[0][0]            
        ____________________________________________________________________________________________________
        maxpooling2d_1 (MaxPooling2D)    (None, 5, 36, 2)      0           activation_4[0][0]               
        ____________________________________________________________________________________________________
        dropout_1 (Dropout)              (None, 5, 36, 2)      0           maxpooling2d_1[0][0]             
        ____________________________________________________________________________________________________
        flatten_1 (Flatten)              (None, 360)           0           dropout_1[0][0]                  
        ____________________________________________________________________________________________________
        dense_1 (Dense)                  (None, 16)            5776        flatten_1[0][0]                  
        ____________________________________________________________________________________________________
        activation_5 (Activation)        (None, 16)            0           dense_1[0][0]                    
        ____________________________________________________________________________________________________
        dense_2 (Dense)                  (None, 16)            272         activation_5[0][0]               
        ____________________________________________________________________________________________________
        activation_6 (Activation)        (None, 16)            0           dense_2[0][0]                    
        ____________________________________________________________________________________________________
        dense_3 (Dense)                  (None, 16)            272         activation_6[0][0]               
        ____________________________________________________________________________________________________
        activation_7 (Activation)        (None, 16)            0           dense_3[0][0]                    
        ____________________________________________________________________________________________________
        dropout_2 (Dropout)              (None, 16)            0           activation_7[0][0]               
        ____________________________________________________________________________________________________
        dense_4 (Dense)                  (None, 1)             17          dropout_2[0][0]                  
        ====================================================================================================
        Total params: 8023

---
## Conclusion

The whole image is not necessary to train the model. Using the comments of more experinced developers I decided to cut those unncessary pixels and reduced the size by 25%. I only used red channel of the image because I assumed that red channel contains more useful information for identifying the road and lanes than green and blue channels. As a result, the size of the image was 18 x 80 x 1. 

In my model, I used 4 convolutional layers with 1 max pooling layer, and 3 more dense layers after flatten the matrix. For each convolutional layer, I decreased the channel size by half. When the size of the channel became 2 in the fourth convolutional layer, I applied max pooling with dropout with 25%. After flatten the matrix, the size of features became 360. I used dense layers with 16 features 4 times. Each epoch took about 100 seconds and I used 10 epoches to train the data. As a result, the car drove by itself without popping onto the edges or out of the edges.


