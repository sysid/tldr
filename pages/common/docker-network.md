# docker network

> Create and manage docker networks.
> More information: <https://docs.docker.com/engine/reference/commandline/network/>.

- List all available and configured networks on docker daemon:

`docker network ls`

- Create a user-defined network:

`docker network create --driver {{driver_name}} {{network_name}}`

- Display detailed information of a space-separated list of networks:

`docker network inspect {{network_name}}`

- Connect a container to a network using a name or ID:

`docker network connect {{network_name}} {{container_name|ID}}`

- Disconnect a container from a network:

`docker network disconnect {{network_name}} {{container_name|ID}}`

- Remove all unused (not referenced by any container) networks:

`docker network prune`

- Remove a space-separated list of unused networks:

`docker network rm {{network_name}}`



# Operations .......................................................................................
```bash
docker network ls
docker network create my-network
docker network inspect bridge

# test DB connection
>>> import psycopg2
>>> conn = psycopg2.connect(host="postgres", port = 5432, database="hi", user="postgres", password="")
>>> conn.info.user
'postgres'
```
## Dockerfile
```bash
# connect to external network
networks:
  default:
    external:
      name: my-pre-existing-network
```

# Concepts .........................................................................................
- The `host` network adds a container on the host’s network stack.
- There is no isolation between the host machine and the container.
  For instance, if you run a container that runs a web server on port 80 using host networking, the web server is available on port 80 of the host machine.
- default is: `bridge` same-host -> exposing and publishing of ports
    The `bridge` network represents the `docker0` network present in all Docker installations.
    Unless you specify otherwise with the `docker run --network=<NETWORK>` option, the Docker daemon connects containers to this network by default.
- CAVEAT: containers on the DEFAULT `bridge` network can only access each other by IP addresses
    no support of automatic service discovery on the default `bridge` network.
    On a USER-DEFINED `bridge` network, containers can resolve each other by name or alias
- `overlay`: cross-host network in swarm mode
- The `none/host` networks are not directly configurable in Docker. However, you can configure the default `bridge` network, as well as your own user-defined `bridge` networks.

## User defined networks
- control which containers can communicate with each other
- connect a container to zero or more of these networks at any given time
- connect and disconnect running containers from networks without restarting the container
- When a container is connected to multiple networks, its external connectivity is provided via the first non-internal network, in lexical order.

## DNS
- Docker daemon runs an embedded DNS server which provides DNS resolution among containers connected to the same user-defined network.
- embedded DNS provides service discovery for any container created with a valid name or net-alias.
- If the embedded DNS server is unable to resolve the request, it will be forwarded to any external DNS servers configured for the container.
- when the container is created, only the embedded DNS server reachable at 127.0.0.11 will be listed in the container’s resolv.conf file.
- do not use the files such as /etc/hosts, /etc/resolv.conf, [instead](https://docs.docker.com/engine/userguide/networking/configure-dns/)
```bash
# provide a DNS server to the container
docker run --dns 192.10.0.2 busybox nslookup google.com
```
