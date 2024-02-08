# Data Version Control

## Setups
현재 나의 경우 `ssh` 세팅을 따르고 있다. 문제는 /mnt에 곧바로 박고 싶지만 자꾸 permission error가 나서 해결하지 못하고 우선은 도커 내에 박아놨다. 이거 수정해야할 것 같다 (도커 날아가면 다 날아감). 혹은 중간에 하루에 한 번 싱크하는 코드를 짜거나 ...
```bash
conda create -n dvc python=3.10
conda activate dvc
pip install dvc dvc_ssh
```
- [SSH Remote setup](https://dvc.org/doc/user-guide/data-management/remote-storage/ssh)

처음 Repository를 설정하면 ssh의 경우 비밀번호 설정형태라서 최초 한 번은 비밀번호를 걸어줘야한다. 다음 중 하나를 해주자. 근데 fetch에서는 비밀번호를 물어보는 설정을 해도 잘 안된다. 그냥 비번 저장하는 식으로 가야한다. 어차피 `config.local`은 원격에 저장 안되니까 걱정안하고 진행해도 괜찮다.
```shell
dvc remote modify --local main password (비밀번호)
# or
dvc remote modify main ask_password true
```

## Basic pull/push
기본적으로 아래 포맷으로 진행된다. Documentation에 따르면 `dvc pull` 하나로 해결되어야 하는데, 그렇게 되질 않는다. 공홈에 가면 `dvc pull`=`dvc fetch` + `dvc checkout`이라고 하길레 `dvc pull`을 한 후에 `dvc pull`을 진행했더니 잘 돌아간다.
```bash
conda activate dvc
git pull origin main
dvc fetch --all-commits
dvc pull
```
- 잊지말고 `git pull origin main`도 진행해줘야 한다. `.dvc` 파일의 업데이트가 먼저 일어나야 `dvc`도 원격 리모트의 상태를 트래킹하고 받아올 수 있음.
> [!question] + What was the error?
> ![[Pasted image 20230918150401.png]]

