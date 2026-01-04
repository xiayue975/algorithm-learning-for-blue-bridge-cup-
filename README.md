# algorithm-learning-for-blue-bridge-cup-
从零开始的算法学习之路&amp;心情笔记
# 🚀 我的算法学习之旅

记录每天算法学习的进步！

## 📅 今日进度 (2026-01-04)

✅**对背包问题的循环方式和状态存储又有了新的理解**

一维储存的话假如01背包 需要从后往前遍历

例如 拿2块的 最多付7块钱

你要是01背包就不能从前往后
因为7块从5块来 而5块又从3块来
这样就会多啊 3块里包含了2+1 而五块里又包含2+2+1 重复了
所以倒着来
反之完全背包就不用倒着来


✅ **学会了最大子段和（Kadane算法）**

### **[题源 洛谷p1115](https://www.luogu.com.cn/problem/P1115#ide)**

一开始考虑的是dp或前缀和
### dp解法
```cpp

//子段长度在外面循环
//然后里面用遍历这个序列
//dp[i][j]表示子串长度为i时候以第j个数开始的字串和
#include<bits/stdc++.h>
using namespace std;
int dp[20005][20005];
int a[20005];
int main()
{
    int n; cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    for (int i = 1; i <= n; i++)
        dp[1][i] = a[i];
    long long  ans = a[1];
    for (int i = 2; i <= n; i++)
        for (int j = 1; j <= n; j++)
        {
            if (j + i - 1 <= n)
                dp[i][j] = dp[i - 1][j] + a[j + i - 1];
            if (dp[i][j] > ans)
                ans = dp[i][j];
        }
    cout << ans;
    return 0;
}
```
### 前缀和
```cpp

#include<bits/stdc++.h>
using namespace std;
int a[200005];
int s[200005];
int main()
{
    int n; cin >> n;
    int ans = -10005;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
        if (a[i] > ans)
            ans = a[i];
        s[i] = s[i - 1] + a[i];
    }
  
    for(int i=1;i<=n;i++)
        for (int j = i+1; j <= n; j++)
        {
            if (s[j] - s[i] > ans)
                ans = s[j] - s[i];
        }
    cout << ans;
    return 0;
}
```
显然是不行滴，因为都是双层循环对于那个数据量肯定会超时。
那么我们只能优化思路或算法 

dp是可以的 只不过要换一种方式定义状态
**定义dp[i]为以i为终点的最大字串和的大小** 

 ~~这里是ds给的思路~~

一开始我想 

`dp[i]=dp[i-1]+a[i] `

但是一想 那不还是前缀和 没判断最大值啊
然后ds告诉我 想想前面字串给当前的贡献 如果前面的字串和加上当前数大于当前数 那就让当前字串和就是前面的字串和加上当前数，反之就是当前数

因为如果将这个数和前面的字串和接起来还不如让它自己单独成和 那么就得舍弃前面的 


### 代码示例
```cpp
#include <iostream>
using namespace std;

int main() {
    int n, a[100005];
    cin >> n;
    for(int i = 0; i < n; i++) cin >> a[i];
    
    int cur = a[0], ans = a[0];
    for(int i = 1; i < n; i++) {
        if(cur > 0) cur += a[i];
        else cur = a[i];
        if(cur > ans) ans = cur;
    }
    
    cout << ans;
    return 0;
}
```
### 或者用我写的dp
```cpp
//dp
#include<bits/stdc++.h>
using namespace std;
int dp[200005];
int a[200005];
int main()
{
    int n;cin>>n;
    for(int i=1;i<=n;i++)
        cin>>a[i];  
    dp[1]=a[1];
    int ans=dp[1];
    for(int i=2;i<=n;i++)
    {
      dp[i]=max(dp[i-1]+a[i],a[i]); 
        if(dp[i]>ans)
        ans=dp[i];
    }
   cout<<ans;
    

    return 0;
}
```
### 学到的知识
1. 暴力解法：O(n²)，会超时
2. Kadane算法：O(n)，高效
3. 核心思想：`cur = max(a[i], cur + a[i])`

### 今日感悟
去他妈的恋爱 还甩老子 你配么臭傻逼

dp的状态定义方式很重要 你比如01背包完全背包之类的 让你求什么你直接定义出来的

但是这个题不一样 你要是按我那样定义肯定超时 但是换一种定义就不会了 重在理解啊
任重而道远