# helm
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


# Helm
https://helm.sh/docs/
https://artifacthub.io/
```bash
helm version

## Repo
helm repo add stable https://charts.helm.sh/stable
helm search repo repo_name/package (stable/postgresql)

# Finding/Checking
helm search repo bitnami/keycloak --versions
helm show chart bitnami/keycloak
helm show readme bitnami/keycloak
helm show values bitnami/keycloak
helm show all bitnami/keycloak

# installing
helm install <rname> <repo>/<chart>
helm install keycloak bitnami/keycloak  --dry-run --debug
helm install keycloak bitnami/keycloak -f values.yaml --set key=value,key=value

# view release status
helm list --all
k get all
helm status keycloak
helm get manifest keycloak
helm get values
helm get values keycloak
helm get notes keycloak
helm get all keycloak | tee /tmp/k.out


# uninstall release
helm uninstall keycloak --keep-history
helm delete keycloak
helm list --all  # empty

# upgrade release (rollout strategy: deployment.yaml)
# specify the same parameters as when the chart was initially deployed, e.g passwdw.
helm search repo bitnami/keycloak --versions
helm install keycloak bitnami/keycloak  --version 2.4.2
helm repo update  # fetch new releases
helm upgrade keycloak bitnami/keycloak --version 2.4.3
helm list
helm history keycloak  # show the revisions

# downgrading
helm rollback keycloak 1 --dry-run --debug
helm rollback keycloak 1
helm history keycloak
k get secrets  # helm keeps relases as secrets in the namespace
```

## Debugging
```bash
helm env
helm list --all
helm status|history
helm history <release> -o yaml
helm get all <release>
kubectl logs
```


## Tricks/Hacks
```bash
export POSTGRES_PASSWORD=$(kubectl get secret --namespace default pg-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode)
kubectl run pg-postgresql-client --rm --tty -i --restart='Never' --namespace default --image docker.io/bitnami/postgresql:11.7.0-debian-10-r0 --env="PGPASSWORD=$POSTGRES_PASSWORD" --command -- psql --host pg-postgresql -U postgres -d postgres -p 5432
```

## Manage Charts
- dependencies are listed in `Charts.yaml` and must be downloaded before installing
[helm_structure](vm::$HOME/vimwiki/help/helm_structure.png)
```bash
# check cloned chart for dependency and install missing
helm dep list
helm dep update  # download dependency charts
helm dep build

helm lint
```

## Configure Repos
- http server
- show local config: `helm env`


## Create Chart
```bash
helm create ourchart
cd ourchart/templates/
rm *.yaml
k create deployment nginx --image=nginx --dry-run=client --output=yaml > deployment.yaml

# run it in oder to get the service yaml
k create deployment nginx --image=nginx
k expose deployment nginx --type=LoadBalancer --port=80 --dry-run=client --output=yaml > service.yaml
k delete deployments.apps nginx

echo "hello test chart" > NOTES.txt

# no tests and other chart deps
rm -fr tests/
rm -fr charts/

# isntall and package
helm install ourchart ./ourchart
helm package ./ourchart --destination ./
```
- GitHub can host charts, use the raw-url for `index.yaml: helm repo index .`
- `index.yaml` is vital for `helm search repo`

## Concepts
- uses CRDs (common resource def)
- zipped yaml files with placeholders, interploation on helm-client
- redeploy after `values.yaml` change
- uninstall/reinstall von pg keeps the data in PV
- pull helm chart and unpack to see what's in it


# Tools
- Helm Repo: chart museum


# Charts
## postgresql
https://artifacthub.io/packages/helm/bitnami/postgresql
-%35% [ ] expose pg via ingress: https://www.enterprisedb.com/docs/kubernetes/cloud_native_postgresql/expose_pg_services/


# Gotchas
- bitnami/postgresql: broken installation stays in PCV, e.g. password
