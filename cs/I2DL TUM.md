
## Lecture 02: Machine Learning Basics

**Supervised Learning:**

New input will be classified according to their nearest neighbor
For 5NN classifier: take the average of 5 nearest neighbors

![[I2DL TUM-1783446464807.webp]]


**Unsupervised learning** means you are given a set of images, don't come with labels, similarity between objects
-> can i find a hyperplane that separates two classes

![[I2DL TUM-1783446643575.webp]]


**Reinforcement learning:**

Based on the environment the interactions get some reward

![[I2DL TUM-1783446688756.webp]]

Work well for game because you can give score for correct behavior
Much difficult for game like chess because you make one move and not sure if it's a good move or not. One move maybe have affect after 20-30 moves.


### Linear Regression

![[I2DL TUM-1783458951032.webp]]


![[I2DL TUM-1783458980752.webp]]

![[I2DL TUM-1783459032431.webp]]

Loss function: measure how good my estimation is
Optimization: changes the model in order to improve the loss function

You can draw a hand-curve that fit the training data but not generalize well.
Key idea of linear regression: Assumption of linearity
-> there is a linear dependency between input and output

![[I2DL TUM-1783459422931.webp]]

![[I2DL TUM-1783459678739.webp]]

#### Linear Regression Loss in Matrix Form
##### 1. Scalar form

$$
J(\theta) = \frac{1}{n}\sum_{i=1}^{n}(\hat{y}_i - y_i)^2
$$

Because the prediction is:
$$
\hat{y}_i = x_i\theta
$$
we can write:
$$
J(\theta) = \frac{1}{n}\sum_{i=1}^{n}(x_i\theta - y_i)^2
$$
##### 2. Matrix form
Stack all input rows into matrix \(X\):
$$
X =
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n
\end{bmatrix}
$$
Stack all target values into vector \(y\):

$$
y =
\begin{bmatrix}
y_1 \\
y_2 \\
\vdots \\
y_n
\end{bmatrix}
$$
Then all predictions can be computed at once:
$$
X\theta =
\begin{bmatrix}
x_1\theta \\
x_2\theta \\
\vdots \\
x_n\theta
\end{bmatrix}
$$
The error vector is:
$$
X\theta - y =
\begin{bmatrix}
x_1\theta - y_1 \\
x_2\theta - y_2 \\
\vdots \\
x_n\theta - y_n
\end{bmatrix}
$$

Let:
$$
r = X\theta - y
$$

Then the loss becomes:
$$
J(\theta) = \frac{1}{n}(X\theta - y)^T(X\theta - y)
$$

or:
$$
J(\theta) = \frac{1}{n}r^T r
$$

---
#### Why do we use \(r^T r\)?

##### 1. Residual vector
The residual vector is:

$$
r = X\theta - y
$$
Written as a column vector:

$$
r =
\begin{bmatrix}
r_1 \\
r_2 \\
\vdots \\
r_n
\end{bmatrix}
$$
Each entry \(r_i\) is one prediction error.

---

##### 2. Transpose the vector

The transpose turns the column vector into a row vector:
$$
r^T =
\begin{bmatrix}
r_1 & r_2 & \cdots & r_n
\end{bmatrix}
$$

---

##### 3. Multiply \(r^T\) by \(r\)
$$
r^T r =
\begin{bmatrix}
r_1 & r_2 & \cdots & r_n
\end{bmatrix}
\begin{bmatrix}
r_1 \\
r_2 \\
\vdots \\
r_n
\end{bmatrix}
$$

This is a dot product.

---
##### 4. Expand the dot product

$$
r^T r = r_1r_1 + r_2r_2 + \cdots + r_nr_n
$$
Since each term is multiplied by itself:

$$
r^T r = r_1^2 + r_2^2 + \cdots + r_n^2
$$
So:
$$
r^T r = \sum_{i=1}^{n} r_i^2
$$
Because:
$$
r_i = x_i\theta - y_i
$$

we get:

$$
r^T r = \sum_{i=1}^{n}(x_i\theta - y_i)^2
$$
Therefore:
$$
(X\theta - y)^T(X\theta - y)
=
\sum_{i=1}^{n}(x_i\theta - y_i)^2
$$
---
##### 5. Small numeric example
Suppose:

$$
r =
\begin{bmatrix}
2 \\
-1 \\
3
\end{bmatrix}
$$
Then:

$$
r^T =
\begin{bmatrix}
2 & -1 & 3
\end{bmatrix}
$$
Now multiply:

$$
r^T r =
\begin{bmatrix}
2 & -1 & 3
\end{bmatrix}
\begin{bmatrix}
2 \\
-1 \\
3
\end{bmatrix}
$$

Expand:

$$
r^T r = 2^2 + (-1)^2 + 3^2
$$

$$
r^T r = 4 + 1 + 9 = 14
$$

---
##### Key idea

$$
\text{vector}^T \times \text{vector}
=
\text{sum of squares of all entries}
$$
So when \(r\) is the error vector:

$$
r^T r
=
\text{sum of squared errors}
$$

![[I2DL TUM-1783462891792.webp]]

gradient = how each parameter θ contributes to the total error

## Lecture 03: Introduction to Neural Networks

