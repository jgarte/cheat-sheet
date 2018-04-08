# Sandro's cheat sheet

## Table of Contents
* [Custom kernel](#custom-kernel)
* [Docker](#docker)
  * [Master](#master)
  * [Slave](#slave)
* [Kubernetes](#kubernetes)
* [Misc](#misc)
  * [ASF](#asf)
  * [Gitea](#gitea)
  * [Linux](#linux)
  * [Music](#music)
  * [Yunohost](#yunohost)

## Docker
````
sudo curl -sSL https://get.docker.com | sh
````

#### Master
````
docker swarm init
````

#### Slave
````
docker swarm join --token TOKEN
````


## Custom kernel
````
sudo SKIP_KERNEL=1 rpi-update

cd linux
KERNEL=kernel7
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2709_defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig

make -j6 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage modules dtbs

mkdir mnt
mkdir mnt/fat32
mkdir mnt/ext4
sudo mount /dev/sdc1 mnt/fat32
sudo mount /dev/sdc2 mnt/ext4

sudo env "PATH=$PATH" make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- INSTALL_MOD_PATH=mnt/ext4 modules_install

sudo cp arch/arm/boot/zImage mnt/fat32/kernel7-ksm.img
sudo cp arch/arm/boot/dts/*.dtb mnt/fat32/
sudo cp arch/arm/boot/dts/overlays/*.dtb* mnt/fat32/overlays/
sudo cp arch/arm/boot/dts/overlays/README mnt/fat32/overlays/
sudo umount mnt/fat32
sudo umount mnt/ext4
````


## Kubernetes
````
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
apt-get update && apt-get install -y kubeadm
````


## Misc

#### ASF
````
sudo apt-get install libunwind8
````

#### Gitea
````
mysql -u root -p
mysql> CREATE DATABASE `gitea` DEFAULT CHARACTER SET `utf8mb4` COLLATE `utf8mb4_general_ci`;
mysql> CREATE USER `gitea`@'localhost' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON `gitea`.* TO `gitea`@`localhost`;
mysql> \q
````

#### Linux
````
passwd -l username
usermod -aG sudo username
mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys

sudo nano /etc/bash.bashrc
  export LC_ALL=en_GB.UTF-8
  export LANG=en_GB.UTF-8
  export LANGUAGE=en_GB.UTF-8
````

#### Music
````
youtube-dl.exe -f m4a --add-metadata -o "%(title)s.%(ext)s" https://www.youtube.com/playlist?list=PLIeMGRbbjabyKx_z0yWcjLCNCgVpDGtFW
````

#### Yunohost
````
pkill -KILL -u pi
bash <(wget -q -O- https://install.yunohost.org/stretch)
yunohost app ssowatconf
````
