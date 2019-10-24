---
layout: post
title: 'Vectoring Words (Word Embeddings)'
description: "Vectoring Words (Word Embeddings)"
author: Stephan
categories: [math, code, python]
tags: [math, code, python]
featured_image_thumbnail: assets/images/posts/2019/hand-drawn-positive-words-vector.jpg
featured_image: assets/images/posts/2019/hand-drawn-positive-words-vector.jpg
featured: false
hidden: false
---


How do you represent a word to a neural network? In a neural network, we are viewing things as a vector of real numbers or vector floats. If we are thinking about something like an image, we can take all the pixels and represent them as numbers and put them in a long row. If we have a black pixel, then the number is 0. If we have white, then it's 1, and everything in between is just grayscale. In the end, we will have a vector that represents that image.

<div style="text-align:center">
{% include image-caption.html imageurl="/assets/images/posts/2019/image-represented-in-matrix.png" title="Pixels" %}
</div>
<br>

If we then take the same image and make it a little bit brighter or dimmer, for example, that vector we have gets then longer or shorter, respectively. If we take an image and we apply a small amount of noise to it, that is similar to jiggling that vector around slightly in that configuration space. So you've got a sense in which two vectors that are close to each other are similar images. The sort of directions in the vector space is then actually meaningful in terms of something that would make sense for images. And the same is true with numbers and whatever else. And this is very useful when you're training because it allows you to say if your neural network is trying to predict a number and that the value you're looking for is ten and it gives you nine, you can say no, but that's close. And if it gave you seven thousand, you can be like, no, and it's not close. And that provides more information that allows the system to learn. And in the same way, you can say, yeah, that's almost the image that I want. Whereas if you give the thing a dictionary of words, say you've got your ten thousand words.

And the usual way of representing this is with a one-hot vector. If you have 10000 words, you have a vector that's ten thousand long (10000 dimensions), and all of the values are zero, apart from one of them, which is one. For example, if we take the first word in the dictionary "a," then that's represented by a one, and then the rest of the 10000 are zeros. And then the second way is like a zero and then a 1 and 0 and so on. But there you're not giving any of those clues. If the thing is looking for one word and it gets a different word, well, you can say is yeah, that's the correct one. Or no, that's not the correct one. Something that you might try, but you shouldn't because it's a stupid idea is rather than giving it as a one-hot vector. You could provide us with a number, but then you've got this indication that like two words that are next to each other in the dictionary are similar. And that's not really true. If you have a language model, and you're trying to predict the next word, and it's saying: "I love playing with my pet ...." The word you're looking for is "cat," and it gives you instead "car," lexical, graphically, they're pretty similar, but you don't want to be saying to a network you are close. Or that was very nearly right, which would be a nonsense prediction. But then if it said like "dog." You should be able to say no, but that's close because that is a plausible completion for that sentence. And the reason that that makes sense is that "cat" and "dog" are similar words.

##### What does it mean for a word to be similar to another word?

The assumption is that word embeddings use is that two words are similar if they are often used in a similar context. If we look at all of the instances of the word "cat" or "dog" in a giant database, then they're going to be surrounded by similar words, like pets, feed or play, and so on. The challenge that word embedding is trying to come up with is how do you represent words as vectors such that two similar vectors are two related words. And possibly so that directions have some meaning as well because then that should allow our networks to be able to understand better what we're talking about in a given text.

If we have a language model that can get an excellent performance of predicting the next word in a sentence. And the architecture of that model is such that it doesn't have that many neurons in its hidden layers. Then it has to be compressing that information down - efficiently.

Let's say for the sake of simplicity; your language model is just taking in a word and trying to guess the next word. We only have to deal with having one word in our input. But our input is this very tall input layer, ten thousand words. And these then feed into a hidden layer of let's say 300 nodes or even smaller. And then coming back out on the other end, you're back out to 10000 again. What comes out the other end is a probability distribution, where we can look at the highest value on the output. And that's what it thinks the next word will be. The point is, we are going from ten thousand three hundred and back out to 10000.

<div style="text-align:center">
{% include image-caption.html imageurl="/assets/images/posts/2019/neural_network_example.png" title="Neural Network" %}
</div>
<br>

This inner layer of 300 nodes has to be doing its task well to encode, compress information about the word passing through the hidden layer.

The idea here is that in the end, when this neural network is fully trained, we can give it any word. After which it's going to provide us with a probability distribution over all of the words in your dictionary. This distribution tells us how likely are each of these words will show up within five words of this first word or ten. If the system can get good at this task, then the weights of this hidden layer in the middle have to encode something meaningful about that input word.

If we imagine the word "cat" comes in. To do well, the probability distribution of surrounding words is going to end up looking rather similar to the output that you would want for the word "dog." It's gonna have to put those two words close together if it wants to do well at this task. And that's all we do.

If we run it on a large enough data set and give it enough compute actually to perform really well, it ends up giving us for each word a vector that's of however many units we have given it. For which the similarity of those vectors expresses something meaningful about how similar the contexts are that those words appear in. And we assume that words that appear in similar contexts are similar.

In the process of doing that, it creates this mapping from the latent space to images. By doing basic arithmetic like just adding and subtracting vectors on the latent space would produce meaningful changes in the image. So what we end up with is that same principle, but for words. So if we take, for example, the vector and it's required by law that all explanations that word embeddings use the same example to start with. If you take the vector for "King," subtractive the vector for "man" and add the vector for "woman," we get another vector out, and if you find the nearest point in your word embeddings to that vector, it's the word "Queen."

Here is a very simple example using google's own [GoogleNews-vectors-negative300](https://drive.google.com/file/d/0B7XkCwpI5KDYNlNUTTlSS21pQmM/edit) model:

```python
import gensim

from google.colab import drive
drive.mount('/content/drive')

!gunzip -f "/content/drive/My Drive/models/GoogleNews-vectors-negative300.bin.gz"

model = gensim.models.KeyedVectors.load_word2vec_format("/content/drive/My Drive/models/GoogleNews-vectors-negative300.bin.gz")

print(model.most_similar_cosmul(positive=['boy', 'woman'], negative=['man']))
# [('girl', 1.01875), ('mother', 0.91107,, ('teenage_girl', .....))]
```
