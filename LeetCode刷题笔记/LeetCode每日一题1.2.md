# 1801. Number of Orders in the Backlog

<https://leetcode.cn/problems/number-of-orders-in-the-backlog>

> 优先队列+模拟

目前不清楚Python3中是如何实现优先队列的，先借鉴他人的解题思路：

类似于在steam，有人来求买有人来求卖，买的肯定要价格高于卖的，愿意花这么多钱去买，或者愿意这么多钱就卖出去。并且考虑一个价格的大小顺序和先来后到，明显就是用队列了。

所以需要两个队列，去记录求买和求卖的订单，并且一直维护最小值。（对于买的价格，取个负数，因为买是反过来的，越高越容易买）

剩下的就是模拟了，考虑两种情况：

- 积压的买或卖订单数量比现在最新卖或买订单多，并且能够匹配，这个好说，直接处理。
- 如果反过来，不够订单，或者匹配不上，那么就要将最新的这个单子，能处理多少处理多少，不够的加入到对应的heap里，等于需要积压了。
故事讲完了，确实就只剩下模拟的代码了，用一个while，不断循环，最后用sum把两个订单列表求和处理一下。

```Python
class Solution:
    def getNumberOfBacklogOrders(self, orders: List[List[int]]) -> int:
        MOD = 10 ** 9 + 7
        buyOrders, sellOrders = [], []
        for price, amount, type in orders:
            if type == 0:
                while amount and sellOrders and sellOrders[0][0] <= price:
                    if sellOrders[0][1] > amount:
                        sellOrders[0][1] -= amount
                        amount = 0
                        break
                    amount -= heappop(sellOrders)[1]
                if amount:
                    heappush(buyOrders, [-price, amount])
            else:
                while amount and buyOrders and -buyOrders[0][0] >= price:
                    if buyOrders[0][1] > amount:
                        buyOrders[0][1] -= amount
                        amount = 0
                        break
                    amount -= heappop(buyOrders)[1]
                if amount:
                    heappush(sellOrders, [price, amount])
        return (sum(x for _, x in buyOrders) + sum(x for _, x in sellOrders)) % MOD
```

官方题解：<https://leetcode.cn/problems/number-of-orders-in-the-backlog/solution/ji-ya-ding-dan-zhong-de-ding-dan-zong-sh-6g22/>
