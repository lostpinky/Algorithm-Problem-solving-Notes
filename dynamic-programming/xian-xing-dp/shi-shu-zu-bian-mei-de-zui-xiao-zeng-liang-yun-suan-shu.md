# 使数组变美的最小增量运算数

[100107. 使数组变美的最小增量运算数](https://leetcode.cn/problems/minimum-increment-operations-to-make-array-beautiful/)\
原问题等价于：nums中任意连续三个数字，必有一个大于等于k的数。

则两个大于等于k的数的间隔取值范围是 \[1, 3]，所以只需要维护最近距离的三个值即可

**转移方程**为：dp1 = min(dp1,dp2,dp3) + max(0, k - nums\[i])

<pre class="language-python"><code class="lang-python"><strong>class Solution:
</strong>    def minIncrementOperations(self, nums: List[int], k: int) -> int:
        # dp1代表使前一个数字大于等于k时的最小增量运算数
        dp1, dp2, dp3 = 0, 0, 0
        
        for i in range(len(nums)):
            dp1,dp2,dp3 = min(dp1,dp2,dp3) + max(0, k - nums[i]), dp1, dp2
        return min(dp1, dp2, dp3)
</code></pre>
