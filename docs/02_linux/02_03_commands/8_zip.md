# Compress & Unzip Files

## `zip`
Note that it's a good convention to `.zip` extension in `(FILENAME)`
``` bash title="Compress with zip"
zip (FILENAME) (TARGET_DIR)
```

If `(TARGET_DIR)` contains subdirectory, than recursive flag of `-r` is required.
``` bash title="Compress with zip with subdirectories"
zip -r (FILENAME) (TARGET_DIR)
```

``` bash title="Unzip"
unzip (FILENAME)
unzip (FILENAME) -d (TARGET_DIR)
```

## `tar`

Flags
* `-z`: Compress with gzip(gz)
* `-j`: Compress with bzip2(bz)
* `-c`: **c**ollect to tar
* `-x`: e**x**tract tar
* `-f`: (Mandatory) Collects file
* `-v`: Verbosity
* `-p`: Compress with ownership

!!! question "What is the difference between `.tar` and `.tar.gz`?"
    `.tar` does NOT compress directories but rather collects all files to archive. Compressed `.tar` file becomes `.tar.gz`. This uses gunzip to compress files.

`(FILENAME)` contains `.tar`
``` bash title="Compress and decompress to tar"
# Compress
tar -cvf (FILENAME) (TARGETDIR)

# Decompress
tar -xvf (FILENAME)
```

``` bash title="Compress and decompress to tar.gz"
# Compress
tar -zcvf (FILENAME) (TARGETDIR)

# Decompress
tar -zxvf (FILENAME)
```

