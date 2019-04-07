---
layout: post
title: Text Classification (Part 2)
description: "Fake News"
tags: [data, code, content]
image:
  path: /images/Newsstand.jpg
  feature: Newsstand.jpg
---

## Exploratory Data Analysis

After collecting our data, the first thing we want to do is to get an understanding of it. Exploratory Data Analysis or short EDA is one of the essential steps in the data analysis process. By trying to make sense of the data, we can start formulating questions and if necessary go back and make changes to where our data came from, and others.

Remember, the number of source URLs is not very well balanced if we are looking at the categories. We started with 134 URLs categorised as `fake` and from our source dataset with only 3 reliable URLs, which we increased to over 70 URLs with our own.

{:refdef: style="text-align: center;"}
![Content by URL Count]({{ site.baseimg }}/images/content_by_url_count.png)
{: refdef}

However, I went ahead and collected as much data as possible and as it turns out correctly so. As we can see in the image below the distribution turned up-side-down. I wasn't expecting that at all.

{:refdef: style="text-align: center;"}
![Content by Article Count]({{ site.baseimg }}/images/content_by_article_count.png)
{: refdef}

So we don't need the extra content we got from the archived data at [webhose.io](https://webhose.io/).

## Sentiment

In natural language analysis, one of the first steps one wants to do is understand the sentiment of a given text. Our data is to 75% neutral, which I guess is a good thing. The interesting part here is the thing that for the rest of our data the negative content overweights the positive content drastically.

{:refdef: style="text-align: center;"}
![Positive-Neutral-Negative Content Distribution]({{ site.baseimg }}/images/Positive-Neutral-Negative_Content_Distribution.png)
{: refdef}

Moreover, we can see that also in the distribution about the number of words in an article and its sentiment weighting. The shorter an article is, the more likely it is that it is negative content.

{:refdef: style="text-align: center;"}
![Positive/Negative Sentiment]({{ site.baseimg }}/images/posneg_sentiment.png)
{: refdef}

## Visualizing differences based only on term frequencies

Occasionally, only term frequency statistics are available. In the case where this may happen like in very large, lost, or proprietary data sets. To help us here we can use the tool [`scattertext`](https://github.com/JasonKessler/scattertext) which is based on [`spaCy`](https://spacy.io/). `TermCategoryFrequencies` function is a corpus representation, that can accept this sort of data, along with any categorised documents that happen to be available.

{% include reliable_vs_unreliable.html %}
.

The critical part to note here is that the data represented in this plot comes from unbalanced data.

## Topic Modeling

Another question we can try to find an answer to is what are the most written about topics in our news content. We can use Gensim/LDA here to find them but there is a more intuitive and interactive way to help visualise topic model using [sklearn](https://scikit-learn.org/stable/index.html) and the python library [pyLDAvis](https://pyldavis.readthedocs.io/en/latest/) which is a port of the fabulous [R package](https://github.com/cpsievert/LDAvis) by [Carson Sievert](https://cpsievert.me/) and [Kenny Shirley](http://www.kennyshirley.com/).

{% gist 15b93ef69472c10df40598c20e42149e %}

pyLDAvis is designed to help users interpret the topics in a topic model that has been fit to a corpus of text data. The package extracts information from a fitted LDA topic model to inform an interactive web-based visualisation.

The pyLDAvis interface:

+ the left panel shows how common and how each topic relates to each other in 2D space.
+ the right panel shows us a list of the most notable word (topics) and its frequency. If we select a topic, we see the topic-specific frequency of each term (red) and the corpus-wide frequency (blue).

<!-- <a href="http://example.com/" target="_blank">Hello, world!</a> -->

{:refdef: style="text-align: center;"}
![LDA visualisation]({{ site.baseimg }}/images/pyLDAvis.png)
{: refdef}

## Linguistic Features

[spaCy](https://spacy.io/) offers excellent tools to process raw text. Most words are rare, and it’s common for words that look entirely different to mean almost the same thing. The same words in a different order can mean something completely different. Even splitting text into useful word-like units can be difficult in many languages. While it’s possible to solve some problems starting from only the raw characters, it’s usually better to use linguistic knowledge to add useful information. That’s exactly what spaCy is designed to do: you put in raw text and get back a Doc object, that comes with a variety of annotations.

To continue working on the EDA and help us analyse our content we shall use spaCy's statistical entity recognition system as well as `spacy-vis`, a visualisation tool using Hierplane.
