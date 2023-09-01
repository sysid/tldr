# kubectl run

> Run pods in Kubernetes. Specifies pod generator to avoid deprecation error in some K8S versions.
> More information: <https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run>.

- Run an nginx pod and expose port 80:

`kubectl run {{nginx-dev}} --image=nginx --port 80`

- Run an nginx pod, setting the TEST_VAR environment variable:

`kubectl run {{nginx-dev}} --image=nginx --env="{{TEST_VAR}}={{testing}}"`

- Show API calls that would be made to create an nginx container:

`kubectl run {{nginx-dev}} --image=nginx --dry-run={{none|server|client}}`

- Run an Ubuntu pod interactively, never restart it, and remove it when it exits:

`kubectl run {{temp-ubuntu}} --image=ubuntu:22.04 --restart=Never --rm -- /bin/bash`

- Run an Ubuntu pod, overriding the default command with echo, and specifying custom arguments:

<<<<<<< HEAD
`kubectl run --generator=run-pod/v1 temp-ubuntu --image=ubuntu:20.04 --command -- echo arg1 arg2 arg3`


# Custom ...........................................................................................
- Gotcha: Deletion does not guarantee Pod termination (scale to zero before)
- running pod only removed/deleted AFTER new deployment pod has passed health check
- image hash is used to determine latest/changed image to trigger deployment
```bash
# restart/stop
k scale deployment chat --replicas=0 -n service
k scale deployment <deply> --replicas=0

# RUN: overwrite entrypoint
k run xxx -i --tty --image=621590899119.dkr.ecr.eu-central-1.amazonaws.com/e4m/gldpm/e4m:a8b9b1577415fa67cf18613f37211baee8d8016d --env="RUN_ENV=local" --command -- /bin/sh
k run xxx --rm -it --image 621590899119.dkr.ecr.eu-central-1.amazonaws.com/e4m/powerpool/powerpool-backend:pp-spike-simulator --env="AWS_ACCESS_KEY_ID=AKIAZBONUNGXSPPLMPNN" --env="AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}" --command -- /bin/bash
k run --rm -it xxx --env=https_proxy=http://host.docker.internal:3128 --env=http_proxy=http://host.docker.internal:3128 --image alpine:3.11 -- sh

# Force Stop
k delete pods <pod> --grace-period=0 --force  # hanging in terminating

# gracefully evict pods, but keep kube-proxy running
k drain <node> --ignore-daemonsets

# Manifest with service-account
apiVersion: v1
kind: Pod
metadata:
  name: twdbg
spec:
  serviceAccountName: sqs-access-service-account
  containers:
  - name: my-container
    image: 621590899119.dkr.ecr.eu-central-1.amazonaws.com/e4m/kubernetes-debugger:latest
    imagePullPolicy: Always
    command: ["/bin/bash"]
    stdin: true
    tty: true
  restartPolicy: Never

```
# Exec into pod ....................................................................................
```bash
k exec -it example-job-tslcn -c server -- sh  # exec into container 'server'

# Run As Root (not working!)
kubectl krew install exec-as  # Plugin Installation
kubectl exec-as -u <username>
kubectl exec-as -u <username> <podname> -- /bin/bash
```

# Copy, Port-Forwarding ...........................................................................
```bash
# copy files to a pod (one-by-one):
k cp file pod:/target

# GOTCHA: https://github.com/kubernetes/kubernetes/issues/58692#issuecomment-395957776
k cp pod:/file pod:/target  # GOTCHA: tar: Removing leading `/' from member names

# copy correct with relative dir, no leading / and retries
k cp --retries=10 simulation-flink-1:resources/results.csv results.csv

# port-forward
k port-forward service/gldpm-backend-test 8888:80
```
## Quarantine pod
- Kubernetes controllers (such as ReplicaSet) and Services match to Pods using selector labels
- removing the relevant labels from a Pod will stop it from being considered by a controller or from being served traffic by a Service.
- its controller will create a new Pod to take its place.
=======
`kubectl run {{temp-ubuntu}} --image=ubuntu:22.04 --command -- echo {{argument1 argument2 ...}}`
>>>>>>> 8695f2cc1524f46d82a7e5aa453d24ae6627ef3c
