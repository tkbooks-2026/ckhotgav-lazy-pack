---
name: phpmysql
description: PHP + MySQL 專案強制規範。當使用者提及「php+mysql」「PHP專案」「資料庫專案」「新專案」「gdkshop」「進銷存」「POS」「合作社」「Laravel」「安裝精靈」「匯入匯出」「備份還原」或連結到 `phpmysql` 的任何專案時載入。內容涵蓋安裝精靈、密碼小眼睛、安全檢查、備份還原、操作紀錄、CRUD 與匯入匯出、PDF/Excel輸出、以及標準目錄結構。
---

# PHP + MySQL 專案開發規範（phpmysql）

## 基本原則

- 使用繁體中文介面
- 所有頁面須支援 RWD（手機 / 平板 / 電腦）
- 每個功能都要有防呆機制
- Laravel 專案採 API + SPA 分離架構（Vue / React），可部署於傳統共享主機

---

## 1. 環境偵測安裝精靈

任何 PHP + MySQL 專案都必須包含安裝精靈，讓使用者**不需要手動編輯任何設定檔**。

### 安裝流程

```
瀏覽器開啟網址 → 安裝精靈啟動
  ├── Step 1: 伺服器環境檢查
  │   ├── PHP 版本 ≥ 8.1
  │   ├── 必要擴充檢測（PDO, MySQLi, JSON, GD, XML, MBString, OpenSSL,
  │   │   fileinfo, BCMath, Ctype, cURL, Filter, Hash, Session, Tokenizer）
  │   ├── 檔案寫入權限（storage, cache, uploads）
  │   └── 顯示 ✅ / ❌ 清單，缺的提示安裝方式
  │
  ├── Step 2: 資料庫設定
  │   ├── 主機（自動填入 localhost / 127.0.0.1）
  │   ├── 埠號（預設 3306）
  │   ├── 資料庫名稱
  │   ├── 帳號
  │   ├── 密碼（小眼睛切換顯示）
  │   └── [測試連線] 按鈕 → 成功才可下一步
  │
  ├── Step 3: 管理員設定
  │   ├── Email（作為登入帳號）
  │   ├── 密碼（小眼睛）
  │   ├── 確認密碼
  │   └── 系統名稱 / 商店名稱
  │
  ├── Step 4: 初始化
  │   ├── 執行資料庫遷移（自動建立資料表）
  │   ├── 寫入 .env 設定檔
  │   ├── 建立管理員帳號
  │   └── 鎖定安裝路由（installed.lock）
  │
  └── 完成 → 跳轉登入頁
```

### 技術實作要求

- 安裝精靈本身不能依賴 `.env`（還沒建立）
- 安裝前檢查 `installed.lock`，已安裝則拒絕存取安裝頁
- 安裝過程每一步必須驗證後才能繼續
- 資料庫連線測試使用使用者輸入的參數即時連線
- 安裝完成後自動產生 `installed.lock` 或寫入資料庫標記
- API 安裝模式：提供 RESTful 安裝端點給 SPA 前端呼叫

---

## 2. 密碼小眼睛功能

**每個密碼輸入框都必須實作：**

```html
<div class="password-wrapper">
  <input type="password" id="password" />
  <button class="toggle-pwd" onclick="togglePassword('password', this)">
    <!-- 眼睛 SVG 或文字 -->
  </button>
</div>
```

```javascript
function togglePassword(id, btn) {
  const input = document.getElementById(id);
  const isPassword = input.type === 'password';
  input.type = isPassword ? 'text' : 'password';
  btn.classList.toggle('visible', isPassword);
}
```

### 適用範圍

- 登入頁面
- 註冊 / 安裝精靈
- 修改密碼
- 資料庫設定頁
- API Token 顯示
- 任何密碼 / 機密輸入欄位
- Laravel 使用 Livewire / Vue 元件封裝，全域共用

---

## 3. 安全性檢查（強制）

### 資料庫安全

