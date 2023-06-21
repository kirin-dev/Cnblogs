# 1813. Sentence Similarity III

<https://leetcode.cn/problems/sentence-similarity-iii/>

> 分割字符串+双指针

```Python
class Solution:
    def areSentencesSimilar(self, sentence1: str, sentence2: str) -> bool:
        lst1 = sentence1.split()
        lst2 = sentence2.split()
        i, j = 0, 0
        while i < len(lst1) and i < len(lst2) and lst1[i] == lst2[i]:
            i += 1
        while j < len(lst1) - i and j < len(lst2) - i and lst1[-j - 1] == lst2[-j - 1]:
            j += 1
        return i + j == min(len(lst1), len(lst2))
```

> $str.split()$返回一个字符串切割后的列表(1.3每日一题)

官方题解：<https://leetcode.cn/problems/sentence-similarity-iii/solution/ju-zi-xiang-si-xing-iii-by-leetcode-solu-vjy7/>
