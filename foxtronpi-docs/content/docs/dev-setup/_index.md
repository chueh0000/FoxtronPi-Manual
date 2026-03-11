---
title: "7. 程式開發環境架設"
type: "docs"
weight: 7
---

# 7. 程式開發環境架設

本公司提供 [GitHub 儲存庫](https://github.com/foxtron-ev/foxtronpi-pyclient)，內含示範用的 Python 程式、相依套件清單 (requirements.txt)，以及完整的開發環境架設說明。使用者可依說明文件快速建立開發環境、安裝套件並執行範例。

## 7.1 WSL 安裝 (Windows Subsystem for Linux)
WSL(Windows Subsystem for Linux)是 Windows 內建的 Linux 相容層，可在不裝虛擬機、不重開機
的情況下，直接在 Windows 上執行 Linux 環境(如 Ubuntu)。
WSL的好處是：
- 在 Windows 裝置上，直接使用 Linux 常用工具(bash、apt、python、gcc、git…等)，與原生 Linux 相似。
- 檔案可與 Windows 共用，開發工具(VS Code)可透過 Remote – WSL 無縫使用。

> 建議：若控制器端是 Windows 系統，在 Windows 中安裝 WSL，於 WSL 的 Linux 環境下進行開發會更加方便、穩定。

1. 安裝 WSL + Ubuntu (22.04)

    以系統管理員身分打開Windows CMD(命令提示字元)或者Windows Powershell，輸入以下命令
    ```cmd
    wsl --install -d Ubuntu-22.04
    ```

    安裝流程說明：
    1. 按下 Enter 後，系統會自動下載與安裝(可看到進度條)。
    2. 安裝完成後，依提示重新啟動(若系統要求)。
    3. 首次啟動 Ubuntu 時，系統會要求你設定 使用者名稱(username)與 密碼(password)，請依畫面指示完成。
    > 註：輸入密碼時，畫面上不會顯示任何字元或符號(屬正常現象)。請直接輸入後按下 Enter 完成設定。

2. 更新 Ubuntu

    若你目前已跳出 Windows CMD(命令提示字元)或者 Windows Powershell，請再以系統管理員身分打開 Windows CMD(命令提示字元)或者 Windows Powershell，輸入以下命令，即可進入 wsl:
    ```bash
    wsl
    ```

    首次進入到Ubuntu系統後，請先更新Ubuntu系統套件，輸入已下指令:
    ```bash
    sudo apt update
    sudo apt -y upgrade
    ```
    按下 Enter 後系統會自動下載與安裝更新。完成後，WSL就算設定完成，即可開始使用。

## 7.2 安裝 Python 與依賴程式庫
1. 安裝 Python3 與 Venv:

    > Venv (virtual environment)
    > Venv是 Python 內建的「虛擬環境」工具。它會在專案裡建立一個獨立的 Python 解譯器與套
    > 件空間，避免不同專案的套件版本互相干擾。
    > Venv好處：
    > - 專案相依「封裝在本資料夾」，不污染系統環境
    > - 可為不同專案使用不同版本的套件/甚至不同 Python 版
    > - 搭配 requirements.txt 快速重建環境

    在 WSL(Ubuntu)中安裝 Python 與 venv:
    ```bash
    sudo apt install python3 python3-venv python3-pip
    ```

2. 安裝依賴程式庫:

    - 將 GitHub 上的程式碼 clone 到本機
        ```bash
        git clone -b main https://github.com/foxtron-ev/foxtronpi-pyclient
        cd foxtronpi-pyclient
        ```

    - 建立與啟用 Venv (其venv名可自訂，此處以Pi為例):
        ```bash
        python3 -m venv Pi
        source Pi/bin/activate
        ```
        > 看到命令列前面出現 (Pi)，表示 venv 已啟用。
        > 如需要關閉venv，可以執行 deactivate 指令，看到命令列前面 (Pi) 消失，表示venv已停用。
    
    - 安裝依賴套件 (在已啟用的 venv 中執行):
        ```bash
        python -m pip install -U pip
        pip install -r requirements.txt
        ```
