# 1819. Number of Different Subsequences GCDs

<https://leetcode.cn/problems/number-of-different-subsequences-gcds/>

> 枚举

```Python
class Solution:
    def countDifferentSubsequenceGCDs(self, nums: List[int]) -> int:
        nums=set(nums)
        ans,maxn=0,max(nums)
        for i in range(1,maxn+1):
            tmp=None
            for x in range(i,maxn+1,i):
                if x in nums:
                    if not tmp:
                        tmp=x
                    else:
                        tmp=gcd(tmp,x)
                    if tmp==i:
                        ans+=1
                        break
        return ans
```

> $gcd(*integers)$返回最大公约数

官方题解：<https://leetcode.cn/problems/number-of-different-subsequences-gcds/solution/xu-lie-zhong-bu-tong-zui-da-gong-yue-shu-ha1j/>