- 所有 SQL 查詢必須使用 **Prepared Statement / Parameterized Query**
- 嚴禁字串拼接 SQL（包含 ORDER BY、LIKE 等）
- 使用 Eloquent ORM 或 PDO Prepared Statements
- 資料表欄位須有適當的資料類型與長度限制
- 密碼不得明碼儲存，使用 `password_hash()` + `PASSWORD_BCRYPT` 或 `PASSWORD_ARGON2ID`
- Token 使用 `random_bytes()` / `bin2hex()` 產生
- 備份檔案不得存放在公開目錄

### 檔案安全

- 所有上傳檔案須檢查：
  - 副檔名白名單（僅允許 `.jpg`, `.png`, `.pdf` 等必要類型）
  - MIME type 驗證（`finfo_file()`）
  - 檔案大小上限
  - 重新命名為隨機檔名（`uniqid()` + 副檔名）
- 上傳目錄禁止執行 PHP（`.htaccess` 或 Nginx 設定關閉）
- 敏感設定檔（.env, config/database.php）禁止公開存取
- Laravel 的 `storage/` 目錄不允許直接 URL 存取

### 程式安全

- CSRF Token：所有 POST/PUT/DELETE 請求必須驗證
- XSS 防護：所有輸出使用 `htmlspecialchars()` / Blade `{{ }}`
- SQL Injection：Prepared Statements + ORM
- Session 安全：
  - `session.use_only_cookies = 1`
  - `session.cookie_httponly = 1`
  - `session.cookie_samesite = "Lax"`
  - Session 逾時機制
- 後端驗證所有輸入（前端驗證僅供 UX，不可仰賴）
- 嚴禁直接暴露 `$_SERVER`, `$_ENV` 等敏感變數

### API 安全

- API 使用 Token（Laravel Sanctum）或 JWT 認證
- API 請求須限速（Rate Limiting）
- CORS 設定僅允許必要來源
- 敏感操作（刪除、匯出）須記入操作紀錄

---

## 4. 備份與還原

### 備份功能

```php
// 備份內容
- 完整資料庫（mysqldump / 逐表匯出）
- .env 設定檔（可選）
- 上傳檔案目錄

// 備份方式
- 手動：管理後台點擊備份
- 自動：可設定排程（每日 / 每週）
- 備份檔案存儲：
  - 本地 storage/backups/
  - S3 / 雲端儲存（可選）
```

### 還原功能

```php
// 還原方式
- 管理後台上傳備份檔 → 一鍵還原
- 還原前自動備份現有資料（防呆）
- 還原完成後顯示結果報告
```

### UI 要求

- 備份清單：檔名、大小、建立時間、狀態
- 每個備份可下載 / 刪除
- 還原前雙重確認對話框
- 備份檔案大小顯示（MB / GB）

---

## 5. 操作紀錄（Activity Log）

所有重要操作必須記錄，不可關閉。

### 記錄內容

```php
// 至少記錄
- 登入 / 登出（含 IP、User-Agent、時間）
- 新增 / 編輯 / 刪除任何資料
- 匯入 / 匯出操作
- 備份 / 還原操作
- 權限變更
- 密碼變更
- 系統設定變更
```

### 資料表結構

```sql
activity_logs
  - id            BIGINT PRIMARY KEY AUTO_INCREMENT
  - user_id       INT (nullable, 系統操作可為 null)
  - action        VARCHAR(50)    -- login, create, update, delete, export, backup, restore
  - description   TEXT           -- 人類可讀描述
  - model_type    VARCHAR(100)   -- 操作的資料表 / 模型
  - model_id      INT (nullable)
  - old_values    JSON (nullable)
  - new_values    JSON (nullable)
  - ip_address    VARCHAR(45)
  - user_agent    TEXT
  - created_at    DATETIME
```

### UI 要求

- 管理後台可查詢操作紀錄
- 篩選：日期範圍、操作類型、使用者
- 分頁顯示
- 紀錄不可刪除（僅系統自動清除過期資料）

