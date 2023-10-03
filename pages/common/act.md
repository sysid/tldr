# act

> Execute GitHub Actions locally using Docker.
> More information: <https://github.com/nektos/act>.

- List the available actions:

`act -l`

- Run the default event:

`act`

- Run a specific event:

`act {{event_type}}`

- Run a specific action:

`act -a {{action_id}}`

- Do not actually run the actions (i.e. a dry run):

`act -n`

- Show verbose logs:

`act -v`

- Run a specific workflow:

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
