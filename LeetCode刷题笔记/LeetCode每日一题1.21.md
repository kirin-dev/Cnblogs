# 1824. Minimum Sideway Jumps

<https://leetcode.cn/problems/minimum-sideway-jumps/>

> dp

![img](https://img2023.cnblogs.com/blog/2975286/202301/2975286-20230121084409363-1862195813.png)

```Python
class Solution:
    def minSideJumps(self, obstacles: List[int]) -> int:
        d = [1, 0, 1]
        for i in range(1, len(obstacles)):
            minCnt = inf
            for j in range(3):
                if j == obstacles[i] - 1:
                    d[j] = inf
                else:
                    minCnt = min(minCnt, d[j])
            for j in range(3):
                if j != obstacles[i] - 1:
                    d[j] = min(d[j], minCnt + 1)
        return min(d)
```

官方题解：<https://leetcode.cn/problems/minimum-sideway-jumps/solution/zui-shao-ce-tiao-ci-shu-by-leetcode-solu-3y2g/>
