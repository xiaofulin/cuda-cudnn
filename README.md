###### 1.安装编译环境
```shell
sudo apt-get install build-essential -y
```

###### 2.关闭 nouveau
```shell
lsmod | grep nouveau

//备份系统黑名单
sudo cp /etc/modprobe.d/blacklist.conf /etc/modprobe.d/blacklist.conf_backup

vi /etc/modprobe.d/blacklist.conf

//在系统黑名单配置文件最下面添加以下代码
# nouveau
blacklist nouveau
options nouveau modeset=0

//更新配置信息使其生效
sudo update-initramfs -u

//重启一下ubuntu
reboot

lsmod | grep nouveau #无返回结果说明禁用成功

```

###### 3.安装NVIDIA官方下载驱动 NVIDIA-Linux-x86_64-515.65.01.run
```shell

sudo chmod +x NVIDIA-Linux-x86_64-515.65.01.run
./NVIDIA-Linux-x86_64-515.65.01.run

```
###### 4.安装指定版本的cuda
```shell
//通过deb包安装
sudo cp cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo dpkg -i cuda-repo-ubuntu1804-11-2-local_11.2.2-460.32.03-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu1804-11-2-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda

//通过run文件安装
sudo chmod +x cuda_11.2.2_460.32.03_linux.run
sudo sh cuda_11.2.2_460.32.03_linux.run

//添加环境变量（对root用户）
vi ~/.bashrc

export PATH=$PATH:/usr/local/cuda-11.2/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.2/lib64
export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/cuda-11.2/lib64

```
