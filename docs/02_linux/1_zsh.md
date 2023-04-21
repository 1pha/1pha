# `zsh`

Why `zsh`?
- `zsh` is more capable of auto-completion, boosting productivity for developers.

## Installing `zsh`
``` bash linenums="1" title="Install zsh"
sudo apt-get update # (1)
sudo apt install zsh
```

1. :man_raising_hand: 자주할수록 잔 에러가 안생김

## `zsh` as default shell
``` zsh
chsh -s $(which zsh)
```

## Pretty ZSH

```zsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone https://github.com/sindresorhus/pure.git ~/.zsh/pure
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.zsh/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
```

* Append the followings to `~/.zshrc` file.
* `source activate` may not work if conda is not installed. After installing anaconda, the command will not provoke an error
```zsh
path+=/opt/conda/bin
TZ=Asia/Seoul
LANG=C.UTF-8
LC_ALL=C.UTF-8
PYTHONDONTWRITEBYTECODE=1
PYTHONUNBUFFERED=1
PYTHONIOENCODING=UTF-8
PYTHONHTTPSVERIFY=0
fpath+=("$HOME/.zsh/pure")\nautoload -U promptinit; promptinit\nprompt pure
source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=111'
source activate
```