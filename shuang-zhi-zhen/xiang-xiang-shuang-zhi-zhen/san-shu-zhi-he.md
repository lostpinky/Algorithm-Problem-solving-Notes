# 三数之和

[15. 三数之和](https://leetcode.cn/problems/3sum/)



1. 对数组排序，枚举第一位数字，然后问题变为 [liang-shu-zhi-he-you-xu-shu-zu.md](liang-shu-zhi-he-you-xu-shu-zu.md "mention")
2. 由于不能出现重复三元组，因此需要跳过重复值，判断当前数字与遍历的前一个数字是否相等，相等则跳过：

* 对于i，若当前数字已遍历过，前一次遍历可选择的范围更大，一定包含了这次的遍历结果，因此直接跳过
* 对于j，k，找到正确答案时分别对 j , k 执行跳过操作，因为和固定，同时跳过不会忽略正确答案

3. 优化操作：

* 若nums\[i] + nums\[i+1] + nums\[i+2] > 0，则后续所有遍历结果均大于0，直接跳出外层循环
* 若nums\[i] + nums\[-1] + nums\[-2] < 0， 则本次i的遍历均小于0，continue进入下一次循环

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        nums.sort()
        ans = []

        for i in range(n-2):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            if nums[i] + nums[i+1] + nums[i+2] > 0:
                break
            if nums[i] + nums[-1] + nums[-2] < 0:
                continue
            j = i + 1
            k = n - 1

            while j < k:
                s = nums[i] + nums[j] + nums[k]
                if s < 0:
                    j += 1
                elif s > 0:
                    k -= 1
                else:
                    ans.append([nums[i],nums[j],nums[k]])
                    j += 1
                    while j < k and nums[j] == nums[j-1]:
                        j += 1
                        continue
                    k -= 1
                    while k > j and nums[k] == nums[k+1]:
                        k -= 1
                        continue
        return ans
```
