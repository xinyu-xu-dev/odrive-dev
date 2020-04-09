基础硬件配置
===
元件清单
---
|元件|描述|数据表|数量|
|:---:|:---:|:---:|:---:|
|电源|24V||1|
|电机驱动|ODrive v3.6|[link](https://odriverobotics.com/shop/odrive-v36)|1|
|双轴电机|D5065|[link](https://odriverobotics.com/shop/odrive-custom-motor-d5065)|1|
|电机壳|NEMA 23 for D5065|[link](https://discourse.odriverobotics.com/t/nema-enclosures-for-d5065-and-d6374-motors/830)|1|
|电机壳面板|aluminium|[link](https://odriverobotics.com/shop/nema23-faceplate-for-d5065-motor)|1|
|编码器|CUI AMT102-V|[link](https://odriverobotics.com/shop/cui-amt-102)|1|
|热缩管|||3|
|紧固件 1|M3 8mm for encoder to enclosure||2|
|紧固件 2|M3 8mm for plate to enclosure||4|
|紧固件 3|M4 8mm for plate to motor||4|

工具清单
---
|工具|描述|数据表|数量|
|:---:|:---:|:---:|:---:|
|||||

基础硬件搭设
===

### 连接电机

### 连接编码器

### 连接电机驱动


配置调试系统
===
下载安装 ODrive 工具包
---

## Linux
以 Ubuntu 16.04 系统为例

1. 安装 Python 3
```
sudo apt install python3
sudo apt install python3 python3-pip
```
2. 安装 ODrive 工具包
```
sudo pip3 install odrive
```
> 安装过程可能出现报错
>
> **报错信息**
>
> ```
> GET ERROR: Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-4w6I54yu/matplotlib/
> ```
> 报错原因可能是由于当前 `setuptools` 版本过旧
>
> **解决办法**
> ```
> pip install --upgrad setptools
> ```
3. 查看设备管理器规则文件是否已自动更新，否则手动添加
```
 echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="1209", ATTR{idProduct}=="0d[0-9][0-9]", MODE="0666"' | sudo tee /etc/udev/rules.d/91-odrive.rules
 sudo udevadm control --reload-rules
 sudo udevadm trigger
```
4. 在 bash 中添加 odrivetool 的路径
```
echo "PATH=$PATH:~/.local/bin/" >> ~/.bashrc
```


基础功能调试
===