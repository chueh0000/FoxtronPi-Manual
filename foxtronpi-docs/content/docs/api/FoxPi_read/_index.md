---
title: "6.1 FoxPi_read"
type: "docs"
weight: 1
---

# 6.1 FoxPi_read
本章節介紹如何透過 FoxtronPi 讀取各類車輛訊號。

建立 FoxtronPi DID 讀取器。

```python
class FoxPiReadDID(client):
```

- `client`: (udsoncan.client.Client) 已開啟的 UDS 客戶端，用於發送 ReadDataByIdentifier 等請求。
- Raises: 任何由 client.read_data_by_identifier() 拋出的通訊錯誤(如逾時、負回應)。

## 6.1.1 FoxPi_Driving_Ctrl()
讀取 DID 0x1001 (FoxPi_Driving_Ctrl)，並解析為實體值。

自動套用 factor/offset 計算與 FF 無效機制，若底層資料為 `0xFF`，回傳字串 "FF" 表示該欄位無效/預設值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "Acc": float | "FF", 
    "Acc_A": int | "FF", 
    "Spd": float | "FF", 
    "Spd_A": int | "FF", 
    "Angle_V": int | "FF", 
    "Angle_Req": int | "FF", 
    "Angle": float | "FF", 
    "Torque_V": int | "FF", 
    "Torque_Req": int | "FF",
    "Torque": float | "FF", 
    "APS_flg": int | "FF", 
    "APS_Sta": int | "FF", 
    "APS_Shift": int | "FF", 
    "APS_Spd": float | "FF"
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Driving_Ctrl()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
Acc: -10.0
Acc_A: 1
Spd: 255.875
Spd_A: 1
Angle_V: 1
Angle_Req: 1
Angle: -900.0
Torque_V: 1
Torque_Req: 1
Torque: -10.0
APS_flg: 1
APS_Sta: 4
APS_Shift: 7
APS_Spd: 20.0
```
{{% /details %}}

{{% details title="說明" open=false %}}
此為 FoxtronPi 車輛動態控制訊號，支援以下控制項目:「加減速度」、「目標車速」、「方向盤角度」、「方向盤扭力」和「檔位切換」。屬可讀/可寫訊號。詳細操作資訊、訊號值定義請參見 [6.2.1 車輛動態操作流程 (FoxPi_Driving_Ctrl)]({{< relref "docs/api/FoxPi_write/FoxPi_Driving_Ctrl" >}})。
- `Acc`: 加減速度命令訊號 (-15 ~ 1.5 m/s²)
- `Acc_A`: 加減速度命令驗證 (0 ~ 1)
- `Spd`: 目標速度命令訊號 (0 ~ 170 km/h)
- `Spd_A`: 目標速度命令驗證 (0 ~ 1)
- `Angle_V`: 方向盤角度控制要求訊號的驗證 (0 ~ 1)
- `Angle_Req`: 要求 EPS 進入角度控制狀態的訊號 (0 ~ 1) 
- `Angle`: 要求 EPS 轉動方向盤的目標角度位置 (-360 ~ 360 deg)
- `Torque_V`: 方向盤扭力控制要求訊號的驗證 (0 ~ 1)
- `Torque_Req`: 要求 EPS 進入扭力控制狀態的訊號 (0 ~ 1)
- `Torque`: 要求 EPS 轉動方向盤扭力 (-3 ~ 3 Nm)
- `APS_flg`: APS 控制命令 VMC 執行應用需求 (0 ~ 1)
- `APS_Sta`: APS 控制狀態 (0 ~ 7)
- `APS_Shift`: APS 檔位控制命令 0 ~ 15
- `APS_Spd`: APS 速度控制命令 (0 ~ 7 km/h)
{{% /details %}}

## 6.1.2 FoxPi_Motion_Status()
讀取 DID 0x1002 (車輛動態狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "VehicleSpeed": float,  
    "LongAcc": float,  
    "LongAcc_V": int, 
    "LatAcc": float, 
    "LatAcc_V": int, 
    "YawRate": float, 
    "YawRate_V": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Motion_Status()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
VehicleSpeed: 0.0
LongAcc: 0.0
LongAcc_V: 0
LatAcc: 0.0
LatAcc_V: 0
YawRate: 0.0
YawRate_V: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
此為 FoxtronPi 車輛速度訊號，支援以下控制項目:「車速」、「縱向加速度」、「縱向加速度訊號驗證」、「橫向加速度」、「橫向加速度訊號驗證」、「偏航率」、「偏航率訊號驗證」。屬可讀訊號。
- `VehicleSpeed`: 車速 (0 ~ 255.875 km/h)
- `LongAcc`: 車輛縱向加速度 (-1.27 ~ 1.27 G's)
- `LongAcc_V`: 車輛縱向加速度訊號驗證 (0 ~ 1)
- `LatAcc`: 車輛橫向加速度 (-1.27 ~ 1.27 G's)
- `LatAcc_V`: 車輛橫向加速度訊號驗證 (0 ~ 1)
- `YawRate`: 車輛偏航率 (-100 ~ 100 Deg/s)
- `YawRate_V`: 車輛偏航率訊號驗證 (0 ~ 1)

> [!WARNING]
> `LongAcc_V`、`LatAcc_V`、`YawRate_V` 為有效性旗標，用於判斷當前感測器是否異常；0＝invalid、1＝valid。
> 當旗標為 0 時，對應的量測值(LongAcc/LatAcc/YawRate)不可信，請忽略。
{{% /details %}}

## 6.1.3 FoxPi_Brake_Status()
讀取 DID 0x1003 (煞車系統狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "Break_Sw": int, 
    "Break_Sw_V": int, 
    "MCPressure": float, 
    "MCPressure_V": int, 
    "PBA_Act": int, 
    "PBA_Flt": int, 
    "ABS_Act": int, 
    "ABS_Flt": int, 
    "EBD_Act": int, 
    "EBD_Flt": int, 
    "ACC_Avail": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Brake_Status()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
Break_Sw: 0
Break_Sw_V: 0
MCPressure: 0.0
MCPressure_V: 0
PBA_Act: 0
PBA_Flt: 0
ABS_Act: 0
ABS_Flt: 0
EBD_Act: 0
EBD_Flt: 0
ACC_Avail: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `Brake_Sw`: 煞車開關訊號 (0 ~ 1)
- `Brake_Sw_V`: 煞車開關訊號驗證 (0 ~ 1)
- `MCPressure`: 總泵壓力 (-9.7 ~ 195 Bar)
- `MCPressure_V`: 總泵壓力訊號驗證 (0 ~ 1)
- `PBA_Act`: PBA 作動狀態 (0 ~ 1)
- `PBA_Flt`: PBA 故障 (0 ~ 1)
- `ABS_Act`: ABS 作動狀態 (0 ~ 1)
- `ABS_Flt`: ABS 故障 (0 ~ 1)
- `EBD_Act`: EBD 作動狀態 (0 ~ 1)
- `EBD_Flt`: EBD 故障 (0 ~ 1)
- `ACC_Avail`: 確認煞車系統可控狀態及訊號穩定性、正確性 (0 ~ 1)

> `Brake_Sw`: 為煞車開關狀態訊號，主要用於判斷煞車踏板是否被踩下。當煞車踏板踩下行程達約 15% 以上，此訊號將被觸發，表示駕駛者在執行煞車動作。
> 0＝inactive、1＝active

> `MCPressure` (Master Cylinder Pressure): 為煞車主缸內的液壓壓力值。（此值可能會有些誤差會出現微小負值(-0.1)，如果出現負值大於1，代表有問題，需檢查車輛是否有問題）
> `Brake_Sw_V`(煞車開關可控狀態) 與 `MCPressure_V` (總泵壓力可控狀態) 為判斷當前 `Brake_Sw` / `MCPressure` 數值是否可信的旗標。0＝invalid、1＝valid

> PBA (Panic Brake Assist) 會依據駕駛者踩踏剎車踏板的速度跟行程，來判斷是不是進入緊急情況。
> PBA_Active: PBA當前的作動狀態 (0＝inactive、1＝active)。
> PBA_Fail: PBA是否有故障 (0＝normal、1＝failed)。

> ACC_Availabliity: 確認煞車系統可控狀態及訊號穩定性、正確性 (0＝unavailable、1＝available)。
{{% /details %}}

## 6.1.4 FoxPi_WheelSpeed()
讀取 DID 0x1004 (車輪速度)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "RR_WhlSpeed": float, 
    "RR_WhlSpeed_V": int, 
    "LR_WhlSpeed": float, 
    "LR_WhlSpeed_V": int, 
    "RF_WhlSpeed": float, 
    "RF_WhlSpeed_V": int, 
    "LF_WhlSpeed": float, 
    "LF_WhlSpeed_V": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_WheelSpeed()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
RR_WhlSpeed: 0.0000
RR_WhlSpeed_V: 0
LR_WhlSpeed: 0.0000
LR_WhlSpeed_V: 0
RF_WhlSpeed: 0.0000
RF_WhlSpeed_V: 0
LF_WhlSpeed: 0.0000
LF_WhlSpeed_V: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `RR_WhlSpeed`: RR 輪速 (0 ~ 255.9375 km/h)
- `RR_WhlSpeed_V`: RR 輪速訊號驗證 (0 ~ 1)
- `LR_WhlSpeed`: LR 輪速 (0 ~ 255.9375 km/h)
- `LR_WhlSpeed_V`: LR 輪速訊號驗證 (0 ~ 1)
- `RF_WhlSpeed`: RF 輪速 (0 ~ 255.9375 km/h)
- `RF_WhlSpeed_V`: RF 輪速訊號驗證 (0 ~ 1)
- `LF_WhlSpeed`: LF 輪速 (0 ~ 255.9375 km/h)
- `LF_WhlSpeed_V`: LF 輪速訊號驗證 (0 ~ 1)

> [!WARNING]
> `RR_WhlSpeed_V`、`LR_WhlSpeed_V`、`RF_WhlSpeed_V`、`LF_WhlSpeed_V` 為判斷當前對應訊號是否可信的旗標；0＝invalid、1＝valid。
{{% /details %}}

## 6.1.5 FoxPi_EPS_Status()
讀取 DID 0x1005 (轉向系統狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "SAS_Angle": float, 
    "SAS_V": int, 
    "SAS_CAL": int, 
    "EPS_AOI_Ctrl": int, 
    "EPS_Flt": int, 
    "EPS_TOI_Act": int,
    "EPS_TOI_Avail": int, 
    "EPS_TOI_Flt": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_EPS_Status()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
SAS_Angle: 0.0
SAS_V: 0
SAS_CAL: 0
EPS_AOI_Ctrl: 0
EPS_Flt: 0
EPS_TOI_Act: 0
EPS_TOI_Avail: 0
EPS_TOI_Flt: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `SAS_Angle`: 當前方向盤的角度值 (-360 ~ 360 deg)
- `SAS_V`: 轉向感測器是否為可控狀態 (0 ~ 1)
- `SAS_Cal`: 轉向系統的絕對轉向角度是否完成校正 (0 ~ 1)
- `EPS_AOI_Ctrl`: 方向盤角度控制狀態 (0 ~ 3)
- `EPS_Flt`: 轉向控制系統失效 (0 ~ 2)
- `EPS_TOI_Act`: 轉向扭力控制系統是否反映 (0 ~ 1)
- `EPS_TOI_Avail`: 轉向扭力控制系統是否可控(EPS 檢查) (0 ~ 1)
- `EPS_TOI_Flt`: 轉向扭力控制系統故障(fail safe 檢查) (0 ~ 1)

> `EPS_AOI_Ctrl`: 角度控制狀態 (0: Control Inhibit, 1: Available, 2: Controlled, 3: Permanent Fail)

> `EPS_Flt`: 轉向系統是否出現失效 (0: No Fail, 2: Permanent Fail, 3: Permanent Fail)

> EPS_TOI_Activation: EPS 系統是否有反應使用者下達的 TOI 轉向控制 (0: No Activation, 1: Activation)

> [!CAUTION]
> EPS_TOI_Available: EPS 內部檢查確認後，TOI 是否為可控狀態 (0: Available, 1: Unavailable)
> 註: 若有問題則無法開啟 TOI 控制。

> EPS_TOI_Fault: TOI 控制下 fail safe 檢查確認 (0: No Fault, 1: Fault)
{{% /details %}}

## 6.1.6 FoxPi_Button_Status()
讀取 DID 0x1006 (方向盤按鍵狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "SWC_ACC_sta": int, 
    "SWC_CANCEL_Sta": int, 
    "SWC_SET_down_Sta": int,
    "SWC_RES_up_Sta": int, 
    "SWC_Distance_Sta": int, 
    "SWC_mode_Sta": int, 
    "SWC_Up_Sta": int, 
    "SWC_Down_Sta": int, 
    "SWC_Left_Sta": int, 
    "SWC_Right_Sta": int, 
    "SWC_RegenDown": int, 
    "SWC_Undefined_Sta": int, 
    "SWC_VR_Sta": int, 
    "SWC_LKA_Sta": int, 
    "SWC_RegenUp": int, 
    "SWC_Trip_Sta": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Button_Status()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
SWC_CANCEL_Sta: 0
SWC_SET_down_Sta: 0
SWC_RES_up_Sta: 0
SWC_Distance_Sta: 0
SWC_mode_Sta: 0
SWC_Up_Sta: 0
SWC_Down_Sta: 0
SWC_Left_Sta: 0
SWC_Right_Sta: 0
SWC_RegenDown: 0
SWC_Undefined_Sta: 0
SWC_VR_Sta: 0
SWC_LKA_Sta: 0
SWC_RegenUp: 0
SWC_Trip_Sta: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `SWC_ACC_Sta`: 方向盤 ACC 按鈕觸發狀態 (0 ~ 1)
- `SWC_CANCEL_Sta`: 方向盤 CANCEL 按鈕觸發狀態 (0 ~ 1)
- `SWC_SET_down_Sta`: 方向盤 SET/↓按鈕觸發狀態 (0 ~ 1)
- `SWC_RES_Up_Sta`: 方向盤 RESUME/↑按鈕觸發狀態 (0 ~ 1)
- `SWC_Distance_Sta`: 方向盤 DISTANCE 按鈕觸發狀態 (0 ~ 1)
- `SWC_mode_Sta`: 方向盤 Mode 按鈕觸發狀態 (0 ~ 1)
- `SWC_Up_Sta`: 方向盤 Up 按鈕觸發狀態 (0 ~ 1)
- `SWC_Down_Sta`: 方向盤 Down 按鈕觸發狀態 (0 ~ 1)
- `SWC_Left_Sta`: 方向盤 Left 按鈕觸發狀態 (0 ~ 1)
- `SWC_Right_Sta`: 方向盤 Right 按鈕觸發狀態 (0 ~ 1)
- `SWC_RegenDown`: 方向盤 Regen Down 按鈕觸發狀態 (0 ~ 1)
- `SWC_Undefined_Sta`: 方向盤 Undefined 按鈕觸發狀態 (0 ~ 1)
- `SWC_VR_Sta`: 方向盤 VR 按鈕觸發狀態 (0 ~ 1)
- `SWC_LKA_Sta`: 方向盤 LKA 按鈕觸發狀態 (0 ~ 1)
- `SWC_RegenUp`: 方向盤 Regen Up 按鈕觸發狀態 (0 ~ 1)
- `SWC_Trip_Sta`: 方向盤 Trip 按鈕觸發狀態 (0 ~ 1)

> (0: Release, 1: Press)
{{% /details %}}

## 6.1.7 FoxPi_Switch_Status()
讀取 DID 0x100A (車身開關狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "Crash_Detect_Status": int, 
    "Driver_Lock_Status": int, 
    "All_Door_Switch_Status": int, 
    "Driver_Door_Switch_Status": int, 
    "Passenger_Door_Switch_Status": int, 
    "Rear_Left_Door_Switch_Status": int, 
    "Rear_Right_Door_Switch_Status": int, 
    "Tailgate_Switch_Status": int, 
    "Hood_Switch_Status": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Switch_Status()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
Crash_Detect_Status: 0
Driver_Lock_Status: 0
All_Door_Switch_Status: 0
Driver_Door_Switch_Status: 0
Passenger_Door_Switch_Status: 0
Rear_Left_Door_Switch_Status: 0
Rear_Right_Door_Switch_Status: 0
Tailgate_Switch_Status: 0
Hood_Switch_Status: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `Crash Detect Status`: 車輛碰撞狀態 (0: Crash Not Detected, 1: Crash Detected)
- `Driver Lock Status`: 駕駛座車門鎖狀態 (0: Unlock, 1: Lock)
- `All Door Switch Status`: 四門開關狀態 (0: All Door Closed, 1: Any Door Open)
- `Driver Door Switch Status`: 駕駛座車門開關狀態 (0: Closed, 1: Open)
- `Passenger Door Switch Status`: 副駕駛座車門開關狀態 (0: Closed, 1: Open)
- `Rear Left Door Switch Status`: 左後座位車門開關狀態 (0: Closed, 1: Open)
- `Rear Right Door Switch Status`: 右後車門開關狀態 (0: Closed, 1: Open)
- `Tailgate Switch Status`: 尾門開關狀態 (0: Closed, 1: Open)
- `Hood Switch Status`: 前蓋開關狀態 (0: Closed, 1: Open)
{{% /details %}}

## 6.1.8 FoxPi_Lamp_Status()
讀取 DID 0x100B (車燈狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "Column_Turn_Lamp_Switch_Status": int, 
    "Column_Dim_Switch_Status": int, 
    "Column_Pass_Switch_Status": int, 
    "Position_Lamp_Status": int, 
    "Low_Beam_Status": int, 
    "High_Beam_Status": int,
    "Right_Daytime_Running_Light_Status": int, 
    "Left_Daytime_Running_Light_Status": int, 
    "Left_Turn_Lamp_Status": int, 
    "Right_Turn_Lamp_Status": int, 
    "Brake_Lamp_Status": int, 
    "Reverse_Lamp_Status": int, 
    "Rear_Fog_Lamp_Status": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Lamp_Status()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
Column_Turn_Lamp_Switch_Status: 0
Column_Dim_Switch_Status: 0
Column_Pass_Switch_Status: 0
Position_Lamp_Status: 0
Low_Beam_Status: 0
High_Beam_Status: 0
Right_Daytime_Running_Light_Status: 0
Left_Daytime_Running_Light_Status: 0
Left_Turn_Lamp_Status: 1
Right_Turn_Lamp_Status: 0
Brake_Lamp_Status: 1
Reverse_Lamp_Status: 0
Rear_Fog_Lamp_Status: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `Column Turn Switch Status`: 方向燈撥桿狀態 (0 ~ 3)
- `Column Pass Switch Status`: 超車燈撥桿狀態 (0 ~ 1)
- `Column Dim Switch Status`: 遠光燈撥桿狀態 (0 ~ 1)
- `Position Lamp Status`: 位置燈狀態 (0 ~ 1)
- `Low Beam Status`: 近光燈狀態 (0 ~ 1)
- `High Beam Status`: 遠光燈狀態 (0 ~ 1)
- `Right Daytime Running Light Status`: 右晝行燈狀態 (0 ~ 1)
- `Left Daytime Running Light Status`: 左晝行燈狀態 (0 ~ 1)
- `Left Turn Lamp Status`: 左方向燈狀態 (0 ~ 1)
- `Right Turn Lamp Status`: 右方向燈狀態 (0 ~ 1)
- `Brake Lamp Status`: 煞車燈狀態 (0 ~ 1)
- `Reverse Lamp Status`: 倒車燈狀態 (0 ~ 1)
- `Rear Fog Lamp Status`: 後霧燈狀態 (0 ~ 1)

> (0: OFF, 1: Left, 2: Right, 3: Invalid)

> (0: Release, 1: Press)

> (0: OFF, 1: ON)
{{% /details %}}

## 6.1.9 FoxPi_Lamp_Ctrl()
讀取 DID 0x100C (車燈控制)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "Position_Lamp_Control_Enable": int, 
    "Position_Lamp": int, 
    "Low_Beam_Control_Enable": int, 
    "Low_Beam": int, 
    "High_Beam_Control_Enable": int, 
    "High_Beam": int, 
    "Right_Daytime_Running_Light_Control_Enable": int, 
    "Right_Daytime_Running_Light": int, 
    "Left_Daytime_Running_Light_Control_Enable": int, 
    "Left_Daytime_Running_Light": int, 
    "Left_TurnLamp_Control_Enable": int, 
    "Left_TurnLamp": int, 
    "Right_TurnLamp_Control_Enable": int, 
    "Right_TurnLamp": int, 
    "Brake_Lamp_Control_Enable": int, 
    "Brake_Lamp": int, 
    "Reverse_Lamp_Control_Enable": int, 
    "Reverse_Lamp": int, 
    "Rear_Fog_Lamp_Control_Enable": int, 
    "Rear_Fog_Lamp": int, 
    "Amblight_Control_Enable": int, 
    "Control_Area": int,
    "RGB_Color": int | "FF", 
    "Bright": int | "FF", 
    "Mode": int | "FF"
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Lamp_Ctrl()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
Low_Beam: 0
High_Beam_Control_Enable: 0
High_Beam: 0
Right_Daytime_Running_Light_Control_Enable: 0
Right_Daytime_Running_Light: 0
Left_Daytime_Running_Light_Control_Enable: 0
Left_Daytime_Running_Light: 0
Left_TurnLamp_Control_Enable: 1
Left_TurnLamp: 1
Right_TurnLamp_Control_Enable: 0
Right_TurnLamp: 0
Brake_Lamp_Control_Enable: 0
Brake_Lamp: 0
Reverse_Lamp_Control_Enable: 0
Reverse_Lamp: 0
Rear_Fog_Lamp_Control_Enable: 0
Rear_Fog_Lamp: 0
Amblight_Control_Enable: 0
Control_Area: 0
RGB_Color: 0
Bright: 0
Mode: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
此為 FoxtronPi 車輛燈光控制訊號，支援以下控制項目:「位置燈」、「近光燈」、「遠光燈」、「晝行燈」、「方向燈」、「煞車燈」、「倒車燈」、「霧燈」和「氣氛燈(選配)」。屬可讀/可寫訊號。詳細操作資訊、訊號值定義請參見 [6.2.2 燈光控制操作流程 (FoxPi_Lamp_Ctrl)]({{< relref "docs/api/FoxPi_write/FoxPi_Lamp_Ctrl" >}})。
- `Position_Lamp_Enable`: 位置燈控制命令啟用 (0 ~ 1)
- `Position_Lamp`: 位置燈控制命令 (0 ~ 1)
- `Low_Beam_Enable`: 近光燈控制命令啟用 (0 ~ 1)
- `Low_Beam`: 近光燈控制命令 (0 ~ 1)
- `High_Beam_Enable`: 遠光燈控制命令啟用 (0 ~ 1)
- `High_Beam`: 遠光燈控制命令 (0 ~ 1)
- `Right_Daytime_Running_Light_Enable`: 右晝行燈控制命令啟用 (0 ~ 1)
- `Right_Daytime_Running_Light`: 右晝行燈控制命令 (0 ~ 1)
- `Left_Daytime_Running_Light_Enable`: 左晝行燈控制命令啟用 (0 ~ 1)
- `Left_Daytime_Running_Light`: 左晝行燈控制命令 (0 ~ 1)
- `Left_TurnLamp_Enable`: 左方向燈控制命令啟用 (0 ~ 1)
- `Left_TurnLamp`: 左方向燈控制命令 (0 ~ 1)
- `Right_TurnLamp_Enable`: 右方向燈控制命令啟用 (0 ~ 1)
- `Right_TurnLamp`: 右方向燈控制命令 (0 ~ 1)
- `Brake_Lamp_Enable`: 煞車燈控制命令啟用 (0 ~ 1)
- `Brake_Lamp`: 煞車燈控制命令 (0 ~ 1)
- `Reverse_Lamp_Enable`: 倒車燈控制命令啟用 (0 ~ 1)
- `Reverse_Lamp`: 倒車燈控制命令 (0 ~ 1)
- `Rear_Fog_Lamp_Enable`: 後霧燈控制命令啟用 (0 ~ 1)
- `Rear_Fog_Lamp`: 後霧燈控制命令 (0 ~ 1)
- `Amblight_Enable`: 氣氛燈控制命令啟用 (0 ~ 1)
- `Control area`: 氣氛燈區域控制命令 (0 ~ 7)
- `RGB 64 Color`: 氣氛燈顏色控制命令 (0 ~ 63)
- `Bright`: 氣氛燈亮度控制命令 (0 ~ 100 %)
- `Breathing_Alert_Mode`: 氣氛燈模式控制命令 (0 ~ 7)
{{% /details %}}

## 6.1.10 FoxPi_Battery_Status()
讀取 DID 0x100D (電池狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "LVBatt12V": float,  
    "HVBattSOC": float,  
    "HVBattTemp": int,  
    "HVBattContactorSta": int, 
    "HVBattErr": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Battery_Status()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
LVBatt12V: 0.0
HVBattSOC: 0.0
HVBattTemp: 0
HVBattContactorSta: 0
HVBattErr: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `LVBatt12V`: 12V 電池電壓 (0 ~ 25.4 V)
- `HVBattSOC`: 高壓電池電量 (0 ~ 100 %)
- `HVBattTemp`: 高壓電池溫度，溫定高於60度系統會自動下電 (-40 ~ 213 Deg^C)
- `HVBattContactorSta`: 高壓電池斷開狀態 (0 ~ 1)
- `HVBattErr`: 高壓電池故障 (0 ~ 1)

> [!TIP]
> LVBatt12V: 顯示12V電池電壓。9~16V為使用範圍，超出範圍需注意!

造成高壓電池斷開的原因有
- 高壓電壓異常: 電壓過高或過低。
- 高壓電流異常: 放電或充電電流過大。
- 高壓絕緣電阻異常: 存在漏電、絕緣材料老化、潮濕導致電阻值下降。
- 溫度異常: 電芯的溫度過高(60度)或過低(-20度)。
- 繼電器故障: 繼電器關閉後，仍有高壓電池的電壓超過60V。
- 通訊異常: BMS無法正常進行通訊
{{% /details %}}

## 6.1.11 FoxPi_Pedal_position()
讀取 DID 0x100F (踏板位置)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "AccelPedalPos": float, 
    "BrakePedalPos": float
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Pedal_position()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
AccelPedalPos: 0.000
BrakePedalPos: 0.00
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `AccelPedalPos`: 電門踏板開度 (0 ~ 100 %)
- `BrakePedalPos`: 煞車踏板開度 (0 ~ 100 %)
{{% /details %}}

## 6.1.12 FoxPi_Motor_Status()
讀取 DID 0x1010 (馬達狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "TqSource": int, 
    "TMTqReq": int,  
    "RealTMTq": int,
    "MotorAvailTq": int, 
    "RegenAvailTq": int, 
    "TMSpd": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Motor_Status()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
TqSource: 0
TMTqReq : 0
RealTMTq: 0
MotorAvailTq: 0
RegenAvailTq: 0
TMSpd: 0
```
{{% /details %}}

