# 通信接口

- [README](../README.md)
    - [通信接口](./004_interfaces_cn.md)
        - [1 引脚排列](#1-引脚排列)
        - [2 本机协议](#2-本机协议)
        - [3 ASCII 协议](#3-ASCII-协议)
        - [4 步进/方向](#4-步进/方向)
        - [5 RC PWM 输入](#5-RC-PWM-输入)
        - [6 端口](#6-端口)

## 1 引脚排列

|通用输入输出口|PWM|主功能|步进/方向|其他|
|:---:|:---:|:---:|:---:|:---:|
|GPIO1|PWM 输入|**UART TX**|电机0 步进信号|模拟输入|
|GPIO2|PWM 输入|**UART RX**|电机0 方向信号|模拟输入|
|GPIO3|PWM 输入|||模拟输入|
|GPIO4|PWM 输入|||模拟输入|
|GPIO5||||模拟输入|
|GPIO6||||数字输入|
|GPIO7|||电机1 步进信号|数字输入|
|GPIO8|||电机1 方向信号|数字输入|

### 1.1 引脚功能优先级

1. PWM, 默认关闭
2. UART，**默认开启**
3. 步进/方向，默认关闭
4. 模拟信号
5. 数字信号

### 1.2 模拟输入

模拟输入信号可用于测量 0 至 3.3 V 的电压值。 ODrive 驱动板使用 12 位分辨率的模拟数字转换器。测量值范围为 0 至 4095。因此电压的分辨率为 0.8 mV。

### 1.3 霍尔模式引脚排列

## 2 本机协议

该协议为 ODrive 工具包与 ODrive 驱动板通信所使用的协议。可通过 USB 或 UART 接口运行。

## 3 ASCII 协议

基于本机协议的简化版本。可通过 USB 或 UART 接口运行。

### 3.1 Arduino

[Arduino Library](https://github.com/madcowswe/ODrive/tree/master/Arduino/ODriveArduino)

## 4 步进/方向

```
odrv0.config.enable_uart = False
odrv0.axis0.config.enable_step_dir = True
```

## 5 RC PWM 输入

## 6 端口

### 6.1 USB

### 6.2 UART

Baud rate: 115,200