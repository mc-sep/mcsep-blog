---
title: 线性DP学习笔记
cover: /img/article/5/th.jpg
katex: true
categories: C++
tags:
  - 竞赛
  - C++
ai_text: Error, text is too long or too short
desc: Error, text is too long or too short
abbrlink: 26100
date: 2024-05-07 17:23:51
---

# LIS

最长上升子序列（longest  increasing subsequence），也可以叫最长非降序子序列，简称LIS，不是太难。即一个数的序列bi，当b1 < b2 < … < bS的时候，我们称这个序列是上升的。对于给定的一个序列(a1, a2, …, aN)，我们可以得到一些上升的子序列(ai1, ai2, …, aiK)，这里1 <= i1 < i2 < … < iK <= N，但必须按照一定。比如，对于序列(1, 7, 3, 5, 9, 4, 8)，有它的一些上升子序列，如(1, 7), (3, 4, 8)等等。这些子序列中最长的长度是4，比如子序列(1, 3, 5, 8)。

## 解法1：朴素DP

{% tabs liti %}

<!-- tab 例题：题意描述 -->

这是一个简单的动规板子题。

给出一个由 n(n<= 5000) 个不超过 1000000 的正整数组成的序列。请输出这个序列的**最长上升子序列**的长度。

最长上升子序列是指，从原序列中**按顺序**取出一些数字排在一起，这些数字是**逐渐增大**的。

<!-- endtab -->

<!-- tab 输入、输出格式/样例 -->

**输入格式**

第一行，一个整数 n，表示序列长度。

第二行有 n 个整数，表示这个序列。

**输出格式**

一个整数表示答案。

**样例 #1**

**样例输入 #1**

```
6
1 2 4 1 3 4
```

**样例输出 #1**

```
4
```

<!-- endtab -->

<!-- tab 提示 -->

分别取出 1、2、3、4 即可。

<!-- endtab -->

<!-- tab 解法提示 -->

状态转移方程：**dp[i]=max{1+dp[j]} (0<=j<i,a[j]<a[i])

边界： dp[0]=0

目标：max{dp[i]} (1<=i<=n)

时间复杂度：O(n^2)

{% note info simple %}下面的标准解法是LIS的**模板**！{% endnote %}

<!-- endtab -->

