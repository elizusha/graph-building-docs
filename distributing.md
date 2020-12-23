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

At all previous steps Blazegraph server and web UI were used. However sometimes another web UI have to be integrated with the same graph data.

Microservice architecture consisting of three containers allows to deploy Blazegraph and generic graph UI ([Yasgui](https://triply.cc/docs/yasgui-api)) on a single machine, locally or in the cloud.

### Using graph admin

To know how to use [graph-admin](https://github.com/elizusha/graph-loader) microservice functionality navigate to [Mounting another web UI](https://github.com/elizusha/graph-loader#mounting-another-web-ui) section.

### Manualy

1. Run following command with external endpoint that will be used to open Yasgui in browser

    ```
    EXTERNAL_ENDPOINT=127.0.0.1:8888
    ```

1. Run Blazegraph docker:

    ```
    BLAZEGRAPH_CONTAINER_ID=`docker run -d lyrasis/blazegraph:2.1.5`
    ```

1. Print Blazegraph container ip:

    ```
    docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $BLAZEGRAPH_CONTAINER_ID
    ```

1. Run Yasgui docker:

    ```
    YASGUI_CONTAINER_ID=`docker run -d --env DEFAULT_SPARQL_ENDPOINT=http://${EXTERNAL_ENDPOINT}/blazegraph/bigdata/sparql erikap/yasgui`
    ```
1. Print Yasgui container ip:

    ```
    docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $YASGUI_CONTAINER_ID
    ```
1. Create local file default.conf and replace blazegraph_ip and yasgui_ip with ip addresses from previous step:

    ```
    server {
        listen       80;
        listen  [::]:80;
        server_name  localhost;

        location /blazegraph/ {
            proxy_pass http://blazegraph_ip:8080/;
        }

        location / {
            proxy_pass http://yasgui_ip/;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }
    }
    ```

1. Run nginx (change port 8888 to match EXTERNAL_ENDPOINT port)

    ```
    docker run -v $(pwd)/default.conf:/etc/nginx/conf.d/default.conf:ro -p 8888:80 -d nginx
    ```

1. Open EXTERNAL_ENDPOINT in browser
