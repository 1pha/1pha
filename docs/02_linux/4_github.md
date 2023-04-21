# GitHub (`gh`)

`gh` is a command to authenticate/login to your account. Basic `git` command is different from `gh`.
### Install `gh`
```zsh title="Installing gh. One may need 'sudo su' to make it happen"
type -p curl >/dev/null || sudo apt install curl -y
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
&& sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```
```zsh title="Upgrade gh"
sudo apt update
sudo apt install gh
```

### GitHub Login through CLI
Then login through `gh`. If you're running this on your server, choose "Paste an authentication token" on third choice, and paste your developer personal access tokens.
```zsh
gh auth login
```
!!! question "What is `gpg keys`?"
    GitHub no longer allows you to type in passwords on CLI, which means no one can clone/pull/commit anything on cli through passwords. Instead, everybody uses **Personal access tokens**. Lookup [here](https://docs.github.com/ko/enterprise-server@3.5/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) for more details

