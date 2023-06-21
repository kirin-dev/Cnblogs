# 2185. Counting Words With a Given Prefix

<https://leetcode.cn/problems/counting-words-with-a-given-prefix/>

> 简单遍历

```Python
class Solution:
    def prefixCount(self, words: List[str], pref: str) -> int:
        res = 0
        for i in words:
            if i.startswith(pref):
                res += 1
        return res
        # return sum(w.startswith(pref) for w in words) 推导式
```

> $str1.startswith(str2)$判断str2是否为str1的前缀

官方题解：<https://leetcode.cn/problems/counting-words-with-a-given-prefix/solution/tong-ji-bao-han-gei-ding-qian-zhui-de-zi-aaq7/>
