# kubectl
https://learnk8s.io/blog/kubectl-productivity
https://learnk8s.io/blog/kubectl-productivity#2-quickly-look-up-resource-specifications

> Command-line interface for running commands against Kubernetes clusters.
> See also `kubectl describe` and other pages for additional information.
> More information: <https://kubernetes.io/docs/reference/kubectl/>.

- List information about a resource with more details:

`kubectl get {{pod|service|deployment|ingress|events...}} -o wide|json`

- Update specified pod with the label 'unhealthy' and the value 'true':

`kubectl label pods {{name}} unhealthy=true`

- List all resources with different types:

`kubectl get all -A`

- Display resource (CPU/Memory/Storage) usage of nodes or pods:

`kubectl top {{pod|node}} --sort-by=memory`

- Print the address of the master and cluster services:

`kubectl cluster-info`

- Display an explanation of a specific field:

`kubectl explain {{pods.spec.containers}} --recursive`

- Print the logs for a container in a pod or specified resource:

`kubectl logs {{pod_name}}`

- Run command in an existing pod:

`kubectl exec {{pod_name}} -- {{ls /}}`

- copy files to a pod:

`k cp file pod:/target`


## Info/Debugging
```bash
# Get all availabe resource names:
k api-resources

kubectl describe <resource>

# Get/Set Context
k config get-contexts
k config use-context e4m-dev

# show current ns
kubectl config view --minify --output 'jsonpath={..namespace}'

# networking
k get nodes -o wide
k describe node <node> -o wide
```

## Filter by Labeling
```bash
k describe pods secure-monolith |grep Label  # find label
k label pods secure-monolith "secure=enabled"  # add label
k get pods -Lapp -Ltier -Lrole  # adds labels as columns
k get pods -lapp==guestbook,role!=slave  # selector (==, !=)
k get pods -l "app=monolith,secure=enabled"

# stream/tail all logs
k get pods --show-labels
k logs -f -l app=gldpm-backend --all-containers
```

## Run/Execute/Restart
```bash
k exec -it pod_name -- /bin/bash  # execute commands in pod
k run my-shell --rm -i --tty --generator=run-pod/v1 --image ubuntu -- bash  # get bash pod

# restart
kubectl scale deployment chat --replicas=0 -n service
k delete pods pod-123
```

## Create/Delete/Update
```bash
# delete (bulk) by file or labels
k delete -f https://k8s.io/examples/application/nginx-app.yaml
k delete deployment,services -l app=nginx

# delete and recreate
k replace -f https://k8s.io/examples/application/nginx/nginx-deployment.yaml --force

# update
k patch serviceaccount default -p '{"imagePullSecrets": [{"name": "myregistrykey"}]}'
```

## Secrets
```bash
# show secrets data:
k describe secrets simulation-secret

# validate secret
k get secret simulation-secret -o jsonpath="{.data.ASSET_CONTROL_TOKEN}" | base64 --decode
```

## Auth/Autz
```bash
kubectl auth reconcile -f my-rbac-rules.yaml --dry-run=client  # apply RBAC objects from manifest

# apply it, not preserving exisitng perms/subjs
kubectl auth reconcile -f my-rbac-rules.yaml --remove-extra-subjects --remove-extra-permissions
```


# k9s...............................................................................................
[k9s commands](https://k9scli.io/topics/commands/)
```
<c-a>                      show all resources/aliases
:<resource> view k8s resource/alias
:ctx switch context
:ns switch namespace
```

## pulses view

## XRay view

## Popeye view
