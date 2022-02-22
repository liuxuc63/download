# One liner for deleting images from a v2 docker registry

Just plug in your own values for registry and repo/image name.

```bash
registry='localhost:5000'
name='my-image'
curl -v -sSL -X DELETE "http://${registry}/v2/${name}/manifests/$(
    curl -sSL -I \
        -H "Accept: application/vnd.docker.distribution.manifest.v2+json" \
        "http://${registry}/v2/${name}/manifests/$(
            curl -sSL "http://${registry}/v2/${name}/tags/list" | jq -r '.tags[0]'
        )" \
    | awk '$1 == "Docker-Content-Digest:" { print $2 }' \
    | tr -d $'\r' \
)"
```

## If all goes well

```bash
* About to connect() to localhost port 5000 (#0)
*   Trying 127.0.0.1...
* Connected to localhost (127.0.0.1) port 5000 (#0)
> DELETE /v2/my-image/manifests/sha256:14f6ecba1981e49eb4552d1a29881bc315d5160c6547fdd100948a9e30a90dff HTTP/1.1
> User-Agent: curl/7.29.0
> Host: localhost:5000
> Accept: */*
>
< HTTP/1.1 202 Accepted
< Docker-Distribution-Api-Version: registry/2.0
< X-Content-Type-Options: nosniff
< Date: Wed, 15 Nov 2017 23:25:30 GMT
< Content-Length: 0
< Content-Type: text/plain; charset=utf-8
<
* Connection #0 to host localhost left intact
```

## Garbage cleanup

Finally, invoke garbage cleanup on the docker-registry container.

For example:

```bash
docker exec -it docker-registry bin/registry garbage-collect /etc/docker/registry/config.yml
```
