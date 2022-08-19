
##### 2080TI训练服务器部署，配置要求
| 系统版本 | 驱动版本 | 
| :----:| :----: | 
| ubuntu 18.04.5 | nvidia-driver-515 | 

##### 操作系统安装包：
1. https://old-releases.ubuntu.com/releases/18.04.5/ubuntu-18.04.5-desktop-amd64.iso

##### 参考文档：
1. https://blog.csdn.net/weixin_39275295/article/details/108158498

##### cuda安装包:
1.https://developer.nvidia.com/cuda-11-0-2-download-archive

##### cudnn安装包：
1. https://developer.nvidia.cn/rdp/cudnn-archive
选择含有11.2的三个deb包
<img width="1345" alt="image" src="https://user-images.githubusercontent.com/18545145/185547490-066be375-64df-401e-83af-a330c316a6ac.png">

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

###### 3.安装NVIDIA驱动
```shell
1.使用 ubuntu-drivers devices 命令查看当前系统推荐nvidia官方驱动


ubuntu-drivers devices

== /sys/devices/pci0000:00/0000:00:01.1/0000:02:00.0 ==
modalias : pci:v000010DEd00001E07sv00001458sd000037A9bc03sc00i00
vendor   : NVIDIA Corporation
driver   : nvidia-driver-470-server - distro non-free
driver   : nvidia-driver-418-server - distro non-free
driver   : nvidia-driver-515 - distro non-free
driver   : nvidia-driver-470 - distro non-free
driver   : nvidia-driver-510 - distro non-free
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-510-server - distro non-free
driver   : nvidia-driver-515-server - distro non-free recommended
driver   : xserver-xorg-video-nouveau - distro free builtin

2.选择推荐的版本（选择不带server因为我们那是ubuntu-desktop版本的，最高版本的驱动即这里的 nvidia-driver-515）

apt install nvidia-driver-515 -y

```
###### 4.安装指定版本的cuda
```shell
//通过deb包安装
sudo cp cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo dpkg -i cuda-repo-ubuntu1804-11-2-local_11.2.2-460.32.03-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu1804-11-2-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda

//通过run文件安装（推荐）
sudo chmod +x cuda_11.2.2_460.32.03_linux.run
sudo sh cuda_11.2.2_460.32.03_linux.run

//添加环境变量（对root用户）
vi ~/.bashrc

export PATH=$PATH:/usr/local/cuda-11.2/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.2/lib64
export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/cuda-11.2/lib64

source ~/.bashrc

```

###### 5.安装cudnn
```shell
1.安装参考链接
https://blog.chintsan.com/archives/561

2.FAQ参考链接
https://blog.csdn.net/xhw205/article/details/116297555

sudo dpkg -i libcudnn8_8.1.1.33-1+cuda11.2_amd64.deb 
sudo dpkg -i libcudnn8-dev_8.1.1.33-1+cuda11.2_amd64.deb 
sudo dpkg -i libcudnn8-samples_8.1.1.33-1+cuda11.2_amd64.deb

3.验证
cp -r /usr/src/cudnn_samples_v8 ~
cd ~/cudnn_samples_v8/mnistCUDNN
make clean
make
./mnistCUDNN


```

