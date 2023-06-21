# 1807. Evaluate the Bracket Pairs of a String

<https://leetcode.cn/problems/evaluate-the-bracket-pairs-of-a-string/>

> 直接遍历/哈希表

直接遍历效率较低(api战士)：

```Python
class Solution:
    def evaluate(self, s: str, knowledge: List[List[str]]) -> str:
        lst = re.findall(r'\(.*?\)', s)
        d = dict(knowledge)
        for i in lst:
            i = i[1: len(i) - 1]
            if i in d:
                s = s.replace('(' + i + ')', d[i])
            else:
                s = s.replace('(' + i + ')', '?')
        return s
```

> $string.replace(old, new, count)$ 替换字符串中的内容

> $import re$ 字符串匹配 正则表达式
>
> - re.sub(a, s)
> - re.findall(a, s)
> - re.compile(a, s)
> - re.match(a, s)

官方哈希题解：

```Python
class Solution:
    def evaluate(self, s: str, knowledge: List[List[str]]) -> str:
        d = dict(knowledge)
        ans, start = [], -1
        for i, c in enumerate(s):
            if c == '(':
                start = i
            elif c == ')':
                ans.append(d.get(s[start + 1: i], '?'))
                start = -1
            elif start < 0:
                ans.append(c)
        return "".join(ans)
```

官方题解：<https://leetcode.cn/problems/evaluate-the-bracket-pairs-of-a-string/solution/ti-huan-zi-fu-chuan-zhong-de-gua-hao-nei-y8d3/>
