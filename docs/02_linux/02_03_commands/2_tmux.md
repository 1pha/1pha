## `tmux`: Running in background, but accessible
```bash title="Installing tmux"
sudo apt-get install tmux
```

### Basic usage
* `tmux` consists of session - pane hierarchy.
`work` is the name of session.
* New session: `tmux new -s work`
* Exit session: **CTRL B, D**
* Resume to session: `tmux attach -t work`
* Divide session into pane:
    - Horizontal spilt: **CTRL B, "**
    - Vertical split: **CTRL B, %**
* Entering Command mode: **CTRL B, :**
    - Removing panes: `kill-pane -t (index)`, where index counts from 0