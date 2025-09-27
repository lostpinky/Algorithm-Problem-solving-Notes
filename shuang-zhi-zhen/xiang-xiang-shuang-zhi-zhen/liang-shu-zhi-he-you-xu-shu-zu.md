# 两数之和-有序数组

[167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)\


双指针从数组左端和右端开始：

1. 如果和大于目标值，那么其他所有数与nums\[r]之和均大于目标值，去掉nums\[r]，将r左移
2. 如果和小于目标值，那么其他所有数与nums\[l]之和均小于目标值，去掉nums\[l]，将l右移

注：每次判断时，l和r对应候选列表的左端和右端，其他数字已被排除，因此无需考虑  

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        n = len(numbers)
        l = 0
        r = n-1

        while l < r:
            s = numbers[l] + numbers[r]
            if s == target:
                return [l+1,r+1]
            elif s < target:
                l += 1
            else:
                r -= 1
```
