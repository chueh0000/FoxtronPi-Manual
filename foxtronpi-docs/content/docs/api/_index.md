---
title: "8. FoxtronPi Library (API) 說明"
type: "docs"
weight: 8
---

# 8. FoxtronPi Library 功能與介面 (API) 說明

本章詳細介紹 FoxtronPi Python Library 的各類 API 接口。

## 8.1 FoxPi_read
`FoxPi_read` 類包含讀取各類訊號的方法。

### 8.1.1 FoxPi_Driving_Ctrl()
- **功能**: 讀取加減速、車速、角度與扭矩命令值。
- **回傳值類型**: `Dict[str, Union[int, float, str]]`
- **範例欄位**: `Acc`, `Spd`, `Angle`, `Torque`, `APS_Shift`

### 8.1.2 FoxPi_Motion_Status()
- **功能**: 讀取車速、縱向/橫向加速度、偏航率等狀態。
- **範例欄位**: `VehicleSpeed`, `LongAcc`, `YawRate`

### 8.1.3 FoxPi_Brake_Status()
- **功能**: 讀取煞車狀態與總泵壓力。
- **範例欄位**: `Break_Sw`, `MCPressure`

### 8.1.5 FoxPi_EPS_Status()
- **功能**: 讀取 EPS 狀態與方向盤角度。
- **範例欄位**: `SAS_Angle`, `EPS_AOI_Ctrl`

## 8.2 FoxPi_write
`FoxPi_write` 類包含寫入控制訊號的方法。

### 8.2.1 FoxPi_Driving_Ctrl(user_input [])
- **功能**: 寫入車輛動態控制指令。
- **參數**: `user_input` [長度 14] 包含 Acc, Spd, Angle, Torque, APS_Shift 等索引值。

### 8.2.2 Driving_Ctrl_toFF()
- **功能**: 將所有動態控制位元寫入 0xFF (保護機制)。

### 8.2.3 FoxPi_Lamp_Ctrl(user_input [])
- **功能**: 寫入燈光控制指令。
- **參數**: `user_input` [長度 25]。

## 8.3 FoxPi_DTC
提供診斷故障碼的讀取與清除功能。

### 8.3.1 Read_DTCs()
- **功能**: 讀取車輛目前的所有 DTC。

### 8.3.2 Clear_DTCs()
- **功能**: 清除所有 DTC。

## 8.4 FoxPi_TP
提供 Tester Present 會話維持功能，避免斷線。
