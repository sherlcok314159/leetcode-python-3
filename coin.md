*题目链接*

https://leetcode-cn.com/problems/coin-change/

*题目介绍*
********************************
*322. 零钱兑换*

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

你可以认为每种硬币的数量是无限的。

示例 1：

输入：coins = [1, 2, 5], amount = 11

输出：3 

解释：11 = 5 + 5 + 1

示例 2：

输入：coins = [2], amount = 3

输出：-1

示例 3：

输入：coins = [1], amount = 0

输出：0
********************************

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        def dp(n):
            # base case
            if n == 0:
                return 0
            elif n < 0:
                return -1
            else:
                res = float("inf")
                for coin in coins:
                    subproblem = dp(n - coin)
                    if subproblem == -1:
                        continue
                    res = min(res, subproblem + 1)
                return res if res != float("inf") else -1

        return dp(amount)
```

*思路*
********************************
* 动态规划
  
  一般这类题目会含有***最值***，在本题求的是最少组合

  **核心思想**：*divide-and-conquer*
  
  如递归般分解为小问题，在本题凑硬币中，可以去掉一个硬币，然后总的组合数相应地+*1*，是不是跟递归一样

  *to divide the problem into the simple version of the same question*
 
  **参照物**：

  *result* 初始化为*float("inf")*,如果最后*result*等于无穷大，则说明没有凑出来

  **穷举**：

  但是，子问题有很多种，*coin*列表有很多元素，到底哪个元素对应的子问题是最优子结构呢？不知道。

  *try all the guesses*

  这也是为什么遍历*coin*列表的原因
********************************
*注意点*

虽然本题说*amount*为正数，但是递归过程中*n-coin*会出现负数

*优化*

********************************
同样的，如递归般，肯定出现冗余子结构，重复运算，所以刚刚那个方案你如果要运行，你会发现超出时间限制

那么类比递归的优化方案，我们能不能也设置一个备忘录，进行**写表-查表**的过程呢？

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        memo = {}

        def dp(n):
            # 查表
            if n in memo:
                return memo[n]
            # base case
            elif n == 0:
                return 0
            elif n < 0:
                return -1
            else:
                res = float("inf")
                for coin in coins:
                    subproblem = dp(n - coin)
                    if subproblem == -1:
                        continue
                    res = min(res, subproblem + 1)
                memo[n] = res if res != float("inf") else -1
                return memo[n]

        return dp(amount)
```

*注意点*
********************************
那么，细心的同学肯定说了，递归的时候不是说列表的空间复杂度比字典低吗？为什么本题还用字典。
因为列表不支持创建

```python
lis = []
lis[2] = 1
#IndexError: list assignment index out of range
```
但是字典可以创建新的键值对，而不受列表索引的束缚。同样的，你也可以先储存基例。但本题能储存的只有*{0:0}*所以没有必要。

*再优化*
********************************
如果同学认真看前面的递归优化，你会发现这个还是不断地在递归调用函数，如果只是不断回溯算表，不调用函数，是不是更好呢？

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        n = amount
        dp = [float("inf")] * (n + 1)
        dp[0] = 0
        for i in range(1,n + 1):
            for coin in coins:
                if coin <= i and dp[i - coin] + 1 < dp[i]:
                    dp[i] = dp[i - coin] + 1
        return dp[n] if dp[n] != float("inf") else -1
```
*注意点*

**写表条件**

这里比前面斐波那契数列写表要更复杂一点，原因在于需要条件写表。

如果为负数，直接舍去，如果写了还变大了，也舍去

**小细节**

*amount*可以赋给*n*，增加代码美观度。

函数是不能修改全局变量除非声明，但是是可以访问的。

因为 *dp[0]* 本身已有，所以索引从1开始就行

这样以后，算法一步一步优化，就已经超过用动态规划的所有同类型算法了
=
*拓展*
********************************
*自身对抗*

本题需要找出最优解，所以在循环结构中*res*不断与*res*比较，这种**自身对抗**的思想很值得借鉴，尤其适合比较找最优解。

同时，生活中也要不断升级，自己跟自己对抗，才能找出属于自己的最优解！