---
title: "6.2.1 車輛動態操作流程 (FoxPi_Driving_Ctrl)"
type: "docs"
weight: 1
---

# 6.2.1 FoxPi_Driving_Ctrl(user_input[])
寫入 DID 0x1001 (FoxPi_Driving_Ctrl)，用來車輛動態控制。
- Parameters: `user_input[14]`
    - `user_input[0]`: AccReq
    - `user_input[1]`: AccReq_A
    - `user_input[2]`: TargetSpd
    - `user_input[3]`: TargetSpd_A
    - `user_input[4]`: Angle_V
    - `user_input[5]`: Angle_Req
    - `user_input[6]`: Angle
    - `user_input[7]`: Torque_V
    - `user_input[8]`: Torque_Req
    - `user_input[9]`: Torque
    - `user_input[10]`: APSVMCReqA_flg
    - `user_input[11]`: APSStaSystem
    - `user_input[12]`: APSShiftPosnReq
    - `user_input[13]`: APSSpeedCMD
- Return type: Bytes (實際寫入的封包內容)，Error 時回傳 None。

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_write import FoxPiWriteDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiWriteDID(client)
    user_input = [-10, 1, 255.875, 1, 1, 1, -900, 1, 1, -10, 1, 4, 7, 20]
    resp = Foxpi.FoxPi_Driving_Ctrl(user_input)
    print(resp)
```

```text
b'\x00\x00d\x01\x00\x07\xff\x01\x01\x01\x00\x00\x00\x00\x01\x01\x00\x00\x00y\xa0'
```
{{% /details %}}

## **前置條件**
> [!CAUTION]
> 本流程為進入 FoxPi_Driving_Ctrl 的必要前置條件；未依流程操作將無法取得車輛控制權。

進行車輛動態訊號 (FoxPi_Driving_Ctrl)`控制時，需要先完成前置安全機制，請依下列步驟執行：
1. 將 [Ctrl_Enable_Switch]({{< relref "docs/api/FoxPi_write/#623-foxpi_ctrl_enable_switch" >}}) 內的 `Ctrl_Enable_Switch` 訊號寫入 `1` (開啟 FoxtronPi 控制許可)。
2. 將 [FoxPi_Driving_Ctrl 內所有訊號寫入 `FF`]({{< relref "docs/api/FoxPi_write/#624-driving_ctrl_toff" >}}) (做為寫入保護初始化)。
3. 將 FoxPi_Driving_Ctrl 內所有訊號寫入 `0`。
4. 將 [Ctrl_Enable_Switch]({{< relref "docs/api/FoxPi_write/#623-foxpi_ctrl_enable_switch" >}}) 內的 `Ctrl_Enable_Switch` 訊號寫入 `0`。
5. 再次將 `Ctrl_Enable_Switch` 寫入 `1` (進入可控狀態)。

> 操作以上流程時車輛檔位需為 P 檔。

> 流程完成後，FoxtronPi系統將持續保有車輛控制權；一旦車輛再次切回 P 檔(含 FoxPi_Driving_Ctrl 中檔位切換/重啟/重上電)，必須重新執行本流程，方可再次進入控制。

> [!CAUTION]

## 6.2.1.1 加減速度訊號操作流程
進行加速度控制前，必須先將驗證旗標 `AccReq_A=1` 拉起；旗標有效後，系統才會接受並執行命令。
- `AccReq`: 加減速度命令訊號 (-15 ~ 1.5 m/s²)
- `AccReq_A`: 加減速度命令驗證 (0 ~ 1)

>車輛處於禁止狀態時，Acc為 0。Acc為正值時，表示為加速度指令，出於安全考量，FoxtronPi系統最大可設定至 1.5 m/s²。Acc為負值時，表示為減速度指令，減速度最大可設定至 -15 m/s²。FoxtronPi系統對減速度無下限保護機制，因此最大可執行至 -15 m/s²。

> 由於 Acc 屬於車輛動態控制，初次使用時請勿一次設定過高的加速度值。建議以 0.05 m/s² 逐步遞增，以避免過度操作造成車輛動態過大，進而產生危險。

> 註: Acc與Acc_A兩者訊號可同時寫入，例如: 一次寫入 `AccReq_A=1`、`AccReq=0.05`，系統會執行加速度 0.05 m/s² 命令。

> 註: 如果此訊號寫入時，車輛當前檔位為P，請手動將檔位切換至D檔，系統才會執行此訊號。

## 6.2.1.2 目標速度訊號操作流程
進行目標速度控制前，必須先將驗證旗標 `TargetSpd_A=1` 拉起；旗標有效後，系統才會接受並執行命令。
- `TargetSpd`: 目標速度命令訊號 (0 ~ 170 km/h)
- `TargetSpd_A`: 目標速度命令驗證 (0 ~ 1)

> 由於 Spd 屬於車輛動態控制，初次使用時請勿設定過高的目標車速值。建議初次實驗參數值可設定 5-20 區間，以避免過度操作造成車輛動態過大，進而產生危險。

> 註: 本系統最大目標車速限制在170 km/h，如下超過170 km/h指令，僅會執行170 km/h。

