# ODrive 实用工具

- [README](../README.md)
    - [ODrive 实用工具](./002_odrive-tool_cn.md)
        - [1 备份配置文件](#1-备份配置文件)
        - [2 Liveplotter](#2-Liveplotter)

ODrive 实用工具是 ODrive 的主机端程序包。主要用于提供一个对设备进行手动控制的交互式 shell 以及固件更新等实用工具。

## 1 备份配置文件

ODrive 实用工具提供对配置文件备份与恢复的功能，也可用于 ODrive 设备间的配置转移。

* 备份配置文件
```bash
odrivetool backup-config my_config.json
```
* 恢复配置文件
```bash
odrivetool restore-config my_config.json
```

## 2 Liveplotter
Liveplotter 是用于 ODrive 实时数据图像化的的工具。启动 Liveplotter 前，需要关闭其他 ODrive 进程实例。之后运行如下代码
```bash
odrivetool liveplotter
```
默认设置将显示驱动板连接的两组编码器反馈的位置信息。如需更改所需的显示参数，可以直接对 `odrivetool.py` 中 liveplotter 功能部分的脚本程序进行修改。`odrivetool.py` 的默认路径为 `/usr/local/bin/`，相关代码如下所示
```python
# If you want to plot different values, change them here.
# You can plot any number of values concurrently.
    cancellation_token = start_liveplotter(lambda: [
        my_odrive.axis0.encoder.pos_estimate,
        my_odrive.axis1.encoder.pos_estimate,
        ])
```

如需更改图像比例参数及采样率等设置，可以直接对 `utils.py` 中 liveplotter 功能部分的脚本程序进行修改。`utils.py` 的默认路径为 `/usr/local/lib/python3.5/dist-packages/odrive/`，相关代码如下所示
```
data_rate = 100
plot_rate = 10
num_samples = 1000
```

> <details><summary markdown="span">:warning:  调用 Liveplotter 过程可能出现报错</summary><div markdown="block">
>
> **报错信息**
> ```bash
> ImportError: No module named '_tkinter', please install the python3-tk package
> ```
> 
> **解决办法**
> ```bash
> sudo apt install python3-tk
> ```
> </div></details>

如需在 ODrive 进程实例中直接调用 Liveplotter，需用使用以下代码
```bash
start_liveplotter(lambda:[所需显示参数1，所需显示参数2...])
```