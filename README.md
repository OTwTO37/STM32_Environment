# 智能温湿度检测系统

基于 STM32 + OneNET + MQTT 的物联网环境监测系统，实现温湿度数据采集、云端上传、远程控制及本地显示功能。

## 功能概述

1. 温湿度采集：DHT11 传感器实时采集环境温湿度
2. 本地显示：OLED 屏幕显示当前数据及多级菜单交互
3. 云端上传：通过 ESP8266 连接 OneNET 平台，MQTT 协议上报数据
4. 远程控制：微信小程序端控制 LED、蜂鸣器报警
5. 断线重连：MQTT 断线自动重连，24h 丢包率 < 5%

## 硬件平台

| 组件 | 型号 | 说明 |
|------|------|------|
| 主控 | STM32F103C8T6 | Cortex-M3 核心 |
| 温湿度传感器 | DHT11 | 单总线数字输出 |
| 无线通信 | ESP8266-01S | WiFi 透传模快 |
| 显示屏 | OLED 0.96寸 | I2C 接口，128x64 |
| 执行器 | LED、蜂鸣器 | GPIO 控制 |


## 快速开始

### 1. 硬件连接

| STM32 引脚 | 外设 |
|-----------|------|
|PA9/PA10|USART TX/RX|
| PA2/PA3 | ESP8266 UART  RX/TX|
| PB8/PB9 | OLED I2C SCL/SDA |
| PA11 | DHT11 Data |
| PA1 | LED |
| PB15 | 蜂鸣器 |
|PB12/PB13/PB14|按键*3|

### 2. 环境配置

1. IDE：Keil uVision 5
2. 库：STM32 标准外设库
3. 烧录：ST-Link / 串口

### 3. OneNET 配置

1. 注册 OneNET 账号，创建 MQTT 产品
2. 获取设备 ID、产品 ID、API Key
3. 在代码中替换对应宏定义

### 4. 编译与烧录
1. Keil 中编译
2. 使用 ST-Link烧录 .hex 文件

## 测试结果

| 测试项 | 预期结果 | 实际结果 |
|------|------|------|
|温湿度采集精度	|误差 ±0.5℃	| 通过|
|MQTT 断线重连	|WiFi 恢复后 10s 内重连	| 通过|
|远程控制响应|	延迟 < 1s	| 通过|
|24h 连续运行|	丢包率 < 5%	| 通过|

## 项目结构

├── Main/
│   ├── main.c
│   ├── stm32f10x_it.c

├── APP/
│   ├── bsp_dht11.c/h
│   ├── bsp_oled.c/h

|——BSP/
│   ├── bsp_Alarm.c/h
│   ├── bsp_delay.c/h
│   ├── bsp_key.c/h
│   ├── bsp_led.c/h
│   ├── bsp_timer.c/h
│   ├── bsp_usart.c/h

├── Onenet/
│   ├── Mqttkit.c/h
│   ├── esp8266.c/h
│   ├── onenet.c/h
│   ├── CJSON.c/h


├── Project/
│   └── *.uvprojx
└── README.md

## 待优化项

1. 增加 WiFi 配网功能（目前 SSID/密码写死）
2. 支持更多传感器（烟雾、光照）
3. 增加本地数据存储（EEPROM 保存阈值）

## 作者

姓名：谈艳芬
学校：中国矿业大学 电子信息工程 2027届
联系方式：15210125079 / 2777392748@qq.com
