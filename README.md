# Graph building documentation

## Project description

There are many sources of schema.org structured data on the internet. Some sources allow efficient querying the data, for example wikidata. However there is no widely available mechanism to make queries to multiple data sources at the same time.

The project goal is to create a prototype of a system that allows executing queries against the data from multiple data sources simultaneously using Google Cloud services. In that particular case, all [data sources](https://bioschemas.org/liveDeploys/) will be from the field of biology.

The project makes it possible to assess the usefulness and usability of such a system to potentially deploy it on a larger scale in the future. Additionally, this project allows to demonstrate how Google Cloud services can be useful for building such a system.

## Content

This repository contains documentation about the effort of collecting structured data from public sources, joining into a shared graph and distributing in an easy-to-use format.

There are several documentation pieces one for each step:

* [Collecting data](collecting.md)
* [Merging data](merging.md)
* [Distributing result graph](distributing.md)
* [Bio data observations](biodata_observations.md)
