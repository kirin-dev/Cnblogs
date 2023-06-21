# 2351. First Letter to Appear Twice

<https://leetcode.cn/problems/first-letter-to-appear-twice>

> 利用集合Set实现哈希表

```Python
class Solution:
    def repeatedCharacter(self, s: str) -> str:
        hash = set()
        for i in s:
            if i in hash:
                return i
            hash.add(i)
```

> 使用set()方法来创建空的集合/将序列和字典转换为集合

Set方法汇总：<https://www.runoob.com/python3/python3-set.html>

官方题解(位运算 用一个32位的二进制数存储哈希表 $O(N)$)：<https://leetcode.cn/problems/first-letter-to-appear-twice/solution/di-yi-ge-chu-xian-liang-ci-de-zi-mu-by-l-oqu1/>

> 注意到字符集的大小为26，因此我们可以使用一个32位的二进制数完美地存储哈希表。如果seen的第i(0≤i<26)位是1，说明第i个小写字母已经出现过。
>
> 具体地，对字符串s进行一次遍历。当遍历到字母c时，记为第i个字母，seen的第i(0≤i<26)位是1，返回c即可；否则，将seen的第i位置为1。

```Python
class Solution:
    def repeatedCharacter(self, s: str) -> str:
        hash = 0
        for ch in s:
            x = ord(ch) - ord("a")
            if hash & (1 << x):
                return ch
            hash |= (1 << x)
```
