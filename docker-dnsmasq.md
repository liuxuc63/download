

```yaml
version: '3.2'

services:
  dnsmasq:
    image: liuxuc63/dnsmasq:latest
    restart: always
    container_name: dnsmasq
    hostname: dnsmasq
    network_mode: "host"
    environment:
      - "TZ=Asia/Shanghai"
    volumes:
      - /opt/dnsmasq/_data/dnsmasq.conf:/etc/dnsmasq.conf
      - /opt/dnsmasq/_data/resolv.dnsmasq.conf:/etc/resolv.dnsmasq.conf
      - /opt/dnsmasq/_data/healthcheck:/healthcheck
    cap_add:
      - NET_ADMIN
    deploy:
      resources:
        limits:
          cpus: '1'
          memory: 32M
    healthcheck:
      test: ["CMD", "sh", "/healthcheck"]
      interval: 30s
      retries: 1

# need docker-compose v2: https://github.com/docker/compose
# docker-compose -p ccdns -f compose.yaml up -d
# docker-compose -p ccdns down
```

```shell
# /opt/dnsmasq/_data/healthcheck
set -e

if [ -z "$(netstat -nltu |grep \:53)" ]; then
  echo "no process listening on port 53!"
  kill 1 && exit 1
fi

echo "health"
```
