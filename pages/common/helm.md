# helm
<<<<<<< HEAD
=======

> A package manager for Kubernetes.
> Some subcommands such as `install` have their own usage documentation.
>>>>>>> 0fddee227248777cf3ac5b14fe5cc9b22649d2b4
> More information: <https://helm.sh/>.

- Create a helm chart:

`helm create {{chart_name}}`

- Add a new helm repository:

`helm repo add {{repository_name}}`

- List helm repositories:

`helm repo list`

- Update helm repositories:

`helm repo update`

- Delete a helm repository:

`helm repo remove {{repository_name}}`

- Install a helm chart:

`helm install {{name}} {{repository_name}}/{{chart_name}}`

- Download helm chart as a tar archive:

`helm get {{chart_release_name}}`

- Update helm dependencies:

`helm dependency update`
