---
title: "8.3 FoxPi_DTC"
type: "docs"
weight: 3
---

# 8.3 FoxPi_DTC
提供 DTC (Diagnostic Trouble Codes) 的讀取/清除功能，內部會在操作前切換 DoIP 邏輯位址至功能位址 (functional address) 以對全車系統廣播，完成後切回實體位址 (physical address)。

```python
class FoxPiDTC(client, doip_client)
```

- `client`: (udsoncan.client.Client) 已開啟的 UDS 客戶端。
- `doip_client`: (doipclient.DoIPClient) DoIP 連線物件，用於切換 ECU 邏輯位址。

## 8.3.1 Read_DTCs()
以 reportDTCByStatusMask(0x0F) 讀取當前問題相關的 DTC(含 pending/confirmed/test_failed 等狀態)。

- Return type: `str | Dict`
- Return value
    - 當無 DTC：回傳 "Success, no DTCs found"
    - 當有 DTC：回傳Dict
        ```yaml
        {
            dtc_obj: { 
                "DTC": "0xA1B2C3", 
                "pending": bool, 
                "confirmed": bool, 
                "test_failed": bool
            }
        }
        ```

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_DTC import FoxPiDTC

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiDTC(client, doip_client)
    resp = Foxpi.Read_DTCs()
    print(resp)
```

```text
Success, no DTCs found
```
{{% /details %}}

## 8.3.2 Clear_DTCs()
以 clearDTC(group=0xFFFFFF) 清除所有 DTC。

- Return type: `str | Exception`
- Return value
    - 成功：回傳 "Clear DTCs successful"
    - 失敗：回傳 Exception

{{% details title="Example Code/Output" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_DTC import FoxPiDTC

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiDTC(client, doip_client)
    resp = Foxpi.Clear_DTCs()
    print(resp)
```

```text
Clear DTCs successful
```
{{% /details %}}