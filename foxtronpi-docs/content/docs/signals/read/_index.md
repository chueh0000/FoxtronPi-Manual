---
title: "5. FoxtronPi 讀取訊號"
type: "docs"
weight: 5
---

# 5. FoxtronPi 讀取訊號

本章節介紹如何透過 FoxtronPi 讀取各類車輛訊號。

## 5.1 控制啟用切換訊號 (FoxPi_Ctrl_Enable_Switch)
- **訊號名稱**: `Ctrl_Enable_Switch`
- **範圍**: 0 (關閉) ~ 1 (啟用)

## 5.2 車輛動態控制訊號 (FoxPi_Driving_Ctrl)
- **Acc**: 加減速度命令 (-15 ~ 1.5 m/s²)
- **Spd**: 目標車速命令 (0 ~ 170 km/h)
- **Angle**: 轉向角度位置 (-360 ~ 360 deg)
- **Torque**: 轉向扭力 (-3 ~ 3 Nm)
- **APS_Shift**: 檔位控制命令 (0 ~ 15)

## 5.3 車輛燈光控制訊號 (FoxPi_Lamp_Ctrl)
包含位置燈、近光燈、遠光燈、晝行燈、方向燈、煞車燈、倒車燈、後霧燈及氣氛燈等。

## 5.4 車輛速度訊號 (FoxPi_Motion_Status)
- **VehicleSpeed**: 車速 (0 ~ 255.875 km/h)
- **LongAcc**: 縱向加速度 (-1.27 ~ 1.27 G's)
- **LatAcc**: 橫向加速度 (-1.27 ~ 1.27 G's)
- **YawRate**: 偏航率 (-100 ~ 100 Deg/s)

## 5.5 煞車狀態訊號 (FoxPi_Brake_Status)
- **Brake_Sw**: 煞車踏板開關 (0: 未踩, 1: 踩下)
- **MCPressure**: 總泵壓力 (-9.7 ~ 195 Bar)

## 5.6 輪速訊號讀取 (FoxPi_WheelSpeed)
可讀取四個輪子的輪速資訊。

## 5.7 角度、扭力回饋與檢查 (FoxPi_EPS_Status)
- **SAS_Angle**: 方向盤角度回饋 (-360 ~ 360 deg)
- **EPS_AOI_Ctrl**: 角度控制狀態 (0: Control Inhibit, 1: Available, 2: Controlled, 3: Permanent Fail)

## 5.8 方向盤按鈕狀態 (FoxPi_Button_Status)
可讀取方向盤上各類按鍵 (ACC, CANCEL, SET, RESUME, Mode, Up/Down/Left/Right 等) 的狀態。

## 5.9 馬達狀態訊號 (FoxPi_Motor_Status)
- **TqSource**: 扭力來源
- **TMSpd**: 馬達轉速 (-17000 ~ 17000 rpm)

## 5.11 電池系統狀態 (FoxPi_Battery_Status)
- **LVBatt12V**: 12V 電池電壓
- **HVBattSOC**: 高壓電池電量 (0 ~ 100 %)
- **HVBattTemp**: 高壓電池溫度

## 5.12 踏板訊號讀取 (FoxPi_Pedal_position)
- **AccelPedalPos**: 油門踏板開度 (0 ~ 100 %)
- **BrakePedalPos**: 煞車踏板開度 (0 ~ 100 %)
