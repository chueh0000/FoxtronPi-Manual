---
title: "8. FoxtronPi Library (API) 說明"
type: "docs"
bookCollapseSection: true
weight: 8
---

# 8. FoxtronPi Library 功能與介面 (API) 說明

本公司將提供 FoxtronPi 的 Python 程式庫，您只需呼叫庫中的函式，即可完成車輛動態控制(如速度、轉角、扭矩等)與訊號讀取(含感測值、狀態位與 DTC)。

為提升安全與易用性，程式庫已封裝 DoIP/UDS 通訊流程、封包解譯和 Security Key 操作，並提供對應的回傳格式。

- 重點特色
    - 控制介面：輸入對應的訊號參數值後，程式會自動完成換算與封包編碼，並將數值正確寫入車端。
    - 資料讀取：程式會自動完成封包解析與數值換算，並以 dict 形式回傳結果。
    - 診斷功能：提供 Read/Clear DTC，協助測試或誤觸緊急按鈕後的清除作業。
    - 安全機制：內建前置安全流程與參數有效性檢查，降低誤寫風險。
    - 機密保護：由於FoxtronPi 通訊交握包含部分機密資訊，本公司已採取對應防護措施，避免任何明碼外洩。
- 使用前置
    - [7. 程式開發環境架設]({{< relref "docs/dev-setup" >}})
    - 依 [6. FoxtronPi 控制訊號操作流程與限制條件]({{< relref "docs/signals/control" >}}) 完成前置流程後，再下發控制命令。

## [8.1 FoxPi_read]({{< relref "docs/api/FoxPi_read" >}})

## [8.2 FoxPi_write]({{< relref "docs/api/FoxPi_write" >}})

## [8.3 FoxPi_DTC]({{< relref "docs/api/FoxPi_DTC" >}})

## [8.4 FoxPi_TP]({{< relref "docs/api/FoxPi_TP" >}})
