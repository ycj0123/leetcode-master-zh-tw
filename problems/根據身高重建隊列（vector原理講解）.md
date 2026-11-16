* [做專案（多個 C++、Java、Go、測開、前端專案）](https://www.programmercarl.com/other/kstar.html)
* [刷演算法（兩個月高強度學演算法）](https://www.programmercarl.com/xunlian/xunlianying.html)
* [背八股（40 天挑戰高頻面試題）](https://www.programmercarl.com/xunlian/bagu.html)




# 貪心演算法：根據身高重建佇列（續集）

在講解[貪心演算法：根據身高重建佇列](https://programmercarl.com/0406.根据身高重建队列.html)中，我們提到了使用 vector（C++ 中的動態陣列（dynamic array））來進行 insert 操作是費時的。

這裡專門寫一篇文章來詳細說一說這個問題。

使用 vector 的程式碼如下：

```CPP
// 版本一，使用 vector（動態陣列）
class Solution {
public:
    static bool cmp(const vector<int> a, const vector<int> b) {
        if (a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort (people.begin(), people.end(), cmp);
        vector<vector<int>> que;
        for (int i = 0; i < people.size(); i++) {
            int position = people[i][1];
            que.insert(que.begin() + position, people[i]);
        }
        return que;
    }
};

```

耗時如下：
![vectorinsert](https://file1.kamacoder.com/i/algo/20201218203611181.png)

其直觀上來看陣列（array）的 insert 操作是 O(n) 的，整體程式碼的時間複雜性（time complexity）是 O(n^2)。

這麼一分析好像和版本二鏈接串列（linked list）實作的時間複雜性是一樣的啊，為什麼提交之後效率會差距這麼大呢？

```CPP
// 版本二，使用 list（鏈接串列）
class Solution {
public:
    // 身高從大到小排（身高相同 k 小的站前面）
    static bool cmp(const vector<int> a, const vector<int> b) {
        if (a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort (people.begin(), people.end(), cmp);
        list<vector<int>> que; // list 底層是鏈接串列實作，插入效率比 vector 高得多
        for (int i = 0; i < people.size(); i++) {
            int position = people[i][1]; // 插入到下標為 position 的位置
            std::list<vector<int>>::iterator it = que.begin();
            while (position--) { // 尋找插入位置
                it++;
            }
            que.insert(it, people[i]);
        }
        return vector<vector<int>>(que.begin(), que.end());
    }
};
```

耗時如下：

![使用鏈接串列](https://file1.kamacoder.com/i/algo/20201218200756257.png)

大家都知道對於普通陣列，一旦定義了大小就不能改變，例如 `int a[10];`，這個陣列 a 至多只能放 10 個元素（element），改不了的。

對於動態陣列，就是可以不用關心初始時候的大小，可以隨意往裡放資料，那麼耗時的原因就在於動態陣列的底層實作。

動態陣列為什麼可以不受初始大小的限制，可以隨意 push_back 資料呢？

**首先 vector 的底層實作也是普通陣列**。

vector 的大小有兩個維度，一個是 size 一個是 capacity，size 就是我們平時用來遍歷（traversal）vector 時候用的，例如：

```
for (int i = 0; i < vec.size(); i++) {

}
```

而 capacity 是 vector 底層陣列（就是普通陣列）的大小，capacity 可不一定就是 size。

當 insert 資料的時候，如果已經大於 capacity，capacity 會成倍擴容，但對外暴露的 size 其實僅僅是 +1。

那麼既然 vector 底層實作是普通陣列，怎麼擴容的？

就是重新申請一個二倍於原陣列大小的陣列，然後把資料都複製過去，並釋放原陣列記憶體（memory）。（對，就是這麼原始粗暴的方法！）

舉一個例子，如圖：
![vector原理](https://file1.kamacoder.com/i/algo/20201218185902217.png)

原 vector 中的 size 和 capacity 相同都是 3，初始化（initialization）為 1 2 3，此時要 push_back 一個元素 4。

那麼底層其實就要申請一個大小為 6 的普通陣列，並且把原元素複製過去，釋放原陣列記憶體，**注意圖中底層陣列的記憶體起始位址（address）已經變了**。

**同時也注意此時 capacity 和 size 的變化，關鍵的地方我都標紅了**。

而在[貪心演算法：根據身高重建佇列](https://programmercarl.com/0406.根据身高重建队列.html)中，我們使用 vector 來做 insert 的操作，此時大家可會發現，**雖然表面上複雜度是 O(n^2)，但是其底層都不知道額外做了多少次全量複製了，所以算上 vector 的底層複製，整體時間複雜性可以認為是 O(n^2 + t × n) 級別的，t 是底層複製的次數**。

那麼是不是可以直接確定好 vector 的大小，不讓它在動態擴容了，例如在[貪心演算法：根據身高重建佇列](https://programmercarl.com/0406.根据身高重建队列.html)中已經給出了有 `people.size` 這麼多的人，可以定義好一個固定大小的 vector，這樣我們就可以控制 vector，不讓它底層動態擴容。

這種方法需要自己模擬（simulation）插入的操作，不僅沒有直接呼叫 insert 介面（interface）那麼方便，需要手動模擬插入操作，而且效率也不高！

手動模擬的過程其實不是很簡單的，需要很多細節，我粗略寫了一個版本，如下：

```CPP
// 版本三
// 使用 vector，但不讓它動態擴容
class Solution {
public:
    static bool cmp(const vector<int> a, const vector<int> b) {
        if (a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort (people.begin(), people.end(), cmp);
        vector<vector<int>> que(people.size(), vector<int>(2, -1));
        for (int i = 0; i < people.size(); i++) {
            int position = people[i][1];
            if (position == que.size() - 1) que[position] = people[i];
            else { // 將插入位置後面的元素整體向後移
                for (int j = que.size() - 2; j >= position; j--) que[j + 1] = que[j];
                que[position] = people[i];
            }
        }
        return que;
    }
};
```

耗時如下：

![vector手動模擬insert](https://file1.kamacoder.com/i/algo/20201218200626718.png)

這份程式碼就是不讓 vector 動態擴容，全程我們自己模擬 insert 的操作，大家也可以直觀地看出是一個 O(n^2) 的方法了。

但這份程式碼在 LeetCode 上統計的耗時甚至比版本一的還高，我們都不讓它動態擴容了，為什麼耗時更高了呢？

一方面是 LeetCode 的耗時統計本來就不太準，忽高忽低的，只能測個大概。

另一方面：可能是就算避免了 vector 的底層擴容，但這個固定大小的陣列，每次向後移動元素賦值（assignment）的次數比方法一中移動賦值的次數要多很多。

因為方法一中一開始陣列是很小的，插入操作，向後移動元素次數比較少，即使有偶爾的擴容操作。而方法三每次都是按照最大陣列規模向後移動元素的。

所以對於兩種使用陣列的方法一和方法三，也不好確定誰優，但一定都沒有使用方法二鏈接串列的效率高！

一波分析之後，對於[貪心演算法：根據身高重建佇列](https://programmercarl.com/0406.根据身高重建队列.html)，大家就安心使用鏈接串列吧！別折騰了，相當於我替大家折騰了一下。

## 總結

大家應該發現了，程式設計語言（programming language）中一個普通容器（container）的 insert、delete 的使用，都可能對寫出來的演算法（algorithm）有很大影響！

如果拋開語言談演算法，除非從來不用程式碼寫演算法純分析，**否則的話，語言功底不到位 O(n) 的演算法可以寫出 O(n^2) 的效能**。

相信在這裡學習演算法的讀者們，都是想在軟體產業長遠發展的，都是要從事程式設計的工作，那麼一定要深耕好一門程式設計語言，這個非常重要！


## 其他語言版本

### Rust

```rust
// 版本二，使用 list（鏈接串列）
use std::collections::LinkedList;
impl Solution{
    pub fn reconstruct_queue(mut people: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let mut queue = LinkedList::new();
        people.sort_by(|a, b| {
            if a[0] == b[0] {
                return a[1].cmp(&b[1]);
            }
            b[0].cmp(&a[0])
        });
        queue.push_back(people[0].clone());
        for v in people.iter().skip(1) {
            if queue.len() > v[1] as usize {
                let mut back_link = queue.split_off(v[1] as usize);
                queue.push_back(v.clone());
                queue.append(&mut back_link);
            } else {
                queue.push_back(v.clone());
            }
        }
        queue.into_iter().collect()
    }
}
```

### Go

Go 中 slice 的 `append` 操作和 C++ 中 vector 的擴容機制基本相同。

說是基本呢，其實是因為大家平時刷題和工作中遇到的資料不會特別大。

具體來說，當目前 slice 的長度小於 **1024** 時，執行 `append` 操作，新 slice 的 capacity 會變成目前的 2 倍；而當 slice 長度大於等於 **1024** 時，slice 的擴容變成了每次增加目前 slice 長度的 **1/4**。

在 Go Slice 的底層實作中，如果 capacity 不夠時，會做一個 reslice 的操作，底層陣列也會重新被複製到另一塊記憶體區域中，所以 `append` 一個元素，不一定是 O(1)，也可能是 O(n) 喔。




