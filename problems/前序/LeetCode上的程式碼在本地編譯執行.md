# LeetCode 上的程式碼想在本地編譯執行？


很多錄友都問過我一個問題，就是 LeetCode 上的程式碼如何在本地編譯（compile）執行？

其實在代碼隨想錄刷題群組裡也經常出現這個場景，就是錄友發一段程式碼上來，問大家這個程式碼怎麼有問題？如果我看到了一般我的回覆：都是把那幾個變數（variable）或者陣列（array）印出一下看看對不對，就知道了。

然後錄友就問了：如何印出日誌（log）呢？

其實在 LeetCode 上印出日誌也挺方便的，我一般除錯（debugging）就是直接在 LeetCode 上印出日誌，偶爾需要把程式碼貼到本地來執行，添加日誌 debug 一下。

在 LeetCode 上直接印出日誌，這個就不用講，C++ 的話想印什麼直接 `cout` 什麼就可以了。

我來說一說 LeetCode 程式碼如何在本地執行。

畢竟我們天天用 LeetCode 刷題，也應該知道 LeetCode 上的程式碼如何在本地編譯執行。

其實挺簡單的，大家看一遍就會了。

我拿我們剛講過的這道題[動態規劃：使用最小花費爬樓梯](https://programmercarl.com/0746.使用最小花费爬楼梯.html)來做示範。

LeetCode 746. 使用最小花費爬樓梯，完整的可以直接在本地執行的 C++ 程式碼如下：

```CPP
#include <iostream>
#include <vector>
using namespace std;

class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp(cost.size());
        dp[0] = cost[0];
        dp[1] = cost[1];
        for (int i = 2; i < cost.size(); i++) {
            dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i];
        }
        return min(dp[cost.size() - 1], dp[cost.size() - 2]);
    }
};

int main() {
    int a[] = {1, 100, 1, 1, 1, 100, 1, 1, 100, 1};
    vector<int> cost(a, a + sizeof(a) / sizeof(int));
    Solution solution;
    cout << solution.minCostClimbingStairs(cost) << endl;
}
```

大家可以拿去跑一跑，直接貼到編譯器（compiler）上就行了。

我用的是 Linux 下 gcc 來編譯的，估計貼到其他編譯器也沒問題。

程式碼中可以看出，其實就是定義一個 `main` 函式（function），構造一個輸入範例，然後定義一個 `solution` 變數，呼叫 `minCostClimbingStairs` 函式就可以了。

此時大家就可以隨意構造測試資料（test data），然後想怎麼印出日誌就怎麼印出日誌，沒有找不出的錯誤（bug）。






-----------------------
<div align="center"><img src='https://file1.kamacoder.com/i/algo/01二维码.jpg' width=450> </img></div>
