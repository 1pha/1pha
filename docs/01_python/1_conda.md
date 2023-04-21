# Anaconda installation

- [Instructions](https://docs.anaconda.com/anaconda/install/linux/)
```bash
installsh=Anaconda3-2023.03-Linux-x86_64.sh
wget https://repo.anaconda.com/archive/${installsh}
shasum -a 256 ${installsh}
bash ${installsh}
```