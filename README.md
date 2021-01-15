# Facial Recogntion using PCA and LDA
---
Images were downloaded from http://www-prima.inrialpes.fr/perso/Gourier/Faces/HPDatabase.htmlw and requires the following citation: 

N. Gourier, D. Hall, J. L. Crowley
Estimating Face Orientation from Robust Detection of Salient Facial Features
Proceedings of Pointing 2004, ICPR, International Workshop on Visual Observation of Deictic Gestures, Cambridge,

## Introduction

This project is meant to show the implementation of combining [Principal Component Analysis](https://en.wikipedia.org/wiki/Principal_component_analysis) and [Linear Discriminant Analysis](https://en.wikipedia.org/wiki/Linear_discriminant_analysis), and their application to facial recognition. A summary of the program is as follows:

1. Read in training images for each class (face) as grayscale and store each image as a vector, where the 2-d image is converted to a 1-d vector by appending each row to the previous. 
2. Stack all the image vectors into a matrix 
3. Compute the PCA transformation matrix
4. Project our sample data onto this new subspace
5. Compute the LDA transformation matrix 
6. Project the PCA data onto this new subspace 

## Principal Component Analysis (PCA)

PCA is an algorithm used for reducing the dimensionality of data by projecting it onto a lower-dimensional subspace that captures most of the variance in the data. The resulting dimensionality reduction can be drastic and allows us to do computations much more quickly.

## Linear Discriminant Anaysis (LDA)

LDA is an algorithm used for reducing the dimensionality of data by projecting it onto a lower-dimensional subspace that best seperates classes. This results in dimensionality reduction and a way to classify different objects.

## Testing
For the program to make the best prediction as to what class the input data belongs to, we will do the following:
1. Read in the image and project it onto the PCA components
2. Project the projected PCA data onto the LDA components.
3. Do a nearest neighbour search on the projected test image and the projected training images in this lower dimensional space.
4. The nearest point is our predicted image.

For the predicted image, I have one image per person that shows the front of their face. This is used to show what the person looks like, not what pose they are in.

## Example Output
We use images from a set of images that were not used for training. We call this the training set. This is to ensure that the classifier and correctly determine the person even if it has never seen that picture before. In addition, we will add various types of noise to the image and see how well it does. 
--image goes here--

**For full details on the algorithm, please see jupyter notebook
