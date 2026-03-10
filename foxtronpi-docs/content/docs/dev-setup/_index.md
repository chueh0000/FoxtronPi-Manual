---
title: "7. 程式開發環境架設"
type: "docs"
weight: 7
---

# 7. 程式開發環境架設

本章介紹如何在 Windows 系統中使用 WSL 架設開發環境。

## 7.1 WSL 安裝 (Windows Subsystem for Linux)
1. **安裝 WSL+Ubuntu (22.04)**:
   ```cmd
   wsl --install -d Ubuntu-22.04
   ```
2. **更新 Ubuntu**:
   ```bash
   sudo apt update
   sudo apt -y upgrade
   ```

## 7.2 安裝 Python 與依賴程式庫
1. **安裝 Python3 與 Venv**:
   ```bash
   sudo apt install python3 python3-venv python3-pip
   ```
2. **Clone 程式碼**:
   ```bash
   git clone -b main https://github.com/foxtron-ev/foxtronpi-pyclient
   cd foxtronpi-pyclient
   ```
3. **建立與啟用 Venv**:
   ```bash
   python3 -m venv Pi
   source Pi/bin/activate
   ```
4. **安裝依賴套件**:
   ```bash
   python -m pip install -U pip
   pip install -r requirements.txt
   ```

## 備註
看到命令列前面出現 `(Pi)`，即表示虛擬環境 (venv) 已啟用。若需關閉，可執行 `deactivate`。
