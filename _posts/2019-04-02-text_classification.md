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

Over the past 20 years and especially since the United States presidential election 2016 there is an increase in unreliable news content. The question which got and still get asked is how can we know which is which. To find an answer I started at [opensources](http://www.opensources.co/) which curates a list of online information sources. The websites listed range from credible news to misleading and outright fake.

This will be one of 4 blog posts:
- collecting news content
- preprocessing data
- create keras model
- next possible step(s)

## Collecting News Content

The source list (csv file) which we can find at [opensources](http://www.opensources.co/)  hold over 800 url's. The first thing we want to do is to filter out all non-valid url's.

{% gist 163ce020ac062cf0e20d8533a132cb4d %}

The resulting list we got back looks off. When we are looking at the categories and their number count we need to do some cleaning:

{:refdef: style="text-align: center;"}
![Count Category]({{ site.baseimg }}/images/CategoryCountList.png)
{: refdef}

We have some oddly named categories and a meagre number of reliable url's in the list so that we will add a few of our own.

### Web Scraping

There are several tools to get the content we want from a website. [Scrapy](https://scrapy.org/) and [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) are the two most popular tools probably out there. The only problem is that every website is different. To determine what the best options there are I decided to look at the NYTimes. As we can see in the image below neither of the mentioned tools won't do it, each paragraph is nested in its separate HTML tag. Now imagine we have to figure out how each website is build of the around 500 url's we have after cleaning and adding our own. There must be more natural ways.

{:refdef: style="text-align: center;"}
![Count Category]({{ site.baseimg }}/images/nytimes_html.png)
{: refdef}

Luckily, the NYTimes offers an API to excess not only their archive but also search for the most current articles. Even better, their developer site has a playground to toy around to see what kind of results you can get.

{% gist 3dac0843b584b388d42a881209089876 %}

The NYTimes search API gives us a wide range of options to query for data, starting from a date range, fields like `day_of_week`, `headline`, `subject` etcetera, as well as `news desk` values like blogs, cars, fashion and many more. Unfortunately, we only get the lead paragraph from our search which doesn't help much if we are looking for the full article. All we get is metadata, but that's it.

## webhose.io

There are a few websites which offer web scraping tools, for example [import.io](https://www.import.io/), [webhose](https://www.webhose.io/) and [dexi.io](https://dexi.io/). We can find a complete list [here](https://www.hongkiat.com/blog/web-scraping-tools/). None are free, but some offer a free trial period. Because of that, I ended up using webhose.io.

Like the NYTimes, webhose.io also has a playground to toy around with the main difference that you can see your search request in different coding languages like ruby or python. However, be mindful, you only have 1000 requests free. On the negative side, we can't excess all url's we have on our list. However, it gives us at least some data.

Also, webhose.io has archived data you can search.

## newsapi

I also found the python API [newsapi](https://newsapi.org/). The API is very similar to the NYTimes API in the way you write your code to collect the content you are looking for. So like NYTimes we mainly get metadata. However, if you are willing to pay, you can get full article content.

{% gist 07ef8221be45378e931f42f923ad5b17 %}

## Conclusion

If we are serious about getting proper up-to-date news content, newsapi is your API of choice. However, for the task ahead of us I will use the data I collected via webhose.io and may come back at a later time to update the project when possible.

Also, there is an already cleaned and fairly large corpus [here](https://github.com/several27/FakeNewsCorpus) we can use.
