# Docker compose section








## Multiple composer files - single network

Example: 
 * A running Zabbix monitoring stack with an existing compose file
 * Add a new application from a new compose and connect to the Zabbix API from the existing network

Let's assume that you have your monitoring stack's `docker-compose.yml` in the `monitoring-web-stack` directory:

```
version: '2'

services:
  zabbix-frontend:
    restart: always
    image: zabbix/zabbix-web-nginx-mysql
    environment:
     - DB_SERVER_HOST=someserver
  [...]
```

When you bring up the compose file, docker will create a dedicated network named monitoring-web-stack_default:

```
# docker network ls | grep monitoring-web-stack
f02aa860337d        monitoring-web-stack_default   bridge              local
```

In your application compose file you need to specify this existing network as `external` and docker will bind containers to it:

```
version: '2'

services:
  some-application:
    restart: always
    image: yourapplication:latest

# Attach to existing network
networks:
  default:
    external:
      name: monitoring-web-stack_default
```

Start your new compose and test the network connection from within the application container (`a5d95c2cc9a2` in the example), using the same service names you defined in the monitoring stack compose:

```
# docker exec -it a5d95c2cc9a2 sh

sh-4.2# ping zabbix-frontend
PING zabbix-frontend (172.18.0.5) 56(84) bytes of data.
64 bytes from monitoring-web-stack_zabbix-frontend_1_df782a3a3528.monitoring-web-stack_default (172.18.0.5): icmp_seq=1 ttl=64 time=0.100 ms
^C
--- zabbix-frontend ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.100/0.100/0.100/0.000 ms

sh-4.2# curl http://zabbix-frontend | head
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3515    0  3515    0     0  45572      0 --:--:-- --:--:-- --:--:-- 45649
<!DOCTYPE html>
<html>
        <head>
                <meta http-equiv="X-UA-Compatible" content="IE=Edge"/>
                <meta charset="utf-8" />
                <meta name="viewport" content="width=device-width, initial-scale=1">
                <meta name="Author" content="Zabbix SIA" />
                <title>Zabbix docker: Zabbix</title>
                <link rel="icon" href="favicon.ico">
                <link rel="apple-touch-icon-precomposed" sizes="76x76" href="img/apple-touc
```			
				
