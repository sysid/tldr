# terraform

> Create and deploy infrastructure as code to cloud providers.
> More information: <https://www.terraform.io/>.

- Initialize a new or existing Terraform configuration:

`terraform init`

- Verify that the configuration files are syntactically valid:

`terraform validate`

- Format configuration according to Terraform language style conventions:

`terraform fmt`

- Generate and show an execution plan:

`terraform plan`

- Build or change infrastructure:

`terraform apply`

- Destroy Terraform-managed infrastructure:

`terraform destroy`


[terraform.pptx]($HOME/dev/s/private/vimwiki/help/terraform.pptx)
# Operations .......................................................................................
- environment variables `TF_VAR_xxx`, hardcode in `*.tf` (i.e. `TF_VAR_ENVIRONMNET_PREFIX`)
```bash
tf init  # initializes .terraform and loads providers, modules and installs backend

# Change Backend Configuration During the Init
# reconfigure is used in order to tell Terraform to not copy the existing state to the new remote state location.
terraform init -backend-config=cfg/s3.dev.tf -reconfigure

# realign state with reality (does not update config files, only re-syncs state)
terraform refresh

# will scan all `*.tf` files in your directory and create the plan.
tf plan -out plan.out

# reprints the output of the `output` section
tf output

# safe safe replace even if no change in config
# allows to rebuild specific resources and avoid a full terraform destroy operation.
terraform plan -replace="aws_instance.example"
terraform apply -replace="aws_instance.example"

#------------------------------------ apply -------------------------------------
# generates state `terraform.tfstate`, runs parallel where possible
tf apply  # generates plan and prompts in one step -> tfstate
tf apply plan.out
tf show

# with variable
tf apply -var tags-repo_url=${GIT_URL}

# apply only one module
tf apply -target=module.s3

#----------------------------------- destroy ------------------------------------
# create a plan before:
tf plan –destroy

# destroy one resource:
tf destroy -target aws_s3_bucket.my_bucket

#------------------------------------ state -------------------------------------
# [How to manage Terraform state](https://blog.gruntwork.io/how-to-manage-terraform-state-28f5697e68fa)
tf state   # show management commands
tf state list  # see whether address/resource exist in state
tf state show <address>  # view attributes stored in state address

# info from state for consumption -> terraform.tfvar
 tf show -json | jq   # run where .terraform is located

# pull remote state into a local copy
# useful if, for example, you originally use a local tf state and then you define backend storage, in S3 or Consul
tf state pull > terraform.tfstate

# import existing resource in infrastructure into state
tf import asw_iam_policy.elastic_post


#------------------------------------ other -------------------------------------
tf fmt
tf validate  # syntax check in directory
tf providers  # list of providers/plugins

# output logging to stderr (TRACE, DEBUG, INFO, WARN or ERROR), alternative: TF_LOG_CORE or TF_LOG_PROVIDER
export TF_LOG=DEBUG

# Visual dependency graph of Terraform resources.
tf graph | dot –Tpng > graph.png
```
