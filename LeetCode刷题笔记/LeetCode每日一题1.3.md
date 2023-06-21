# 2042. Check if Numbers Are Ascending in a Sentence

<https://leetcode.cn/problems/check-if-numbers-are-ascending-in-a-sentence>

> 字符串分割

```python
class Solution:
    def areNumbersAscending(self, s: str) -> bool:
        arr = s.split()
        tmp = -1
        for i in arr:
            if i.isdigit():
                if tmp < int(i):
                    tmp = int(i)
                else:
                    return False
        return True
```

> $string.split(separator, max)$将字符串拆分为列表
>
> 参数separator可选：规定分割字符串时要使用的分隔符 默认值为空白字符
>
> 参数max可选：规定要执行的拆分数 默认值为-1 即“所有出现次数”

> $string.isdigit()$判断字符串是否为数字

官方题解：<https://leetcode.cn/problems/check-if-numbers-are-ascending-in-a-sentence/solution/jian-cha-ju-zi-zhong-de-shu-zi-shi-fou-d-uhaf/>
