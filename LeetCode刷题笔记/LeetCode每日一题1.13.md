# 2287. Rearrange Characters to Make Target String

<https://leetcode.cn/problems/rearrange-characters-to-make-target-string/>

> 哈希表计数

统计字符串 s 和 target 中每个字符出现的次数，记为 cnt1 和 cnt2。

接下来，对于 target 中的每个字符，我们计算 cnt1 中该字符出现的次数除以 cnt2 中该字符出现的次数，取最小值即可。

```Python
class Solution:
    def rearrangeCharacters(self, s: str, target: str) -> int:
        cnt1 = Counter(s)
        cnt2 = Counter(target)
        return min(cnt1[c] // v for c, v in cnt2.items())
```

> $Counter(string)$
>
> - 从Collections集合模块中引入集合类Counter
>
> - Counter(a)可以打印出数组a中每个元素出现的次数
>
> - Counter(a).most_common(2)可以打印出数组中出现次数最多的元素。参数2表示的含义是：输出几个出现次数最多的元素。

官方题解：<https://leetcode.cn/problems/rearrange-characters-to-make-target-string/solution/zhong-pai-zi-fu-xing-cheng-mu-biao-zi-fu-v5te/>
