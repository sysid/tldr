# docker

> Manage Docker containers and images.
> Some subcommands such as `docker run` have their own usage documentation.
> More information: <https://docs.docker.com/engine/reference/commandline/cli/>.

- List all docker containers (running and stopped):

`docker ps --all`

- Start a container from an image, with a custom name:

`docker run --name {{container_name}} {{image}}`

- Start or stop an existing container:

`docker {{start|stop}} {{container_name}}`

- Pull an image from a docker registry:

`docker pull {{image}}`

- Display the list of already downloaded images:

`docker images`

- Open a shell inside a running container:

`docker exec -it {{container_name}} {{sh}}`

- Remove a stopped container:

`docker rm {{container_name}}`

- Fetch and follow the logs of a container:

`docker logs -f {{container_name}}`


# Operations .......................................................................................
[docker-run]($HOME/.cache/tldr/pages/common/docker-run.md)
[docker-logs]($HOME/.cache/tldr/pages/common/docker-logs.md)
[docker-volume]($HOME/.cache/tldr/pages/common/docker-volume.md)
[docker-build]($HOME/.cache/tldr/pages/common/docker-build.md)


# Debugging ........................................................................................
[Debugging Containers](https://medium.com/@betz.mark/ten-tips-for-debugging-docker-containers-cde4da841a1d)

1. View stdout history with the `logs` command.
  Anything that gets written to stdout for the process that is pid 1

2. Stream stdout with the `attach` command.
   If you want to see what is written to stdout: `docker attach <container>`

3. `docker exec -it <container> /bin/sh`

4. If the entry point is specified on the command line then it overrides any entry point set in the dockerfile when the image was built.
  `docker run -d -p 80:80 --entrypoint /bin/sh /myrepo/mydjangoapp`

5. Get process stats with the `top` command.

6. View container details with the `inspect` command.
   Most valuable use of inspect for me in the past has been getting the values of environment vars.

7. View image layers with the `history` command.
   `docker history sysid/twvim_full`

8. Run one process in each container.
   one is less of a debugging tip, and more of a best practice that will definitely make debugging, and reasoning about the state of your container a lot easier.

```bash
### DNS
docker run busybox nslookup google.com  # ;; connection timed out; no servers could be reached
finch run --dns 160.50.250.20 busybox nslookup google.com  # ** server can't find google.com: NXDOMAIN

### Tooling: install basics
apt update
apt -y install dnsutils iputils-ping traceroute vim procps pcregrep curl net-tools python3-pip unzip
apt -y install openssh-client

docker run --volume="$PWD":/app -it python:3-slim bash -c "pip3 install --upgrade pip; pip3 install pipenv; cd /app; apt-get update && apt-get install --yes awscli bc bsdmainutils curl freetds-dev g++ gcc git jq sshpass tdsodbc openssh-client rsync unixodbc-dev zip; pipenv sync --bare --three; pipenv install --skip-lock --dev"
```
