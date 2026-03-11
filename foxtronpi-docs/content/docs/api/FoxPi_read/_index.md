---
title: "8.1 FoxPi_read"
type: "docs"
weight: 1
---

# 8.1 FoxPi_read
建立 FoxtronPi DID 讀取器。

```python
class FoxPiReadDID(client):
```

- `client`: (udsoncan.client.Client) 已開啟的 UDS 客戶端，用於發送 ReadDataByIdentifier 等請求。
- Raises: 任何由 client.read_data_by_identifier() 拋出的通訊錯誤(如逾時、負回應)。

## 8.1.1 FoxPi_Driving_Ctrl()
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

## 8.1.2 FoxPi_Motion_Status()
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

## 8.1.3 FoxPi_Brake_Status()
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
ACC Avail: 0
```
{{% /details %}}


## 8.1.4 FoxPi_WheelSpeed()
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

## 8.1.5 FoxPi_EPS_Status()
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

## 8.1.6 FoxPi_Button_Status()
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


## 8.1.7 FoxPi_Switch_Status()
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

## 8.1.8 FoxPi_Lamp_Status()
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

## 8.1.9 FoxPi_Lamp_Ctrl()
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

## 8.1.10 FoxPi_Battery_Status()
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

doip_client = DoIPClient(DOIP_SERVER_IP, DOIP_LOGICAL_ADDRESS, protocol_version=3)
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

## 8.1.11 FoxPi_Pedal_position()
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

## 8.1.12 FoxPi_Motor_Status()
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

## 8.1.13 FoxPi_Shifter_allow()
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

## 8.1.14 FoxPi_Ctrl_Enable_Switch()
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
