# Generate adversarual images to fool a MNIST classifier on TensorFlow
I am trying to generate the adversary images based on MNIST datas

## Contents
1. README.md -- this one
2. mnist_adversary.ipynb
* train linear regression model and ConvNet model of MNIST classifier based on TensorFlow tutorial called [Deep MNIST for experts](https://www.tensorflow.org/get_started/mnist/pros)
* extract the image of digit two which classified correctly by the classifiers
* generate the adverasry images using trained both models
3. mnist_extract_imgs.p
* pickle the extracted image of digit two
4. MNIST_weight
* weight parameter values of ConvNet model and linear regression model are saved
5. MNIST_generated
* images comparing original image, delta, and generated images are saved

## How to generate adversary image by using the code
#### linear regression model
1. run every cell in the section called **Common Part**
2. set the values of epochs and batch size, and run all cells under section called **linear regression model**
#### ConvNet model
1. run evert cell in the section called **Common Part**
2. set the values of epochs and batch size, and run all cells under section called **ConvNet model**

## Experiments on parameter values and analysis

## References
* [Breaking Linear Classifiers on ImageNet](http://karpathy.github.io/2015/03/30/breaking-convnets/)
* [Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572)
* 
