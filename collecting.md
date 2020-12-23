# Collecting data

## Overview

We assessed several ways of collecting data from relevant websites.

### BMUSE

[BMUSE](https://github.com/HW-SWeL/BMUSE) is a useful ready-to-use tool for collecting structured data.
However it has some limitations.
- It downloads the listed pages so to download an entire website you need a list of all site pages. Often it can be obtained by inspecting [sitemap.xml file](https://developers.google.com/search/docs/advanced/sitemaps/overview) which is not always available however.
- It downloads pages and parses data at the same time. However this may not be the best solution because downloading html pages takes quite a long time. At the same time sometimes it's necessary to change the format of how data is parsed. It's useful to be able to separate the website download and the extraction of structured data from it.

### Custom data extractor

Due to limitations of BMUSE, we decided that for this project it's better to implement a custom data extractor.
We implemented data extractor (https://github.com/elizusha/scraper) integrated with [Google Cloud Storage](https://cloud.google.com/storage)(GCS) and collected data from 8+ websites parsing and converting to a single format along the way.

The data extractor consists of two parts: scraper and parser.

## Scraper

Finds all site pages recursively following links and saves htmls in GCS bucket.

Used libraries:

* [requests](https://requests.readthedocs.io/en/master/api/#requests) - to get headers of the page
* [selenium](https://selenium-python.readthedocs.io) - to load pages and get html content, with js scripts
* [bs4](https://www.crummy.com/software/BeautifulSoup/bs4/doc) - to extract links from the html
* [urllib.robotparser](https://docs.python.org/3/library/urllib.robotparser.html) - to check robots.txt requirements

Result: directory in GCS with html pages and mapping from urls to html filenames.


### Observations

#### Scraping large websites

Some website may be pretty large, dozens of thousands of pages. Scraping such websites using a single process takes a lot of time, several days sometimes. As part of this project we didn't spend too much time to download large websites completely, tools should be improved in order to address this problem.

#### robots.txt

robots.txt is a common way for website owners to prevent bots from scraping their websites. When you download website pages you should check robots.txt, because many websites are not actually available for downloading.

## Parser

Takes saved html pages from GCS, extracts json-ld data, parses into a named graph and saves it back to GCS in nq format files.

Used libraries:

* [extruct](https://github.com/scrapinghub/extruct) - extract json-ld from html
* [rdflib](https://rdflib.readthedocs.io/en/stable/index.html) - to parse json-ld into named graph and serialize in nquards

Result: directory in GCS with nq files.

### Observations

#### rdflib parsing issue

At the time of parser writing, rdflib wasn't work properly due to changes in the schema.org and it was fixed locally.
http/https schema error: fix - https://github.com/RDFLib/rdflib/pull/1125

#### Graph storage format

There are many data format options. We looked at two of them: ntriples and nquads.
The use of quads allows to specify the site address as the name of the knowledge graph. This allows to identify the source of the data later.

#### Data samples

* disprot.org:

    https://console.cloud.google.com/storage/browser/wikidata-collab-1-graph-public/disprot.org

* mobidb.org:

    https://console.cloud.google.com/storage/browser/wikidata-collab-1-graph-public/mobidb.org
