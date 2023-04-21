## `rsync`: Send files to remote server
``` bash title="Basic rsync usage"
rsync -e 'ssh -p 20000' LOCALDIR REMOTEDIR
```
Flags

* `-v`: verbosity
* `-r`: recursive
* `-a`: archive mode
* `-z`: Data compress
* `-h`: Human-readable
* `-e`: Extra ssh flags to feed

Sometimes permission issue may be raised. Use `chmod -R 777 (TARGET_DIR)` to add permission

Reference

* [Rsync basics](https://www.lesstif.com/system-admin/rsync-data-backup-12943658.html)
* [Changing ports](https://gkstamin.tistory.com/entry/Linux-rsync%EC%8B%9C-ssh-%ED%8F%AC%ED%8A%B8-%EB%B3%80%EA%B2%BD-%EB%B0%8F-%EC%82%AC%EC%9A%A9)