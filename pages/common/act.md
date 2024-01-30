# act

> Execute GitHub Actions locally using Docker.
> More information: <https://github.com/nektos/act>.

- [l]ist the available jobs:

`act -l`

- Run the default event:

`act`

- Run a specific event:

`act {{event_type}}`

- Run a specific [j]ob:

`act -j {{job_id}}`

- Do [n]ot actually run the actions (i.e. a dry run):

`act -n`

- Show [v]erbose logs:

`act -v`

- Run a specific [W]orkflow with the push event:

`act push -W {{path/to/workflow}}`


# Custom ...........................................................................................
```bash
# Proxy config:
act --env https_proxy=http://host.docker.internal:3128 --env http_proxy=http://host.docker.internal:3128 --container-architecture linux/amd64
dk exec -it ... bash
npm config set proxy http://host.docker.internal:3128
npm config set https-proxy http://host.docker.internal:3128
npm install
```
- exec into container for debugging
- Gotcha: Proxy config within container (nothing is working, e.g. npm install)
- looks like the proxy config is part of the workflow setting:
  This command already sets the proxy correct:
  docker exec cmd=[bash --noprofile --norc -e -o pipefail /var/run/act/workflow/2]
