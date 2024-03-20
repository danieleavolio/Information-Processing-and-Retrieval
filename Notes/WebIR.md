# Web IR

`Well behaved search`: A search engine that returns relevant results for a query. I think.

Which sites have the same structure? Something like `news`.

`Link dynamics`: Links inside a web that redirects to other web pages in other sites.

## Page Rank

### What is Page Rank

It's an algorithm that estimates the importance of a page.

Imagine having 4 links:

- news
- cooking
- sport
- sport

If you like sport, you will have more probability to get to one of the sport pages. Moreover, from the sport page you have more probabiltiy to get to the other sport page, and not to the cooking or news page.

`Reddit`: Is a good example because Reddit has a lot of links to other pages. We will call it `HUB`, because it's the site you go to find the link to other pages.

## Crawling

`Question`: How to tell if a search engine is good?

`Answer`: If they have a own crawler.

`Crawler`: An agent that uses an algorithm to find websites and decides which one to visit.

#### Google VS Microsoft

Why does Google and Microsoft want you to use their own search engine?

`Data`: They want to collect data for the links you click.
It's good for `link dynamics`.
Google has a lot more data than Microsoft, because they have more users who use their search engine and products.

This is called *User Behavior on the `SERP`* (Search Engine Result Page).

### DuckDuckGo

`DuckDuckGo`: A search engine that doesn't track you.

But does it have a crawler?

`Answer`: Yes, it uses the Bing crawler. So you will get the same results as Bing.

### Some other search engines

- [You](https://you.com): It's a search engine made by one of the best NLP researchers.
- [Kagi](https://kagi.com): A pay to use search engine. They justify it by saying that they will give the best results.
- [Mojeek](https://www.mojeek.com): A search engine that doesn't track you.
- [Mwmble](https://mwmble.com): A search engine that is curated by humans.

## Alrernative to Page Rank
I didn't hear anything about it.


## What are metacrawlers?

`Metacrawler`: A search engine that uses other search engines to get the results.

# Anchor Text

`Anchor Text`: The text that is used to link to another page.

An example:
> - This is a [link](https://www.google.com) to Google.
> - This is a [link](https://www.bing.com) to Bing.
> - Sometimes the [link bing](https://www.bing.com) is not the same as the anchor text.


# Web Document Collection

We are talking about web pages as documents. The problem is that the documents don't share the same structure. It's possible to have very big documents and very small documents. Even more, nowadays content can be `dynamic generated`. Just think about LLMs.

# Crawling

Imagine we want to crawl pages. We need to start somewhere. We start from `seed pages`. A good starting point is `Wikipedia`. Starting from Wikipedia we can find a lot of links to other pages.

`URLs frontier`: Is like a queue of URLs that we want to visit and we haven't visited yet.

If you are building a crawler you can do something like this:
```
HEAD /index.html 
GET /index.html
```

`Simple operations`:
- extract urls
- place each url in a queue
- featch url in queue

`Problems`:
- Web pages are dynamic
- Traps (infinite loops)
- Computational cost

## What a crawler must and should do

Must do:
- Politenes: respect and explicit rules
  - crawl allowed pages
  - respect robots.txt
- Robustness: Spider traps <- avoid

Shoul do:
- Freshness: update the index
- Quality: avoid spam
- Efficiency: minimize the number of requests
- Coverage: find all the pages

Another way to tell a crawler to not crawl the page is insertin in anchor links the `rel="nofollow"` attribute.

### Basic architecture

![Crawler architecture](https://www.researchgate.net/publication/284167864/figure/fig1/AS:895444032950272@1590501902546/Basic-web-crawler-Architecture-3.png)

`Question`: Which one is the most problematic part of the architecture?

`Answer`: The `DNS` part. If you don't get the IP of the sites you can't do anything.l

`Question`: How do I know if I already visited some page?

`Answer`: You can use a `hash` to store the visited pages and check the bytes of the page.

## Document Similarity

We have to take text and compare them to see if they are similar.

- `n-grams`: A sequence of `n` words. It's a way to represent a document.
This is not so much used because it's a little bit too much computational expensive.

- `sketches`: A way to represent a document in a vector.

## Distributed Crawling

We will use more than one machine to crawl the web. In our case, more threads.

We can use some `hash` to split the partition to crawl.

`Mercator Scheme`: The Mercator scheme organizes web crawling by dividing the web into regions, assigning each to a specific crawler. This allows for parallel crawling, improving efficiency and scalability. Coordination among crawlers ensures comprehensive coverage and prevents duplication.

### Front queue

Prioritize URL assigning integers between `1` and `k`. There are some heuristics for assigning it.

### Back queue

It takes the URL from a `front queue`.
It's kept non empty while the crawl is in progress and only `contains URLs from a single HOST`. 
`Why this`? Because there is `no more cost for DNS`, since we don't need to resolve the IP again. It's based on the TCP connection and handshake stuff.

