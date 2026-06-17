---
name: 13-php-env
title: PHP + MySQL 開發必要要求
description: 開發 PHP+MySQL 時的必要要求 — 環境檢查、PHP 擴展/設定、MySQL 編碼/權限、專案結構、Composer 初始化
---

# PHP + MySQL 開發必要要求

## 步驟 1：偵測當前環境

執行以下指令，標記已裝/未裝：

| 工具 | 指令 | 預期 |
|------|------|------|
| PHP | `php --version` | 8.1+ |
| MySQL / MariaDB | `mysql --version` | 任何版本 |
| Composer | `composer --version` | 2.0+ |
| Docker (選用) | `docker --version` | 任何版本 |

若缺少工具，提供三種安裝方式讓使用者選擇：

### 選項①：直接安裝（適合上傳虛擬主機）
- **PHP**：`winget install --id PHP.PHP.8.4 -e --source winget`
- **Composer**：`winget install --id Composer.Composer -e --source winget`
- **MySQL**：`winget install --id Oracle.MySQL -e --source winget`
- 若 winget 不可用，引導至對應官網下載

### 選項②：Docker（適合 NAS 部署）
產生 `docker-compose.yml`，執行 `docker compose up -d` 啟動

### 選項③：Laragon（適合快速本地開發）
引導至 https://laragon.org/download/ 下載安裝

## 步驟 2：PHP 必要擴展

確認已啟用的 PHP 擴展。執行 `php -m` 檢查，以下為開發 PHP+MySQL 的必需擴展：

| 擴展 | 用途 | 啟用方式 (php.ini) |
|------|------|-------------------|
| `mysqli` | MySQL 傳統連接 | `extension=mysqli` |
| `pdo_mysql` | MySQL PDO 連接（推薦） | `extension=pdo_mysql` |
| `mbstring` | 多國語系字串處理 | `extension=mbstring` |

選用但建議開啟：

| 擴展 | 用途 |
|------|------|
| `openssl` | 密碼雜湊、HTTPS 請求 |
| `json` | JSON 編解碼 |
| `fileinfo` | 檔案類型偵測 |
| `gd` / `imagick` | 圖片處理 |
| `zip` | 壓縮檔案 |
| `curl` | HTTP 請求 |
| `intl` | 國際化支援 |

若缺少則在 `php.ini` 中移除對應行前的 `;` 註解。

## 步驟 3：php.ini 開發建議設定

確認 `php.ini` 包含以下值：

```
display_errors = On
error_reporting = E_ALL
date.timezone = Asia/Taipei
upload_max_filesize = 32M
post_max_size = 32M
memory_limit = 256M
max_execution_time = 300
```

> ⚠️ 上線前務必將 `display_errors` 改為 `Off`，避免暴露原始碼路徑。

## 步驟 4：MySQL 必要要求

### 4.1 字元編碼與排序

建立資料庫時使用 Unicode：

```sql
CREATE DATABASE `app` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

建立資料表時指定編碼：

```sql
CREATE TABLE `users` (
  `id` INT AUTO_INCREMENT PRIMARY KEY,
  `name` VARCHAR(100) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

> **utf8mb4** 支援 Emoji 與罕見字，**utf8mb4_unicode_ci** 是通用排序規則。
> 避免使用 `utf8`（MySQL 的 utf8 最多只到 3 bytes，不支援 Emoji）。

### 4.2 連線校對

確認 PHP 連線 SQL 後執行：
```sql
SET NAMES 'utf8mb4' COLLATE 'utf8mb4_unicode_ci';
```

PDO 連線範例：
```php
$dsn = 'mysql:host=127.0.0.1;port=3306;dbname=app;charset=utf8mb4';
$pdo = new PDO($dsn, 'user', 'password', [
    PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    PDO::ATTR_EMULATE_PREPARES => false,
]);
```

### 4.3 使用者權限原則

開發時不要使用 root，建立專屬使用者：
```sql
CREATE USER 'app_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON `app`.* TO 'app_user'@'localhost';
FLUSH PRIVILEGES;
```

## 步驟 5：專案結構初始化

詢問使用者專案名稱，建立以下結構：

```
專案名稱/
├── public/              # Web 根目錄（DocumentRoot）
│   └── index.php        # 進入點
├── src/                 # PHP 原始碼
├── config/              # 設定檔
│   └── database.php     # 資料庫連線設定
├── sql/                 # SQL 遷移腳本
│   └── 001_create_users.sql
├── vendor/              # Composer（自動產生）
├── composer.json        # Composer 設定
├── .gitignore           # Git 忽略規則
└── README.md            # 專案說明
```

> **建議**：`DocumentRoot` 指向 `public/`，避免直接存取 `src/` 或 `config/`。

## 步驟 6：Composer 自動載入

建立或更新 `composer.json`：
```json
{
  "name": "yourname/project",
  "autoload": {
    "psr-4": {
      "App\\": "src/"
    }
  },
  "require": {
    "php": ">=8.1"
  }
}
```

執行 `composer install` 產生自動載入。在 `public/index.php` 引入：
```php
<?php
require_once __DIR__ . '/../vendor/autoload.php';
```

## 步驟 7：初始 .gitignore

寫入 `.gitignore`：
```
vendor/
.env
*.log
.DS_Store
thumbs.db
```

## 步驟 8：除錯模式

開發期間在進入點啟用：
```php
<?php
error_reporting(E_ALL);
ini_set('display_errors', '1');
```

> ⚠️ 上線前移除這兩行。

## 步驟 9：啟動開發伺服器

### 內建 PHP Server
```bash
php -S localhost:8000 -t public/
```

### Docker
```bash
docker compose up -d
# 瀏覽 http://localhost:8080
```

### Laragon
開啟 Laragon → 右鍵 → Web → 啟動

## 驗證清單

完成後逐一確認：

| # | 項目 | 確認方式 |
|---|------|---------|
| 1 | PHP 版本 | `php --version` (8.1+) |
| 2 | MySQL 連線 | PHP 執行 `new PDO(...)` 成功 |
| 3 | Composer 載入 | `vendor/autoload.php` 可引入 |
| 4 | php.ini 設定 | `display_errors = On` 已啟用 |
| 5 | MySQL 編碼 | 資料庫使用 `utf8mb4` |
| 6 | 專案結構 | `public/` `src/` `config/` 已建立 |
| 7 | 開發伺服器 | `http://localhost:8000` 可訪問 |
| 8 | .gitignore | `vendor/` `.env` 已忽略 |

## 平台提示
- **Windows**：確認 PHP + MySQL 在 PATH 中
- **macOS/Linux**：`winget` 改用 `brew` / `apt`
