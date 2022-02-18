
#### how to download

```shell
mkdir -p /usr/local/lib/docker/cli-plugins

wget -c https://github.com/liuxuc63/download/raw/main/docker-compose-linux-x86_64  -o /usr/local/lib/docker/cli-plugins/docker-compose

chmod +x /usr/local/lib/docker/cli-plugins/docker-compose

ln -s /usr/local/lib/docker/cli-plugins/docker-compose /usr/local/bin/xcompose
```
