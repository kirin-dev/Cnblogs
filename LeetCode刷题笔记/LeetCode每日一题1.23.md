# 2303. Calculate Amount Paid in Taxes

<https://leetcode.cn/problems/calculate-amount-paid-in-taxes/>

> 模拟

```Python
class Solution:
    def calculateTax(self, brackets: List[List[int]], income: int) -> float:
        ans = prev = 0
        for upper, percent in brackets:
            ans += max(0, min(income, upper) - prev) * percent
            prev = upper
        return ans / 100
```

官方题解：<https://leetcode.cn/problems/calculate-amount-paid-in-taxes/solution/ji-suan-ying-jiao-shui-kuan-zong-e-by-le-jv5s/>
