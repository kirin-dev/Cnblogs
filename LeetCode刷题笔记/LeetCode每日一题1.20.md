# 1817. Finding the Users Active Minutes

<https://leetcode.cn/problems/finding-the-users-active-minutes/>

> 哈希表

```Python
class Solution:
    def findingUsersActiveMinutes(self, logs: List[List[int]], k: int) -> List[int]:
        mp = defaultdict(set)
        for i, t in logs:
            mp[i].add(t)
        ans = [0] * k
        for s in mp.values():
            ans[len(s) - 1] += 1
        return ans
```

> defaultdict(set) 去重字典作哈希表

官方题解：<https://leetcode.cn/problems/finding-the-users-active-minutes/solution/cha-zhao-yong-hu-huo-yue-fen-zhong-shu-b-p5f6/>
