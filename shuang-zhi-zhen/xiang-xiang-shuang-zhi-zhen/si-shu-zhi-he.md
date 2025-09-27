# 四数之和

[18. 四数之和](https://leetcode.cn/problems/4sum/)\


[san-shu-zhi-he.md](san-shu-zhi-he.md "mention")的加强版，增加一层外层循环即可



```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        n = len(nums)
        nums.sort()
        ans = []

        for i in range(n-3):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            if nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target:
                break
            if nums[i] + nums[-1] + nums[-2] + nums[-3] < target:
                continue

            for j in range(i+1, n-2):
                # 注意此处一定要判断j > i + 1，不能因为与nums[i]相等而跳过
                if j > i + 1 and nums[j] == nums[j-1]:
                    continue
                if nums[i] + nums[j] + nums[j+1] + nums[j+2] > target:
                    break
                if nums[i] + nums[j] + nums[-1] + nums[-2] < target:
                    continue
                x = j + 1
                y = n - 1

                while x < y:
                    s = nums[i] + nums[j] + nums[x] + nums[y]
                    if s < target:
                        x += 1
                    elif s > target:
                        y -= 1
                    else:
                        ans.append([nums[i],nums[j],nums[x],nums[y]])
                        x += 1
                        while x < y and nums[x] == nums[x-1]:
                            x += 1
                        y -= 1
                        while y > x and nums[y] == nums[y+1]:
                            y -= 1
        return ans
```
