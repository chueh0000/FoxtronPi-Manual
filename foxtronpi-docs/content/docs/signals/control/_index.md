---
title: "6. 控制訊號操作流程與限制條件"
type: "docs"
weight: 6
---

# 6. 控制訊號操作流程與限制條件

在進行控制操作前，請務必了解操作流程與安全限制。

## 6.1 控制啟用流程 (FoxPi_Ctrl_Enable_Switch)
> [!CAUTION]
> 當需進行 FoxtronPi 訊號控制時，必須先將 Ctrl_Enable_Switch 旗標設定為 `1`，系統才會允許進行車輛控制與訊號讀取作業。該旗標作為啟用控制功能的前置條件，若未正確設定，所有控制操作將被系統拒絕執行。

## [6.2 車輛動態操作流程 (FoxPi_Driving_Ctrl)]({{< relref "docs/signals/control/FoxPi_Driving_Ctrl" >}})

## [6.3 燈光控制操作流程 (FoxPi_Lamp_Ctrl)]({{< relref "docs/signals/control/FoxPi_Lamp_Ctrl" >}})