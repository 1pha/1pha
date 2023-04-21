
## Mounting External disk
``` bash title="Mounting external disk"
sudo fdisk -l
sudo mkfs.ext4 /dev/sda # (1)
sudo mkdir /mnt/hdd
sudo mount /dev/sda /mnt/hdd
```

1. Please check your disk name: `dev/sda`. This command formats hard disk.

!!! question "What is `ext4`?"
    The `mkfs` command is used in Linux and Unix operating systems to create a file system on a storage device. The `-t` option specifies the type of file system to be created. `ext2`, `ext3`, and `ext4` are all different versions of the Linux file system.   
    - `ext2` is the second extended file system and was the default file system for Linux for many years. It does not support journaling, which means that it can take longer to recover data after a crash or power failure.   
    - `ext3` is an extension of the `ext2` file system and includes journaling. Journaling helps to reduce the time it takes to recover data after a crash or power failure. It is backward-compatible with `ext2`, which means that you can mount an `ext2` file system as an `ext3` file system.   
    - `ext4` is the fourth extended file system and includes improvements over `ext3`. It has faster file system checks and supports larger file systems and files. It also includes a number of other features, such as delayed allocation and multiblock allocation, which improve performance and reduce fragmentation.   
    When using the `mkfs` command with the `-t` option, you should specify the appropriate file system type for your needs. For example, if you are creating a file system on a large storage device with large files, `ext4` may be a good choice.


부팅 시 자동으로 적용되게 하려면 `/etc/fstab`에 넣어줘야한다. 넣어줄 때는 아래의 구성요소들을 갖춰야한다.
``` fstab
UUID=<UUID> <mount_point> <file_system_type> defaults 0 2

# e.g.
UUID=6D2B-0058 /mnt/my_external_drive ext4 defaults 0 2
```

1. **How to check `<UUID>`?**: `sudo blkid` and check file systems UUID
2. **What is `<mount point>`?**: Mount point is the directory that you have mounted with `mount` command
3. **What is `<file_system_type>`?**: 
4. **What are `defaults 0 2`?**: The defaults option specifies the default mount options, and the 0 2 at the end specifies the dump and file system check options. You can leave these as they are for most cases.

!!! question "What is `0 2` and `0 0`?"
    In the /etc/fstab file, the 0 2 and 0 0 at the end of each line specify the dump and file system check options for the corresponding file system.   
    The first number (dump) specifies whether or not the file system should be backed up using the dump command. A value of 0 means that the file system should not be backed up, while a value of 1 means that it should.   
    The second number (file system check) specifies the order in which the file system check should be performed at boot time. A value of 0 means that the file system should not be checked, while a value of 1 means that it should be checked first. The value 2 means that it should be checked after all file systems with a value of 1 have been checked.   
    In general, the dump option is not used anymore and is usually set to 0. The file system check option is more important, as it determines the order in which file systems are checked for errors during the boot process.   
    In your case, if a file system in your /etc/fstab file has 0 0 instead of 0 2, it means that the file system will not be checked for errors during the boot process. This might be appropriate for some types of file systems that are known to be very stable and reliable, such as read-only file systems or file systems on removable media.   
    However, it's generally a good idea to have all file systems checked for errors during the boot process. If you want to change the file system check option for a particular file system in /etc/fstab, you can edit the corresponding line and change the 0 0 to 0 2.