---
layout: post
<!-- date : 2017-12-31 -->
excerpt: "Image captioning in Hindi ?"
categories: [projects]
comments: True
permalink: /projects/imgcaption/
---

## Culturally aware Hindi Captioning 

Given an image, humans can easily infer the salient entities in it, and describe the scene effectively, such as, where objects are located (in a forest or in a kitchen?), what attributes an object has (brown or white?), and, importantly, how objects interact with other objects in a scene (running in a field, or being held by a person etc.)

The task of visual description aims to develop visual systems that generate contextual descriptions about objects in images. Visual description is challenging because it requires recognizing not only objects (bear), but other visual elements, such as actions (standing) and attributes (brown), and constructing a fluent sentence describing how objects, actions, and attributes are related in an image (such as the brown bear is standing on a rock in the forest).

Over the years a lot of research work has focused on implementing image description models for the same , the most prominent of the work being done Fei Fei Li and Andrej Karpathy . 

Karpathy et al. extended the multi-modal embeddings model for image description . Rather than directly mapping entire images and sentences into a common embedding space, their model embeds more fine-grained units, i.e., fragments of images (objects) and sentences (dependency tree fragments), into a common space and receive some very good results .


The idea of this project is to explore whether image captioning models work for different cultural context as well or is still an open ended problem when used across datasets of different cultures .

### Model used for Feature Extraction and Description

![Vgg architecture](vgg.png){:height="300px" width="600px"}

- Photo Feature Extractor. This is a 16-layer VGG model pre-trained on the ImageNet dataset.I have pre-processed the photos with the VGG model (without the output layer) and will use the extracted features predicted by this model as input.

- Sequence Processor. This is a word embedding layer for handling the text input, followed by a Long Short-Term Memory (LSTM) recurrent neural network layer . The Sequence Processor has a 256 unit LSTM . 

- Decoder  Both the feature extractor and sequence processor output a fixed-length vector. These are merged together and processed by a Dense layer to make a final prediction.

### Data Set Preparation

- For the purpose of this project  due to lack of datasets available  , a dataset was  prepared from news websites such as aajtak and dainik  bhaskar , the images were mined with help of beautiful soup in Python .

- The Dataset incorporates about 40,000 images over news items , and i assume that the images mined had relevant captions and context most of the times . Although , the context of the image and it’s caption in case of a news item might not always be relevant. ** This lead to a big obstacle in the project **  

### The Problem of Hindi Word Vectors

Any text processing model needs a word embedding matrix trained for efficient representation of it’s word vectors , several such  embedding matrices are available for English Text Corpus , but not for Hindi .  The  additional problem here was the presence of numerous named entities  in the dataset  because it was collected from news websites, which made the presence of unknown words extremely high . The problem was sorted out by training  a custom word embedding matrix from the Wikipedia Hindi dumps , which provided reasonable presence of such named entities . 

### Example of the input feed to the model 
Given the following image sentence pair , the  model was given the following inputs :

- (image, यूक्रेन में संसद से सड़क तक कोहराम )
- (image, <start> यूक्रेन )  
- (image, <start> यूक्रेन में )
- (image, <start> यूक्रेन में संसद )
- (image, <start> यूक्रेन में संसद से )
  and so on , the words were replaced by word  vectors .

At the onset i was training my model on a GTX 750X which isn't a great gpu to be honest , this took close to 30 days to train and still didn't produce good results courtesy of the fact that the dataset was a little vague. At present the institute has 3 GTX 1080's one of which i am utilizing , and i am also manually preparing a culturally annotated dataset for this problem.

Another challenge for this is the fact that traditionally image captioning is as generic as you can make it , but here you have to delve into specifics even more as the captions need to be culturally relevant! 

