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
- 