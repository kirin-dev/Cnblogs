# 753. Cracking the Safe

<https://leetcode.cn/problems/cracking-the-safe/>

> Hierholzer 算法可以在一个欧拉图中找出欧拉回路。

看不太懂题目，先码个题解：

```Python
class Solution:
    def crackSafe(self, n: int, k: int) -> str:
        seen = set()
        ans = list()
        highest = 10 ** (n - 1)

        def dfs(node: int):
            for x in range(k):
                nei = node * 10 + x
                if nei not in seen:
                    seen.add(nei)
                    dfs(nei % highest)
                    ans.append(str(x))

        dfs(0)
        return "".join(ans) + "0" * (n - 1)
```

官方题解：<https://leetcode.cn/problems/cracking-the-safe/solution/po-jie-bao-xian-xiang-by-leetcode-solution/>
