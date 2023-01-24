# docker build

> Build an image from a Dockerfile.
> More information: <https://docs.docker.com/engine/reference/commandline/build/>.

- Build a docker image using the Dockerfile in the current directory:

`docker build .`

- Build a docker image from a Dockerfile at a specified URL:

`docker build {{github.com/creack/docker-firefox}}`

- Build a docker image and tag it:

`docker build --tag {{name:tag}} .`

- Build a docker image with no build context:

`docker build --tag {{name:tag}} - < {{Dockerfile}}`

- Do not use the cache when building the image:

`docker build --no-cache --tag {{name:tag}} .`

- Build a docker image using a specific Dockerfile:

`docker build --file {{Dockerfile}} .`

- Build with custom build-time variables:

`docker build --build-arg {{HTTP_PROXY=http://10.20.30.2:1234}} --build-arg {{FTP_PROXY=http://40.50.60.5:4567}} .`



# Building .......................................................................................
[docker.pptx]($HOME/dev/s/private/vimwiki/help/docker.pptx)

- cache busting: combine `RUN apt-get update with apt-get install` in the same `RUN` statement
- use `gosu` to downgrade from `root`
- separate layer is created any time files are added to the container image. This includes FROM, RUN, ADD, and COPY
`--cache-from` is used to specify one or more images to use as a cache source to look for existing layers that can be reused, rather than recreating them
```bash
docker build --no-cache --progress=plain --build-arg TWINE_USERNAME=qqe4m00 --build-arg TWINE_PASSWORD="$TWINE_PASSWORD"  --build-arg NO_PROXY='localhost,127.0.0.*,10.*,192.168.*,kubernetes.docker.internal,*.bmwgroup.net' -t poi .

# get IP of build host on MacOS
export SYSID_HOST="http://$(ifconfig | grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" | grep -v 127.0.0.1 | awk '{ print $2 }' | cut -f2 -d: | head -n1):3128"
# forward proxy to build host
finch build --build-arg TWINE_USERNAME=qqe4m00 --build-arg TWINE_PASSWORD="$TWINE_PASSWORD" \
--build-arg NO_PROXY='localhost,127.0.0.*,10.*,192.168.*,kubernetes.docker.internal,*.bmwgroup.net' \
--build-arg http_proxy=${SYSID_HOST} \
--build-arg https_proxy=${SYSID_HOST} \
--build-arg HTTP_PROXY=${SYSID_HOST} \
--build-arg HTTPS_PROXY=${SYSID_HOST} \
-t poi .

# cache loading
docker pull 621590899119.dkr.ecr.eu-central-1.amazonaws.com/e4m/gldpm/e4m:latest || true
docker build \
--cache-from 621590899119.dkr.ecr.eu-central-1.amazonaws.com/e4m/gldpm/e4m:latest \
--build-arg COMMIT_SHA="${{ github.sha }}" \
--build-arg TWINE_USERNAME="${{ secrets.TWINE_USERNAME }}" \
--build-arg TWINE_PASSWORD="${{ secrets.TWINE_PASSWORD }}" \
-t gldpm-backend .

docker images  # list images
docker rmi $(docker images -f dangling=true -q)  # dangling images

# save image as tar (gotcha: must have enough memory)
docker save img > img.tar
```

## Gotcha Building
- `pipenv lock --requirements > requirements.txt` fails when local install with `-e .` in Pipfile
- copy fails: check `.dockerignore` !!!


# Security and Credentials ........................................................................
## squash
will take all the filesystem layers produced by a build and collapse them into a single new layer.
1. add a secret file in my first layer,
2. then use the secret file in my second layer,
3. and the finally remove my secret file in the third layer,
4. and then build with the --squash flag.
- `envsubst < ./Dockerfile | docker build -squash -t ${DOCKIMG}:${VERSION} . -f -`



# Entrypoint, Run, Cmd (Gotcha: K8S) ..............................................................
- `ENTRYPOINT` should be defined when using the container as an executable.
- `ENTRYPOINT` defines the process that runs as PID 1 in the container, and the `CMD` adds options to it.
- `ENTRYPOINT`, and `CMD` are always converted to arrays, so stick with array
- If you don’t specify an `ENTRYPOINT` but you do specify a `CMD` then the implicit entrypoint is `/bin/sh -c`. Don't do it!
- Anything that appears after `docker run <img>` is passed to the container and treated as `CMD` argument.
- CMD is merely a default, if we pass non-option arguments to docker run, they will override the value of CMD
```bash
ENTRYPOINT ["/bin/chamber", "exec", "production", "--"]
CMD ["/bin/service", "-d"]
# Putting these togetheri:
["/bin/chamber", "exec", "production", "--", "/bin/service", "-d"]
```

