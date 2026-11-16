

# 如何在 GitHub 上提交 PR（pull request）


* 如何提交程式碼
* 合併不規範
  * 提交訊息不規範
  * Markdown 程式碼格式
  * pull request 裡的 commit 數量
  * 程式碼註解
  * 說明具體是哪種方法
  * 程式碼規範
  * 程式碼邏輯
  * 處理衝突

以下以在 [https://github.com/youngyangyang04/leetcode-master](https://github.com/youngyangyang04/leetcode-master) 上提交 PR 為例

## 如何合併程式碼

首先來說一說如何合併程式碼，不少錄友還不太會使用 GitHub，所以這裡也做一下科普。

我特意申請一個新的 GitHub 帳號，給大家做一個示範。

需要強調一下，一個 commit 就只更新一道題目，不要很多題目一起放在一個 commit 裡，那樣就很亂。

首先 LeetCode-Master 每天都有更新，如何保持你 fork 到自己的儲存庫（repository）是最新的版本呢？

點擊這裡 Fetch upstream。

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20230721172815.png' width=500 alt=''></img></div>

點擊之後，這裡就會顯示最新的資訊了
<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210516213032568.png' width=500 alt=''></img></div>

注意這時是你的遠端儲存庫為最新版本，本機還不是最新的，本機要 git pull 一下。

基於最新的版本，大家再去提交程式碼。

如何提交程式碼呢？首先把自己的程式碼提交到自己 fork 的遠端儲存庫中，然後 open pull request，如圖：

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210516215102296.png' width=500 alt=''></img></div>

點擊 open pull request 之後，就是如下畫面，一個 pull request 有多個 commit。

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210516215646937.png' width=500 alt=''></img></div>

然後就是給 pull request 新增備註，pull request 是對本次 commit 的一個總結。如果一個 pull request 就一個 commit，那麼就和 commit 的備註保持一致。然後點擊 create pull request 就可以了

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210516220219891.png' width=500 alt=''></img></div>

此時你就提交成功了，我會在專案中的 pull requests 處理列表裡看到你的請求。
<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210516220502485.png' width=500 alt=''></img></div>

然後如果你發現自己的程式碼沒有合併，多半是有問題，如果有問題都會在 pull request 裡給出留言的，

## 注意事項

### 提交訊息不規範


大家提交程式碼的時候有兩個地方需要寫備註，一個是 commit，一個是 pull request，pull request 包含多個 commit。

commit 說清楚本檔案多了哪些修改，而 pull request 則是對本次合併的所有 commit 做一個總結性描述。

commit 備註，舉例：新增 Rust python3，那麼 commit 備註就是：新增 0001 兩數之和 Rust python3 版本

而 pull request 如果只有一個 commit，那麼也就同樣是：新增 0001 兩數之和 Rust python3 版本

如果是多個 commit，則把本次 commit 都描述一遍。

### Markdown 語法

關於 Markdown 程式碼格式，例如新增 C++ 程式碼，需要有程式碼區塊語法

\`\`\`C++
C++ 程式碼
\`\`\`

例如這個 commit，在新增 Java 程式碼的時候，就直接新增程式碼
<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210512141514272.png' width=500 alt=''></img></div>

正確的格式應該是這樣：
<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210513101029336.png' width=500 alt=''></img></div>

一般發現問題，我也會在程式碼中給出評論：

<div align="center"><img src='https://file1.kamacoder.com/i/algo/2021051309401135.png' width=500 alt=''></img></div>

這樣大家也可以學習一些提交程式碼的規範方面的知識


有的錄友是新增了程式碼區塊語法，但沒有標記是哪種語言，這樣的話程式碼就不會針對特定語言進行語法高亮度顯示（syntax highlighting）了，也比較影響閱讀，例如：

<div align="center"><img src='https://file1.kamacoder.com/i/algo/2021051214212374.png' width=500 alt=''></img></div>

提交 python 程式碼的話，要註明好是 python2 還是 python3

例如這樣：

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210513174147165.png' width=500 alt=''></img></div>

當然 python2 的話，只這麼寫就行

\`\`\`python
python 程式碼
\`\`\`

### pull request 裡的 commit 數量


有的錄友是一個 pull request 裡有很多 commit（一個 commit 是一道題目的程式碼）。

有的錄友是一個 pull request 裡只有一個 commit。

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210512221535670.png' width=500 alt=''></img></div>

其實如果大家是平時一天寫了兩三道題目的話，那麼分三個 commit，一個 pull request 提交上來就行。

一個 pull request 一個 commit 也可以，這樣大家就會麻煩一點。

但注意一個 pull request 也不要放太多的 commit，一旦有一道題目程式碼不合格，我沒有合併，就會影響這個 pull request 裡其他所有程式碼的合併了。

### 程式碼註解

提交的程式碼最好要有註解，這樣也方便讀者理解。

例如這位錄友，在提交 Java 程式碼的時候，按照題解的意思對 Java 版本的程式碼進行了註解，這就很棒👍

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210512212151438.png' width=500 alt=''></img></div>

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210513101321112.png' width=500 alt=''></img></div>

當然如果大家感覺已有的程式碼不符合以上要求的話，例如程式碼思路不夠清晰不夠規範、註解不夠友善，依然歡迎提交最佳化程式碼，要記得詳細註解喔。

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210516082342756.png' width=500 alt=''></img></div>

### 說明具體是哪種方法

有的題解有兩種甚至三四種解法，在新增程式碼的時候，註解上也清楚說明具體是哪一種方法的版本。

下面這位錄友做得就很好

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210512221951251.png' width=500 alt=''></img></div>


<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210513101551819.png' width=500 alt=''></img></div>

有的題解是一起給出了多道題目的講解，例如專案中 `0102.二元樹的層次序走訪.md` 中有八道題目，那麼大家新增程式碼的時候應該在程式碼註解上，或者直接寫上是哪個題目的程式碼。


### 程式碼規範


大家提交程式碼要規範，當然程式碼可以在 LeetCode 上執行通過是最基本的。

雖然我主張沒有絕對正確的程式碼風格，但既然是給 LeetCode-Master 提交程式碼，盡量遵循 Google 程式設計規範。

經常看我的程式碼的錄友應該都知道，我的程式碼風格嚴格按照 Google C++ 程式設計規範來的，這樣看上去會比較整潔。

大家提交程式碼的時候遇到規範性問題，例如哪裡應該有空白、哪裡沒有空白，可以參考我的程式碼來寫。

有一位錄友在提交程式碼的時候會把之前的程式碼做一下規範性的調整，這就很棒。

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210514093012603.png' width=500 alt=''></img></div>

**程式碼規範從你我做起！**


### 程式碼邏輯

**提交的程式碼要按照題解思路來寫**。

雖然大家自己發揮想像空間是好的，但是題解還是要一脈相承，讀者看完題解，發現程式碼和題解不是同一個思路的話，那和重新讀程式碼有什麼區別呢。

所以和題解不是同一個思路的程式碼，除非詳細註解了自己的思路或者寫一段自己程式碼的描述說明思路和最佳化的地方，否則我就不會通過合併了喔。

大家的程式碼最好也將關鍵地方放上註解，這樣有助於別人快速理解你的程式碼。


### 處理衝突

在合併的過程中還要處理衝突的程式碼，理解大家程式碼的思路，解決衝突，然後在 LeetCode 提交一下，確保是沒問題的。

例如同一道題目，一位錄友提交了，我還沒處理合併，另一位錄友對這道題也提交了程式碼，這樣就會發生衝突
<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210514092248192.png' width=500 alt=''></img></div>

大家提交程式碼的熱情太高了，我有時候根本處理不過來，但我必須當天處理完，否則第二天程式碼衝突會越來越多。
<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210514091457392.png' width=500 alt=''></img></div>

一天晚上分別有兩位錄友提交了 30 多道 Java 程式碼，全部衝突，解決衝突處理得我脖子疼[哭]

那麼在處理衝突的時候保留誰的程式碼、刪掉誰的程式碼呢？

我一定是看誰程式碼邏輯和題解一致、程式碼風格好、註解友善，就保留誰的。

所以例如當你想提交 Java 程式碼的時候，即使發現該題解已經有 Java 版本了，只要你的程式碼寫得好，一樣可以提交，我審查合格一樣可以合併進程式碼庫。


### 不要做額外修改

確保這種額外檔案不要提交。

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210514093430534.png' width=500 alt=''></img></div>

還有新增不同方法的時候，直接用內文格式寫哪種方法就可以了，不要新增目錄，例如這樣，這樣整篇文章目錄結構就有影響了。

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210513102640556.png' width=500 alt=''></img></div>

前面不要加 `## 前序遍歷（preorder traversal）（疊代（iteration）法）`，直接寫 `前序遍歷（疊代法）` 就可以了。

當然這裡也沒有給程式碼區塊標記上對應的語言，應該是

\`\`\` Go
Go 語言程式碼
\`\`\`


## 對程式碼保持敬畏

有的錄友甚至提交的程式碼並不是本題的程式碼，雖然我是鼓勵大家提交程式碼的，但是大家貢獻程式碼的時候也要對自己的程式碼有敬畏之心，自己的程式碼是要給很多讀者看的。

* 程式碼執行無誤
* 寫得夠不夠簡潔
* 註解清不清晰
* 備註規不規範

這也是培養大家以後協同工作的一種能力。

## 最佳化

目前 leetcode-master 中大部分題解已經補充了其他語言，但如果你發現了可以最佳化的地方，依然可以提交 PR 來最佳化。

甚至發現哪裡有語病，也歡迎提交 PR 來修改，例如下面：就是把「下表」更正為「下標（subscript）」

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210811144337.png' width=500 alt=''></img></div>

不用非要寫出超強的程式碼才能提交 PR，只要發現文章中有任何問題，或者錯別字，都歡迎提交 PR，成為 contributor。

<div align="center"><img src='https://file1.kamacoder.com/i/algo/20210927113149.png' width=500 alt=''></img></div>

## 特別注意

git add 之前，要 git diff 查看一下，本次提交所修改的程式碼是不是自己修改的，是否誤刪，或者誤加了檔案。

提交程式碼，不要使用 git push -f 這種命令，要足夠了解 -f 意味著什麼。



