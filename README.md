# NPA165_NewsScarpe
# 內政部警政署165 全民防騙網_爬蟲

此爬蟲工具專門針對「內政部警政署165全民防騙網」的新聞內容進行設計，旨在自動化地從網站抓取新聞文章資料。

---

## 🚧 遇到的問題與解決方案

### 1. 動態頁面載入

- **問題描述**  
網站使用的是動態渲染技術(SPA)，這意味著當頁面第一次加載時，許多內容尚未立即出現，它會在後續的時間點動態地加載進來。
- **解決方案** 
使用了 Selenium 的 `WebDriverWait` 功能，這允許我們對一個特定的條件進行等待直到它成真。例如，我們可以等待某個特定的元素出現。這確保進行後續操作之前，目標元素已經完全載入到頁面中。

### 2. `Stale Element Reference` 問題

- **問題描述**  
當我們對一個頁面的元素進行操作後（例如點擊），然後返回原始頁面，我們原先獲取的元素引用可能已不再有效，這時再操作這個元素會出現 `stale element reference` 錯誤。
- **解決方案** 
我們在每次循環中都重新獲取元素的引用，這樣可以確保我們使用的元素引用都是最新的，避免了上述問題。

### 3. 動態生成的HTML屬性

- **問題描述**  
網站的HTML結構中存在如 `_ngcontent-xxx` 這樣的屬性，由於是由Angular或其他前端框架動態生成，每次刷新網頁時，`xxx` 的值都可能改變，使得我們無法直接依賴它來定位元素。
- **解決方案** 
我們選擇不直接依賴這些動態生成的屬性。取而代之的是，我們使用了如XPATH、CSS類或其他固定的HTML屬性。，這確保了爬蟲的穩定性。

### 4. 錯誤訊息的重要性

- **問題描述**  
在開發過程中，經常會遇到如 "invalid selector" 這樣的錯誤訊息。
- **解決方案** 
這些錯誤訊息為我們提供了寶貴的反饋。每當出現錯誤，我們都會詳細檢查錯誤的描述，並依此進行適當的調整，確保選擇器或程式碼的其他部分是正確的。

### 5. 回到基礎

- **問題描述**  
當面對複雜的問題時，高級或複雜的方法有時候並不總是最好的解決方案。
- **解決方案** 
在遇到難以解決的問題時，我們選擇回到基本的方法。在這裡，經過多次嘗試，我們確定使用 `.articleList > li` 這個簡單的CSS選擇器是最可行的方法。

## 🔩 技術細節

- **爬蟲工具**: 使用了 Selenium，它允許我們模擬真實的瀏覽器操作，特別適合動態頁面的內容抓取。
- **資料儲存**: 我們將抓取到的新聞內容存儲在 pandas 的 DataFrame 中，最後將 DataFrame 儲存為 Excel 檔案，方便後續分析和閱讀。
- **錯誤處理**: 我們考慮到了多種可能的錯誤，例如元素未能正確載入、頁面加載超時等，並提供了相應的錯誤處理機制。

## 結論

爬取動態網頁確實帶有許多挑戰，特別是當目標網頁使用了如Angular、React等的前端框架。然而，通過持續的嘗試、深入的技術研究和從錯誤中獲得的教訓，我們能夠逐步優化爬蟲，並成功地獲取了所需的數據。
經過細致的計劃和多次優化，我們開發出了一個功能完善的爬蟲工具，能夠有效抓取「內政部警政署165全民防騙網」上的新聞內容。

## 工具包

以下是在此爬蟲項目中所使用的Python工具包和模組：

### Pandas
為數據分析和操作提供了大量工具。在本項目中，主要使用它來創建和管理數據框架（DataFrame），以及保存數據到Excel文件中。

```python
import pandas as pd
```
### 📅Datetime
用於處理日期和時間。

```python
from datetime import datetime
```

### 🌐Selenium
一個強大的工具包，主要用於自動化網頁瀏覽器操作。
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.support import expected_conditions as EC
```
