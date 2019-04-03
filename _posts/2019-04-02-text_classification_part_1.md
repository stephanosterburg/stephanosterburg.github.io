---
layout: post
title: Text Classification
description: "Fake News"
tags: [data, code, content]
image:
  path: /images/Newsstand.jpg
  feature: Newsstand.jpg
---

## Reliable or Unreliable?

Over the past two decades and especially since the United States presidential election 2016 there is an increase in unreliable news content. The question which got and still get asked is how can we know which is which. To find an answer I started at [opensources](http://www.opensources.co/) which curates a list of online information sources. The websites listed range from credible news to misleading and outright fake.

The following post is part one in a series of four blog posts:
- collecting news content
- preprocessing data
- create keras model
- next possible step(s)

## Collecting News Content

The source list (CSV file) which we can download from [opensources](http://www.opensources.co/)  has over 800 URLs. The first thing we want to do is to filter out all non-valid URLs.

{% gist 163ce020ac062cf0e20d8533a132cb4d %}

The resulting list we got back looks off. When we are looking at the categories and their number count we find that we need to do some cleaning:

{:refdef: style="text-align: center;"}
![Count Category]({{ site.baseimg }}/images/CategoryCountList.png)
{: refdef}

We have some oddly named categories and a meagre number of reliable URLs in the list. To address this imbalance, I added about 70 URLs I consider reliable (hopefully) of my own, which doesn't balance the scale very much but at least pushes it into the right direction. Plus, let us see what and how much content we can scrap from the world wide web before we do something about the source list.

### Web Scraping

There are several tools to get content from a website, [scrapy](https://scrapy.org/) and [beautifulsoup](https://www.crummy.com/software/BeautifulSoup/) are the two most popular tools probably out there. In both cases, we need to inspect the HTML code of a website. If you are interested to learn more about it, [here is an excellent tutorial with tips and tricks using python](https://hackernoon.com/web-scraping-tutorial-with-python-tips-and-tricks-db070e70e071).

In any case, the main problem we are facing here is that every website has a different underlying architecture. To determine what the best options there are I decided to look at [NYTimes](https://www.nytimes.com/). As we can see in the image below, each paragraph is in its separate HTML tag. To collect content only from the NYTimes website won't be impossible, just bloody difficult and neither of the mentioned tools won't do it in the long run. Considering that we have over 500 URLs and for the scope of this project is not visible. There must be much simpler ways to tackle this.

{:refdef: style="text-align: center;"}
![Count Category]({{ site.baseimg }}/images/nytimes_html.png)
{: refdef}

## API

Luckily, the NYTimes offers an API to excess not only their archive but also search for the most current articles. Even better, their developer site has a playground to toy around to see what kind of results you can get and fine tune your query.

{% gist 3dac0843b584b388d42a881209089876 %}

The NYTimes search API gives us a wide range of options to query for data, starting from with date range and fields like `day_of_week`, `headline`, `subject` etcetera, as well as `news desk` values like blogs, cars, fashion and many more.

Unfortunately, we only get the lead paragraph from our search which doesn't help much if we are looking for the full articles. At the end all we get is metadata, but that's it.

## webhose.io

There are a few websites which offer web scraping tools, for example [import.io](https://www.import.io/), [webhose](https://www.webhose.io/) and [dexi.io](https://dexi.io/). We can find an extended list [here](https://www.hongkiat.com/blog/web-scraping-tools/). None are free, but some offer a free trial period. Because of that, I ended up using webhose.io.

Like the NYTimes, webhose.io also has a playground to fine tune our query with the main difference that you can see your search request in different coding languages like ruby or python. However, be mindful, you only have 1000 requests free.

Unfortunately, we can't get excess to all the URLs we have on our list. However, it gives us at least some data. Also, webhose.io has archived data we can search.

## newsapi

The final tool I used is the python API from [newsapi](https://newsapi.org/). The API is very similar to the NYTimes API in the way you write your code to collect the content we want. Like at the NYTimes we mainly get metadata. However, if we are willing to pay, we can get the full article content.

{% gist 07ef8221be45378e931f42f923ad5b17 %}

## Conclusion

If we are serious about getting proper up-to-date news content, I  believe that the API from newsapi is your API of choice. However, for the task ahead of us I ended up using the data I collected via webhose.io and may come back at a later time to update the project.

Also, there is an already polished and fairly large corpus [here](https://github.com/several27/FakeNewsCorpus) we can use.
