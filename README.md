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
  
| train epochs\epsilon | 0.007 | 0.01 | 0.07 | 0.1 | 0.25 |  
|---|---|---|---|---|---|  
| 20000(accuracy:≈0.9227) | 0 | 0 | 0.599 | 0.799 | 1 |  
| 10000(accuracy:≈0.9227) | 0 | 0 | 0.599  | 0.799 | 1 |  
| 5000(accuracy:≈0.9225) | 0 | 0 | 0.5 | 0.6 | 1 |   
| 1000(accuracy:≈0.9152) | 0 | 0| 0.3  | 0.5 | 1 |   
| 200(accuracy:≈0.9027) | 0 | 0 | 0.3 | 0.5 | 1 |  
| 40(accuracy:≈0.87) | 0 | 0 | 0.3 | 0.3 | 0.89 |   


#### ConvNet model
I try to see the relationship between:  
case1. how many epochs the model is trained and how many epochs are needed to fool the trained netowrk(=how erasy it is to fool the network). On playing with those hyper parameters, I set learning rate on generating adversary images to 0.2
result:
the numbers on the table are error rate on classification of adversary images 
  
| train epochs\adverasry epochs | 10 | 30 | 50 | 70 | 100 | 150 |  
|---|---|---|---|---|---|---|  
| 20000(accuracy:0.9934) | 0.02 | 0.1 | 0.4 | 0.9 | 1 | 1 | 
| 10000(accuracy:0.9915) | 0 | 0.04 | 0.26  | 0.66 | 1 | 1 | 
| 5000(accuracy:0.9872) | 0 | 0.04 | 0.26 | 0.52 | 0.92 | 1 | 
| 1000(accuracy:0.9601) | 0 | 0.12 | 0.34 | 0.78 | 0.94 | 1 | 


case2. traininng rate on generating adversary images and how many epochs do I need on adversary image generation. On testing those parameters, I trained the model for 20000 epochs.
result:
the numbers on the table are error rate on classification of adversary images  
  
| adversary epochs\adverary learning rate | 0.1 | 0.2 | 0.4 | 0.8 | 1 |  
|---|---|---|---|---|---|  
| 10 | 0 | 0 | 0.02 | 0.2 | 0.44 |   
| 30 | 0.02 | 0.06| 0.74 | 1 | 1 |   
| 50 | 0.04 | 0.4 | 1 | 1 | 1 |  
| 70 | 0.12 | 0.86 | 1 | 1 | 1 |  
| 100 | 0.42 | 1 | 1 | 1 | 1 |  
| 150 | 0.86 | 1 | 1 | 1 | 1 |  

case3. how many epochs the model is trained and value of epsilon used for adding perturbation to generate adversary images
result:  
the numbers on the table are error rate on classification of adversary images  
  
| train epochs\epsilon | 0.007 | 0.01 | 0.07 | 0.1 | 0.25 | 0.4 | 0.6 | 0.8 |  
|---|---|---|---|---|---|---|---|---|  
| 20000(accuracy:0.9934) | 0.02 | 0.02 | 0.18 | 0.42 | 0.72 | 0.82 | 0.86 | 0.92 |  
| 10000(accuracy:0.9915) | 0 | 0.02 | 0.2  | 0.36 | 0.84 | 0.88 | 0.88 | 0.88 |  
| 5000(accuracy:0.9872) | 0 | 0 | 0.34 | 0.5 | 0.96 | 0.98 | 0.98 | 0.98 |  
| 1000(accuracy:0.9601) | 0.02 | 0.02 | 0.38 | 0.54 | 0.92 | 0.98 | 0.98 | 0.98 |  

### observation
##### general
While I can deceive linear model by just adding the weight parameter of the digit six due to its linearly,  the noise added on ConvNet model are always not classified as digit six, instead they are classified as 1, 5, or 7 most of the time and not consistent among the noise data. It is also rare to see that generated adcersary images disguise as digit six when I try to fool the ConvNet model by adding perturbation to original images.  

##### parameters
* As the accuracy of the model increases, it tends to become easier to fool the linear model. I think it means that netowrk learns the feature value of each class properly and thus the weight added as noise also works properly to be classified as digit six.
* It seems that the model with higher accuracy tends to have more resistance to the perturbation attack(ConvNet-case3), but the difference between each model is not so big and can be within error margin.
* I cannot find string relationship between the accuracy of the ConvNet model and number of epochs needed to disguise original images to digit six for 100 percent sure.
* It seems that ConvNet model is more resistant to adverasry image attack. By comparing the error rate table for the linear model and case3 of ConvNet model at train epoch is 20000 with epsilon value of 0.1, error rate on linear model reaches at aournd 0.8 while that on the ConvNet is 0.42.    

## References
* [Breaking Linear Classifiers on ImageNet](http://karpathy.github.io/2015/03/30/breaking-convnets/)
* [Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572)
* [AdversarialMNIST/generate_adversarial_examples.ipynb](https://github.com/jmgilmer/AdversarialMNIST/blob/master/generate_adversarial_examples.ipynb)
