# Distributing result graph

## Saving graph

When the data is loaded, the result graph is stored inside a Docker container.
To make using the merged data easier we can save a docker container with loaded data into a ready-to-use docker image.

* [Save docker container into image](https://docs.docker.com/engine/reference/commandline/commit/)

Output of this command is docker image name.

After that the docker image can be saved either in public [DockerHub](https://hub.docker.com/) or in [Google Cloud Registry](https://cloud.google.com/container-registry).

To use [Google Cloud Registry](https://cloud.google.com/container-registry) follow this [tutorial](https://cloud.google.com/container-registry/docs/quickstart).

## Using saved graph

After saving the graph, the person who wants to get a result graph with all the data will just have to run a single command:

```
docker run -d -p [outside_port]:8080 [docker_image_public_name]
```

e.g.

```
docker run -d -p 8889:8080 gcr.io/wikidata-collab-1/full_graph
```

After that the result graph is running inside a Docker container as before saving and the web UI can be used to query the data.

## Mounting another web UI

At all previous steps blazegraph server and web ui were used. However sometimes another web UI have to be integrated with the same graph data.

Microservice architecture consisting of three containers allows to deploy blazegraph and generic graph UI ([Yasgui](https://triply.cc/docs/yasgui-api)) on a single machine, locally or in the cloud.

* [Tutorial](https://docs.google.com/document/d/1fsF2bX8Mn2o1EkbGxAW--1ojwZC-RYnZ_65Rv_pZi2o/edit)
    //TODO: write tutorial here
