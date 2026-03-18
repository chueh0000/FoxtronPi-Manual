---
title: "6.2 FoxPi_write"
type: "docs"
bookCollapseSection: true
weight: 2
---

# 6.2 FoxPi_write
本章節介紹如何透過 FoxtronPi 控制車輛。

> [!CAUTION]
> 當需進行 FoxtronPi 訊號控制時，必須先將 [Ctrl_Enable_Switch]({{< relref "docs/api/FoxPi_write/#623-foxpi_ctrl_enable_switch" >}}) 旗標設定為 `1`。該旗標作為啟用控制功能的前置條件，若未正確設定，所有控制操作將被系統拒絕執行。

建立 FoxtronPi DID 讀取器。

```python
class FoxPiWriteDID(client):
```

- `client`: (udsoncan.client.Client) 已開啟的 UDS 客戶端，用於發送 WriteDataByIdentifier 等請求。
- Raises: 任何由 client.write_data_by_identifier() 拋出的通訊錯誤(如逾時、負回應)。

## [6.2.1 車輛動態操作流程 (FoxPi_Driving_Ctrl)]({{< relref "docs/api/FoxPi_write/FoxPi_Driving_Ctrl" >}})

## [6.2.2 燈光控制操作流程 (FoxPi_Lamp_Ctrl)]({{< relref "docs/api/FoxPi_write/FoxPi_Lamp_Ctrl" >}})

## 6.2.3 FoxPi_Ctrl_Enable_Switch()
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

## 6.2.4 Driving_Ctrl_toFF()
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