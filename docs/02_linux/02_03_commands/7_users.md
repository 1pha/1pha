# Ubuntu Users

## Add User

``` bash title="Add user"
adduser 1pha
sudo usermod -aG sudo 1pha
```
This is simply adding a user into `sudo` group. If one account requires belonging to a `docker` group, to avoid typing `sudo` in every commands, simply `sudo usermod -aG docker 1pha`.

## Checking out to other User
``` bash title="checkout"
su 1pha
```