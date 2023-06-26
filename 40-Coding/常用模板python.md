# 基础算法
# 数据结构
## 栈
```python
# lc 1003
st = []
for c in map(ord, s):
	if c > ord('a') and (len(st) == 0 or c - st.pop() != 1):
		return False
	if c < ord('c'):
		st.append(c)
```
# 字符串
## 字符串抠单词
```python
# lc 1455

class Solution:
    def isPrefixOfWord(self, sentence: str, searchWord: str) -> int:
        i, index, n = 0, 1, len(sentence)
        while i < n:
            start = i
            while i < n and sentence[i] != ' ':
                i += 1
            end = i
            if sentence[start:end].startswith(searchWord):
                return index
            index += 1
            i += 1
        return -1
```

## 双指针
```python
# lc 1023

class Solution:
    def camelMatch(self, queries: List[str], pattern: str) -> List[bool]:
        n = len(queries)
        res = [True] * n
        for i in range(n):
            p = 0
            for c in queries[i]:
                if p < len(pattern) and pattern[p] == c:
                    p += 1
                elif c.isupper():
                    res[i] = False
                    break
            if p < len(pattern):
                res[i] = False
        return res
```

## 进制转换
num1转换成b进制的num2
```python
#lc 1016
num2 = ""
while num1:
	num2 = str(num1 % b) + num2
	num1 //= b
```

## 负二进制转换
我们可以判断 n 从低位到高位的每一位，如果该位为 1，那么答案的该位为 1，否则为 0。如果该位为 1，那么我们需要将 n 减去 k。接下来我们更新$n=\lfloor n / 2 \rfloor$,$ k=−k$。继续判断下一位。

最后，我们将答案反转后返回即可。
```python
# lc 1017
def baseNeg2(self, n: int) -> str:
	ans = []
	k = 1
	while n:
		if n % 2:
			ans.append("1")
			n -= k
		else:
			ans.append("0")
		n //= 2
		k *= -1
	return "".join(ans[::-1]) or "0"
```

# 数组
## 一次循环求出数组中两个最大值和最小值
```python
# lc 1913

n = len(nums)
# 数组中最大的两个值
mx1, mx2 = max(nums[0], nums[1]), min(nums[0], nums[1])
# 数组中最小的两个值
mn1, mn2 = min(nums[0], nums[1]), max(nums[0], nums[1])
for i in range(2, n):
	tmp = nums[i]
	if tmp > mx1:
		mx1, mx2 = tmp, mx1
	elif tmp > mx2:
		mx2 = tmp
	if tmp < mn1:
		mn1, mn2 = tmp, mn1
	elif tmp < mn2:
		mn2 = tmp
```

## 直接遍历二维数组的每一列
- zip(\*grid) 能将各行合并成列
```python
# lc 6333

class Solution:
    def findColumnWidth(self, grid: List[List[int]]) -> List[int]:
        ans = []
        for col in zip(*grid):
            mx = 0
            for x in col:
                mx = max(mx, len(str(x)))
            ans.append(mx)
        return ans
```

## 排序后每次取出数组最小数和最大的数
排序后，每次取出的最小和最大的数就是 $\textit{nums}[i]$ 和 $\textit{nums}[n-1-i]$

```python
# lc 2465

nums.sort()
return len(set(nums[i] + nums[-i - 1] for i in range(len(nums) // 2)))
```

## 将数组中的0移至数组末尾
```python
# lc 2460

# 用额外数组
zeros = [x for x in nums if x == 0]
no_zeros = [x for x in nums if x]
return no_zeros + zeros

# 原数组修改
j, n = 0, len(nums)
for i in range(n - 1):
	if nums[i]:
		if nums[i] == nums[i + 1]:
			nums[i] *= 2
			nums[i + 1] = 0
		nums[j] = nums[i]
		j += 1
if nums[-1]:
	nums[j] = nums[-1]
	j += 1
for i in range(j, n):
	nums[i] = 0
return nums
```

# 数学
## 最小公倍数
a, b的最小公倍数等于a * b // gcd(a, b)
```python
# lc 2413

def gcd(a, b):
	return b if gcd(a, a % b) else 0

return a * b // gcd(a, b)
```

