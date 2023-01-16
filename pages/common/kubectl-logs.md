# kubectl logs

> Show logs for containers in a pod.
> More information: <https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs>.

- Show logs for a single-container pod:

`kubectl logs {{pod_name}}`

- Show logs for a specified container in a pod:

`kubectl logs --container {{container_name}} {{pod_name}}`

- Show logs for all containers in a pod:

`kubectl logs --all-containers={{true}} {{pod_name}}`

- Stream pod logs:

`kubectl logs --follow {{pod_name}}`

- Stream logs for a specified container in a pod:

`kubectl logs --follow --container {{container_name}} {{pod_name}}`

- Show pod logs newer than a relative time like `10s`, `5m`, or `1h`:

`kubectl logs --since={{relative_time}} {{pod_name}}`

- Show the 10 most recent logs in a pod:

`kubectl logs --tail={{10}} {{pod_name}}`

# Custom  ...........................................................................................
```bash
# stream/tail all logs
k get pods --show-labels
k logs -f -l app=gldpm-backend --all-containers
k logs --tail 100000 -l app=poi-fb-backend-prod --all-containers
k logs -f -l app=gldpm-backend-prod -c gldpm-backend-prod  # one container
```
