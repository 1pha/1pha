## Checking storage for certain directory

``` bash
ls | xargs du -sh
```
!!! question "What is `xargs`?"
    Sometimes you want outputs from a certain command such as list, and feed them to other commands. This is available through `xargs`. Above command allows `ls` stdout to be stdin of `du -sh` command.
