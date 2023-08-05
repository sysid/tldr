# gh

> Work seamlessly with GitHub.
> Some subcommands such as `gh config` have their own usage documentation.
> More information: <https://cli.github.com/>.

- Clone a GitHub repository locally:

`gh repo clone {{owner}}/{{repository}}`

- Create a new issue:

`gh issue create`

- View and filter the open issues of the current repository:

`gh issue list`

- View an issue in the default web browser:

`gh issue view --web {{issue_number}}`

- Create a pull request:

`gh pr create`

- View a pull request in the default web browser:

`gh pr view --web {{pr_number}}`

- Check out a specific pull request locally:

`gh pr checkout {{pr_number}}`

- Check the status of a repository's pull requests:

`gh pr status`


# Custom ...........................................................................................
- [gh environment](https://cli.github.com/manual/gh_help_environment)
- renaming repos does NOT break the exisiting remote urls

---
<!--ID:1691166851926-->
1. How to see what's on for me in Github?
> `gh status
<!--ID:1691166851928-->
1. How to find a repo in GH?
> ```bash
> gh search repos <name>
> gh search repos --archived
>
> # list all forks/public/private
> gh clone-org -n -s "fork:only" sysid
> gh clone-org -n -s "is:private" sysid
> gh clone-org -n -s "archived:true" sysid
> ```
<!--ID:1691166851929-->
1. How to run GH (deploy) action?
> ```bash
> gh actions  # get help
> gh workflow run deploy.yml --ref v2.1.1 environment=int
> gh workflow run deploy.yml --ref main environment=int
> # list
> gh run list --workflow=deploy.yml
> ```

---
```bash
gh status  # show my total status
gh repo fork https://github.com/lotabout/skim --clone --remote  # forks a repo

### Merge Requests: view merge request
gh pr list
gh pr view --web 29
gh -R e4m/pp-powerpool-backend pr checks 29  # select repot to act on

gh run watch 544737  # show status
gh run view 544737 --log-failed  # view result of workflow run
gh run view 123456 --log
gh run view --job=647919  # select vie Job-ID

gh run rerun --failed 544737

### Clone all repors
gh extension install https://github.com/sysid/gh-clone-org.git
gh clone-org -y e4m -n
# clone private repos
gh clone-org -n sysid -s "is:private"


### list all repos sorted by age
PAGER="less -FX" gh repo list -L 130 --json name,updatedAt,isPrivate,isFork,isArchived,diskUsage
cat repos.out | jq '.|sort_by(.updatedAt)[].updatedAt'
PAGER="less -FX" gh repo list --json name,updatedAt | jq '.|sort_by(.updatedAt)[].updatedAt'

```

## Config, Authentication
- configure authentication: [auth](git.md#Authentication)
- ~/.config/gh/config.yml
```bash
# Authentication
echo xxx | gh auth login --hostname atc-github.azure.cloud.bmw --with-token
gh auth status

# Alternative
GH_ENTERPRISE_TOKEN=xxx GH_HOST=atc-github.azure.cloud.bmw gh repo list e4m
```
### Gotcha
- git authentication working with MacOS secring plugin, but not gh
- Token must be visible in environment

## Alias
[Extending the GitHub CLI](https://www.aaron-powell.com/posts/2021-01-22-extending-the-github-cli/)
[Scripting with GitHub CLI | The GitHub Blog](https://github.blog/2021-03-11-scripting-with-github-cli/)
- comma in name ok
- location: ~/.config/gh/config.yml
```bash
# Setting Multiline
gh alias set -s hello - << 'EOF'
echo "Hello, \e[32m$USER\e[0m!"
EOF

cat command.txt | gh alias set user -

# Parameter Expansion
gh alias set --shell igrep 'gh issue list --label="$1" | grep "$2"'
```
