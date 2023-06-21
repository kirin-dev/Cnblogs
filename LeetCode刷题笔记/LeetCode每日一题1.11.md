# 2283. Check if Number Has Equal Digit Count and Digit Value

<https://leetcode.cn/problems/check-if-number-has-equal-digit-count-and-digit-value/>

> 直接模拟/哈希表

```Python
class Solution:
    def digitCount(self, num: str) -> bool:
        tar = list(num)
        lst = [0] * len(num)
        for i in range(len(num)):
            lst[i] = str(num.count(str(i)))
        if lst == tar: return True
        else: return False
```

> $str.count(sub, start= 0,end=len(string))$返回子字符串在字符串中出现的次数

```Python
class Solution:
    # from collections import Counter
    def digitCount(self, num: str) -> bool:
        h = Counter(num)
        for idx, v in enumerate(num):
            if h[str(idx)] != int(v):
                return False
        return True
```

> enumerate(iteration, start)函数默认包含两个参数，其中iteration参数为需要遍历的参数，比如字典、列表、元组等，start参数为开始的参数，默认为0（不写start那就是从0开始）。enumerate函数有两个返回值，第一个返回值为从start参数开始的数，第二个参数为iteration参数中的值。

官方题解：<https://leetcode.cn/problems/check-if-number-has-equal-digit-count-and-digit-value/solution/pan-duan-yi-ge-shu-de-shu-zi-ji-shu-shi-ozwa7/>
