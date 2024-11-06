# Environment Setup

This section covers step by step guidance for Environment Setup.

## Docker Setup:
- Login to Ubuntu 22.04 system
```console
ssh -i ~/.ssh/bsp root@XXX.XXX.XX.XX
```

- Using root user
```console
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
```console
ssh-keygen -t rsa
cd ~/.ssh/
ls
```

- Use this key for next GitHub setup
```console
cat id_rsa.pub
```

## Setup new key for GitHub in web
- Step 1

<kbd>![Step 1](/env-setup/ssh1.png)<kbd>

- Step 2

<kbd>![Step 2](/env-setup/ssh2.png)<kbd>

- Step 3

<kbd>![Step 3](/env-setup/ssh3.png)<kbd>

- Step 4

<kbd>![Step 5](/env-setup/ssh4.png)<kbd>
	
## Setup Java
- Setup
```console
apt install openjdk-11-jdk
```

- Check	
```console
java-version
```	

## (Optional) Setup Local Spark
- Prepare folder
```console
mkdir ~/spark
cd ~/spark/
wget https://dlcdn.apache.org/spark/spark-3.5.3/spark-3.5.3-bin-hadoop3.tgz
tar -xvzf spark-3.5.3-bin-hadoop3.tgz 
vi ~/.bashrc 
```

- Vi Content
```console
export SPARK_HOME=~/spark/spark-3.5.3-bin-hadoop3
export PATH=$SPARK_HOME/bin:$PATH
export PATH=$SPARK_HOME/sbin:$PATH
```

- Check
```console
source ~/.bashrc 
spark-shell 
exit
```

### <p align="center">[[Previous] README](../README.md)									[[Next] Oracle Database Setup](../oracle-database/oracle-db.md)</p>