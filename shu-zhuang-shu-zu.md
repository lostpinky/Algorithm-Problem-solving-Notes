# 树状数组

[树状数组](https://oi-wiki.org/ds/fenwick/)是一种支持 **单点修改** 和 **区间查询** 的，代码量小的数据结构。

普通树状数组维护的信息及运算要满足 **结合律** 且 **可差分**（具有逆运算的运算），如加法（和）、乘法（积）、异或等。

树状数组能快速求解信息的原因：我们总能将一段前缀\[1, n]拆成 **不多于log n段区间**，使得这**log n**段区间的信息是**已知的**。

现定义**c**数组用来储存原始数组**a**某段区间的和，则**c\[x]管辖的区间是\[x - lowbit(x) + 1, x]**

举个例子，![](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7) c\[88]管辖的是哪个区间？

因为88  = 0b 0101 1000，其二进制最低位的 `1` 以及后面的 `0` 组成的二进制是 `1000`，即 8，所以c\[88]管辖 8 个a数组中的元素。

```python
def lowbit(x):
    """
    x 的二进制中，最低位的 1 以及后面所有 0 组成的数。  
    lowbit(0b01011000) == 0b00001000
            ~~~~~^~~
    lowbit(0b01110010) == 0b00000010
            ~~~~~~~^~
    """
    return x & -x
```

## 树状数组模板（维护前缀最大值） 

```python
class BIT:
    def init(self, n: int):
        self.tree = [-inf] * n
        
    def update(self, i: int, val: int) -> None:
        while i < len(self.tree):
            self.tree[i] = max(self.tree[i], val) 
            i += i & -i
    
    def pre_max(self, i: int) -> int:
        mx = -inf
        while i > 0:
            mx = max(mx, self.tree[i])
            i &= i - 1
        return mx
```
