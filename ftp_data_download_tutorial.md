# FTP 資料下載教學筆記

本教學將介紹如何透過 FTP 命令列下載檔案，並說明三種不同的方法，包括：

1. FTP 登入教學  
2. 方法一：單筆下載  
3. 方法二：批次下載（限當前目錄）  
4. 方法三：遞迴下載所有檔案（包含子目錄）

---

## ✅ 一、FTP 登入教學

### 步驟 1：打開終端機（Terminal）

macOS 或 Linux 可使用內建終端機，Windows 可使用 `cmd` 或安裝如 Git Bash / WSL。

### 步驟 2：輸入 FTP 指令並登入伺服器

```bash
ftp ftp.example.com(IP)
```

登入後依序輸入使用者帳號與密碼：

```text
Name (ftp.example.com:user): your_username (輸入帳號)
Password: your_password (輸入密碼)
```

登入成功後會出現 `ftp>` 提示符號，表示可以開始操作。

---

## ✅ 二、方法一：單筆下載（一次一個檔案）

### 步驟 1：切換本機存放資料的路徑（例如桌面）

```bash
ftp> lcd ~/Desktop
```

### 步驟 2：列出 FTP 伺服器上的檔案

```bash
ftp> ls
```

### 步驟 3：使用 `get` 指令逐一下載檔案

```bash
ftp> get Itri-QC高度計資料_202412010937-12180937by241218093752.csv
ftp> get Itri-QC高度計資料_202412180937-12180940by241218094018.csv
```

---

## ✅ 三、方法二：使用 `mget` 批次下載（限當前目錄）

### 步驟 1：切換本機存放資料的路徑

```bash
ftp> lcd ~/Desktop
```

### 步驟 2：啟用自動確認（避免每個檔案都要輸入 y/n）

```bash
ftp> prompt
```

### 步驟 3：使用萬用字元下載所有 `.csv` 檔案

```bash
ftp> mget *.csv
```

⚠️ 注意：此方法**無法下載子目錄內的檔案**，僅限當前目錄。

---

## ✅ 四、方法三：下載所有檔案（包含子目錄）

若要一次下載 FTP 上的所有檔案與資料夾（含子目錄），建議改用 `wget` 或 `lftp` 工具。

### 方法 3-1：使用 `wget`（推薦）

請先安裝 wget（macOS 可用 `brew install wget`）：

```bash
wget --recursive --no-parent --user=your_username --password=your_password ftp://ftp.example.com/path/
```

說明：

- `--recursive`：遞迴下載（含子目錄）
- `--no-parent`：不回到上層目錄
- `--user` / `--password`：FTP 帳號密碼
- `ftp://...`：FTP 路徑，可指定目標目錄

### 方法 3-2：使用 `lftp`（高階用法）

```bash
lftp -u your_username,your_password ftp://ftp.example.com
```

登入後使用：

```bash
mirror -c -e /remote/path /local/path
```

參數說明：

- `-c`：繼續中斷的下載
- `-e`：刪除本地端不存在於遠端的檔案
- `/remote/path`：遠端 FTP 目錄
- `/local/path`：本機儲存位置

---

## 🔚 結語

- 若只需單層目錄的資料，可用 FTP 內建指令（`get`, `mget`）。
- 若需整個 FTP 檔案樹（含子目錄），建議使用 `wget` 或 `lftp`。
- 切記保護帳號密碼，避免帳密外洩。
