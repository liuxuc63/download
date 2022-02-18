# download



```shell

# dnsmasq healthcheck

if [ -z "$(netstat -nltu |grep \:53)" ]; then
  echo "no process listening on port 53!"
  exit 1
fi

```
