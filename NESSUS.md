Simple Nessus implementation

Note: Nesus does not support (yet) persistent volumes, I use this as a one-shotter with data export

Compose implementation: 

```
# cat docker-compose.yml

version: '3.1'

services:
  nessus:
    image: tenableofficial/nessus
    restart: always
    container_name: nessus
	
    env_file:
      - .env

    environment:
      USERNAME: ${NESSUS_ADMIN_USERNAME}
      PASSWORD: ${NESSUS_ADMIN_PASSWORD}
      ACTIVATION_CODE: ${NESSUS_ACTIVATION_CODE}

    ports:
      - 8834:8834
```


Env file for password decouple from compose:

```
# cat .env

NESSUS_ADMIN_USERNAME=some_admin
NESSUS_ADMIN_PASSWORD=some_password
NESSUS_ACTIVATION_CODE=XXXX-XXXX-XXXX-XXXX
```