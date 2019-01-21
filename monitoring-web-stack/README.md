# monitoring-web-stack

### Stack for Zabbix monitoring web frontends

Fully dockerized web services for monitoring with Zabbix (zabbix frontend, grafana, phpmyadmin, static index)

The web services are published via nginx automatic reverse proxy (thanks to https://github.com/jwilder/nginx-proxy), by setting the corresponding `VIRTUAL_HOST` environmental variable

**Important**: Zabbix Server engine and MySQL database not included in this stack, they are external (see `DB_SERVER_HOST`)


### Environment

Sample environment file `.env` contains the passwords needed by the containers: 

```
MYSQL_PASSWORD=somePassword
PINGER_DB_PASS=anotherPassword
```