> [!NOTE]
> 註: 若同時設定「目標車速」與「加 / 減速度」，系統會以加速度執行控制。因此，除非必要，建議優先使用「加 / 減速度」，避免混用。

## 6.2.1.3 方向盤角度控制流程
- `Angle_V`: 方向盤角度控制要求訊號的驗證 (0 ~ 1)
- `Angle_Req`: 要求 EPS 進入角度控制狀態的訊號 (0 ~ 1)
- `Angle`: 要求 EPS 轉動方向盤的目標角度位置 (-360 ~ 360 deg)

> 註: 方向盤角度控制目前僅支援 ±360°；若命令超出此範圍，系統將以 ±360° 為上限執行。

> 註: 角度控制時，轉向阻力會明顯增加，但駕駛仍可人工介入操作 (EPS會解離)。

1. 請先確認已完成 [**前置條件**](#前置條件)
2. 單獨寫入 `Angle_V=1`、`Angle_Req=0`、`Angle=0`
3. 等待200ms後，寫入 `Angle_V=1`、`Angle_Req=1`、`Angle=0`
4. 再等待200ms，同批寫入 `Angle_V=1`、`Angle_Req=1`、`Angle=<目標角度>`
    - 註: 當完成本步驟後，後續流程不再需要 200 ms 的等待，惟遇人工介入操作導致EPS解離，請重新2-4步驟。

> [!NOTE]
> 進行方向盤角度控制前，請先確認 `Torque_V`／`Torque_Req`／`Torque` 均未啟用(=0)；接著依下列步驟操作，否則系統將不接受也不執行方向盤角度命令。

> [!WARNING]
> 違反以下角度控制限制條件，EPS 將解離(不做動)
> - 方向盤角度命令與真實方向盤轉角相差超過 100 deg。
> - 方向盤轉角轉速超過 500 deg/s。
> - 轉角超過正負 450 deg。
> - 方向盤轉動慣性超過 3 Nm。

> [!WARNING]
> 如遇 EPS 解離，需先將 `Angle_V`、`Angle_Req`、`Angle` 都設為0後，再重新2-4步驟流程。

## 6.2.1.4 方向盤扭矩控制流程
- `Torque_V`: 方向盤扭力控制要求訊號的驗證 (0 ~ 1)
- `Torque_Req`: 要求 EPS 進入扭力控制狀態的訊號 (0 ~ 1)
- `Torque`: 要求 EPS 轉動方向盤扭力 (-3 ~ 3 Nm)

> 註: 扭力控制時，會有轉向阻力，但駕駛仍可人工介入操作

1. 請先確認已完成 [**前置條件**](#前置條件)
2. 單獨寫入 `Torque_V=1`／`Torque_Req=0`／`Torque=0`
3. 寫入 `Torque_V=1`／`Torque_Req=1`／`Torque=0`
4. 同批寫入 `Angle_V=1`、`Angle_Req=1`、`Angle=<目標扭矩>`

> [!NOTE]
> 進行方向盤扭矩控制前，請先確認 `Angle_V`／`Angle_Req`／`Angle` 均未啟用(=0)；接著依下列步驟操作，否則系統將不接受也不執行方向盤扭矩命令。

> [!WARNING]
> 違反以下扭矩控制限制條件，EPS 將解離(不做動)
> - 方向盤轉動慣性超過 3 Nm。
> - 扭矩命令超過 3 Nm。
> - 扭矩命令變化率超過 8 Nm/s。
> - 轉角超過正負 90deg 以上。

> [!WARNING]
> 如遇 EPS 解離，需先將 `Torque_V`、`Torque_Req`、`Torque` 都設為0後，再重新2-4步驟流程。

## 6.2.1.5 APS檔位控制流程
- `APSVMCReqA_flg`: APS 控制命令 VMC 執行應用需求 (0 ~ 1)
    - `0`: Non-Applicable, `1`: Applicable
- `APSStaSystem`: APS 控制狀態 (0 ~ 7)
    - `0`: Disable
    - `1`: PiRev (FoxPi保留)
    - `2`: Active
    - `3`: Failed
    - `4`: Remote Actived (不支援)
- `APSShiftPosnReq`: APS 檔位控制命令 (0 ~ 15)
    - `0`: Default
    - `1`: NA
    - `2`: P
    - `3`: NA
    - `4`: N
    - `5`: D
    - `6`: Failure
    - `7`: R
- `APSSpeedCMD`: 設定速度控制值 APS_Speed (0 ~ 7)

> [!NOTE]
> 進行APS檔位控制前，必須先確認 [允許檔位控制訊號讀取 (FoxPi_Shifter_allow)]({{< relref "docs/api/FoxPi_read#6113-foxpi_shifter_allow" >}}) 訊號值是否皆為1，再設定 `APSVMCReqA_flg=1`、`APSStaSystem=2`，系統才會接受並執行目標命令。

> [!TIP]
> 註: `APSSpeedCMD` 控制出於安全考量最大值為 7km/h，若需進行安全低速實驗，可使用 `APSSpeedCMD` 的速度控制功能來操作，當變速檔位為 R(倒車)時，車輛僅能透過此速度控制進行移動。
