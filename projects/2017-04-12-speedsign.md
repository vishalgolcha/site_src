---
layout: post
<!-- date : 2017-04-12 -->
excerpt: "A hackathon sans GPUs"
categories: [projects]
comments: True
permalink: /projects/speedsign/
---

## BITS-ACM Map My India Hackathon 

[Competition Outline](https://www.kaggle.com/c/mapmyindia2) | [Github Link](https://github.com/vishalgolcha/Speed-Sign-Detector-Kaggle)

In this competition, MapmyIndia asked us to locate Speed Signs from StreetView Scene Images. The Speed Signs are divided into these six classes: Speed Limit 20, Speed Limit 30, Speed Limit 40, Speed Limit 50, Speed Limit 60, Speed Limit 80.

At the onset we had only 5 days for the contest . We also had a relatively small dataset needed per say for a deep learning task. 

Deep Learning was out of the picture , we had to make a model that could train on our non-gpu systems and do that fairly quickly .

Some sample images are shown belows:

![sample 1](speed/ss1.jpg)
![sample 2](speed/ss2.jpg)
![sample 3](speed/ss3.jpg)
![sample 4](speed/ss4.jpg) 


The takehome message from these examples and a few more of them was that if we can identify  Red **CIRCULAR** objects in the image we might stand a good chance at predicting the right kind of bounding boxes for detecting speed signs. Histogram of oriented gradients is known for it's ablity to detect shapes in an image .

##A Short Detour of HOG Descriptors 

Just as its name described, HoG feature compute the gradients of all pixels of an image patch/block. It computes both the gradient’s magnitude and orientation, that’s why it’s called “oriented”, then it computes the histogram of the oriented gradients by separating them to 9 ranges. 

They calculate the gradients in a discrete fashion in both the x and y direction in an image. HOG feature descriptors and their extensions remain one of the few options for object detection and localization that can remotely compete with the recent successes of deep neural networks .

HOG descriptors capture such outline information, and are simpler, less powerful, and faster (~20x) alternatives to neural networks. In addition, HOG features can be extracted via the CPUs of a laptop or computing cluster, and need not rely on high performance graphical processing units (GPU) that may not be available to all users. All the more reason to use them for this contest.

![HOG VISUALIZE](speed/hog2.png){:height="300px" width="450px" .center-image}

*Hog Visualization for a speed sign*

So we trained a couple features here , HoG features were of more importance than the *Haar Cascades* we also used to map the circular region.

A big advantage with the haar cascades and it's training in python is that it predicts the bounding boxes for you so don't have to implement the Intersection over Union and Image Pyramids with Sliding Windows to find the best bounding box , which is tedious , and led to significant wastage of time during contest (although it was good to implement this from scratch for learning)

So ultimately we used these features in a layered fashion with an SVM , at each layer (HoG  + Haar) combined with an SVM and a different gave a few bounding boxes as results which were subsequently eliminated in the layers ahead. We also replaced SVM Classifier with Adaboost to get better results . The Results are shown below :

![sample 1](speed/rs1.jpg)

![sample 2](speed/rs2.jpg)

![sample 3](speed/rs3.jpg)

![sample 4](speed/rs4.jpg)


