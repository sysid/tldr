# kubectl
https://learnk8s.io/blog/kubectl-productivity

> Command-line interface for running commands against Kubernetes clusters.
> Some subcommands such as `kubectl run` have their own usage documentation.
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


# Ops, Operations........................................................................................
- version can be the same as or up to one minor version earlier or later than the Kubernetes version of your cluster

[kubectl logs]($HOME/.cache/tldr/pages/common/kubectl-logs.md)
[kubectl get]($HOME/.cache/tldr/pages/common/kubectl-get.md)
[kubectl run]($HOME/.cache/tldr/pages/common/kubectl-run.md)

# Debugging ........................................................................................
- [look up resource definition](https://learnk8s.io/blog/kubectl-productivity#2-quickly-look-up-resource-specifications)
```bash
# print environment
kubectl exec kube -- env

# deploy kubernete-debugger image into namespace
k run twdbg --rm -it --image=621590899119.dkr.ecr.eu-central-1.amazonaws.com/e4m/kubernetes-debugger:latest --comand -- /bin/bash

k logs xxx -p  # previous (crashed)
k get events -w  # watch
k get all -A
```

# Create/Delete/Update ............................................................................
```bash
# create
k describe deployment asset-control  # get parameters to replace __xxx__ (analog deployer)
k apply -f deployment.yaml
k get deployments.apps

# delete (bulk) by file or labels
k delete deployment,services -l app=nginx
k delete -f https://k8s.io/examples/application/nginx-app.yaml

# delete and recreate
k replace -f https://k8s.io/examples/application/nginx/nginx-deployment.yaml --force
```

# Certifcates, Ingress ............................................................................
## Certs
```bash
k get certificate -o wide  # show issuer and status
```

# Installation/Configuration ......................................................................
- Gotcha: manage `kubeclt` with asdf: manage version skew (client/server)
- Configuration Location: `~/.kube/config`
```bash
# get cluster credentials, does not matter which project
aws eks update-kubeconfig --profile e4m-dev-gldpm --region eu-central-1 --name e4m-test --alias e4m-dev-test-gldpm
```
