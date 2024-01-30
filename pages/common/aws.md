# aws

> The official CLI tool for Amazon Web Services.
> Some subcommands such as `aws s3` have their own usage documentation.
> More information: <https://aws.amazon.com/cli>.

- Configure the AWS Command-line:

`aws configure wizard`

- Configure the AWS Command-line using SSO:

`aws configure sso`

- Get the caller identity (used to troubleshoot permissions):

`aws sts get-caller-identity`

- List AWS resources in a region and output in YAML:

`aws dynamodb list-tables --region {{us-east-1}} --output yaml`

- Use auto prompt to help with a command:

`aws iam create-user --cli-auto-prompt`

- Get an interactive wizard for an AWS resource:

`aws dynamodb wizard {{new_table}}`

- Generate a JSON CLI Skeleton (useful for infrastructure as code):

`aws dynamodb update-table --generate-cli-skeleton`

<<<<<<< HEAD

```bash
aws configure list-profiles  # show availabe named profiles
aws configure list  # show current profile
aws-who-am-i --profile bmwsso

aws iam list-users
aws sts get-caller-identity
aws sts get-caller-identity --profile e4m-orbit-dev

aws --profile tw sts get-caller-identity

alias orbit-3cp-dev='aws eks update-kubeconfig --profile e4m-dev-3cp --region eu-central-1 --name e4m-test'
alias orbit-3cp-mock-dev='aws eks update-kubeconfig --profile e4m-dev-3cp-mock --region eu-central-1 --name e4m-test'
alias orbit-gitlab-dev='aws eks update-kubeconfig --profile e4m-dev-gitlab --region eu-central-1 --name e4m-test'
alias orbit-keycloak-dev='aws eks update-kubeconfig --profile e4m-dev-keycloak --region eu-central-1 --name e4m-test'
alias orbit-login='cdh login bmwsso'
alias orbit-pcv-dev='aws eks update-kubeconfig --profile e4m-dev-pcv --region eu-central-1 --name e4m-test'
alias orbit-powerpool-dev='aws eks update-kubeconfig --profile e4m-dev-powerpool --region eu-central-1 --name e4m-test'

# blow will switch cluster context and kcn will set namespace on currant context
alias k-dev='kubectl config use-context e4m-dev'
alias k-playpen='kubectl config use-context e4m-playpen'
alias k-prod='kubectl config use-context e4m-prod'
alias kcn='kubectl config set-context $(kubectl config current-context) --namespace'
```
=======
- Display help for a specific command:

`aws {{command}} help`
>>>>>>> 46a054215a8f11a8256941e42082b97a25d1bcb0