## Example: Wrapper
- The entrypoint script will see `date` as the argument, it will call `exec date`.
- can put anything else in that script
```bash
# entrypoint.sh
#!/bin/bash
exec "$@"

# Dockerfile:
ADD entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["date"]
```


# Dockerfile .......................................................................................
```bash
# Example: Minimal Dockerfile
FROM ubuntu
ARG test
RUN echo $test

# build it
docker build --build-arg test=hello .
```

## Environment Variables
[docker.pptx]($HOME/dev/s/private/vimwiki/help/docker.pptx)

### ARG
- available during the build of a Docker image (RUN etc), not after the image is created and containers are started from it (ENTRYPOINT, CMD).
- You can use ARG values to set ENV values to work around that.
```bash
      ARG GIT_HASH  # get from CLI
      ENV GIT_HASH=${GIT_HASH:-dev}  # pass into container
```
- can be easily inspected after an image is built, by viewing the docker history of an image, poor choice for sensitive data.
- `docker build --build-arg some_variable_name=a_value`
- Multistage build: ARGs only last for the build phase of a single image. For the multistage, renew the ARG after `FROM` instructions.

[Predefined Args](https://docs.docker.com/engine/reference/builder/#predefined-args)
- variables that you can use without a corresponding ARG instruction in the Dockerfile
- http_proxy ARGs are predefined, use with: `docker build --build-arg HTTPS_PROXY=https://my-proxy.example.com.`

### ENV
- available during the build, as soon as you introduce them with an ENV instruction.
- accessible by containers started from the final image.
- can be overridden when starting a container: `docker run -e "env_var_name=another_value"`


## Multistage Builds, Minimize Image size
[Tiny Container Challenge: Building a 6kB Containerized HTTP Server!](https://devopsdirective.com/posts/2021/04/tiny-container-image/)
[Multi-stage builds | Docker Documentation](https://docs.docker.com/build/building/multi-stage/)
```bash
# https://sourcery.ai/blog/python-docker/
FROM python:3.7-slim as base

# Setup env
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONFAULTHANDLER 1


FROM base AS python-deps

# Install pipenv and compilation dependencies
RUN pip install pipenv
RUN apt-get update && apt-get install -y --no-install-recommends gcc

# Install python dependencies in /.venv
COPY Pipfile .
COPY Pipfile.lock .
RUN PIPENV_VENV_IN_PROJECT=1 pipenv install --deploy


FROM base AS runtime

# Copy virtual env from python-deps stage
COPY --from=python-deps /.venv /.venv
ENV PATH="/.venv/bin:$PATH"

# Create and switch to a new user
RUN useradd --create-home appuser
WORKDIR /home/appuser
USER appuser

# Install application into container
COPY . .

# Run the application
ENTRYPOINT ["python", "-m", "http.server"]
CMD ["--directory", "directory", "8000"]
```


# Optimize build time .............................................................................
[src](https://dev.to/stevesmashcode/tips-on-building-a-docker-image-quickly-in-a-ci-cd-2ph7)
- Use `.dockerfileignore`: The first step of docker build is tarball the entire directory and send the data to docker.
- Things that don't often change should be pushed up in the file: initial package installs, static environment variables
- Docker has a concept of caching, but the first line in the dockerfile that has a change, all lines after it will no longer be cached.
- Don't use a wildcard with the COPY command. Docker will never cache that line. Be explicit!


##  typical flow:
```bash
FROM base-cotnainer

# Add all static environment variables, folders, users...
ENV IS_APPLICATION=1
RUN mkdir app
WORKDIR /app
RUN useradd -ms /bin/sh newuser
COMMAND ["python3", "main.py"]

# Install all base image packages
RUN apt install ...

# Copy over your library depency files (Python example below)
COPY Pipfile /app/Pipfile
COPY Pipfile.lock /app/Pipfile.lock

# Install your libraries (Python example below)
RUN pipenv install --deploy

# Lastly, copy the code over
COPY . /app/
```

## Use Buildkit
- Use docker's new library BUILDKIT and inline cache information into your builds. (This is a simple and big one)
  There are three things we need to set to get proper image caching in a pipeline:
  - Set the `DOCKER_BUILDKIT` environment variable to 1
  - Add a build argument to tell the new BUILDKIT library to line caching information in each image layer: `--build-arg BUILDKIT_INLINE_CACHE=1`
  - Tell the docker build command where you can get your image from to cache `--cache-from [remote-repo-url]/[CONTAINER_NAME]:latest`
- The full command would look something like this:
  `DOCKER_BUILDKIT=1 docker build --build-arg BUILDKIT_INLINE_CACHE=1 --cache-from [remote-repo-url]/[CONTAINER_NAME]:latest --tag [CONTAINER_NAME]` .
