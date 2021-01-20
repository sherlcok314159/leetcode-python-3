*题目链接*

https://leetcode-cn.com/problems/climbing-stairs/

*题目介绍*
*******************************
爬楼梯

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

输入： 2

输出： 2

解释： 有两种方法可以爬到楼顶。

1 阶 + 1 阶

2 阶

****************

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [9 for i in range(n + 1)]
        dp[0], dp[1] = 1, 1
        for i in range(2, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        return dp[-1]
```

*思路*

这个题目虽然没有显性*DP*的关键词，但是根据题目所给的操作，可以将本题分解为子问题求解的结构，跟一般的*DP*相比，**少了对抗求最值**

此题采用写表，具体解释读者可以参照[斐波那契数列](fibo.md)
