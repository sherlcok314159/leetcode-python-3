*题目链接*

https://leetcode-cn.com/problems/edit-distance/

*题目介绍*
*******************************
编辑距离

给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 

你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符
 
示例 1：

输入：word1 = "horse", word2 = "ros"
输出：3
 
解释：

horse -> rorse (将 'h' 替换为 'r')

rorse -> rose (删除 'r')

rose -> ros (删除 'e')

****

```python

class Solution:
    def minDistance(self, w1: str, w2: str) -> int:
        def dp(i, j):
            #base case
            if i == -1:
                return j + 1
            elif j == -1:
                return i + 1
            elif w1[i] == w2[j]:
                return dp(i - 1, j - 1)
            else:
                return min(
                    dp(i, j - 1) + 1, 
                    dp(i - 1, j) + 1, 
                    dp(i - 1, j - 1) + 1
                    )

        return dp(len(w1) - 1, len(w2) - 1)
```

**原因**

这道题也是动态规划的原因是
   * 包含最值问题
   * 不断对子问题的求解

*思路*

**指针**

对这种字符串，子序列问题，常常用两个指针(最后一个索引)，不断动态向前

**基例**

如果*i*，*j*有一个等于-*1*，这个就说明*w1*,*w2*其中有一个长度为*0*，即为*None*，那最优操作就是直接复制一下另一个

**状态转移方程**

类比一下零钱兑换，无非就是单操作和多操作的区别，一个是自对抗，一个是多对抗

********************************
*优化*

同理，动态规划可以通过***写表-查表***来解决,不同点是键和值变成了两个

```python

class Solution:
    def minDistance(self, w1: str, w2: str) -> int:
        memo = {}

        def dp(i, j):
            # 查表
            if (i, j) in memo:
                return memo[(i, j)]
            # base case
            elif i == -1:
                return j + 1
            elif j == -1:
                return i + 1
            elif w1[i] == w2[j]:
                memo[(i,j)] = dp(i - 1, j - 1)
            else:
                memo[(i, j)] = min(
                    dp(i, j - 1) + 1,
                    dp(i - 1, j) + 1, 
                    dp(i - 1, j - 1) + 1
                )
            return memo[(i, j)]

        return dp(len(w1) - 1, len(w2) - 1)
```
*注意点*

* 基例中两者直接相等，不是*return*，而是直接写表
* *else*分支下面的子句应该没有*return*语句，否则会报错，*return*应该放到与*else*同一地位的地方。因为要等到迭代好了之后才最后写入表。
* 因为元组空间复杂度低于列表，所以采用元组


*拓展*
********************************

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        visited = set()
        queue = collections.deque([(word1, word2, 0)])

        while True:
            w1, w2, d = queue.popleft()
            if (w1, w2) not in visited:
                if w1 == w2:
                    return d
                visited.add((w1, w2))
                while w1 and w2 and w1[0] == w2[0]:
                    w1 = w1[1:]
                    w2 = w2[1:]
                d += 1
                queue.extend([(w1[1:], w2[1:], d), (w1, w2[1:], d), (w1[1:], w2, d)])
```

这是*beat 100*%的范例，不过我认为大家掌握上面的通法就行，这种比较偏的还是跳过为好！