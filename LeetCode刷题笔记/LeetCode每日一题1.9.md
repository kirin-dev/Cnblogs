# 1806. Minimum Number of Operations to Reinitialize a Permutation

<https://leetcode.cn/problems/minimum-number-of-operations-to-reinitialize-a-permutation/>

> 直接模拟/数学推导

```Python
class Solution:
    def reinitializePermutation(self, n: int) -> int:
        perm = [i for i in range(n)] # (列表推导式)
        tar = perm.copy()
        cnt = 0
        while True:
            cnt += 1
            perm = [perm[n // 2 + (i - 1) // 2] if i % 2 else perm[i // 2] for i in range(n)] # (列表推导式)
            if perm == tar:
                return cnt
```

> list.copy() 拷贝列表

> reversed(seq) reversed函数返回一个反转的迭代器。(seq可以是 tuple, string, list 或 range。)

```Python
class Solution:
    def reinitializePermutation(self, n: int) -> int:
        if n == 2:
            return 1
        step, pow2 = 1, 2
        while pow2 != 1:
            step += 1
            pow2 = pow2 * 2 % (n - 1)
        return step
```

官方题解(数学推导思路)：<https://leetcode.cn/problems/minimum-number-of-operations-to-reinitialize-a-permutation/solution/huan-yuan-pai-lie-de-zui-shao-cao-zuo-bu-d9cn/>
