# 最大区间

[蓝桥杯——最大区间](https://www.lanqiao.cn/problems/17152/learning/?isWithAnswer=true)

```python
n = int(input())
nums = list(map(int, input().split()))

def right_bigger():
    st = []
    right = [-1] * n    
    for i in range(n):
        while st and nums[st[-1]] > nums[i]:
            right[st.pop()] = i - 1
        st.append(i)
    return right

def left_bigger():
    st = []
    left = [0] * n
    for i in range(n-1, -1, -1):
        while st and nums[st[-1]] > nums[i]:
            left[st.pop()] = i + 1
        st.append(i)
    return left

left, right = left_bigger(), right_bigger()

ans = 0
for i in range(n):
    ans = max(ans, nums[i] * (right[i] - left[i] + 1))

print(ans)
```
