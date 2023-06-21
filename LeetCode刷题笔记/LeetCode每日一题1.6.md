# 2180. Count Integers With Even Digit Sum

<https://leetcode.cn/problems/count-integers-with-even-digit-sum/>

> 从0开始，每10个数有5个满足条件。减去10的倍数后再暴力。

```Python
class Solution:
    def countEven(self, num: int) -> int:
        tmp = num // 10
        res = -1 + tmp * 5
        tar = tar0 = tmp * 10
        i = 0
        while tar:
            i += tar % 10
            tar //= 10
        if i % 2 == 0:
            res += 1    
        else:
            tar0 -= 1
        while tar0 + 2 <= num:
            res += 1
            tar0 += 2
        return res
```

官方题解：<https://leetcode.cn/problems/count-integers-with-even-digit-sum/solution/tong-ji-ge-wei-shu-zi-zhi-he-wei-ou-shu-rvqol/>
