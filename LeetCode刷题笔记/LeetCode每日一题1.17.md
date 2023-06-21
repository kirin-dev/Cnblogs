# 1814. Count Nice Pairs in an Array

<https://leetcode.cn/problems/count-nice-pairs-in-an-array/>

> 哈希表

```Python
class Solution:
    def countNicePairs(self, nums: List[int]) -> int:
        res = 0
        cnt = Counter()
        for i in nums:
            j = int(str(i)[::-1])
            res += cnt[i - j]
            cnt[i - j] += 1
        return res % (10 ** 9 + 7)
```

官方题解：<https://leetcode.cn/problems/count-nice-pairs-in-an-array/solution/tong-ji-yi-ge-shu-zu-zhong-hao-dui-zi-de-ywux/>
