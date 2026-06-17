---
name: 13-php-env
title: PHP + MySQL 開發環境建置
description: 偵測並安裝 PHP、MySQL、Composer，提供三種安裝方式讓使用者選擇
---

# PHP + MySQL 開發環境建置

## 步驟 1：偵測當前已安裝工具

執行以下指令檢查，標記哪些已裝、哪些未裝：

| 工具 | 指令 | 預期 |
|------|------|------|
| PHP | `php --version` | 8.1+ |
| MySQL / MariaDB | `mysql --version` | 任何版本 |
| Composer | `composer --version` | 2.0+ |
| Docker (選用) | `docker --version` | 任何版本 |

## 步驟 2：詢問安裝方式

根據偵測結果，列出已裝與未裝項目，然後詢問使用者：

> 「請問你想用哪種方式安裝缺少的環境？」

提供三個選項：

### 選項①：直接安裝（適合上傳虛擬主機）
逐項安裝到系統：
- **PHP**：`winget install --id PHP.PHP.8.4 -e --source winget`
  - 若 winget 不可用，引導至 https://windows.php.net/download/
  - 安裝後檢查 PHP 是否在 PATH，若無則加入 `C:\tools\php` 到使用者 PATH
  - 產生或修改 `php.ini`：啟用 `extension=mysqli`、`extension=pdo_mysql`、`extension=mbstring`
- **Composer**：`winget install --id Composer.Composer -e --source winget`
  - 若 winget 不可用，引導至 https://getcomposer.org/download/
- **MySQL**：`winget install --id Oracle.MySQL -e --source winget`
  - 或安裝 MariaDB：`winget install --id MariaDB.Server -e --source winget`
  - 安裝時記錄 root 密碼，引導使用者執行 `mysql_secure_installation`

### 選項②：Docker（適合 NAS 部署、團隊協作）
產生 `docker-compose.yml` 到專案目錄：
```yaml
version: '3.8'
services:
  php:
    image: php:8.4-apache
    ports:
      - "8080:80"
    volumes:
      - ./:/var/www/html
  mysql:
    image: mysql:8.4
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: app
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql
volumes:
  mysql_data:
```
- 確認 Docker Desktop 已安裝（若無引導至 https://www.docker.com/products/docker-desktop/）
- 執行 `docker compose up -d` 啟動

### 選項③：Laragon（適合快速啟動、純本地開發）
- 引導使用者在瀏覽器下載：https://laragon.org/download/
- 安裝後開啟 Laragon → Menu → PHP → Version → 切換到 8.x
- 確認 MySQL 已啟動（Laragon 預設內建 MariaDB）

## 步驟 3：驗證安裝

根據選擇的方式，執行對應的驗證：

| 工具 | 驗證指令 |
|------|---------|
| PHP | `php --version` (8.1+) |
| MySQL | `mysql --version` |
| Composer | `composer --version` (2.0+) |
| Docker | `docker compose version` (選項②) |

## 平台提示
- **Windows**：以上指令適用，使用 `npm.cmd` 避免 PowerShell 權限問題
- **macOS/Linux**：`winget` 不可用，改用 `brew install php` / `apt install php` 等套件管理器
