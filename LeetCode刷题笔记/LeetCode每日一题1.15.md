# 2293. Min Max Game

<https://leetcode.cn/problems/min-max-game/>

> 模拟/递归

```Python
class Solution:
    def minMaxGame(self, nums: List[int]) -> int:
        while len(nums) > 2:
            nums = sum([[min(nums[i], nums[i + 1]), max(nums[i + 2], nums[i + 3])] for i in range(0, len(nums), 4)], [])
        return min(nums)
```

```Python
class Solution:
    def minMaxGame(self, nums: List[int]) -> int:
        n = len(nums)
        while n>1:
            for i in range(0,n//2):
                if (i % 2)==0:
                    nums[i] = min(nums[2*i],nums[2*i+1])
                else:
                    nums[i] = max(nums[2*i],nums[2*i+1])
            n = n//2
        return nums[0]
```

官方题解：<https://leetcode.cn/problems/min-max-game/solution/ji-da-ji-xiao-you-xi-by-leetcode-solutio-ucpt/>
