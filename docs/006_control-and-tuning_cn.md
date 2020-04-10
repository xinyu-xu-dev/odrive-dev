# 控制与整定

- [README](../README.md)
    - [控制与整定](./006_control-and-tuning_cn.md)
        - [1 控制系统](#1-控制系统)
        - [2 系统整定](#2-系统整定)

## 1 控制系统

电机控制器采用级联式（位置-速度-电流）控制回路。

### 1.1 位置环

位置控制器只采用比例控制。
```
pos_error = pos_setpoint - pos_feedback
vel_cmd = pos_error * pos_gain + vel_feedforward
```

### 1.2 速度环

速度控制器采用比例-积分控制。
```
vel_error = vel_cmd - vel_feedback
current_integral += vel_error * vel_integrator_gain
current_cmd = vel_error * vel_gain + current_integral + current_feedforward
```

### 1.3 电流环

电流控制器采用比例-积分控制。
```
current_error = current_cmd - current_fb
voltage_integral += current_error * current_integrator_gain
voltage_cmd = current_error * current_gain + voltage_integral (+ voltage_feedforward when we have motor model)
```

## 2 系统整定

* `odrv0.axis0.controller.config.pos_gain = 设定值`
* `odrv0.axis0.controller.config.vel_gain = 设定值`
* `odrv0.axis0.controller.config.vel_integrator_gain = 设定值`