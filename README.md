# Generate adversarual images to fool a MNIST classifier on TensorFlow
This repository tries to generate adversary images of MNIST dataset by expanding [Deep MNIST for Experts](https://www.tensorflow.org/get_started/mnist/pros). This README explanes about the contents of repository, the process to generate adversary images, and Observation on the adversary image generation.

## Contents
1. README.md
2. [mnist_adversary.ipynb](https://github.com/mariko-poyo/adversary-images-challenge/blob/master/mnist_adversary.ipynb)  
  This file contains functions to:
  * train linear regression model and ConvNet model of MNIST classifier based on TensorFlow tutorial called [Deep MNIST for Experts](https://www.tensorflow.org/get_started/mnist/pros)
  * extract the image of digit two which classified correctly by the classifiers
  * generate the adverasry images using trained both models
3. [mnist_extract_imgs.p](https://github.com/mariko-poyo/adversary-images-challenge/blob/master/mnist_extract_imgs.p)
  * This file stores the array of extracted image of digit two in pickle format.
4. [MNIST_weight(directory)](https://github.com/mariko-poyo/adversary-images-challenge/tree/master/MNIST_weight)
  * This directory stores the weight parameter values of ConvNet model and linear regression model
5. [MNIST_generated(directory)](https://github.com/mariko-poyo/adversary-images-challenge/tree/master/MNIST_generated)
  * This directory stores images comparing original image, delta, and generated images

## Process of generating adversary image by using the code
Please check [MNIST_generated](https://github.com/mariko-poyo/adversary-images-challenge/tree/master/MNIST_generated) to see the generated results quickly. The procedures to generate the adversary images of digit two are as follows:
#### linear regression model
1. run every cell in the section called **Preparation**
2. set hyper parameters at the first cell of the section named **linear regression model**
3. run all cells under section named **linear regression model**
#### ConvNet model
1. run every cell in the section called **Preparation**
2. set hyper parameters at the first cell of the section named **ConvNet model**
3. run all cells under section named **ConvNet model**

## Experiments on hyper parameter values
In this section, I changed some hyper parameter values and observed the relationships between them. The results for each comparison are shown as tables below. For observation, please look at next section.  
The numbers on the tables are error rate on classification of adversary images.
#### linear regression model
Changing the training epochs value and epsilon of the noise added to original image, and see the relationship between them.
  
| train epochs\epsilon | 0.007 | 0.01 | 0.07 | 0.1 | 0.25 |  
|---|---|---|---|---|---|  
| 20000(accuracy:≈0.9227) | 0 | 0 | 0.599 | 0.799 | 1 |  
| 10000(accuracy:≈0.9227) | 0 | 0 | 0.599  | 0.799 | 1 |  
| 5000(accuracy:≈0.9225) | 0 | 0 | 0.5 | 0.6 | 1 |   
| 1000(accuracy:≈0.9152) | 0 | 0| 0.3  | 0.5 | 1 |   
| 200(accuracy:≈0.9027) | 0 | 0 | 0.3 | 0.5 | 1 |  
| 40(accuracy:≈0.87) | 0 | 0 | 0.3 | 0.3 | 0.89 |   


#### ConvNet model 
case1. Changing number of epochs the model is trained and number of epochs needed to fool the trained netowrk(=how erasy it is to fool the network). On playing with those hyper parameters, learning rate on generating adversary images is set to 0.2 
  
| train epochs\adverasry epochs | 10 | 30 | 50 | 70 | 100 | 150 |  
|---|---|---|---|---|---|---|  
| 20000(accuracy:0.9934) | 0.02 | 0.1 | 0.4 | 0.9 | 1 | 1 | 
| 10000(accuracy:0.9915) | 0 | 0.04 | 0.26  | 0.66 | 1 | 1 | 
| 5000(accuracy:0.9872) | 0 | 0.04 | 0.26 | 0.52 | 0.92 | 1 | 
| 1000(accuracy:0.9601) | 0 | 0.12 | 0.34 | 0.78 | 0.94 | 1 | 


case2. Changing traininng rate on generating adversary images and number epochs needed for adversary image generation. On testing those parameters, the model is trained for 20000 epochs.
  
| adversary epochs\adverary learning rate | 0.1 | 0.2 | 0.4 | 0.8 | 1 |  
|---|---|---|---|---|---|  
| 10 | 0 | 0 | 0.02 | 0.2 | 0.44 |   
| 30 | 0.02 | 0.06| 0.74 | 1 | 1 |   
| 50 | 0.04 | 0.4 | 1 | 1 | 1 |  
| 70 | 0.12 | 0.86 | 1 | 1 | 1 |  
| 100 | 0.42 | 1 | 1 | 1 | 1 |  
| 150 | 0.86 | 1 | 1 | 1 | 1 |  

case3. Chaging number of epochs the model is trained and value of epsilon used for adding perturbation to generate adversary images.
  
| train epochs\epsilon | 0.007 | 0.01 | 0.07 | 0.1 | 0.25 | 0.4 | 0.6 | 0.8 |  
|---|---|---|---|---|---|---|---|---|  
| 20000(accuracy:0.9934) | 0.02 | 0.02 | 0.18 | 0.42 | 0.72 | 0.82 | 0.86 | 0.92 |  
| 10000(accuracy:0.9915) | 0 | 0.02 | 0.2  | 0.36 | 0.84 | 0.88 | 0.88 | 0.88 |  
| 5000(accuracy:0.9872) | 0 | 0 | 0.34 | 0.5 | 0.96 | 0.98 | 0.98 | 0.98 |  
| 1000(accuracy:0.9601) | 0.02 | 0.02 | 0.38 | 0.54 | 0.92 | 0.98 | 0.98 | 0.98 |  

### Observation
##### general
Although I can deceive linear model by just adding the weight parameter of the digit six due to its linearly,  the noise added on ConvNet model are always not classified as digit six, instead they are classified as 1, 5, or 7 most of the time and not consistent among the noise data. It is also rare to see that generated adcersary images disguise as digit six when I try to fool the ConvNet model by adding perturbation to original images.  

##### parameters
* As the accuracy of the model increases, it tends to become easier to fool the linear model. It is possible that netowrk learns the feature value of each class properly and thus the weight added as noise also works properly to be classified as digit six.
* It seems that the model with higher accuracy tends to have more resistance to the perturbation attack(ConvNet-case3), but the difference between each model is not so big and can be within error margin.
* I cannot find string relationship between the accuracy of the ConvNet model and number of epochs needed to disguise original images to digit six for 100 percent sure.
* It seems that ConvNet model is more resistant to adverasry image attack. By comparing the error rate table for the linear model and case3 of ConvNet model at train epoch is 20000 with epsilon value of 0.1, error rate on linear model reaches at aournd 0.8 while that on the ConvNet is 0.42. One of the possibilities is the ConvNet succeeded to learn some non-linear feature of the digit and those makes the difference between their resistancy to adverary attack.    

## References
* [Deep MNIST for Experts](https://www.tensorflow.org/get_started/mnist/pros)
* [Breaking Linear Classifiers on ImageNet](http://karpathy.github.io/2015/03/30/breaking-convnets/)
* [Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572)
* [AdversarialMNIST/generate_adversarial_examples.ipynb](https://github.com/jmgilmer/AdversarialMNIST/blob/master/generate_adversarial_examples.ipynb)