{{% details title="說明" open=false %}}
- `TqSource`: 扭力來源顯示 (0 ~ 9)
- `TMTqReq`: 駕駛踩下油門後，經過系統運算轉發的扭力值 (-530 ~ 530 Nm)
- `RealTMTq`: 馬達端實際扭力值 (-1022 ~ 1022 Nm)
- `MotorAvailTq`: 當下電機可輸出的最大正扭矩，其正值為可供車輛進行驅動 (0 ~ 1022 Nm)
- `RegenAvailTq`: 當前電機可輸出的最大負扭矩，其負值為可供車輛進行發電 (-1022 ~ 0 Nm)
- `TMSpd`: 當前馬達轉速值 (-17000 ~ 17000 rpm)

> TqSource: 扭力來源
> - 0: Invalid 當前扭力有異常，一般狀態下不會出現。
> - 1: Internal 為駕駛踩踏油門控制的開度經過本公司設計的內部演算法運算後，轉換為扭力命令，發送至馬達端。
> - 2: ABS 剎車系統命令，當煞車系統介入做動時，會對內部油門扭力命令產生干預，系統會判斷當前狀態，將其對應的扭力發送至馬達。
> - 3: RollABS 剎車系統命令，當煞車系統介入做動時，會對內部油門扭力命令產生干預，系統會判斷當前狀態，將其對應的扭力發送至馬達。
> - 4: TCS 剎車系統命令，當煞車系統介入做動時，會對內部油門扭力命令產生干預，系統會判斷當前狀態，將其對應的扭力發送至馬達。
> - 5: CC 為方向盤上可以設定車速的相關功能，屬於油門以外的外部干預來源。
> - 6: ADAS 為方向盤上可以設定車速的相關功能，屬於油門以外的外部干預來源。
> - 7: MSA 為方向盤上可以設定車速的相關功能，屬於油門以外的外部干預來源。
> - 8: APS 屬於油門以外的扭力來源，會拋出對應控制速度，由系統轉為扭力後，轉發至馬達。
> - 9: Force Idle 為車上 ADAS 系統強行發送零扭命令去切斷車輛動力。
{{% /details %}}

