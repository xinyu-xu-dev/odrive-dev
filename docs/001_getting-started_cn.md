# 快速入门

## 基础硬件配置

### 元件清单
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

### 工具清单
|工具|描述|数据表|数量|
|:---:|:---:|:---:|:---:|
|||||

## 基础硬件搭设

### 连接电机

### 连接编码器

### 连接电机驱动


## 下载安装 ODrive 工具包

### Linux
以 Ubuntu 16.04 系统为例

1. 安装 [Python 3](https://www.python.org/downloads/)
```bash
sudo apt install python3
sudo apt install python3 python3-pip
```
2. 安装 ODrive 工具包
```bash
sudo pip3 install odrive
```
> :warning:
>
> 安装过程可能出现报错
>
> **报错信息**
>
> ```bash
> GET ERROR: Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-4w6I54yu/matplotlib/
> ```
> 报错原因可能是由于当前 `setuptools` 版本过旧
>
> **解决办法**
> ```bash
> pip install --upgrade setptools
> ```
3. 查看设备管理器规则文件是否已自动更新，否则手动添加
```bash
 echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="1209", ATTR{idProduct}=="0d[0-9][0-9]", MODE="0666"' | sudo tee /etc/udev/rules.d/91-odrive.rules
 sudo udevadm control --reload-rules
 sudo udevadm trigger
```
4. 在 bash 中添加 odrivetool 的路径
```bash
echo "PATH=$PATH:~/.local/bin/" >> ~/.bashrc
```

## 启动 `odrivetool` 工具包
---
开启新终端窗口，键入以下代码启动 ODrive 的主交互工具界面
```bash
odrivetool
```
<img src="./images/image_001-01.png" width="60%">

通过 USB 线缆连接 ODrive 电机驱动至主机，等待终端返回确认已连接 ODrive 的信息
```markdown
Connected to ODrive SERIAL_NUMBER as odrv0
```
<img src="./images/image_001-02.png" width="60%">

## 基础命令行

### 查看驱动板供电电压
```
odrv0.vbus_voltage
```

### 设定参数极限

`设定类命令行 = 设定值`

**电流极限**
```
odrv0.axis0.motor.config.current_lim = 设定值(A)
```
出于安全原因，初始默认值设定为 10A。该设定无法实现强劲的性能，但足以用于确认电机驱动稳定状态。当确认 ODrive 运行成功后，该值可升至 60A 以提高性能。

当设定值高于 60A 时，需要通过修改电流范围的命令以修改电流放大器增益。

**电流范围**
```
odrv0.axis0.motor.config.requested_current_range
```
初始默认值设定为 60A。

**速度极限**
```
odrv0.axis0.controller.config.vel_limit = 设定值(每秒计数)
```
初始默认值设定为 20000。

**校定电流**
```
odrv0.axis0.motor.config.calibration_current = 设定值(A)
```
初始默认值设定为 10A。

### 设置其他硬件参数

`设定类命令行 = 设定值`

**制动电阻阻值**
```
odrv0.config.brake_resistance = 设定值(Ohm)
```
初始默认值设定为 0.5Ohm。

**电机极对数**
```
odrv0.axis0.motor.config.pole_pairs = 设定值(对数)
```
初始默认值设定为 7。

**电机类型**
```
odrv0.axis0.motor.config.motor_type = 设定值
```
初始默认值设定为 MOTOR_TYPE_HIGH_CURRENT。

**伺服器精度**
```
odrv0.axis0.encoder.config.cpr = 设定值(每转计数)
```
初始默认值设定为 8192。

### 保存参数设定

```
odrv0.save_configuration()
```

```
odrv0.reboot()
```

## 电机的位置控制


## 其他控制模式

## 监视时钟

```
sudo apt install python3-tk
```