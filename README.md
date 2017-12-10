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
### To see the comparison between original images and generated images
Please check [MNIST_generated](https://github.com/mariko-poyo/adversary-images-challenge/tree/master/MNIST_generated)

### To follow the procedure from the beginning
#### linear regression model
1. run every cell in the section called **Preparation**
2. set hyper parameters at the first cell of the section named **linear regression model**
3. run all cells under section named **linear regression model**
#### ConvNet model
1. run every cell in the section called **Preparation**
2. set hyper parameters at the first cell of the section named **ConvNet model**
3. run all cells under section named **ConvNet model**

## Experiments on hyper parameter values
#### linear regression model
I change the training epochs value and epsilon of the noise added to original image, and see the relationship between them.
result:
the numbers on the table are error rate on classification of adversary images 

#### ConvNet model
I try to see the relationship between:  
* how many epochs the model is trained and how many epochs are needed to fool the trained netowrk(=how erasy it is to fool the network). On playing with those hyper parameters, I set learning rate on generating adversary images to 0.2
result:
the numbers on the table are error rate on classification of adversary images 
  
| train epochs\adverasry epochs | 10 | 30 | 50 | 70 | 100 | 150 |  
|---|---|---|---|---|---|---|  
| 20000(accuracy:0.9934) | 0.02 | 0.1 | 0.4 | 0.9 | 1 | 1 | 
| 10000(accuracy:0.9915) | 0 | 0.04 | 0.26  | 0.66 | 1 | 1 | 
| 5000(accuracy:0.9872) | 0 | 0.04 | 0.26 | 0.52 | 0.92 | 1 | 
| 1000(accuracy:0.9601) | 0 | 0.12 | 0.34 | 0.78 | 0.94 | 1 | 


* traininng rate on generating adversary images and how many epochs do I need on adversary image generation. On testing those parameters, I trained the model for 20000 epochs.
result:
the numbers on the table are error rate on classification of adversary images 

### observation
On linear regression model, I need to add the weight parameter of the ttarget class, which is the class of digit six for this time

## References
* [Breaking Linear Classifiers on ImageNet](http://karpathy.github.io/2015/03/30/breaking-convnets/)
* [Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572)
* [AdversarialMNIST/generate_adversarial_examples.ipynb](https://github.com/jmgilmer/AdversarialMNIST/blob/master/generate_adversarial_examples.ipynb)
