# Python 教學釋疑集

## 📚 目錄

1. [問題一：lambda 為什麼需要用 `x=i`？](#問題一：lambda-為什麼需要用-`x=i`？)
2. [問題二：if expression 判斷變數是否有內容](#問題二：if-expression-判斷變數是否有內容)
3. [問題三：pycache 與 pyc 檔案是什麼？](#問題三：pycache-與-pyc-檔案是什麼？)
4. [問題四：pyttsx3 無法發聲的常見問題與排除方式](#問題四：pyttsx3-無法發聲的常見問題與排除方式)
5. [問題五：import 套件失敗](#問題五：import-套件失敗)
6. [問題六：pandas 指令講解](#問題六：pandas-指令講解)
7. [問題七：matplotib 指令講解](#問題七：matplotib-指令講解)
---


## 問題一：lambda 為什麼需要用 `x=i`？

![image](https://hackmd.io/_uploads/rJZ6RHnGlx.png)


### 🧠 重點觀念：lambda 是「延後執行」的函式

你可以把這句想成一張「備忘錄」：

```python
lambda: print(i)
```

意思是：等你按下按鈕的時候，才會去找 `i` 的值並印出來。
但如果你用的是共用的 `i` 變數，那等按下去時，`i` 已經變成最後一個值（例如 2）了。



### 👨‍💻 錯誤寫法逐回合模擬

```python
for i in range(3):
    btn = Button(text=str(i), command=lambda: print(i))
```

#### 🔁 第 1 回合 i = 0
- 建立按鈕文字："0"
- 備忘錄：按下去時印 `i`

#### 🔁 第 2 回合 i = 1
- 建立按鈕文字："1"

#### 🔁 第 3 回合 i = 2
- 建立按鈕文字："2"

#### 📌 點任一按鈕時：
- 執行 `print(i)` → 此時 i = 2
- 所以三顆按鈕都印出 2 ❌



### ✅ 正確寫法

```python
for i in range(3):
    btn = Button(text=str(i), command=lambda x=i: print(x))
```

#### 說明：
- `lambda x=i:` 表示把當下的 `i` 值記到 `x` 中，這樣就不受後續 i 改變的影響。



### ✅ 點按鈕結果：
- 第一顆印 0
- 第二顆印 1
- 第三顆印 2



### 🍱 便當比喻總結

| 寫法                    | 行為說明 |
|-------------------------|----------|
| `lambda: print(i)`      | 等按下去時才去看 `i`，結果三個都看到最後的值 |
| `lambda x=i: print(x)`  | 當下就把 `i` 的值存進 `x`，所以每個 lambda 各自記住不同的值 |



## 問題二：if expression 判斷變數是否有內容

![image](https://hackmd.io/_uploads/BkUs1Lhzle.png)

### ✅ 你可以這樣比喻：

```python
if expression:
```

就像是在問：**「這個箱子有沒有東西？」**



### ✅ 範例說明

```python
expression = ""
if expression:
    print("有東西")
else:
    print("沒東西")    # ✅ 印出這行
```

```python
expression = "456+7"
if expression:
    print("有東西")    # ✅ 印出這行
else:
    print("沒東西")
```



### ✅ 判斷對照表

| 變數類型 | 判斷為 False 的情況 |
|----------|----------------------|
| 字串     | `""` 空字串          |
| 數字     | `0`                  |
| List     | `[]` 空列表          |
| Dict     | `{}` 空字典          |
| None     | `None`               |



### 🧠 建議記住：

> Python 幫你做了很多「真假判斷」的簡化，不用寫 `len(expression) > 0`。  
> 使用 `if expression:` 是 Pythonic 的寫法，簡潔且可讀性好！

---

## 問題三：pycache 與 pyc 檔案是什麼？


![line_oa_chat_250610_153856](https://hackmd.io/_uploads/ByBp50LXlg.jpg)


你紅框圈起來的 `__pycache__/__init__.cpython-37.pyc` 是 Python 正常執行過程中**自動產生的快取檔案（bytecode 檔）**，並不是錯誤。



### ✅ 為什麼會出現這個 .pyc 檔？

當你執行一個 Python 模組（例如 `main.py`）並匯入該資料夾內的模組時，Python 解譯器會：

1. 將 `.py` 檔案編譯為 `.pyc`（Compiled Python Code）。
2. 將 .pyc 檔案放進 `__pycache__` 資料夾。
3. 檔名中的 `cpython-37` 表示使用 CPython 3.7 解譯器產生的版本。



### ✅ 可以刪掉嗎？

可以，但下次執行又會重新產生。
建議**不用理它**，也不用版本控制 `.pyc`。



## 問題四：pyttsx3 無法發聲的常見問題與排除方式

## ✅ 問題描述

在執行以下程式碼時，雖然 `pyttsx3` 套件已安裝，但卻沒有聲音輸出：

```python
import pyttsx3
engine = pyttsx3.init()
engine.say(f"請 {patient_id} 號 {name} 到 {room} 報到")
engine.runAndWait()
```



## 🔍 常見原因與解法

### 1️⃣ 音訊輸出裝置錯誤或無法使用
- 確認喇叭是否開啟、未靜音
- 若透過遠端桌面使用，音效可能無法播放
- 有多個音訊裝置時可能被導向錯誤輸出

### ✅ 解法：
```python
import pyttsx3
engine = pyttsx3.init()
voices = engine.getProperty('voices')
for i, voice in enumerate(voices):
    print(f"{i}: {voice.name} ({voice.languages})")

engine.setProperty('voice', voices[0].id)  # 嘗試使用不同 index
```


### 2️⃣ Windows 語音引擎（SAPI）未正確安裝

#### ✅ 方法一：使用「執行視窗」開啟語音設定
1. `Win + R` → 輸入 `ms-settings:speech` → Enter
2. 檢查是否有預設語音引擎（如中文、英文等）

#### ✅ 方法二：手動開啟語音設定
1. 設定 → 時間與語言 → 語音
2. 檢查是否有預設語音引擎（如中文、英文等）



### 3️⃣ 語音內容變數為空

確認 `patient_id`、`name`、`room` 不為空，否則可能會跳過發聲：

```python
print(f"請 {patient_id} 號 {name} 到 {room} 報到")  # 先確認字串內容
```

### 4️⃣ 被其他軟體鎖住音訊設備

如 Zoom、Skype、音效卡軟體可能佔用音源導致無法播放



## 問題五：import 套件失敗

![line_oa_chat_250611_152800](https://hackmd.io/_uploads/SJwUF0UQlg.jpg)
![line_oa_chat_250611_152802](https://hackmd.io/_uploads/B1DUKC87le.jpg)


### ❗ 錯誤訊息範例

```text
ImportError: DLL load failed while importing _multiarray_umath: 找不到指定的模組。
ImportError: numpy.core._multiarray_umath failed to import
```

---

### 🔍 問題原因

numpy 與 pandas 套件版本不相依


造成的常見原因之一是：

#### 1. pip 與 conda 混用導致環境衝突

- `conda` 安裝的 numpy 會搭配相依的 C/C++ DLL 檔案
- `pip` 安裝的版本可能會覆蓋掉 conda 安裝的檔案
- 最終導致 DLL 找不到或載入失敗

---

### ✅ 解決方法

請依以下步驟完整清除並重新安裝 `numpy` 和 `pandas`（建議全部使用 conda）：

```bash=
#方法一
#需要在終端機操作
pip uninstall numpy pandas
conda uninstall numpy pandas
conda install -c conda-forge numpy pandas
```


```bash=
#方法二
#在IDE上操作
!pip uninstall numpy pandas
!conda uninstall numpy pandas
!conda install -c conda-forge numpy pandas
```


> ✅ 建議：統一使用 `conda` 管理套件，比較穩定、避免相依性問題。

---


## 問題六：pandas 指令講解

![image](https://hackmd.io/_uploads/SJ4DYoP7ex.png)

## 📅 2.1 日期欄位處理

```python
df['上架日期'] = pd.to_datetime(df['上架日期'])
df['上架年份'] = df['上架日期'].dt.year
```

### ✅ 詳細說明：
- `pd.to_datetime()`：將字串轉換為 `datetime` 格式，便於時間的運算與分析。
- `.dt.year`：使用 `.dt` 屬性存取日期中的年分資訊。

🔗 官方文件：
- [`pd.to_datetime`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.to_datetime.html)
- [`Series.dt`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.html)

---

## ⏱️ 2.2 片長欄位拆解

```python
df['片長_int'] = df['片長'].str.extract(r'(\d+)').astype(float)
```

### ✅ 詳細拆解與說明：

```text
df['片長'].str.extract(r'(\d+)')
          ▲     ▲        ▲
          │     │        └─── ➄ ')'：結束群組。
          │     └───────── ➃ '\d+'：表示數字（\d）出現一次以上（+ 是量詞）。
          │
          └─────────────── ➂ '('：開始一個群組，會被 extract 捕捉。
```

- `.str`：表示這是字串操作，適用於 Series 資料。
- `.extract()`：根據正規表達式，擷取字串中指定的部分。
- `r'(\d+)'`：Python 原始字串，避免跳脫字元誤判。
- `(\d+)`：正規表達式語法，括號表示要擷取的群組，`\d+` 代表「一個或多個數字」。
- `.astype(float)`：將擷取出來的字串數字轉換為浮點數。

---

```python
df['片長類型'] = df['片長'].str.extract(r'([a-zA-Z ]+)')
```

df['片長'].str.extract(r'([a-zA-Z ]+)')
          ▲     ▲         ▲
          │     │         └─── ➄ ')'：群組結尾，用來封閉括號區段。
          │     └─────────── ➃ '[a-zA-Z ]+'：匹配一個或多個英文字母或空格。
          │
          └───────────────── ➂ '('：正規表示式的群組開頭，用於 `.extract()` 擷取。


- 類似地，這行程式碼是擷取 `片長` 欄位中的英文片長單位，如 "Minutes"、"Seasons"。
- `[a-zA-Z ]+`：表示擷取英文單字與空格的連續字元。

🔗 官方文件：
- [`Series.str.extract`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.str.extract.html)
- [`astype`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.astype.html)

---

## 📚 2.3 多值欄位拆分

```python
df['題材清單'] = df['題材'].str.split(', ')
df['演員清單'] = df['演員'].fillna('').str.split(', ')
```

### ✅ 詳細說明：

    df['欄位'].str.split(', ')
               ▲          ▲
               │          └─── 分隔符號（字串）：逗號加空格。
               └───────────── 使用 .str 操作整欄字串欄位。



- `.str.split(', ')`：
  - 將字串以逗號與空格為界切割，轉換為 Python list。
  - 範例：`"Drama, Comedy"` → `["Drama", "Comedy"]`

---
    df['欄位'].fillna('')
               ▲
               └─── 補上空字串取代缺失值（NaN）



- `.fillna('')`：
  - 將缺失值（NaN）填補為空字串，避免 `.str` 操作時發生錯誤。

🔗 官方文件：
- [`Series.str.split`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.str.split.html)
- [`fillna`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.fillna.html)


## 問題七：matplotib 指令講解

![image](https://hackmd.io/_uploads/H1zQ5iPXel.png)

    df.groupby(['上架年份', '類別']).size().unstack().fillna(0)
    ▲         ▲                 ▲       ▲         ▲
    │         │                 │       │         └─── ➄ `.fillna(0)`：將 NaN 缺漏值補為 0。
    │         │                 │       └──────────── ➃ `.unstack()`：將內層索引（類別）轉為欄位（形成橫向表格）。
    │         │                 └──────────────────── ➂ `.size()`：計算每個群組的資料數（即作品數量）。
    │         └────────────────────────────────────── ➁ `['上架年份', '類別']`：群組依據的欄位列表（必須是 list）。
    └─────────────────────────────────────────────── ➀ `df.groupby(...)`：以「多欄位」對資料進行群組聚合。


## ✅ 補充說明

- `.groupby(['上架年份', '類別'])`：以 list 格式傳入兩個欄位，進行多層群組。
- `.size()`：統計每個群組的筆數，會回傳一個多層索引的 Series。
- `.unstack()`：將第二層索引（類別）轉為欄位，形成交叉表。
- `.fillna(0)`：補齊缺失欄位的空值，避免後續繪圖或分析錯誤。





### 🧩 為什麼要使用 `['欄位1', '欄位2']`（list）

```python
df.groupby(['上架年份', '類別'])
```

#### ✅ 正確寫法
- 使用 list 傳入多個欄位名稱（如：上架年份 + 類別）
- Python 語法中 `['a', 'b']` 表示一個清單，才能一次傳入多欄位

#### ❌ 錯誤寫法
```python
df.groupby('上架年份', '類別')  # ❌
```
- 這樣會被當作兩個獨立參數傳入，造成錯誤：`TypeError`

---

### 🎯 `ax = 指令介紹

![image](https://hackmd.io/_uploads/HJVi2svXel.png)


```python
ax = year_type.plot(kind='bar', stacked=True, figsize=(12,6))
```
    year_type.plot(kind='bar', stacked=True, figsize=(12,6))
    ▲         ▲      ▲           ▲           ▲
    │         │      │           │           └─── ➄ `figsize=(12,6)`：圖表大小，寬 12 吋、高 6 吋。
    │         │      │           └─────────────── ➃ `stacked=True`：啟用堆疊長條圖（各類別值會疊在一起）。
    │         │      └─────────────────────────── ➂ `kind='bar'`：指定圖的類型為 bar chart（直條圖）。
    │         └───────────────────────────────── ➁ `.plot()`：pandas 的繪圖介面，內部呼叫 matplotlib。
    └──────────────────────────────────────────── ➀ `year_type`：資料來源（需為 DataFrame 結構）。

---

## ✅ 補充說明

- `.plot()`：Pandas 對 Matplotlib 的封裝，可快速繪圖。
- `kind='bar'`：畫直條圖（也可改為 `'line'`、`'pie'` 等）。
- `stacked=True`：開啟堆疊效果（疊在同一根柱子上，適合組成比較）。
- `figsize=(12, 6)`：圖表大小設定，單位為英吋（inch），預設為 (6.4, 4.8)。
- 
### 🎯 為什麼使用 `ax = ...`

```python
ax = df.plot(kind='bar', stacked=True)
```

#### ✅ 說明
- `df.plot()` 回傳的是一個 `Axes` 物件（即圖表本身）
- 儲存為變數 `ax` 代表圖的控制器，可進一步設定標題、標籤等細節

#### 優點
- 可同時控制多個子圖（subplot）
- 適合進階繪圖、客製圖表風格
- 面向物件編程風格（OOP）更清楚、彈性大

---

### 🆚 `ax.set_title()` vs `plt.title()`

| 用法 | 適用時機 | 優點 |
|------|----------|------|
| `ax.set_title()` | ✅ 推薦使用 | 明確設定某張圖的標題 |
| `plt.title()`    | ⛔ 不建議複雜場景 | 只適合簡單單一圖，無法分辨多圖表 |

#### ✅ 推薦寫法：

```python
ax = df.plot()
ax.set_title("不同年份上架的電影與影集數量")
```

#### ⚠️ 傳統寫法（只適合簡單圖表）：

```python
df.plot()
plt.title("不同年份上架的電影與影集數量")
```


### 🔍 plt.tight_layout() 是什麼？


```python
plt.tight_layout()
```
✅ 功能：
👉 自動調整子圖(subplots)或圖表元件（標題、軸標籤、圖例）的位置，讓它們不會重疊、超出畫面、擠在一起。


### 🎯 什麼情況需要用？
#### 不用 tight_layout() 時：
```python
plt.show()
```
🔺 可能出現：
- 標題被切掉
- y 軸標籤重疊
- x 軸標籤重疊
- 子圖之間距太近

#### 加上 tight_layout() 後：
```python
plt.tight_layout()
plt.show()
```
✅ 效果明顯改善，間距會自動調整，看起來更整齊。

### 📦 技術原理簡單說：
它會根據圖表的實際元素空間，自動幫你排版、調整邊界 (padding)。
避免你手動去調整 fig.subplots_adjust(...) 等繁雜設定。

### 📝 注意事項：
必須在所有圖表繪製後、plt.show() 前呼叫。
對於多子圖（subplot）特別重要。

| 用法                   | 說明                 |
| -------------------- | ------------------ |
| `plt.tight_layout()` | 自動優化圖表排版，避免重疊與裁切問題 |
| `plt.show()` 之前使用    | ✅ 建議加上，幾乎沒有副作用     |
