# Merging data

## Knowledge Graph

Nquads from the previous step can be loaded into a single graph, so it’s possible to make queries to the scraped data.

In this project we used [Blazegraph](https://blazegraph.com/) as a graph engine, running inside of a [Docker container](https://hub.docker.com/r/lyrasis/blazegraph).

## Graph loader

We implemented a tool (https://github.com/elizusha/graph-loader) that allows to build a Blazegraph instance with data from multiple sources.

### Observations

#### Multiple schema versions

There can be multiple versions of the same structured data. For example, `schema.org` can be referenced either with legacy "http://" prefix or with modern "https://" prefix. Another example is `bioschemas.org` which mostly became a part of `schema.org` already but still can be referenced as `bioschemas.org` in the data. This should be kept in mind when querying the merged data.

#### Licensing

Note, that the data has its own license and you might not always be able to use it, please always check how the data you plan to use has the license permissive enough for your goals.

#### Loading data method

SPARQL supports multiple ways of loading the data. It's possible to reference a local file to load in the query:

```
LOAD <file:///path-to-file/file.nq>
```

However, the local file should be visible to the server, which means in case of Docker that this file should be copied or mounted to the container. It might not always be possible or easy to do.

Alternatively, it's possible to load the data by directly specifying it in the query:

```
INSERT DATA { GRAPH <graph-name> { ntriples } }
```

However, there's a limit on the size of a single request to Blazegraph, so a large data portion should be partitioned in smaller chunks, which results in slower loading.
