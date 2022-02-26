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


