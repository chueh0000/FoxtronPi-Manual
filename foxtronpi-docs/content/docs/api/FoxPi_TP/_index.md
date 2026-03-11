---
title: "8.4 FoxPi_TP"
type: "docs"
weight: 4
---

# 8.4 FoxPi_TP
提供 Tester Present 維持會話的功能(避免超時結束診斷會話)。

```python
class FoxPiTP(client)
```

- `client`: (udsoncan.client.Client) 已開啟的 UDS 客戶端。

> [!TIP]
> 本功能是避免長時間未傳送任何讀取/寫入封包，導致被車輛系統斷線

## 8.4.1 TesterPresent()
送出 Tester Present 要求，維持當前連線，避免斷線。

{{% details title="Example Code" open=false %}}
```python
from doipclient import DoIPClient
from doipclient.connectors import DoIPClientUDSConnector
from udsoncan.client import Client
from common import get_uds_client
from client_config import DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS
from FoxPi_TP import FoxPiTP

doip_client = DoIPClient(DOIP_SERVER_IP, DoIP_LOGICAL_ADDRESS, protocol_version=3)
uds_connection = DoIPClientUDSConnector(doip_client)
assert uds_connection.is_open
with Client(uds_connection, request_timeout=4, config=get_uds_client()) as client:

    Foxpi = FoxPiTP(client)
    Foxpi.TesterPresent()
```
{{% /details %}}