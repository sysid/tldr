# docker run

> Run a command in a new Docker container.
> More information: <https://docs.docker.com/engine/reference/commandline/run/>.

- Run command in a new container from a tagged image:

`docker run {{image:tag}} {{command}}`

- Run command in a new container in background and display its ID:

`docker run --detach {{image}} {{command}}`

- Run command in a one-off container in interactive mode and pseudo-TTY:

`docker run --rm --interactive --tty {{image}} {{command}}`

- Run command in a new container with passed environment variables:

`docker run --env '{{variable}}={{value}}' --env {{variable}} {{image}} {{command}}`

- Run command in a new container with bind mounted volumes:

`docker run --volume {{/path/to/host_path}}:{{/path/to/container_path}} {{image}} {{command}}`

- Run command in a new container with published ports:

`docker run --publish {{host_port}}:{{container_port}} {{image}} {{command}}`

- Run command in a new container overwriting the entrypoint of the image:

`docker run --entrypoint {{command}} {{image}}`

- Run command in a new container connecting it to a network:

`docker run --network {{network}} {{image}}`


# Operations .......................................................................................
```bash
# list processes
docker top <id>

# get all relevant info:
docker inspect <id>

docker system prune  # one rules all
docker rmi -f $(docker images --quiet --filter "dangling=true")

## Copy Data
docker cp foo.txt mycontainer:/foo.txt
```

## Containers: Run and Manage
```bash
# overwrite entrypoint
docker run -it --rm --network powerpool_ppnet -p 80:80 --entrypoint bash --name xxx -u root pp
docker run -it --rm \
    -p 8888:8888 -p 6080:80 \
    -p 5901:5900 \
    -e VNC_PASSWORD=mypassword \
    -h twhost \
    -e USER=root \
    -v $HOME/dev:/home/tw/dev -v $HOME/binx:/home/tw/binx -v $HOME/nbs:/home/tw/nbs \
    --security-opt seccomp=$HOME/seccomp_chrome.json \
    --network mynet \
    tw/sysid

d container ls -a
d container prune
```

### Start Automatically
[Start Automatically](https://docs.docker.com/engine/admin/start-containers-automatically/#use-a-restart-policy)
- If you manually stop a container, its restart policy is ignored
- Restart policies only apply to containers.
