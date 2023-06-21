# 1658. Minimum Operations to Reduce X to Zero

<https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/>

> 滑动窗口

```Python
class Solution:
    def minOperations(self, nums: List[int], x: int) -> int:
        n = len(nums)
        total = sum(nums)

        if total < x:
            return -1
        
        right = 0
        lsum, rsum = 0, total
        ans = n + 1
        for left in range(-1, n - 1):
            if left != -1:
                lsum += nums[left]
            while right < n and lsum + rsum > x:
                rsum -= nums[right]
                right += 1
            if lsum + rsum == x:
                ans = min(ans, (left + 1) + (n - right))
        
        return -1 if ans > n else ans

```

官方题解：<https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/solution/jiang-x-jian-dao-0-de-zui-xiao-cao-zuo-s-hl7u/>