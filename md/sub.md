*题目链接*

https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/

*题目介绍*
********************************
最长连续递增序列
给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列。

示例 1：

输入：nums = [1,3,5,4,7]

输出：3

解释：最长连续递增序列是 [1,3,5], 长度为3。

尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为 5 和 7 在原数组里被 
4 隔开。 
********************************

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        n = len(nums)
        # 初始化
        dp = [1] * n
        if not nums:
            return 0
        for i in range(1, n):
            if nums[i] > nums[i - 1]:
                dp[i] = dp[i - 1] + 1
        return max(dp)
```

*思路*

很多时候，感觉是可以用*DP*去做的，一开始基例还行，到**状态转移方程**这就没有状态了，继而导致这一道题卡住。接下来，讲一下，动态规划的一般步骤

![](https://github.com/sherlcok314159/leetcode-python-3/blob/main/Images/sub.png)

