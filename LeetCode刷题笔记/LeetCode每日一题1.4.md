# 1802. Maximum Value at a Given Index in a Bounded Array

<https://leetcode.cn/problems/maximum-value-at-a-given-index-in-a-bounded-array/>

> 二分查找

除非优化到$O(\log{maxSum})$，否则超时

![img](https://img2023.cnblogs.com/blog/2975286/202301/2975286-20230104125947100-2100115429.png)

对应以下代码(主要是对确定的$nums[index]$求和时的算法不够快)：

```Python
class Solution:
    def maxValue(self, n: int, index: int, maxSum: int) -> int:
        begin, end = 1, maxSum
        while begin < end:
            mid = begin + (end - begin + 1) // 2
            tol = self.getSum(n, index, mid)
            if tol < maxSum:
                begin = mid
            elif tol > maxSum:
                end = mid - 1
            else:
                return mid
        return begin
    def getSum(self, n: int, index: int, flag: int) -> int:
        res = -flag
        i = j = index
        tmp = flag
        while i >= 0:
            res += tmp
            if tmp > 1:
                tmp -= 1
            i -= 1
        while j < n:
            res += flag
            if flag > 1:
                flag -= 1
            j += 1
        return res
```

以下为官方给出的贪心+二分的代码示例：

```Python
class Solution:
    def maxValue(self, n: int, index: int, maxSum: int) -> int: 
        begin, end = 1, maxSum
        while begin < end:
            mid = (begin + end + 1) // 2
            if self.valid(mid, n, index, maxSum):
                begin = mid
            else:
                end = mid - 1
        return begin

    def valid(self, mid: int, n: int, index: int, maxSum: int) -> bool:
        begin = index
        end = n - index - 1
        return mid + self.cal(mid, begin) + self.cal(mid, end) <= maxSum

    def cal(self, big: int, length: int) -> int:
        if length + 1 < big:
            small = big - length
            return ((big - 1 + small) * length) // 2
        else:
            ones = length - (big - 1)
            return (big - 1 + 1) * (big - 1) // 2 + ones
```

还有纯数学推导的$O(1)$算法，见下述链接：

官方题解：<https://leetcode.cn/problems/maximum-value-at-a-given-index-in-a-bounded-array/solution/you-jie-shu-zu-zhong-zhi-ding-xia-biao-c-aav4/>