### 實作建議

```php
// Laravel helper function — 全域可呼叫
activity_logs('create', '新增商品 #123「可口可樂」', Product::class, $product->id);

// Controller trait — 自動記錄 CRUD
use App\Traits\LogsActivity;
```

---

## 6. CRUD + 匯入匯出

### 基本 CRUD 原則

每個資料模組都必須包含：
- **列表頁**：表格顯示 + 搜尋 + 分頁 + 排序
- **新增頁**：表單驗證 + 重複檢查 + 防呆
- **編輯頁**：載入現有資料 + 編輯存檔
- **刪除**：軟刪除（`deleted_at`）為主，硬刪除需雙重確認
- **檢視**：單筆資料詳細頁

### 匯入功能

- 支援 CSV / Excel 匯入
- 上傳後可預覽（顯示前 5 筆）
- 自動比對標頭欄位
- 匯入報告：成功幾筆、失敗幾筆、失敗原因
- 失敗資料可下載錯誤清單
- 防止重複匯入（依唯一值檢查）

### 匯出功能

- 列表頁必須有匯出按鈕
- 可選擇匯出全部或篩選後的資料
- 支援格式：Excel（.xlsx）+ CSV

---

## 7. PDF 與 Excel 輸出

### PDF 輸出

- 使用 DomPDF / TCPDF / Barryvdh Laravel-DomPDF
- 所有報表須可輸出 PDF
- 含頁碼、日期、標題
- 支援直印 / 橫印
- 中文字型支援

### Excel 輸出

- 使用 PhpSpreadsheet / Laravel-Excel
- 所有資料列表須可輸出 Excel
- 匯出檔案名含日期（如 `商品清單_20260616.xlsx`）
- 支援多工作表（如有需要）

### 輸出範圍

- 商品清單
- 庫存報表
- 銷售報表
- 進貨明細
- 會員清單
- 操作紀錄
- 任何列表頁面的資料

---

## 8. 標準目錄結構（Laravel 專案）

```
專案/
├── app/
│   ├── Http/Controllers/
│   │   ├── Admin/            管理後台控制器
│   │   └── Api/              API 控制器
│   ├── Models/
│   ├── Services/             商業邏輯層
│   └── Traits/
├── database/
│   ├── migrations/
│   └── seeders/
├── resources/
│   ├── views/
│   │   ├── install/          安裝精靈
│   │   ├── admin/            後台頁面
│   │   └── components/       共用元件
│   └── js/
├── routes/
│   ├── web.php
│   └── api.php
├── storage/
│   ├── backups/              備份檔案
│   └── exports/              暫存匯出檔
└── public/
    └── uploads/              使用者上傳檔案
        └── .htaccess         禁止執行 PHP
```

---

## API + SPA 分離架構（Laravel 11+）

```
backend/                    Laravel API 專案
  ├── app/Http/Controllers/Api/
  ├── app/Models/
  ├── database/migrations/
  ├── routes/api.php
  ├── app/Helpers.php       全域 helper
  ├── app/Providers/
  └── app/Exceptions/

frontend/                   Vue 3 SPA 前端
  ├── src/
  │   ├── api/              axios 封裝層
  │   ├── stores/           Pinia 狀態管理
  │   ├── router/           Vue Router
  │   ├── views/           頁面元件
  │   ├── layouts/         共用佈局
  │   └── components/      共用元件
  ├── dist/                編譯輸出（上傳至主機）
  └── index.html            入口
```

---

## 啟動關鍵詞

當使用者說以下內容時自動載入此 skill：
- 「php+mysql」
- 「PHP 專案」/「資料庫專案」/「新專案」
- 「gdkshop」
- 「合作社」/「進銷存」/「POS」
- 「Laravel」/「安裝精靈」
- 「匯入匯出」/「備份還原」
- 連結到 `phpmysql` 的任何專案
