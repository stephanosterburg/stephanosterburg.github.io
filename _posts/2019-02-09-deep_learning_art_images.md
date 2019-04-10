---
layout: post
title: Deep Learning and Art Images
description: "An interpretation of the paper"
author: stephan
categories: [data, code, art, deep learning, beginner]
image: assets/images/GoogleArt_cropped.png
featured: false
---

At [kaggle](https://www.kaggle.com) we can find a dataset containing a collection of art images of google images, yandex images and from [The Virtual Russian Museum](http://rusmuseumvrm.ru/collections/?lang=en). The dataset has about 9000 images with five categories:
1. Drawings and Watercolors
2. Paintings
3. Sculpture
4. Graphic Art
5. Iconography (old Russian art)

Because it is a classification problem, I wanted to use my newly learned knowledge in deep learning and convolutional neural networks (CNN). However, first things first, the downloaded zip file has two more zip files, which it turns out are somewhat similar in context but not quiet. So I decided to combine the two into one dataset.

## Approach

When I started to research how to tackle the issue for image classification, I found three possible options:
* `train_test_split`, we have to create the train and test data ourselves.
* `flow_from_dataframe`, we need to create first the dataframe ourselves.
* `flow_from_dictonary`, here we don't need to do any extra work.

I, as you may guessed it already, opted for the later.

## How deep is too deep?

To answer that question, here we need to know one fact first. I am working on a MacBook Pro (2015) with a 2.2 GHz Intel Core i7 processor and 16GB of RAM.

So, how deep is too deep? How many layers can I run on my computer without feeling the pain? To find out I approached it very gingerly, step by step. Or shall I say layer by layer?

For the first try, I had only a few layers, which helped with the computation time. However, the result showed a test accuracy of less than 50%.

```python
model = models.Sequential([
	Conv2D(32, (3, 3), activation='relu', input_shape=(150, 150, 3)),
	MaxPooling2D((2, 2)),

	Conv2D(64, (3, 3), activation='relu'),
	MaxPooling2D((2, 2)),

	Flatten(),
	Dense(64, activation='relu'),
	Dense(128, activation='relu'),
	Dense(5, activation='sigmoid')
])
```

To improve the test accuracy, I kept adding layers. I ended up adding several more layers, including `Dropout` layers to help to avoid over-fitting.


```python
model = Sequential([
    Conv2D(32, (3, 3), input_shape=input_shape, activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    Dropout(0.2),

    Conv2D(32, (3, 3), activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    Dropout(0.2),

    Conv2D(64, (3, 3), activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    Dropout(0.2),

    Flatten(),
    Dense(128, activation='relu'),
    Dropout(0.2),
    Dense(64, activation='relu'),
    Dropout(0.2),
    Dense(5, activation='softmax')
])
```

With the simple sequential model the computational time wasn't too bad but with the  model, seen above, the time went up from several minutes per epochs to nearly 30 minutes per epochs.

### epochs/batch_size

An `epochs` is an iteration over the entire provided data; for example, if we have `epochs=25` we iterate 25 times over the data. The `batch_size` is the number of samples that will propagate through the network, by default 32. In our case where we have about 8000 images in the training set, we have 250 batches per epochs.

The question is, do we decrease the batch_size to 16 or increase the number to 64? If we decrease the number, we have 500 batches vs 125 batches if we increase the number. Large batch size result in faster processing time and vice versa. In regards to epochs, the model improves with more but only to a point. They start to plateau in accuracy as they converge.

## Pre-Trained Network

To improve not only the accuracy but also the processing time (so I hoped), I decided to use a pre-trained network. There are a few we can choose from, for example [VGG16](https://keras.io/applications/#vgg16), [VGG19](https://keras.io/applications/#vgg19), [InceptionV3](https://keras.io/applications/#inceptionv3), and [ResNet50](https://keras.io/applications/#resnet50) etcetera. I resorted to the [VGG](https://arxiv.org/abs/1409.1556) model to keep it simple for the time being.

**VGG** is a convolutional neural network model for image recognition proposed by the **Visual Geometry Group in the University of Oxford**, where *VGG16* refers to a VGG model with 16 weight layers, and *VGG19* refers to a VGG model with 19 weight layers.

Because we have an already trained model, all we have to do is add at least two more layers (`Flatten` and the output `Dense` layer) to test what the pre-trained network can do with our dataset. In my case I ended up adding six layers in total:

```python
model = Sequential([
    vgg_model,
    Flatten(),
    Dense(32, activation='relu'),
    Dense(64, activation='relu'),
    Dense(128, activation='relu'),
    Dense(64, activation='relu'),
    Dense(5, activation='softmax')
])
```

Now, the accuracy improved to 96.68%, up >5% from my previous model, but not the processing time.

## AWS

In the end, it took several hours to run the model. Moreover, it makes it even more painful if you forget to change your default setting on your MacBook and the computer goes into sleep mode, and nothing get processed at all.

To free up the computer I resorted to - welcome - [paperspace](https://www.paperspace.com).

Which allows me to prototype locally (prove of concept) and compute in the cloud.

## Conclusion

Use where you can pre-trained networks and if you are using large data use cloud services.
