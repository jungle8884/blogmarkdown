---
title: ESP32AT指令集探索
categories:
  - IOT
  - ESP32
tags:
  - ESP32
  - AT
author:
  - Jungle
date: 2023-03-08 14:52:56
---

# ESP32AT指令集探索

## 基本指令

> 官网：[基础 AT 命令集 - ESP32 - — ESP-AT 用户指南 latest 文档 (espressif.com)](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html)

- [AT](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-at)：测试 AT 启动
- [AT+RST](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-rst)：重启模块
- [AT+GMR](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-gmr)：查看版本信息
- [AT+CMD](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-cmd)：查询当前固件支持的所有命令及命令类型
- [AT+GSLP](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-gslp)：进⼊ Deep-sleep 模式
- [ATE](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-ate)：开启或关闭 AT 回显功能
- [AT+RESTORE](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-restore)：恢复出厂设置
- [AT+SAVETRANSLINK](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-savet)：设置开机 [透传模式](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/index_of_abbreviations.html#term-6) 信息
- [AT+TRANSINTVL](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-transintvl)：设置 [透传模式](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/index_of_abbreviations.html#term-6) 模式下的数据发送间隔
- [AT+UART_CUR](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-uartc)：设置 UART 当前临时配置，不保存到 flash
- [AT+UART_DEF](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-uartd)：设置 UART 默认配置, 保存到 flash
- [AT+SLEEP](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sleep)：设置 sleep 模式
- [AT+SYSRAM](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysram)：查询当前剩余堆空间和最小堆空间
- [AT+SYSMSG](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysmsg)：查询/设置系统提示信息
- [AT+SYSMSGFILTER](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysmsgfilter)：启用或禁用 [系统消息](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/index_of_abbreviations.html#term-8) 过滤
- [AT+SYSMSGFILTERCFG](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysmsgfiltercfg)：查询/配置 [系统消息](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/index_of_abbreviations.html#term-8) 的过滤器
- [AT+SYSFLASH](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysflash)：查询或读写 flash 用户分区
- [AT+SYSMFG](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysmfg)：查询或读写 [manufacturing nvs](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/index_of_abbreviations.html#term-manufacturing-nvs) 用户分区
- [AT+RFPOWER](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-rfpower)：查询/设置 RF TX Power
- [AT+SYSROLLBACK](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysrollback)：回滚到以前的固件
- [AT+SYSTIMESTAMP](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-settime)：查询/设置本地时间戳
- [AT+SYSLOG](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-syslog)：启用或禁用 AT 错误代码提示
- [AT+SLEEPWKCFG](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-wkcfg)：设置 Light-sleep 唤醒源和唤醒 GPIO
- [AT+SYSSTORE](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysstore)：设置参数存储模式
- [AT+SYSREG](https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32/AT_Command_Set/Basic_AT_Commands.html#cmd-sysreg)：读写寄存器