# Environment Setup

This section covers step by step guidance for Environment Setup.

## Docker Setup:
- Login to Ubuntu 22.04 system
```shell script
ssh -i ~/.ssh/bsp root@XXX.XXX.XX.XX
```

- Using root user
```shell script
sudo su -
```

- Install docker components
```shell script
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
apt-get install docker-compose-plugin
sudo docker run hello-world
```

## Generate SSH-key
- Generate key from Ubuntu system
```shell script
ssh-keygen -t rsa
cd ~/.ssh/
ls
cat id_rsa.pub
```

## Setup new key for GitHib in web
- Step 1
![Step 1](/env-setup/ssh1.png)

- Step 2
![Step 2](/env-setup/ssh2.png)

- Step 3
![Step 3](/env-setup/ssh3.png)

- Step 4
![Step 4](/env-setup/ssh4.png)