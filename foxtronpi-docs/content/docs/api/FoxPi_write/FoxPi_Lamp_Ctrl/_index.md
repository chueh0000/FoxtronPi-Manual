---
title: "6.2.2 燈光控制操作流程 (FoxPi_Lamp_Ctrl)"
type: "docs"
weight: 2
---

# 6.2.2 FoxPi_Lamp_Ctrl(user_input [])
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

---

車輛燈光控制訊號可同時調整位置燈、近光燈、遠光燈、晝行燈、方向燈、煞車燈、倒車燈、霧燈和氣氛燈(選配功能)。在執行各項車燈控制之前，系統將依據每一控制項目所對應的控制旗標進行檢查，僅在相關旗標已正確啟用時，才允許後續控制操作。

> [!TIP]
> 車燈控制支援一次性寫入(旗標與燈態)，例如: `Position_Lamp_Enable=1`、`Position_Lamp=1` 同時寫入，系統即會點亮位置燈。

> [!WARNING]
> 因目前 FoxtronPi 系統限制，以下三個組合無法同時開啟(車燈不會點亮)，請避免這些組合同時操作
> - 位置燈、近光燈、遠光燈、右晝行燈
> - 左方向燈、右方向燈、煞車燈、左晝行燈
> - 後霧燈、倒車燈、氣氛燈

## 6.2.2.1 位置燈控制流程
- `Position_Lamp_Enable`: 位置燈控制命令啟用 (0 ~ 1)
- `Position_Lamp`: 位置燈控制命令 (0 ~ 1)

## 6.2.2.2 近光燈控制流程
- `Low_Beam_Enable`: 近光燈控制命令啟用 (0 ~ 1)
- `Low_Beam`: 近光燈控制命令 (0 ~ 1)

## 6.2.2.3 遠光燈控制流程
- `High_Beam_Enable`: 遠光燈控制命令啟用 (0 ~ 1)
- `High_Beam`: 遠光燈控制命令 (0 ~ 1)

## 6.2.2.4 左 / 右晝行燈控制流程
- `Right_Daytime_Running_Light_Enable`: 右晝行燈控制命令啟用 (0 ~ 1)
- `Right_Daytime_Running_Light`: 右晝行燈控制命令 (0 ~ 1)
- `Left_Daytime_Running_Light_Enable`: 左晝行燈控制命令啟用 (0 ~ 1)
- `Left_Daytime_Running_Light`: 左晝行燈控制命令 (0 ~ 1)

## 6.2.2.5 左、右方向燈控制流程
- `Left_TurnLamp_Enable`: 左方向燈控制命令啟用 (0 ~ 1)
- `Left_TurnLamp`: 左方向燈控制命令 (0 ~ 1)
- `Right_TurnLamp_Enable`: 右方向燈控制命令啟用 (0 ~ 1)
- `Right_TurnLamp`: 右方向燈控制命令 (0 ~ 1)

> [!NOTE]
> 註: 方向燈啟動後預設為恆亮。若需閃爍顯示，請於程式內自行定義閃爍頻率。

## 6.2.2.6 煞車燈控制流程
- `Brake_Lamp_Enable`: 煞車燈控制命令啟用 (0 ~ 1)
- `Brake_Lamp`: 煞車燈控制命令 (0 ~ 1)

## 6.2.2.7 倒車燈控制流程
- `Reverse_Lamp_Enable`: 倒車燈控制命令啟用 (0 ~ 1)
- `Reverse_Lamp`: 倒車燈控制命令 (0 ~ 1)

## 6.2.2.8 後霧燈控制流程
- `Rear_Fog_Lamp_Enable`: 後霧燈控制命令啟用 (0 ~ 1)
- `Rear_Fog_Lamp`: 後霧燈控制命令 (0 ~ 1)

## 6.2.2.9 氣氛燈控制流程(選配功能)
- `Amblight_Enable`: 氣氛燈控制命令啟用 (0 ~ 1)
- `Control area`: 氣氛燈區域控制命令 (0 ~ 7)
    - `0`: OFF
    - `1`: IP
    - `2`: Console
    - `3`: 左前門區
    - `4`: 右前門區
    - `5`: 左後門區
    - `6`: 右後門區
    - `7`: 全區域
- `RGB 64 Color`: 氣氛燈顏色控制命令 (0 ~ 63)
    - `0`: LightPink
    - `1`: Crimson
    - `2`: PaleVioletRed
    - `3`: HotPink
    - `4`: DeepPink
    - `5`: MediumVioletRed
    - `6`: Orchid
    - `7`: Violet
    - `8`: Magenta
    - `9`: DarkMagenta
    - `10`: MediumOrchid
    - `11`: DarkVoilet
    - `12`: DarkOrchid
    - `13`: Indigo
    - `14`: BlueViolet
    - `15`: MediumPurple
    - `16`: MediumSlateBlue
    - `17`: SlateBlue
    - `18`: DarkSlateBlue
    - `19`: Blue
    - `20`: MediumBlue
    - `21`: MidnightBlue
    - `22`: DarkBlue
    - `23`: Navy
    - `24`: RoyalBlue
    - `25`: CornflowerBlue
    - `26`: DoderBlue
    - `27`: LightSkyBlue
    - `28`: SkyBlue
    - `29`: DeepSkyBlue
    - `30`: PaleTurquoise
    - `31`: Cyan
    - `32`: DarkTurquoise
    - `33`: DarkCyan
    - `34`: MediumTurquoise
    - `35`: Auqamarin
    - `36`: MediumAquamarine
    - `37`: MediumSpringGreen
    - `38`: SpringGreen
    - `39`: SeaGreen
    - `40`: LightGreen
    - `41`: PaleGreen
    - `42`: LimeGreen
    - `43`: Lime
    - `44`: Chartreuse
    - `45`: GreenYellow
    - `46`: Beige
    - `47`: LightGoldenrodYellow
    - `48`: Ivory
    - `49`: LightYellow
    - `50`: Yellow
    - `51`: LemonChiffon
    - `52`: Gold
    - `53`: BlanchedAlmond
    - `54`: NavajoWhite
    - `55`: Bisque
    - `56`: DarkOrange
    - `57`: LightSalmon
    - `58`: Coral
    - `59`: OrangeRed
    - `60`: Tomato
    - `61`: LightCoral
    - `62`: IndianRed
    - `63`: Red
- `Bright`: 氣氛燈亮度控制命令 (0 ~ 100)
- `Breathing_Alert_Mode`: 氣氛燈模式控制命令 (0 ~ 7)
    - `0`: OFF
    - `1`: Welcome
    - `2`: Breath Red
    - `3`: Flash Keep Red
    - `4`: Switch to Red
    - `5`: Switch to Blue
    - `6`: Flash Keep
    - `7`: Breath Keep