---
title: "8.2 FoxPi_write"
type: "docs"
weight: 2
---

# 8.2 FoxPi_write
建立 FoxtronPi DID 讀取器。

```python
class FoxPiWriteDID(client):
```

- `client`: (udsoncan.client.Client) 已開啟的 UDS 客戶端，用於發送 WriteDataByIdentifier 等請求。
- Raises: 任何由 client.write_data_by_identifier() 拋出的通訊錯誤(如逾時、負回應)。

## 8.2.1 FoxPi_Driving_Ctrl(user_input[])
寫入 DID 0x1001 (FoxPi_Driving_Ctrl)，用來車輛動態控制。
- Parameters: `user_input[14]`
    - `user_input[0]`: Acc
    - `user_input[1]`: Acc_A
    - `user_input[2]`: Spd
    - `user_input[3]`: Spd_A
    - `user_input[4]`: Angle_V
    - `user_input[5]`: Angle_Req
    - `user_input[6]`: Angle
    - `user_input[7]`: Torque_V
    - `user_input[8]`: Torque_Req
    - `user_input[9]`: Torque
    - `user_input[10]`: APS_flg
    - `user_input[11]`: APS_Sta
    - `user_input[12]`: APS_Shift
    - `user_input[13]`: APS_Spd
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

## 8.2.2 Driving_Ctrl_toFF()
將 DID 0x1001 (FoxPi_Driving_Ctrl) 全欄位寫為 FF，用於觸發寫入保護。
- Parameters: none
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
    resp = Foxpi.Driving_Ctrl_toFF()
    print(resp)
```

```text
b'\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff'
```
{{% /details %}}

## 8.2.3 FoxPi_Lamp_Ctrl(user_input [])
寫入 DID 0x100C (FoxPi_Lamp_Ctrl)，用來點亮/關閉車燈。
- Parameters: `user_input[25] (int)`
    - `user_input[0]:` Position_Lamp_Enable
    - `user_input[1]:` Position Lamp
    - `user_input[2]:` Low_Beam_Enable
    - `user_input[3]:` Low_Beam
    - `user_input[4]:` High_Beam_Enable
    - `user_input[5]:` High_Beam
    - `user_input[6]:` Right_Daytime_Running_Light_Enable
    - `user_input[7]:` Right_Daytime_Running_Light
    - `user_input[8]:` Left_Daytime_Running_Light_Enable
    - `user_input[9]:` Left Daytime Running Light
    - `user_input[10]`: Left TurnLamp Enable
    - `user_input[11]`: Left TurnLamp
    - `user_input[12]`: Right_TurnLamp_Enable
    - `user_input[13]`: Right_TurnLamp
    - `user_input[14]`: Brake_Lamp_Enable
    - `user_input[15]`: Brake_Lamp
    - `user_input[16]`: Reverse_Lamp_Enable
    - `user_input[17]`: Reverse Lamp
    - `user_input[18]`: Rear Fog Lamp Enable
    - `user_input[19]`: Rear_Fog_Lamp
    - `user_input[20]`: Amblight_Enable
    - `user_input[21]`: Control area
    - `user_input[22]`: RGB 64 Color
    - `user_input[23]`: Bright
    - `user_input[24]`: Breathing_Alert_Mode
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
    user_input = [1,1,0,0,1,1,1,1,0,0,1,1,1,1,1,1,0,0,1,1,1,7,63,100,7]
    resp = Foxpi.FoxPi_Lamp_Ctrl(user_input)
    print(resp)
```

```text
b'\xf3\xfc\xfc?d\x07'
```
{{% /details %}}

## 8.2.4 FoxPi_Ctrl_Enable_Switch()
- Parameters: `user_input[1]`
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
    user_input = [0]
    resp = Foxpi.FoxPi_Ctrl_Enable_Switch(user_input)
    print(resp)
```

```text
b'\x00'
```
{{% /details %}}