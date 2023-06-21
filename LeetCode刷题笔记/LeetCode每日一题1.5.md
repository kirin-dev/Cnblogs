# 1803. Count Pairs With XOR in a Range

<https://leetcode.cn/problems/count-pairs-with-xor-in-a-range/>

> 哈希表/字典树

> 异或的题如果是静态统计很可能01trie可以被哈希表替换

先给出一个$O(N^2)$的暴力解法(显然超时)：

```Python
class Solution:
    def countPairs(self, nums: List[int], low: int, high: int) -> int:
        res = 0
        for i in range(0, len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] ^ nums[j] in range(low, high + 1):
                    res += 1
        return res
```

抄个优化思路：<https://leetcode.cn/problems/count-pairs-with-xor-in-a-range/solution/bu-hui-zi-dian-shu-zhi-yong-ha-xi-biao-y-p2pu/>

```Python
class Solution:
    def countPairs(self, nums: List[int], low: int, high: int) -> int:
        ans, cnt = 0, Counter(nums)
        high += 1
        while high:
            nxt = Counter()
            for x, c in cnt.items():
                # high%2*cnt[(high-1)^x] 相当于 cnt[(high-1)^x] if high%2 else 0
                ans += c * (high % 2 * cnt[(high - 1) ^ x] - low % 2 * cnt[(low - 1) ^ x])
                nxt[x >> 1] += c
            cnt = nxt
            low >>= 1
            high >>= 1
        return ans // 2
```

官方题解(字典树，无Python3解法)：<https://leetcode.cn/problems/count-pairs-with-xor-in-a-range/solution/tong-ji-yi-huo-zhi-zai-fan-wei-nei-de-sh-cu18/>
