# docker volume

> Manage Docker volumes.
> More information: <https://docs.docker.com/engine/reference/commandline/volume/>.

- Create a volume:

`docker volume create {{volume_name}}`

- Create a volume with a specific label:

`docker volume create --label {{label}} {{volume_name}}`

- Create a `tmpfs` volume a size of 100 MiB and an uid of 1000:

`docker volume create --opt {{type}}={{tmpfs}} --opt {{device}}={{tmpfs}} --opt {{o}}={{size=100m,uid=1000}} {{volume_name}}`

- List all volumes:

`docker volume ls`

- Remove a volume:

`docker volume rm {{volume_name}}`

- Display information about a volume:

`docker volume inspect {{volume_name}}`

- Remove all unused local volumes:

`docker volume prune`

- Display help for a subcommand:

`docker volume {{subcommand}} --help`



# Operations .......................................................................................

```bash
# see content of named volume
docker run --rm -i -v=sim-data:/tmp/myvolume busybox find /tmp/myvolume

# to find and remove dangling volumes:
docker volume ls -f dangling=true
docker volume rm <volume name>
docker volume prune

# remove named voluems
dc down -v
```
