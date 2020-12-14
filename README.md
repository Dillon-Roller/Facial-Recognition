# Facial Recogntion using PCA and LDA
---
Images were downloaded from http://www-prima.inrialpes.fr/perso/Gourier/Faces/HPDatabase.htmlw and requires the following citation: 

N. Gourier, D. Hall, J. L. Crowley
Estimating Face Orientation from Robust Detection of Salient Facial Features
Proceedings of Pointing 2004, ICPR, International Workshop on Visual Observation of Deictic Gestures, Cambridge,

## Introduction

This project is meant to show the implementation of [Principal Component Analysis](https://en.wikipedia.org/wiki/Principal_component_analysis) and [Linear Discriminant Analysis](https://en.wikipedia.org/wiki/Linear_discriminant_analysis), and their application to facial recognition. The algorithm is done in a few steps, which we will detail soon:
1. Read in training images for each class (face) as grayscale and store each image as a vector, where the 2-d image is converted to a 1-d vector by appending each row to the previous. 
2. Stack all the image vectors into a matrix $X$ 
3. Compute the PCA transformation matrix $P = \texttt{PCA}(X)$
4. Project our sample data $X$ onto this new subspace to get $X_{pca} = X \times P $
5. Compute the LDA transformation matrix $L = \texttt{LDA}(X_{pca})$
6. Project the PCA data onto this new subspace to get $X_{lda} = X_{pca} \times L $
7. $X_{lda}$ is our sample data with reduced dimensionality

## Principal Component Analysis (PCA)

PCA is an algorithm used for reducing the dimensionality of data by projecting it onto a lower-dimensional subspace that captures most of the variance in the data. The resulting dimensionality reduction can be drastic and allows us to do computations much more quickly.

### Algorithm:
1. Compute the [singular value decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition) of $X: \textbf{e}, \textbf{λ} = \texttt{SVD}(X)$ where $\textbf{e}$ is the set of [eigenvectors](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors), sorted by decreasing [eigenvalue](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors).
2. We need to choose how many dimensions $k$ to keep for the construction of our subspace. The energy recovered given a dimension $k$ is given by: $$ \texttt{energy}(X, \textbf{λ}, k) = \sum_{i=1}^k \frac{\textbf{λ}_i}{\| \mathbf{X} \|_F^2} $$ where $\| \cdot \|_F$ is the [Frobenius norm](https://en.wikipedia.org/wiki/Matrix_norm#Frobenius_norm). This sum is the percentage of variance retained. Therefore, we can run this function with increasing $k$ until the percentage we want is met.
3. Keep the first $k$ eigenvectors and stack them into a matrix $P = (\textbf{e}_1, \textbf{e}_2, ..., \textbf{e}_k)$
4. Matrix $P$ is used to project the data onto the PCA components.

## Linear Discriminant Anaysis (LDA)

LDA is an algorithm used for reducing the dimensionality of data by projecting it onto a lower-dimensional subspace that best seperates classes. This results in dimensionality reduction and a way to classify different objects.

### Algorithm:
1. Compute the mean image in each class: $$ \mu_i = \frac{1}{n_i}\sum_{\textbf{x}\in X_i}^{n_i} \textbf{x} $$ where $i$ is the class, $n_i$ is the number of samples for that class, and $X_i$ is the image matrix for that class (where each row is an image in that class).
2. Compute the mean of all images: $$ \mu_{overall} = \frac{1}{N}\sum_{\textbf{x} \in X}^N \textbf{x} $$ where $N$ is the total number of images for training.
3. Compute the within-class scatter matrix: $$S_W = \sum_{i=1}^c S_i$$ where $$ S_i = \sum_{\textbf{x}\in X_i}^{n_i} (\textbf{x} - \mu_i)(\textbf{x} - \mu_i)^T$$ where $c$ is the number of classes, $T$ is the [matrix transpose](https://en.wikipedia.org/wiki/Transpose), and multiplication is with regards to the [outer product](https://en.wikipedia.org/wiki/Outer_product).
4. Compute the between-class pscatter matrix](https://en.wikipedia.org/wiki/Scatter_matrix): $$ S_B = \sum_{i=1}^c n_i(\mu_i - \mu_{overall})(\mu_i - \mu_{overall})^T $$
5. Compute $\textbf{e} = \texttt{SVD}(S_W^{-1} \times S_B)$
6. Keep the first $c - 1$ eigenvectors to form a basis for the projection: $L =  (\textbf{e}_1, \textbf{e}_2, ..., \textbf{e}_{c-1})$
7. Matrix $L$ is used to project the data onto the LDA components.

## Testing
For the program to make the best prediction as to what class the input data belongs to, we will do the following:
1. Read in the image and project it onto the PCA components
2. Project the projected PCA data onto the LDA components.
3. Do a nearest neighbour search on the projected test image and the projected training images in this $k$ dimensional space.
4. The nearest point is our predicted image.

For the predicted image, I have one image per person that shows the front of their face. This is used to show what the person looks like, not what pose they are in.