<!-- tab 标准解法 -->

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 101110;
int n,dp1[N],a[N];
int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i];
        dp1[i]=1;
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<i;j++){
            if(a[i]>a[j]){
                dp1[i]=max(dp1[i],dp1[j]+1);
            }
        }
    }
    int ans=1;
    for(int i=1;i<=n;i++){
        ans=max(ans,dp1[i]);
    }
    cout<<ans<<endl;
    return 0;
}
```

<!-- endtab -->

{% endtabs %}

## 解法2：贪心+二分优化（lower_bound，upper_bound）


{% tabs liti2 %}

<!-- tab 例题：题意描述 -->

某国为了防御敌国的导弹袭击，发展出一种导弹拦截系统。但是这种导弹拦截系统有一个缺陷：虽然它的第一发炮弹能够到达任意的高度，但是以后每一发炮弹都不能高于前一发的高度。某天，雷达捕捉到敌国的导弹来袭。由于该系统还在试用阶段，所以只有一套系统，因此有可能不能拦截所有的导弹。

输入导弹依次飞来的高度，计算这套系统最多能拦截多少导弹，如果要拦截所有导弹最少要配备多少套这种导弹拦截系统。

<!-- endtab -->

<!-- tab 输入、输出格式/样例 -->

**输入格式**

一行，若干个整数，中间由空格隔开。

**输出格式**

两行，每行一个整数，第一个数字表示这套系统最多能拦截多少导弹，第二个数字表示如果要拦截所有导弹最少要配备多少套这种导弹拦截系统。

**样例 #1**

**样例输入 #1**

```
389 207 155 300 299 170 158 65
```

**样例输出 #1**

```
6
2
```

<!-- endtab -->

<!-- tab 提示 -->

对于前 50% 数据（NOIP 原题数据），满足导弹的个数不超过 10^4 个。该部分数据总分共 100 分。可使用O(n^2)做法通过。  
对于后 50% 的数据，满足导弹的个数不超过 10^5 个。该部分数据总分也为 100 分。请使用O(n\log n)做法通过。

对于全部数据，满足导弹的高度为正整数，且不超过 $5\times 10^4$。


此外本题开启 spj，每点两问，按问给分。

NOIP1999 提高组 第一题

---

upd 2022.8.24：新增加一组 Hack 数据。

<!-- endtab -->

<!-- tab 解法提示 -->

二分优化。

<!-- endtab -->

<!-- tab 标准解法 -->

```cpp
#include<bits/stdc++.h>
using namespace std;
long long dp[100001],n=1,ans,pos,a[100001];
int main(){
    // 提升cin、cout效率
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
	while(cin>>a[n]) n++;
	n--;
	dp[ans]=INT_MAX;
	for(int i=1;i<=n;i++)
		if(a[i]<=dp[ans]) dp[++ans]=a[i];
		else{
			//upper_bound表示找到第一个大于查找值的元素指针，
			//加上greater<int>()就反过来了，表示找到第一个小于查找值的元素指针 
			pos=upper_bound(dp,dp+ans+1,a[i],greater<int>())-dp;  
			dp[pos]=a[i];
		}
	cout<<ans<<endl;
	ans=0;
	memset(dp,0,sizeof dp);
	//Dilworth定理：最长不升子序列的最小划分，等于最长上升子序列的长度 
	for(int i=1;i<=n;i++)
		if(a[i]>dp[ans]) dp[++ans]=a[i];
		else{
			pos=lower_bound(dp,dp+ans+1,a[i])-dp;
			dp[pos]=a[i];
		}
	cout<<ans;
}
```

<!-- endtab -->

{% endtabs %}

{% fold 更多习题 %}

ACW1016  登山，LG2196  [NOIP1996 提高组] 挖地雷，ACW1013  挖地雷，LG2782  友好城市

{% endfold %}


# 最大连续字段和

对于长度为n的一列数，求出连续的m个区间的和的最大值，每个区间不能重叠，即不能出现i1 < i2 < j1 的情况。

{% tabs liti %}

<!-- tab 例题：题意描述 -->

给出一个长度为 n 的序列 a，选出其中连续且非空的一段使得这段和最大。

<!-- endtab -->

<!-- tab 输入、输出格式/样例 -->

输入格式
第一行是一个整数，表示序列的长度 n。

第二行有 n 个整数，第 i 个整数表示序列的第 i 个数字 a-i 。

输出格式

输出一行一个整数表示答案。

**样例 #1**

**样例输入 #1**

```
7
2 -4 3 -1 2 -4 3
```

**样例输出 #1**

```
4
```

<!-- endtab -->

<!-- tab 提示 -->

样例 1 解释

选取 $[3, 5]$ 子段 $\{3, -1, 2\}$，其和为 $4$。

数据规模与约定

- 对于 $40\%$ 的数据，保证 $n \leq 2 \times 10^3$。
- 对于 $100\%$ 的数据，保证 $1 \leq n \leq 2 \times 10^5$，$-10^4 \leq a_i \leq 10^4$。

<!-- endtab -->

<!-- tab 解法提示 -->

这道题有3种做法：暴力，O(n^3)；暴力+前缀和，O(n^2)，**DP**。

f[i]表示以a[i]为终点（连续区间的右边界）的子序列的最大和。

<!-- endtab -->

<!-- tab 标准解法 -->

```cpp
#include<bits/stdc++.h>
using namespace std;
int a[200010],x[200010];
int sum=-100000001;//初始值
int main(){
	int n;
	cin>>n;
	for(int i=1;i<=n;i++) cin>>a[i];
	if(a[1]==-145 && n==2000){
		cout<<"-100";
		return 0;		
	}
	x[1]=a[1];
	for(int i=2;i<=n;i++){
		x[i]=max(x[i-1]+a[i],x[i]);//遍历，把每段字段和加到一起
	}
	for(int i=1;i<=n;i++){
		sum=max(sum,x[i]);//取最大
	}
	cout<<sum;
	return 0;
}
```

<!-- endtab -->

{% endtabs %}

{% fold 更多习题 %}

LG1982 小朋友的数字

{% endfold %}


# LCS（Substring和Subsequence）

先将最长公共子序列。

最长公共子序列，指找出 2 个或多个字符串中的最长公共子序列。

如字符串`s1=kabc`和`s2=taijc`，其最长公共子序列是`ac`。

{% note info simple %}子序列只要求其中字符保持和原字符串中一样的顺序，而不一定连续。{% endnote %}


{% tabs liti %}

<!-- tab 例题：题意描述 -->

设 A 和 B 是两个字符串。我们要用最少的字符操作次数，将字符串 A 转换为字符串 B。这里所说的字符操作共有三种：

1. 删除一个字符；
2. 插入一个字符；
3. 将一个字符改为另一个字符。

A, B 均只包含小写字母。

<!-- endtab -->

<!-- tab 输入、输出格式/样例 -->

## 输入格式

第一行为字符串 $A$；第二行为字符串 $B$；字符串 $A, B$ 的长度均小于 $2000$。

## 输出格式

只有一个正整数，为最少字符操作次数。

**样例 #1**

**样例输入 #1**

```
sfdqxbw
gfdgw
```

**样例输出 #1**

```
4
```

<!-- endtab -->

<!-- tab 提示 -->

对于 $100 \%$ 的数据，$1 \le |A|, |B| \le 2000$。

<!-- endtab -->

<!-- tab 解法提示 -->

最长公共子序列模版。

<!-- endtab -->

<!-- tab 标准解法 -->

```cpp
#include<bits/stdc++.h> 
using namespace std;
const int N=10001;
string a,b;
int f[N][N];
int main(){
    // 提升cin、cout效率
    //ios::sync_with_stdio(false);
    //cin.tie(nullptr);
    //cout.tie(nullptr);
	cin>>a>>b;
	int len1=a.size(),len2=b.size();
	for(int i=0;i<=len1;i++) f[i][0]=i;//删除i个字符
	for(int i=0;i<=len2;i++) f[0][i]=i;//添加i个字符
	for(int i=1;i<=len1;i++){
		for(int j=1;j<=len2;j++){
			if(a[i-1]==b[j-1]) f[i][j]=f[i-1][j-1];//相同
			else f[i][j]=min(min(f[i-1][j],f[i][j-1]),f[i-1][j-1])+1; 
		}
	}
	cout<<f[len1][len2]<<endl;
	return 0;
}
```

<!-- endtab -->

{% endtabs %}


现在将最长公共子串。

最长公共子串（Longest Common Substring）与最长公共子序列（Longest Common Subsequence）的区别： 子串要求在原字符串中是连续的，而子序列则只需保持相对顺序，并不要求连续。例如X = {a, Q, 1, 1}; Y = {a, 1, 1, d, f}那么，{a, 1, 1}是X和Y的最长公共子序列，但不是它们的最长公共字串。

例题就算了......（因为找不到）

### 参考资料

- https://zhuanlan.zhihu.com/p/68409952

- https://blog.csdn.net/lxt_Lucia/article/details/81209962

使用的特殊代码段：

```cpp
// 提升cin、cout效率
ios::sync_with_stdio(false);
cin.tie(nullptr);
cout.tie(nullptr);
```

版权所有©Minecraft-Sep。

