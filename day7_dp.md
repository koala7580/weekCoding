# 0/1 背包问题

## DKP的实现

### method_1  基本动态规划

```
输入:重量{W1,W2,……Wn},价值 {V1,V2,……Vn},背包容量C
输出:装入背包的物品编号和最大价值
 
1. 初始化最大价值maxValue=0;结果子集S=Φ;
2. 对集合{1,2,---,n}的每个子集T,执行下述操作;
   2.1 初始化背包的价值value=0;背包的重量weight=0;
   2.2 对子集T的每一个元素:
       2.2.1 如果 weight+Wj<C ,则 weight = weight+Wj; value = value+Vj
       2.2.2 否则,转步骤2考察下一子集
3. 输出子集S中的各元素和最大价值maxValue;
```

递归定义：

$$ c[i, w]=\begin{cases} 0, &if&i=0&or&w = 0 \\c[i-1,w], &if&w_i>w \\max(v_i+c[i-1,w-w_i]),&if&i>0&and&w\geq w_i\end{cases} $$

### code

```python
def KnapSack(w, v, n, c):
    # 初始化0行0列
    value = [[0 for j in range(c + 1)] for i in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(1, c + 1):
            value[i][j] = value[i - 1][j]
            # 背包总容量够放当前物体，遍历前一个状态考虑是否置换
            if j >= w[i - 1] and value[i][j] < value[i - 1][j - w[i - 1]] + v[i - 1]:
                value[i][j] = value[i - 1][j - w[i - 1]] + v[i - 1]
    for x in value:
        print(x)
    return value            
```

*参考链接* ： [动态规划之01背包问题及其优化](https://blog.csdn.net/qq_34178562/article/details/79959380)

### method_2 改进——序偶法

![](https://github.com/koala7580/weekCoding/blob/master/DKP_value.png)

![](https://github.com/koala7580/weekCoding/blob/master/DKP_algorithm.png)

```python
def Knapsack(n, c, w, p, m):
    F = [0 for i in range(n)]
    P[1] = W[1] = 0
    l = h = 1	# S[0] 的首端和末端；l是 S[i-1] 的首端，h是 S[i-1] 的末端
    F[1] = next_s = 2 	# P 和 W 中第一个空位；next_s指示P/W中可以存放（P,W）序偶的第一个位置
    for i in range(1, n):	# 生成S^i
        k = 1
        u = max(在l<=r<=h中使得W(r)+w_i<=M的最大r)		# u 指示由S^i-1生成S_1^i的最大有效位置
        for j in range(l, u+1):		# 生成S_1^i,同时归并
            pp, ww = p[j]+p[i], w[j]+w[i] 
            while k<=h and w[k]<ww:		# 从S_i-1中取元素并归并
                p[next_s] = p[k]
                w[next_s] = w[k]
                next_s += 1
                k += 1
            # 按照支配规则考虑（pp，ww）及S^i-1中的序偶
            if k<= h and w[k]==ww :
                pp = max(pp, p[k])
                k += 1
            if pp>p[next_s]:
                p[next_s], w[next_s] = pp, ww
                next_s += 1
            while k<=h and p[k]<=p[next_s]:		#清除S^i-1中的序偶
                k += 1
        while k<=h:
            p[next_s], w[next_s] = p[k], w[k]
            next_s += 1
            k += 1
        #对S^i+1置初值
        l = h + 1
        h = next_s -1
        F[i+1] = next_s
    递归求取xn,xn-1,……，x1
    Parts()
```

*参考链接*：[献上超可爱的马老师的ppt](https://github.com/koala7580/weekCoding/blob/master/5_%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92_%E7%AC%AC%E4%BA%94%E7%AB%A0_4.pdf)

# problem  132. Palindrome Partitioning II

## decrible

Given a string *s*, partition *s* such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of *s*.

**Example:**

```
Input: "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

*题意*：输入字符串，将其进行分割，分割后的各个子串必须是回文结构，要求最少的分割次数。

## method

对一个字符串，考虑所有可能的分割。设 dp[i] [j] 表示第i个字符到第j个字符是否构成回文，若是则 dp[i] [j] = True,否则为False。那么，根据回文的对称性， dp[i] [j]构成回文需要满足：

- 输入字符串 s[i] == s[j] 
- 上式还需满足：1）j - i <= 1 , 即 i、j 相邻或者 i=j ;      2) dp[i+1] [j-1] =True

在得到所有的回文子串分割情况 dp 数组之后， 需要进一步考虑最少的分割次数。用一维数组 d[] 来存储，d[i] 表示第i个字符到最后一个字符所构成的子串的最小分割次数，这里的 i 限制为必须是回文可分割的，即 dp[i] [j]=1。

### code

```python
class Solution(object):
    def minCut(self, s):
        """
        :type s: str
        :rtype: int
        """
        n = len(s)
        #dp[][] 保存两两关系
        #d[i] 代表从i开始到最后所需要的切割次数
        dp = [[False for i in range(n)] for j in range(n)]
        d = [-1 for i in range(n+1)]
        for i in range(n-1, -1, -1):
            d[i] = n-1-i
            for j in range(i, n):
                if (s[i] == s[j]) and ( (j-i<=1) or (dp[i+1][j-1]) ):
                    dp[i][j] = True
                    d[i] = min(d[i], d[j+1]+1)
        return d[0]
```

*参考链接*：[切割回文](https://blog.csdn.net/zmdsjtu/article/details/73896161)



