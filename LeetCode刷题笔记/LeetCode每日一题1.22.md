# 1815. Maximum Number of Groups Getting Fresh Donuts

<https://leetcode.cn/problems/maximum-number-of-groups-getting-fresh-donuts/>

> 状态转移dp/位运算

```Python
class Solution:
    def maxHappyGroups(self, batchSize: int, groups: List[int]) -> int:
        kWidth = 5
        kWidthMask = (1 << kWidth) - 1

        cnt = Counter(x % batchSize for x in groups)

        start = 0
        for i in range(batchSize - 1, 0, -1):
            start = (start << kWidth) | cnt[i]

        @cache
        def dfs(mask: int) -> int:
            if mask == 0:
                return 0

            total = 0
            for i in range(1, batchSize):
                amount = ((mask >> ((i - 1) * kWidth)) & kWidthMask)
                total += i * amount

            best = 0
            for i in range(1, batchSize):
                amount = ((mask >> ((i - 1) * kWidth)) & kWidthMask)
                if amount > 0:
                    result = dfs(mask - (1 << ((i - 1) * kWidth)))
                    if (total - i) % batchSize == 0:
                        result += 1
                    best = max(best, result)

            return best

        ans = dfs(start) + cnt[0]
        dfs.cache_clear()
        return ans
```

```Python
# Python 代码只添加优化的逻辑，位运算见其他语言
class Solution:
    def maxHappyGroups(self, m: int, groups: List[int]) -> int:
        ans = 0
        cnt = [0] * m
        for x in groups:
            x %= m
            if x == 0:
                ans += 1  # 直接排在最前面
            elif cnt[-x]:
                cnt[-x] -= 1  # 配对
                ans += 1
            else:
                cnt[x] += 1

        @cache  # 记忆化
        def dfs(left: int, cnt: Tuple[int]) -> int:
            res = 0
            cnt = list(cnt)
            for x, c in enumerate(cnt):  # 枚举顾客
                if c:  # cnt[x] > 0
                    cnt[x] -= 1
                    res = max(res, (left == 0) + dfs((left + x + 1) % m, tuple(cnt)))  # x 从 0 开始，这里要 +1
                    cnt[x] += 1
            return res
        return ans + dfs(0, tuple(cnt[1:]))  # 转成 tuple 这样能记忆化
```

官方题解：<https://leetcode.cn/problems/maximum-number-of-groups-getting-fresh-donuts/solution/de-dao-xin-xian-tian-tian-quan-de-zui-du-pzra/>
