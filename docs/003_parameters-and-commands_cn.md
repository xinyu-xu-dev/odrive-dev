# 基础参数与指令

- [README](../README.md)
    - [基础参数与指令](./003_parameters-and-commands_cn.md)
        - [1 基于各电机的独立指令](#1-基于各电机的独立指令)
        - [2 系统监控指令](#2-系统监控指令)
        - [3 通用系统指令](#3-通用系统指令)
        - [4 无反馈模式配置](#4-无反馈模式配置)

## 1 基于各电机的独立指令

多数情况下，ODrive 连接的两部电机可以实现独立控制。

### 1.1 状态机

|电机状态|代码值|
|:---:|:---:|
|AXIS_STATE_UNDEFINED|0|
|AXIS_STATE_IDLE|1|
|AXIS_STATE_STARTUP_SEQUENCE|2|
|AXIS_STATE_FULL_CALIBRATION_SEQUENCE|3|
|AXIS_STATE_MOTOR_CALIBRATION|4|
|AXIS_STATE_SENSORLESS_CONTROL|5|
|AXIS_STATE_ENCODER_INDEX_SEARCH|6|
|AXIS_STATE_ENCODER_OFFSET_CALIBRATION|7|
|AXIS_STATE_CLOSED_LOOP_CONTROL|8|
|AXIS_STATE_LOCKIN_SPIN|9|
|AXIS_STATEAXIS_STATE_LOCKIN_SPIN_LOCKIN_SPIN|10|

1. `AXIS_STATE_IDLE`
电机停止运行，关停 PWM 信号。
2. `AXIS_STATE_STARTUP_SEQUENCE`
运行启动程序。
3. `AXIS_STATE_FULL_CALIBRATION_SEQUENCE`
运行电机校正程序，随后进行编码器偏移校正。或当 `odrv0.axis0.encoder.config.use_index` 设定值为 `True` 时，运行编码器索引信号搜索。
4. `AXIS_STATE_MOTOR_CALIBRATION`
测量电机相电阻和相电感。
    * 运行以下代码保存校正结果。保存配置后，在下次设备启动后不再需要对电机再次进行校正；
    ```
    odrv0.axis0.motor.config.pre_calibrated = True
    ```
    * 该状态将对参数 `odrv0.axis0.motor.config.phase_resistance` 和 `odrv0.axis0.motor.config.phase_inductance` 进行整定。
5. `AXIS_STATE_SENSORLESS_CONTROL`
运行无反馈模式控制
    * 电机必须已通过校正，即参数 `odrv0.axis0.motor.is_calibrated` 应为 `True`；
    * `odrv0.axis0.controller.control_mode` 必须设定为 `True`。
6. `AXIS_STATE_ENCODER_INDEX_SEARCH`
驱动电机以某一方向进行运转直至遍历编码器索引信号。该状态只能通过运行以下代码进入
    ```bash
    odrv0.axis0.encoder.config.use_index = True
    ```
7. `AXIS_STATE_ENCODER_OFFSET_CALIBRATION`
驱动电机以某一方向持续运转几秒后再驱动电机返回，以此测量编码器位置反馈与电极间的偏差。
    * 电机必须已通过校正，即参数 `odrv0.axis0.motor.is_calibrated` 应为 `True`；
    * 编码器校正顺利完成后，参数 `odrv0.axis0.encoder.is_ready` 将被设定为 `True`。
8. `AXIS_STATE_CLOSED_LOOP_CONTROL`
运行闭环控制。
    * 具体行为取决于控制模式的设定；
    * 电机必须已通过校正，即参数 `odrv0.axis0.motor.is_calibrated` 应为 `True`，且编码器已就绪，即参数 `odrv0.axis0.encoder.is_ready` 应为 `True`。

### 1.2 启动程序

默认状态下，ODrive 在启动时不会对电机进行任何操作，电机状态将直接置为 `AXIS_STATE_IDLE`

如需更改启动程序，则需将以下相对应的启动程序参数设定为 `True`。ODrive 将依照以下次序依次启动相应已使能的启动程序。

1. `odrv0.axis0.config.startup_motor_calibration`
2. `odrv0.axis0.config.startup_encoder_index_search`
3. `odrv0.axis0.config.startup_encoder_offset_calibration`
4. `odrv0.axis0.config.startup_closed_loop_control`
5. `odrv0.axis0.config.startup_sensorless_control`

### 1.3 控制模式

默认的控制模式为位置控制。

如需更改控制模式，则运行以下代码
```
odrv0.axis0.controller.config.control_mode = 设定值
```
|电机控制模式|代码值|
|:---:|:---:|
|CTRL_MODE_VOLTAGE_CONTROL|0|
|CTRL_MODE_CURRENT_CONTROL|1|
|CTRL_MODE_VELOCITY_CONTROL|2|
|CTRL_MODE_POSITION_CONTROL|3|
|CTRL_MODE_TRAJECTORY_CONTROL|4|

可能用到的参数设定指令
```
odrv0.axis0.controller.pos_setpoint = 设定值
odrv0.axis0.controller.vel_setpoint = 设定值（计数/秒）
odrv0.axis0。controller.current_setpoint = 设定值（安培）
```

## 2 系统监控指令

### 2.1 编码器位置与速度反馈值

### 2.2 电机电流与力矩估计值

## 3 通用系统指令

### 3.1 保存配置

### 3.2 诊断指令

## 4 无反馈模式配置