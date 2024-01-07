# kubectl get

> Get Kubernetes objects and resources.
> More information: <https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get>.

- Get all namespaces in the current cluster:

`kubectl get namespaces`

- Get nodes in a specified [n]amespace:

`kubectl get nodes --namespace {{namespace}}`

- Get pods in a specified [n]amespace:

`kubectl get pods --namespace {{namespace}}`

- Get deployments in a specified [n]amespace:

`kubectl get deployments --namespace {{namespace}}`

- Get services in a specified [n]amespace:

`kubectl get services --namespace {{namespace}}`

- Get all resources in a specified [n]amespace:

`kubectl get all --namespace {{namespace}}`

- Get Kubernetes objects defined in a YAML manifest [f]ile:

<<<<<<< HEAD
`kubectl get -f {{path/to/manifest.yaml}}`


# Custom Information ..............................................................................
```bash
# List information about a resource with more details:
k get {{pod|service|deployment|ingress|events...}} -o wide|json

# Nodes
k describe node <xxx>
k get nodes -o wide

# FILTER
k get pods --field-selector status.phase=Running,status.phase!=Evicted
k get pods --field-selector=status.phase==Succeeded   # find all ended pods

# NETWORKING
k describe node <node> -o wide
```
# Other ............................................................................................
```bash
# Print the address of the master and cluster services:
k cluster-info
k version  # !!! version skew

# Get/Set Context
k config get-contexts
k config use-context e4m-dev
k config view --minify --output 'jsonpath={..namespace}'  # show current ns

# Get all availabe resource names:
k api-resources

# Display resource (CPU/Memory/Storage) usage of nodes or pods:
k top {{pod|node}} --sort-by=memory

# Display an explanation of a specific field:
k explain {{pods.spec.containers}} --recursive
```
=======
`kubectl get --file {{path/to/manifest.yaml}}`
>>>>>>> 11ada1bc2471b6cab31280bd03f19ec66dc626c1
