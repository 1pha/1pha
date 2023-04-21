# `ssh` Connection

## 1. Starting `ssh`
```zsh
sudo apt-get update
sudo apt-get install openssh-server
sudo systemctl restart sshd
```

## 2. `ssh` without passwd: use pub keys

### (1) Create a New Key
```bash
cd ~/.ssh
ssh-keygen -t rsa -f id_rsa
ls
>>> id_rsa id_rsa.pub
```

### (2) Send `id_rsa.pub` to remote server
Send **public** key to remote server
```zsh
scp -P 22 id_rsa.pub (user)@(IP):(ABS_PATH)
```

### (3) Edit keys at your remote server
```zsh
ssh (user)@(IP) -p 22
mkdir ~/.ssh # ~/.ssh 디렉토리가 없는 경우
chmod 700 ~/.ssh

cat id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### (4) Upload new IP on Config

!!! question "Where is _ssh configuration_?"
    - _In general_ ssh configuration file `config` stays in your `~/.ssh/` directory.
    - Basic setup as follows
    - Put your `id_rsa` key name in `IdentityFile`

    ```config
    Host (HOSTNAME)
      HostName (IP)
      User (USERNAME)
      Port (PORT-NUMBER)
      IdentityFile ~/.ssh/id_rsa
    ```

####  Proxy Jumps
* [Reference](https://superuser.com/questions/1528212/vscode-ssh-with-multiple-hops)
```conf
Host HostA
  HostName hostA
  User userA

Host HostB
  HostName hostB
  User userB
  ProxyJump HostA
```
* One can add `IdentityFile` on final target server `HostB`

!!! question "Who needs proxy?"
    - Some servers are not accessible even with VPN
    - lab-2080 server is the only available server access thorugh VPN and other servers can be accessed via lab-2080 server.
    - Also, VSCode IDE is available for non-accessible servers with VPN with proxy jump settings.