## 圆与矩形是否有重叠
[Loading Question... - 力扣（LeetCode）](https://leetcode.cn/problems/circle-and-rectangle-overlapping/solutions/188831/circle-and-rectangle-overlapping-by-ikaruga/)
> 将圆心转移到第一象限，看矩形右上角与圆心的距离是否小于圆的半径

```python
class Solution:
    def checkOverlap(self, radius: int, xCenter: int, yCenter: int, x1: int, y1: int, x2: int, y2: int) -> bool:
        x, y = (x1 + x2) / 2, (y1 + y2) / 2
        p = [abs(xCenter - x), abs(yCenter - y)]
        q = [x2 - x, y2 - y]
        u = [max(p[0] - q[0], 0), max(p[1] - q[1], 0)]
        return u[0] ** 2 + u[1] ** 2 <= radius ** 2
```

# 树和图
## 图的保存
```python
# lc 1042

g = [[] for _ in range(n)]
for u, v in paths:
	g[u - 1].append(v - 1)
	g[v - 1].append(u - 1)
```

## 树上倍增算法
[1483. 树节点的第 K 个祖先 题解 - 力扣（LeetCode）](https://leetcode.cn/problems/kth-ancestor-of-a-tree-node/solutions/2305895/mo-ban-jiang-jie-shu-shang-bei-zeng-suan-v3rw/)
```python
# lc 1483
class TreeAncestor:

    def __init__(self, n: int, parent: List[int]):  # 能够初始化出每个节点的所有祖先
        m = n.bit_length() - 1  # 总共可以有m代祖先
        pa = [[p] + [-1] * m for p in parent]  # [p, -1, -1, ...]
        for i in range(m):  # pa[x][0]、pa[x][1], ..., pa[x][m - 1]; m代祖先
            for x in range(n):  # n个节点
                if (p := pa[x][i]) != -1:  # pa[x][i + 1] = pa[pa[x][i]][i]; pa[x][i] = -1 -> pa[x][i + 1] = -1
                    pa[x][i + 1] = pa[p][i]
        self.pa = pa
        
    def getKthAncestor(self, node: int, k: int) -> int:  # 返回节点第k个祖先
        for i in range(k.bit_length()):
            if (k >> i) & 1:  # k 的二进制从低到高第 i 位是 1; k表示成二进制时，其中的1表示为当前节点的某个祖先
                node = self.pa[node][i]  
                if node < 0: break
        return node


# Your TreeAncestor object will be instantiated and called as such:
# obj = TreeAncestor(n, parent)
# param_1 = obj.getKthAncestor(node,k)
```


## 宽度优先遍历
### 二叉树的宽度优先遍历
[Loading Question... - 力扣（LeetCode）](https://leetcode.cn/problems/fHi6rV/solutions/2315762/python3javacgo-yi-ti-yi-jie-bfs-by-lcbin-e4vo/)
```python
# lc 6335

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def replaceValueInTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # 1. 把下一排的节点值加起来 next_level_sum = 0
        # 2. 再次遍历，更新每个点左右儿子的值
        #   next_level_sum - children_sum
        # if node is None:
        #   return None
        root.val = 0
        q = [root]
        while q:
            tmp = q
            q = []

            next_level_sum = 0
            for node in tmp:
                if node.left:
                    q.append(node.left)
                    next_level_sum += node.left.val
                if node.right:
                    q.append(node.right)
                    next_level_sum += node.right.val
            
            for node in tmp:
                children_sum = (node.left.val if node.left else 0) + (node.right.val if node.right else 0)
                if node.left: node.left.val = next_level_sum - children_sum
                if node.right: node.right.val = next_level_sum - children_sum
        
        return root
```

### 八个方向的宽度优先遍历
```python
# lc LCP41

class Solution:
    def flipChess(self, chessboard: List[str]) -> int:
        def bfs(i: int, j: int) -> int:
            q = deque([(i, j)])
            g = [list(row) for row in chessboard]
            cnt = 0
            while q:
                i, j = q.popleft()
                for a, b in dirs:
                    x, y = i + a, j + b
                    while 0 <= x < m and 0 <= y < n and g[x][y] == "O":  # 按着当前方向寻找
                        x, y = x + a, y + b
                    if 0 <= x < m and 0 <= y < n and g[x][y] == "X":  # 当前方向的末尾是"X"
                        x, y = x - a, y - b  # 减去末尾
                        cnt += max(abs(x - i), abs(y - j))
                        while x != i or y != j:  # 倒着将该方向赋值成黑子
                            g[x][y] = "X"
                            q.append((x, y))
                            x, y = x - a, y - b  
            return cnt


        m, n = len(chessboard), len(chessboard[0])
        dirs = [(a, b) for a in range(-1, 2) for b in range(-1, 2) if a != 0 or b != 0]  # 八个方向
        return max(bfs(i, j) for i in range(m) for j in range(n) if chessboard[i][j] == ".")
```

## 四色定理
```python
# lc 1042
class Solution:
    def gardenNoAdj(self, n: int, paths: List[List[int]]) -> List[int]:
        g = [[] for _ in range(n)]
        for u, v in paths:
            g[u - 1].append(v - 1)
            g[v - 1].append(u - 1)
        
        color = [0] * n;
        for i, nodes in enumerate(g):
            color[i] = (set(range(1, 5)) - {color[j] for j in nodes}).pop()
        return color
```

## Dijkstra
### 朴素版
```python
# lc 6336

class Graph:

    def __init__(self, n: int, edges: List[List[int]]):
        g = [[inf] * n for _ in range(n)]
        for x, y, w in edges:
            g[x][y] = w
        self.g = g

    def addEdge(self, edge: List[int]) -> None:
        self.g[edge[0]][edge[1]] = edge[2]

	# 朴素 Dijkstra
	# 时间复杂度 O(n^2)
    def shortestPath(self, start: int, end: int) -> int:
        n = len(self.g)
        dis = [inf] * n
        dis[start] = 0
        vis = [False] * n

        while True:
            x = -1
            for i in range(n):
                if not vis[i] and (x < 0 or dis[x] > dis[i]):
                    x = i
            if x < 0 or dis[x] == inf:  # x无法从起点到达
                return -1
            if x == end:
                return dis[x]
            vis[x] = True

            for y, w in enumerate(self.g[x]):
                if dis[y] > dis[x] + w:
                    dis[y] = dis[x] + w  



# Your Graph object will be instantiated and called as such:
# obj = Graph(n, edges)
# obj.addEdge(edge)
# param_2 = obj.shortestPath(start, end)
```

## Floyd
```python
# lc 6336

class Graph:

    def __init__(self, n: int, edges: List[List[int]]):
        g = [[inf] * n for _ in range(n)]
        for x, y, w in edges:
            g[x][y] = w
        for i in range(n):
            g[i][i] = 0

        # 定义f[k][i][j]表示除了i和j以外，从i到j的路径中间点上至多为k的时候
        # 从i到j的最短路的长度
        # 分类讨论：
        # 从i到j的最短路中间至多为k - 1
        # 从i到j的最短路中间至多为k：说明k一定为中间节点
        # f[k][i][j] = min(f[k - 1][i][j], f[k - 1][i][k] + f[k - 1][k][j])
        # f[i][j] = min(f[i][j], f[i][k] + f[k][j])
        for k in range(n):
            for i in range(n):
                for j in range(n):
                    s = g[i][k] + g[k][j]
                    if s < g[i][j]:
                        g[i][j] = s
                    # g[i][j] = min(g[i][j], g[i][k] + g[k][j])
        
        self.g = g 

    def addEdge(self, edge: List[int]) -> None:
        # O(n^2)
        g = self.g 
        n = len(g)
        x, y, w = edge 
        if w >= g[x][y]:  # 无效更新
            return 
        g[x][y] = w  # (x, y)作为中间路径
        for i in range(n):
            for j in range(n):
                s = g[i][x] + g[x][y] + g[y][j]
                if s < g[i][j]:
                    g[i][j] = s
                    
                # g[i][j] = min(g[i][j], g[i][x] + g[x][y] + g[y][j])

    def shortestPath(self, start: int, end: int) -> int:
        dis = self.g[start][end]
        return dis if dis < inf else -1



# Your Graph object will be instantiated and called as such:
# obj = Graph(n, edges)
# obj.addEdge(edge)
# param_2 = obj.shortestPath(start, end)
```

# 动态规划
## 记忆化搜索
[分享｜从集合论到位运算，常见位运算技巧分类总结！ - 力扣（LeetCode）](https://leetcode.cn/circle/discuss/CaOJ45/)
```python
# lc 1595

class Solution:
    def connectTwoGroups(self, cost: List[List[int]]) -> int:
        # 包含了非负整数的集合S可以压缩成一个数字
        n, m = len(cost), len(cost[0])
        min_cost = [min(col) for col in zip(*cost)]

        @cache
        def dfs(i: int, j: int) -> int:
            if i < 0:  # 左边线连满了
                return sum(c for k, c in enumerate(min_cost) if j >> k & 1)  # j >> k & 1: k还在集合j中，指k还没连线
            return min(dfs(i - 1, j &~(1 << k)) + c for k, c in enumerate(cost[i]))  # j &~(1 << k)：从集合j中删除k
        return dfs(n - 1, (1 << m) - 1)  # (1 << m) - 1: 全集0, ..., m - 1
```