## 6.1.13 FoxPi_Shifter_allow()
讀取 DID 0x1011 (換檔允許狀態)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "ExtTqAllow_flg": int, 
    "ExtShftAllow_flg": int, 
    "ExtDoorAllow_flg": int
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Shifter_allow()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
ExtTqAllow_flg: 0
ExtShftAllow_flg: 0
ExtDoorAllow_flg: 1
```
{{% /details %}}

{{% details title="說明" open=false %}}

- `ExtTqAllow_flg`: 確認當前車輛電池、電機、DCDC是否為正常狀態，來判定外部扭力是否為可接收狀態 (0 ~ 1)
- `ExtShftAllow_flg`: 確認當前車輛電池、電機、DCDC和EPB是否為正常狀態，來判定外部檔位是否為可接收狀態 (0 ~ 1)
- `ExtDoorAllow_flg`: 確認當前車輛四個車門、尾門和引擎蓋是否為關閉狀態 (0 ~ 1)

> (0: Not Allow, 1: Allow)

> [!NOTE]
> FoxPi_Shifter_allow 為進入 [6.2.1.5 APS檔位控制流程]({{< relref "docs/api/FoxPi_write/FoxPi_Driving_Ctrl#6215-aps檔位控制流程" >}})前的允許條件判斷。系統會檢查三個訊號，僅當所有條件皆滿足(=1)時，車輛才會切入 APS 控制。任一條件不滿足時，將禁止進入 APS 控制。
{{% /details %}}


## 6.1.14 FoxPi_Ctrl_Enable_Switch()
讀取 DID 0x1012 (FoxtronPi控制啟用開關)，並將回應位元組解析為實體值。

- Return type: `Dict[str, Union[int, float, str]]`
- Return value
    ```yaml
    {
    "Ctrl_Enable_Switch": int | "FF"
    }
    ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_read import FoxPiReadDID

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiReadDID(client)
    resp = Foxpi.FoxPi_Ctrl_Enable_Switch()
    for i,j in resp.items():
        print(f"{i}: {j}")
```

```text
Ctrl_Enable_Switch: 1
```
{{% /details %}}

{{% details title="說明" open=false %}}
此為 FoxtronPi 控制旗標，屬可讀/可寫訊號。詳細操作資訊、訊號值定義請參見 [6.2.3 Ctrl_Enable_Switch]({{< relref "docs/api/FoxPi_write/#623-foxpi_ctrl_enable_switch" >}})。
- `Ctrl_Enable_Switch`: FoxtronPi 控制命令啟用 (0 ~ 1)
{{% /details %}}