
# 手把手帶你實作 storage engine


之前在 [刷題攻略登上榜首](https://mp.weixin.qq.com/s/wZRTrA9Rbvgq1yEkSw4vfQ)這篇文章中說過，Carl 不僅寫了刷題攻略，還寫了很多優秀的開放原始碼（open source）專案。

在知識星球裡也有很多夥伴問我關於專案的選擇，**相信如果是 C++ 後端（back end）開發路線的話，基本都會去做 WebServer 伺服器（server）**。

我在[知識星球](https://programmercarl.com/other/kstar.html)給夥伴答疑，包括看了這麼多履歷，**發現 WebServer 這個專案是真的多，有點太常見了**。

所以今天我把自己曾經開發的 KV storage engine 給大家介紹一下，大家可以拿去當作自己的專案經驗。

**相信只要是做後端的同學應該都要熟悉 NoSQL 資料庫（database）Redis 吧，那麼應該知道 Redis 的 storage engine 是跨越列表（skip list）實作的**。

現在很多雲端服務商提供的雲端資料庫，其底層都是用了 Facebook 開放原始碼的 RocksDB，而 RocksDB 的底層是 Google 開放原始碼的 LevelDB，**而 LevelDB 的核心實作也是跨越列表**。

所以大家應該知道跨越列表的應用有多麼廣泛了。

那麼為什麼這個專案非常適合大家用來做自己的專案經驗呢？

如果你是後端開發的話，你在履歷上一定會寫熟悉或者了解 Redis 吧，那麼可以進一步介紹一下自己的專案用跨越列表實作了 Redis 核心引擎。

面試官一定會非常感興趣的，然後你就可以和面試官侃侃而談你是如何用跨越列表實作這個 KV storage engine 的。

**瞬間格調就高了，有沒有！**

我在 18 年的時候，用跨越列表實作了一個輕量級 KV storage engine，程式碼也寫得非常規範，熟悉我的錄友應該知道，我的程式碼嚴格按照 Google C++ style 來的。

因為當時我是想把這個專案國際化的，註解和 README 都是英文的，但最近我把這個專案又翻譯成中文了，方便大家理解。

給大家先隨意看一段程式碼，我在註解中其實就已經在講解跨越列表的執行原理了。程式碼使用了 C++ 模板（template）程式設計，這樣介面（interface）支援任意類型的資料（包括自己自訂的類別（class））

![](https://file1.kamacoder.com/i/algo/20221104121454.png)

專案網址：**https://github.com/youngyangyang04/Skiplist-CPP**

這個專案中的程式碼品質是非常高的，無論是 C++ 特性的運用，還是程式碼風格都是絕對拿得出手的！

好了，自誇完畢，然後給大家正式介紹一下這個專案

## KV storage engine

本專案就是基於跨越列表實作的輕量級鍵值（key-value）型 storage engine，使用 C++ 實作。支援插入資料、刪除資料、查詢資料、資料展示、資料寫入磁碟、檔案載入資料，以及資料庫大小顯示。

在隨機讀寫情況下，該專案每秒可處理寫入請求數（QPS）：24.39w，每秒可處理讀取請求數（QPS）：18.41w

## 專案展示

![](https://file1.kamacoder.com/i/algo/20221104121509.png)

檔案功能：

* `main.cpp` 包含 `skiplist.h` 使用跨越列表進行資料操作
* `skiplist.h` 跨越列表核心實作
* `README.md` 中文介紹
* `README-en.md` 英文介紹
* `bin` 產生可執行檔（executable file）目錄（directory）
* `makefile` 編譯腳本（script）
* `store` 資料寫入磁碟的檔案存放在這個資料夾
* `stress_test_start.sh` 壓力測試（stress test）腳本
* `LICENSE` 使用協定（protocol）


## 提供介面

* `insertElement`（插入資料）
* `deleteElement`（刪除資料）
* `searchElement`（查詢資料）
* `displayList`（展示已存資料）
* `dumpFile`（資料寫入磁碟）
* `loadFile`（檔案載入資料）
* `size`（回傳資料規模）

## storage engine 資料表現

### 插入操作

跨越列表樹高：18

採用隨機插入資料測試：


|插入資料規模（萬筆） |耗時（秒） |
|---|---|
|10 |0.316763 |
|50 |1.86778 |
|100 |4.10648 |


每秒可處理寫入請求數（QPS）：24.39w

### 取資料操作

|取資料規模（萬筆） |耗時（秒） |
|---|---|
|10|0.47148 |10|
|50|2.56373 |50|
|100|5.43204 |100|

每秒可處理讀取請求數（QPS）：18.41w

## 專案執行方式

```
make            // complie demo main.cpp
./bin/main      // run
```

執行螢幕截圖：（其中展示了插入資料，刪除資料，展示資料等等功能）

![](https://file1.kamacoder.com/i/algo/20221104121525.png)

如果想自己寫程式使用這個 KV storage engine，只需要在你的 CPP 檔案中 `#include "skiplist.h"` 就可以了。

可以執行如下腳本測試 KV storage engine 的效能（當然你可以根據自己的需求進行修改）

```
sh stress_test_start.sh
```

專案網址：**https://github.com/youngyangyang04/Skiplist-CPP**

**大家免費使用的同時，別忘了給個 star、fork，支持一下！** 錄友如果最後拿到 offer 了，也別忘了和我報個喜喔。

