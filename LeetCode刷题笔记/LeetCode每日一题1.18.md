# 1825. Finding MK Average

<https://leetcode.cn/problems/finding-mk-average/>

> 有序数组+线段树/有序列表维护左右k区间，然后分类讨论中间的值

码个题解：

```Python
class Node:
    def __init__(self):
        self.left = self.right = None
        self.val = 0
class segtree:
    def __init__(self):
        self.root = Node()
        self.build(self.root,0,10**5)
    def build(self,node,l,r):
        if l == r:return 
        mid = (l+r)//2
        node.left = Node()
        node.right = Node()
        self.build(node.left,l,mid)
        self.build(node.right,mid+1,r)
    def add(self,node,idx,val,l=0,r=10**5):
        if l == r:
            node.val += val*idx
            return
        mid = (l+r)//2
        if idx <= mid:
            self.add(node.left,idx,val,l,mid)
        else:
            self.add(node.right,idx,val,mid+1,r)
        node.val += val*idx
    def sums(self,node,left,right,l=0,r=10**5):
        if l == left and r == right:return node.val
        mid = (l+r)//2
        res = 0
        if left <= mid:
            res += self.sums(node.left,left,min(right,mid),l,mid)
        if right >= mid+1:
            res += self.sums(node.right,max(left,mid+1),right,mid+1,r)
        return res
from sortedcontainers import SortedList
class MKAverage:
    def __init__(self, m: int, k: int):
        self.m = m
        self.k = k
        self.d = deque([])
        self.s = SortedList()
        self.t = segtree()
        self.root = self.t.root

    def addElement(self, num: int) -> None:
        self.d.append(num)
        self.s.add(num)
        self.t.add(self.root,num,1)
        if len(self.d) > self.m:
            p = self.d.popleft()
            self.s.remove(p)
            self.t.add(self.root,p,-1)
    def calculateMKAverage(self) -> int:
        if len(self.d) < self.m:return -1
        m,d,s,t,root,k = self.m,self.d,self.s,self.t,self.root,self.k
        lens = m-2*k
        res = 0
        aa,dd = s[k-1],s[-k]
        if aa == dd:return aa
        bb = s.bisect_right(aa)
        res += (bb-k)*aa
        cc = s.bisect_left(dd)
        res += (m-cc-k)*dd
        res += t.sums(root,aa+1,dd-1)
        return res//lens
```

```Python
from sortedcontainers import SortedList
class MKAverage:
    def __init__(self, m: int, k: int):
        self.m,self.k = m,k
        self.d = deque([])
        self.s = SortedList()
        self.sums = 0
    def addElement(self, num: int) -> None:
        d,s,k,m = self.d,self.s,self.k,self.m
        if len(d) < 2*k:
            d.append(num)
            s.add(num)
            return
        elif len(d) < m:
            d.append(num)
        elif len(d) == m:
            d.append(num)
            p = d.popleft()
            aa,bb = s[k-1],s[-k]
            if p <= aa:self.sums -= s[k]
            elif p >= bb:self.sums -= s[-k-1]
            else:self.sums -= p
            s.remove(p)
        aa,bb = s[k-1],s[-k]
        if num <= aa:self.sums += aa
        elif num > bb:self.sums += bb
        else:self.sums += num
        s.add(num)
    def calculateMKAverage(self) -> int:
        if len(self.d) < self.m:return -1
        return self.sums//(self.m-self.k*2)
```

官方题解：<https://leetcode.cn/problems/finding-mk-average/solution/qiu-chu-mk-ping-jun-zhi-by-leetcode-solu-sos6/>
