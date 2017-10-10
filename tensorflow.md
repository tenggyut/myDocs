##TensorFlow

###CNN

output size = `(W−F+2P)/S+1`

- F: filter size
- W: input size
- P: padding
- S: stride

if `P=(F−1)/2`, then `input size = output size`

###Shape

shape is a vector which holds the dimensionality number of a tensor. the first element's value equals the length of the array, and the second element's value equals the length of one element of the array, and etc..

###Activation Function

- 线性整流函数（Rectified Linear Unit, ReLU): `f(x) = max(0, x)`