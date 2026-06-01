# LeetCode 解题完全指南 —— Python 版

> 目标：遇到任何题目都能在 3 分钟内找到切入点，有模板可套，有思路可循。

---

## 目录

1. [通用解题框架](#1-通用解题框架) — 卡住时的思考流程
2. [数据结构速查](#2-数据结构速查) — 每种数据结构的 Python 操作与复杂度
3. [算法模式模板](#3-算法模式模板) — 14 种核心模式的代码模板
4. [Python 内置函数与技巧](#4-python-内置函数与技巧) — 你需要的所有轮子
5. [高频题型逐类拆解](#5-高频题型逐类拆解) — 每类题的思考路径
6. [边界与调试检查清单](#6-边界与调试检查清单) — 提交前的最后一步

---

## 1. 通用解题框架

### 1.1 拿到题目的前 3 分钟

```
第1步：读题 + 标注关键信息
  ├── 输入类型是什么？（数组、字符串、链表、树、图？）
  ├── 输出类型是什么？（bool、int、list、node？）
  ├── 数据规模多大？（决定 O(n²) 还是 O(n log n)）
  └── 有没有特殊约束？（有序？唯一？负数？）

第2步：手动跑一个例子
  └── 用最简单的输入在纸上走一遍，确认你理解了题目

第3步：暴力解法是什么？
  └── 哪怕 O(n²) 也行，先想出来，再优化
```

### 1.2 数据规模 → 时间复杂度对应表

| 数据规模 N | 允许的最大复杂度 | 典型算法 |
|-----------|-----------------|---------|
| ≤ 20 | O(2ⁿ) | 回溯、状态压缩 DP |
| ≤ 100 | O(n³) | Floyd-Warshall |
| ≤ 1,000 | O(n²) | 双层循环、DP |
| ≤ 10⁵ | O(n log n) | 排序、二分、堆 |
| ≤ 10⁶ | O(n) | 单次遍历、双指针、滑动窗口 |
| ≤ 10⁹ | O(log n) | 二分查找、数学公式 |

### 1.3 完全没有思路时的「关键词 → 算法」映射

**看到这个关键词 → 优先想这个算法：**

```
"所有可能/所有组合/所有排列"     → 回溯 (Backtracking)
"最短/最少/最小步数"             → BFS
"最大/最多/最长"                 → DP 或 贪心
"是否存在/能否"                  → DP 或 回溯
"所有子数组/子串"               → 滑动窗口 或 前缀和
"有序数组"                       → 二分查找 或 双指针
"Top K / 第K个最大/最小"         → 堆 (Heap) 或 快速选择
"连续子数组"                     → 滑动窗口 或 前缀和
"两数之和/三数之和"              → 哈希表 或 双指针
"链表相交/环/中点"               → 快慢指针
"树的层序/最短路径"              → BFS
"树的深度/直径/路径"             → DFS (递归)
"图的最短路径"                   → BFS (无权) / Dijkstra (加权)
"图是否存在环"                   → 拓扑排序 / 并查集
"区间重叠/合并"                  → 排序 + 贪心
"股票买卖"                       → DP 或 贪心
"打家劫舍/背包"                  → DP
"回文/子序列"                    → DP 或 中心扩展
```

### 1.4 解题四步法（每题都这样走）

```
Step 1: 暴力枚举 → 先想最笨的办法，确保理解正确
Step 2: 找重复计算 → 哪里在浪费？能用 DP/记忆化/双指针优化吗？
Step 3: 选数据结构 → 哈希表加速查找？堆取最值？栈处理括号？
Step 4: 写代码 + 跑用例 → 正常用例 → 边界用例 → 大用例
```

---

## 2. 数据结构速查

### 2.1 数组 / 列表 (List)

```python
# ========== 创建 ==========
arr = []
arr = [0] * n                     # 长度为 n 的全 0 数组
arr = [i for i in range(n)]      # [0, 1, 2, ..., n-1]
arr = [[0]*m for _ in range(n)]  # n×m 二维数组（注意：不能用 [[0]*m]*n）

# ========== 遍历 ==========
for i in range(len(arr)):        # 按索引
    pass
for i, v in enumerate(arr):      # 索引 + 值
    pass
for v in arr:                    # 只要值
    pass

# ========== 增删改查 ==========
arr.append(x)                     # 尾部添加 O(1)
arr.pop()                         # 尾部删除 O(1)
arr.pop(i)                        # 指定位置删除 O(n)
arr.insert(i, x)                  # 指定位置插入 O(n)
arr.remove(x)                     # 删除第一个值为 x 的元素 O(n)
arr.index(x)                      # 第一个值为 x 的索引 O(n)
x in arr                          # 是否存在 O(n)
arr.count(x)                      # 计数 O(n)

# ========== 切片 ==========
arr[i:j]                          # [i, j) 左闭右开
arr[i:j:k]                        # 步长 k
arr[::-1]                         # 反转（创建新列表）
arr.reverse()                     # 原地反转 O(n)

# ========== 排序 ==========
arr.sort()                        # 原地升序 O(n log n)
arr.sort(reverse=True)            # 原地降序
arr.sort(key=lambda x: x[1])      # 按元素的第二个字段排序
sorted_arr = sorted(arr)          # 返回新列表

# ========== 常用内置函数 ==========
len(arr)                          # 长度
sum(arr)                          # 求和
min(arr) / max(arr)               # 最值
all(arr) / any(arr)               # 全真 / 有真
list(map(fn, arr))                # 批量映射
list(filter(fn, arr))             # 批量过滤
list(zip(arr1, arr2))             # 配对 [(a1,b1), (a2,b2), ...]

# ========== 前缀和（高频！）==========
def build_prefix_sum(arr):
    n = len(arr)
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i+1] = prefix[i] + arr[i]
    return prefix
# 子数组 arr[i:j] 的和 = prefix[j] - prefix[i]   (O(1) 查询！)
```

### 2.2 哈希表 (Dict / HashMap)

```python
# ========== 创建 ==========
d = {}
d = dict()
d = {k: v for k, v in zip(keys, values)}
from collections import defaultdict
d = defaultdict(int)              # 不存在的 key 默认 0
d = defaultdict(list)             # 不存在的 key 默认 []
d = defaultdict(set)              # 不存在的 key 默认 set()

# ========== 增删改查 ==========
d[key] = value                    # 增/改 O(1)
val = d[key]                      # 查（key 不存在报错）O(1)
val = d.get(key, default)         # 查（key 不存在返回 default）
d.pop(key)                        # 删 O(1)
d.pop(key, default)               # 删（不存在返回 default）
key in d                          # 是否存在 O(1)
del d[key]                        # 删 O(1)

# ========== 遍历 ==========
for k in d:                       # 遍历 key
for k, v in d.items():            # 遍历 key + value
for v in d.values():              # 遍历 value

# ========== 常用操作 ==========
len(d)                            # 键值对个数
d.keys() / d.values() / d.items() # 返回视图（可迭代）
list(d.keys())                    # 转为列表

# ========== 计数器（高频！）==========
from collections import Counter
c = Counter(arr)                  # {元素: 频次}
c.most_common(k)                  # 频次最高的 k 个 [('a', 5), ('b', 3)]
c['x'] += 1                       # 更新计数
c1 + c2                           # 合并计数
c1 - c2                           # 差（保留正数）

# ========== 有序字典（LRU 缓存用）==========
from collections import OrderedDict
od = OrderedDict()
od.move_to_end(key)               # 移到末尾
od.popitem(last=False)            # 弹出第一个（FIFO）
od.popitem(last=True)             # 弹出最后一个（LIFO）
```

### 2.3 集合 (Set)

```python
s = set()
s = {1, 2, 3}
s.add(x)                          # 添加 O(1)
s.remove(x)                       # 删除（不存在报错）O(1)
s.discard(x)                      # 删除（不存在不报错）
x in s                            # 是否存在 O(1)
# 集合运算
s1 | s2                           # 并集
s1 & s2                           # 交集
s1 - s2                           # 差集
s1 ^ s2                           # 对称差集
```

### 2.4 栈 (Stack) — 用 List 模拟

```python
stack = []
stack.append(x)                   # 压栈 O(1)
stack.pop()                       # 弹栈 O(1)
stack[-1]                         # 查看栈顶 O(1)
len(stack) == 0                   # 判空

# ========== 单调栈模板（找下一个更大元素）==========
def next_greater(nums):
    n = len(nums)
    res = [-1] * n
    stack = []                     # 存索引，栈内值单调递减
    for i in range(n):
        while stack and nums[stack[-1]] < nums[i]:
            idx = stack.pop()
            res[idx] = nums[i]     # 找到了 idx 的下一个更大元素
        stack.append(i)
    return res
# 下一个更小元素：while 条件改成 > 即可
# 上一个更大/更小：倒序遍历即可
```

### 2.5 队列 (Queue)

```python
# ========== 用 List（不够高效，pop(0) 是 O(n)）==========
# 不推荐

# ========== deque（双端队列，推荐）==========
from collections import deque
q = deque()
q.append(x)                       # 队尾入 O(1)
q.popleft()                       # 队首出 O(1)
q.appendleft(x)                   # 队首入 O(1)
q.pop()                           # 队尾出 O(1)
q[0]                              # 查看队首
q[-1]                             # 查看队尾
len(q)                            # 长度

# ========== 优先队列 / 堆 (Heap) ==========
import heapq
heap = []
heapq.heappush(heap, x)           # 入堆 O(log n)
heapq.heappop(heap)               # 出堆（最小值）O(log n)
heap[0]                           # 查看最小值 O(1)
# 最大堆：存负值
heapq.heappush(heap, -x)
max_val = -heapq.heappop(heap)
# 自定义排序：存元组 (priority, item)
heapq.heappush(heap, (priority, item))
# 批量建堆 O(n)
heapq.heapify(arr)
# 找前 K 大：建小顶堆，维护大小为 k
def top_k_largest(arr, k):
    heap = []
    for x in arr:
        heapq.heappush(heap, x)
        if len(heap) > k:
            heapq.heappop(heap)
    return heap
# 找前 K 小：存负数 或 用 nlargest
heapq.nlargest(k, arr)            # 前 K 大 O(n log k)
heapq.nsmallest(k, arr)           # 前 K 小 O(n log k)
```

### 2.6 链表 (Linked List)

```python
# ========== 定义 ==========
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# ========== 遍历（迭代版）==========
cur = head
while cur:
    # do something with cur.val
    cur = cur.next

# ========== 遍历（递归版）==========
def traverse(node):
    if not node:
        return
    # do something (前序)
    traverse(node.next)
    # do something (后序)

# ========== 反转链表（必背！）==========
def reverse(head):
    prev = None
    cur = head
    while cur:
        nxt = cur.next             # ★ 先保存下一个
        cur.next = prev            # 反转指向
        prev = cur                 # 前移
        cur = nxt                  # 前移
    return prev

# ========== 快慢指针找中点 ==========
def find_middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next           # 走一步
        fast = fast.next.next      # 走两步
    return slow                    # fast 到头时 slow 在中点

# ========== 环形链表检测 ==========
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False

# ========== 环形链表找入口（必背公式）==========
def detect_cycle_entry(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:           # 相遇后
            slow = head            # slow 回到头
            while slow != fast:    # 同速前进
                slow = slow.next
                fast = fast.next
            return slow            # 再次相遇处 = 入口
    return None

# ========== 合并两个有序链表 ==========
def merge(l1, l2):
    dummy = ListNode(0)
    cur = dummy
    while l1 and l2:
        if l1.val < l2.val:
            cur.next = l1
            l1 = l1.next
        else:
            cur.next = l2
            l2 = l2.next
        cur = cur.next
    cur.next = l1 or l2            # 接上剩余的
    return dummy.next

# ========== 第 K 个节点（K 从 1 开始）==========
def kth_node(head, k):
    cur = head
    for _ in range(k - 1):
        if not cur: return None
        cur = cur.next
    return cur

# ========== 倒数第 K 个节点 ==========
def kth_from_end(head, k):
    fast = slow = head
    for _ in range(k):             # fast 先走 k 步
        if not fast: return None
        fast = fast.next
    while fast:                    # 一起走到尾
        slow = slow.next
        fast = fast.next
    return slow
```

### 2.7 二叉树 (Binary Tree)

```python
# ========== 定义 ==========
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# ========== DFS 前序（根→左→右）迭代版 ==========
def preorder(root):
    res = []
    stack = [root]
    while stack:
        node = stack.pop()
        if node:
            res.append(node.val)
            stack.append(node.right)  # ★ 先右后左
            stack.append(node.left)
    return res

# ========== DFS 中序（左→根→右）迭代版 ==========
def inorder(root):
    res = []
    stack = []
    cur = root
    while cur or stack:
        while cur:                   # 一路向左
            stack.append(cur)
            cur = cur.left
        cur = stack.pop()            # 弹出最左
        res.append(cur.val)
        cur = cur.right              # 转向右子树
    return res

# ========== DFS 后序（左→右→根）迭代版 ==========
def postorder(root):
    # 技巧：前序(根右左) 的反转 = 后序(左右根)
    res = []
    stack = [root]
    while stack:
        node = stack.pop()
        if node:
            res.append(node.val)
            stack.append(node.left)
            stack.append(node.right)
    return res[::-1]

# ========== BFS 层序遍历 ==========
from collections import deque
def level_order(root):
    if not root: return []
    res = []
    q = deque([root])
    while q:
        level = []
        for _ in range(len(q)):     # ★ 固定当前层的大小
            node = q.popleft()
            level.append(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.append(level)            # 每层一个列表
    return res

# ========== 树的深度 ==========
def max_depth(root):
    if not root: return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))
# BFS 版：return len(level_order(root))  # 层序遍历的层数

# ========== 树的直径 ==========
def diameter(root):
    res = [0]
    def depth(node):                 # 后序遍历求每个节点为根的深度
        if not node: return 0
        L = depth(node.left)
        R = depth(node.right)
        res[0] = max(res[0], L + R)  # 经过该节点的路径长度
        return 1 + max(L, R)         # 该节点的深度
    depth(root)
    return res[0]

# ========== 验证 BST ==========
def is_valid_bst(root):
    def check(node, low, high):
        if not node: return True
        if node.val <= low or node.val >= high:
            return False
        return check(node.left, low, node.val) and check(node.right, node.val, high)
    return check(root, float('-inf'), float('inf'))

# ========== 最近公共祖先 (LCA) ==========
def lca(root, p, q):
    if not root or root == p or root == q:
        return root
    left = lca(root.left, p, q)
    right = lca(root.right, p, q)
    if left and right:              # p 和 q 分别在两侧
        return root
    return left or right            # 在找到的那一侧

# ========== 二叉搜索树的最近公共祖先 ==========
def lca_bst(root, p, q):
    # BST 版本：利用有序性，O(log n)
    while root:
        if p.val < root.val and q.val < root.val:
            root = root.left
        elif p.val > root.val and q.val > root.val:
            root = root.right
        else:
            return root             # p 和 q 分列两侧，当前就是 LCA
```

### 2.8 图 (Graph)

```python
# ========== 图的表示 ==========
# 邻接表（最常用）
graph = {0: [1, 2], 1: [0, 3], 2: [0], 3: [1]}
# 邻接矩阵（稠密图）
matrix = [[0]*n for _ in range(n)]

# ========== BFS（无权最短路径）==========
def bfs(graph, start):
    from collections import deque
    visited = set([start])
    q = deque([start])
    while q:
        node = q.popleft()
        # 处理当前节点
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                q.append(neighbor)
    return visited

# ========== DFS（连通分量 / 环检测）==========
def dfs(graph, node, visited):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)

# ========== DFS 迭代版（用栈）==========
def dfs_iter(graph, start):
    visited = set()
    stack = [start]
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            for neighbor in graph[node]:
                stack.append(neighbor)
    return visited

# ========== 拓扑排序（Kahn BFS 版）==========
def topological_sort(n, prerequisites):
    # n: 节点数, prerequisites: [(a,b)] 表示 a 依赖 b (b→a)
    from collections import deque
    graph = [[] for _ in range(n)]
    indegree = [0] * n              # 入度表
    for a, b in prerequisites:
        graph[b].append(a)          # b -> a
        indegree[a] += 1

    q = deque([i for i in range(n) if indegree[i] == 0])  # 入度为 0 的入队
    order = []
    while q:
        node = q.popleft()
        order.append(node)
        for neighbor in graph[node]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                q.append(neighbor)

    return order if len(order) == n else []  # 空 = 有环

# ========== 并查集 (Union-Find) ==========
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))  # 初始每个节点是自己的父节点
        self.rank = [1] * n           # 按秩合并
        self.count = n                # 连通分量数

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # ★ 路径压缩
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False              # 已经在同一个集合
        if self.rank[px] < self.rank[py]:  # 小树挂大树
            px, py = py, px
        self.parent[py] = px
        self.rank[px] += self.rank[py]
        self.count -= 1
        return True                   # 合并成功

    def connected(self, x, y):
        return self.find(x) == self.find(y)
```

### 2.9 字符串 (String)

```python
# ========== 基本操作 ==========
s[i]                              # 取字符 O(1)
len(s)                            # 长度
s + t                             # 拼接 O(n+m)
s * n                             # 重复
s[i:j]                            # 切片 O(j-i)
s.find(t)                         # 子串 t 第一次出现的位置，找不到返回 -1
s.find(t, start)                  # 从 start 位置开始找
s.rfind(t)                        # 最后一次出现的位置
s.count(t)                        # 子串 t 出现次数
s.startswith(t) / s.endswith(t)   # 以 t 开头/结尾
t in s                            # t 是否为 s 的子串

# ========== 大小写 ==========
s.upper() / s.lower()
s.capitalize()                    # 首字母大写
s.isupper() / s.islower()
s.isdigit() / s.isalpha() / s.isalnum()

# ========== 分割与拼接 ==========
s.split()                         # 按空白字符分割
s.split(',')                      # 按逗号分割
s.split(',', 1)                   # 最多分割 1 次
s.rsplit(',', 1)                  # 从右边分割
lines = s.splitlines()            # 按换行符分割
','.join(arr)                     # 用逗号拼接
''.join(arr)                      # 直接拼接

# ========== 去除空白 ==========
s.strip()                         # 去两端空白
s.lstrip() / s.rstrip()           # 去左/右空白

# ========== 替换 ==========
s.replace(old, new)               # 全部替换
s.replace(old, new, count)        # 只替换前 count 次

# ========== 字符 ↔ 数字 ==========
ord('a')                          # 97
chr(97)                           # 'a'
ord('A')                          # 65
ord('0')                          # 48

# ========== 列表 ↔ 字符串 ==========
list(s)                           # "abc" → ['a', 'b', 'c']
''.join(arr)                      # ['a', 'b', 'c'] → "abc"

# ========== ★ 不可变的含义 ★ ==========
# Python 字符串不可变！所有操作都返回新字符串
# s[0] = 'x'  ← 这是错的！
# 要修改只能用：s = s[:i] + 'x' + s[i+1:]  或转成 list 再 join 回来
```

---

## 3. 算法模式模板

### 3.1 双指针 (Two Pointers)

```
适用条件：有序数组 / 需要 O(n) 扫描 / 找两个元素满足条件
核心思想：两个指针从不同位置出发，根据条件移动其中一个
```

**模板 A：左右指针（对撞指针）—— 用于有序数组的两数/三数之和**

```python
def two_sum_sorted(nums, target):
    """有序数组的两数之和"""
    l, r = 0, len(nums) - 1
    while l < r:
        s = nums[l] + nums[r]
        if s == target:
            return [l, r]
        elif s < target:
            l += 1                 # 和太小，左指针右移（增大和）
        else:
            r -= 1                 # 和太大，右指针左移（减小和）
    return []

# 扩展：三数之和
def three_sum(nums):
    """找所有不重复的三元组，和为 0"""
    nums.sort()                    # ★ 先排序！
    res = []
    n = len(nums)
    for i in range(n - 2):
        if i > 0 and nums[i] == nums[i-1]:  # 跳过重复的 i
            continue
        l, r = i + 1, n - 1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s == 0:
                res.append([nums[i], nums[l], nums[r]])
                while l < r and nums[l] == nums[l+1]: l += 1  # 跳过重复
                while l < r and nums[r] == nums[r-1]: r -= 1
                l += 1
                r -= 1
            elif s < 0:
                l += 1
            else:
                r -= 1
    return res

# 盛最多水的容器
def max_area(height):
    l, r = 0, len(height) - 1
    res = 0
    while l < r:
        area = min(height[l], height[r]) * (r - l)
        res = max(res, area)
        if height[l] < height[r]:   # 移动较矮的一边
            l += 1
        else:
            r -= 1
    return res
```

**模板 B：快慢指针 —— 用于链表 / 数组原地修改**

```python
def remove_duplicates(nums):
    """删除有序数组的重复项，返回新长度（原地修改）"""
    if not nums: return 0
    slow = 0                       # slow 指向已处理区的末尾
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]
    return slow + 1

def move_zeroes(nums):
    """将所有 0 移到末尾"""
    slow = 0
    for fast in range(len(nums)):
        if nums[fast] != 0:
            nums[slow], nums[fast] = nums[fast], nums[slow]
            slow += 1
```

### 3.2 滑动窗口 (Sliding Window)

```
适用条件：子数组/子串问题 + 需要满足某个条件
核心思想：右边界扩张直到满足条件，左边界收缩直到不满足
```

**模板：可变大小窗口**

```python
def sliding_window(s):
    """求满足条件的最长子串/最短子串"""
    from collections import defaultdict
    window = defaultdict(int)      # 窗口内的字符/元素计数
    left = 0
    res = 0                        # 或 float('inf') 如果求最小

    for right in range(len(s)):
        # 1. 窗口右侧扩张
        window[s[right]] += 1

        # 2. 窗口左侧收缩（当条件不满足时）
        while 条件不满足:
            window[s[left]] -= 1
            if window[s[left]] == 0:
                del window[s[left]]
            left += 1

        # 3. 此时窗口满足条件，更新结果
        res = max(res, right - left + 1)

    return res
```

**实例：无重复字符的最长子串**

```python
def length_of_longest_substring(s):
    window = set()
    left = 0
    res = 0
    for right in range(len(s)):
        while s[right] in window:     # 重复了就收缩
            window.remove(s[left])
            left += 1
        window.add(s[right])
        res = max(res, right - left + 1)
    return res
```

**实例：最小覆盖子串（Hard，背这个）**

```python
def min_window(s, t):
    """s 中覆盖 t 所有字符的最小子串"""
    from collections import Counter
    need = Counter(t)
    window = {}
    have, need_cnt = 0, len(need)  # 已满足的字符种类数，需要满足的总种类数
    left = 0
    res = s + 'x'                  # 初始化为超长字符串
    res_len = float('inf')

    for right in range(len(s)):
        c = s[right]
        window[c] = window.get(c, 0) + 1
        if c in need and window[c] == need[c]:
            have += 1               # 刚好满足了一种字符

        while have == need_cnt:     # 所有字符都满足了，尝试收缩
            if right - left + 1 < res_len:
                res = s[left:right+1]
                res_len = right - left + 1

            c = s[left]
            window[c] -= 1
            if c in need and window[c] < need[c]:
                have -= 1
            left += 1

    return res if res_len != float('inf') else ''
```

**模板：固定大小窗口**

```python
def fixed_window(arr, k):
    """大小为 k 的固定窗口"""
    window_sum = sum(arr[:k])       # 初始窗口
    res = window_sum
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i-k]  # 进一个，出一个
        res = max(res, window_sum)
    return res
```

### 3.3 二分查找 (Binary Search)

```
适用条件：有序数组 / 答案在一个范围内且单调（二分答案）
核心思想：每次排除一半
```

**模板 A：在有序数组中找目标值**

```python
def binary_search(nums, target):
    """标准二分：找到任意一个等于 target 的位置"""
    l, r = 0, len(nums) - 1
    while l <= r:
        mid = l + (r - l) // 2      # ★ 防溢出写法
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            l = mid + 1
        else:
            r = mid - 1
    return -1
```

**模板 B：找第一个 ≥ target 的位置（下界 / bisect_left）**

```python
def lower_bound(nums, target):
    """第一个 >= target 的位置，即 bisect_left"""
    l, r = 0, len(nums)             # ★ 注意 r 是 len(nums)，不是 len(nums)-1
    while l < r:
        mid = l + (r - l) // 2
        if nums[mid] >= target:     # mid 满足条件，往左找
            r = mid
        else:                        # mid 不满足条件，往右找
            l = mid + 1
    return l                         # l 就是插入位置
```

**模板 C：找第一个 > target 的位置（上界 / bisect_right）**

```python
def upper_bound(nums, target):
    """第一个 > target 的位置，即 bisect_right"""
    l, r = 0, len(nums)
    while l < r:
        mid = l + (r - l) // 2
        if nums[mid] > target:      # mid 满足条件，往左找
            r = mid
        else:                        # mid 不满足条件，往右找
            l = mid + 1
    return l
```

**模板 D：二分答案（猜一个值，验证是否可行）**

```python
def binary_answer(possible_range):
    """在范围内二分猜答案，配合 check() 函数"""
    l, r = min_val, max_val         # 答案的可能范围
    while l < r:
        mid = l + (r - l + 1) // 2  # ★ 偏右，避免死循环（求最大时）
        # mid = l + (r - l) // 2    # ★ 偏左，求最小时用这个
        if check(mid):              # mid 可行
            l = mid                  # 试试更大的（求最大时）
            # r = mid                # 试试更小的（求最小时）
        else:
            r = mid - 1             # mid 不可行，往小了找
            # l = mid + 1            # 求最小时
    return l
```

**内置模块：**

```python
import bisect
pos = bisect.bisect_left(arr, x)    # 第一个 >= x 的位置
pos = bisect.bisect_right(arr, x)   # 第一个 > x 的位置
bisect.insort_left(arr, x)          # 插入并保持有序
```

### 3.4 回溯 (Backtracking)

```
适用条件：所有组合/排列/子集/路径
核心思想：做选择 → 递归 → 撤销选择
时间复杂度：通常是指数级 O(2ⁿ) 或 O(n!)
```

**通用模板：**

```python
def backtrack(路径, 选择列表):
    if 满足结束条件:
        res.append(路径[:])          # ★ 复制一份，不能直接 append 引用
        return

    for 选择 in 选择列表:
        if 选择不合法:              # 剪枝
            continue
        做选择                       # 将选择加入路径
        backtrack(路径, 新的选择列表)
        撤销选择                     # ★ 关键！恢复状态
```

**实例：全排列**

```python
def permute(nums):
    res = []
    used = [False] * len(nums)

    def dfs(path):
        if len(path) == len(nums):
            res.append(path[:])      # ★ 复制
            return
        for i in range(len(nums)):
            if used[i]:
                continue
            used[i] = True           # 做选择
            path.append(nums[i])
            dfs(path)
            path.pop()               # 撤销选择
            used[i] = False

    dfs([])
    return res
```

**实例：组合（子集）**

```python
def subsets(nums):
    res = []

    def dfs(start, path):
        res.append(path[:])          # 每个节点都是一个子集
        for i in range(start, len(nums)):
            path.append(nums[i])
            dfs(i + 1, path)         # ★ i+1 保证不重复选
            path.pop()

    dfs(0, [])
    return res

# 迭代版：位运算
def subsets_bit(nums):
    n = len(nums)
    res = []
    for mask in range(1 << n):       # 0 到 2^n - 1
        subset = []
        for i in range(n):
            if mask >> i & 1:        # 第 i 位是否为 1
                subset.append(nums[i])
        res.append(subset)
    return res
```

**实例：组合总和（可重复选）**

```python
def combination_sum(candidates, target):
    res = []

    def dfs(start, path, remain):
        if remain == 0:
            res.append(path[:])
            return
        if remain < 0:
            return
        for i in range(start, len(candidates)):
            path.append(candidates[i])
            dfs(i, path, remain - candidates[i])  # ★ 还是 i，表示可重复选
            path.pop()

    dfs(0, [], target)
    return res
```

**实例：N 皇后**

```python
def solve_n_queens(n):
    res = []
    board = [['.']*n for _ in range(n)]
    cols, diag1, diag2 = set(), set(), set()  # 列、主对角线、副对角线

    def dfs(row):
        if row == n:
            res.append([''.join(row) for row in board])
            return
        for col in range(n):
            d1, d2 = row - col, row + col  # ★ 对角线的索引公式
            if col in cols or d1 in diag1 or d2 in diag2:
                continue
            cols.add(col); diag1.add(d1); diag2.add(d2)
            board[row][col] = 'Q'
            dfs(row + 1)
            board[row][col] = '.'
            cols.remove(col); diag1.remove(d1); diag2.remove(d2)

    dfs(0)
    return res
```

### 3.5 动态规划 (DP)

```
适用条件：最优子结构 + 重叠子问题 / "最值"问题 / 方案数问题
核心思想：定义状态 → 找状态转移方程 → 确定初始值 → 确定遍历顺序

DP 思考五步法：
1. 定义 dp[i] 或 dp[i][j] 的含义
2. 找出 dp[i] 和 dp[i-1] 的关系（状态转移方程）
3. 确定初始值 dp[0]
4. 确定遍历顺序
5. 举例验证
```

**模板 A：一维 DP**

```python
# 爬楼梯：每次爬 1 或 2 阶
def climb_stairs(n):
    if n <= 2: return n
    dp = [0] * (n + 1)
    dp[1], dp[2] = 1, 2
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]     # ★ 这么简单的转移方程，面试先列出来
    return dp[n]

# 空间优化版（只用两个变量）
def climb_stairs_opt(n):
    if n <= 2: return n
    a, b = 1, 2
    for _ in range(3, n + 1):
        a, b = b, a + b
    return b

# 打家劫舍
def rob(nums):
    if not nums: return 0
    if len(nums) == 1: return nums[0]
    dp = [0] * len(nums)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    for i in range(2, len(nums)):
        dp[i] = max(dp[i-1], dp[i-2] + nums[i])  # ★ 偷 or 不偷
    return dp[-1]

# 最大子数组和（Kadane 算法）
def max_subarray(nums):
    dp = nums[0]                     # dp[i] = 以 nums[i] 结尾的最大子数组和
    res = dp
    for i in range(1, len(nums)):
        dp = max(dp + nums[i], nums[i])  # 要么接上，要么重开
        res = max(res, dp)
    return res
```

**模板 B：二维 DP**

```python
# 不同路径
def unique_paths(m, n):
    dp = [[1]*n for _ in range(m)]   # 第一行和第一列都是 1
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
    return dp[-1][-1]

# 最小路径和
def min_path_sum(grid):
    m, n = len(grid), len(grid[0])
    dp = [[0]*n for _ in range(m)]
    dp[0][0] = grid[0][0]
    for i in range(1, m):
        dp[i][0] = dp[i-1][0] + grid[i][0]
    for j in range(1, n):
        dp[0][j] = dp[0][j-1] + grid[0][j]
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
    return dp[-1][-1]

# 最长公共子序列 (LCS)
def longest_common_subsequence(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0]*(n+1) for _ in range(m+1)]
    for i in range(1, m+1):
        for j in range(1, n+1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[-1][-1]
```

**模板 C：背包问题**

```python
# 0-1 背包
def knapsack_01(weights, values, capacity):
    n = len(weights)
    dp = [0] * (capacity + 1)       # ★ 一维优化，倒序遍历
    for i in range(n):
        for w in range(capacity, weights[i] - 1, -1):  # ★ 倒序！
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[capacity]

# 完全背包（每种物品无限个）—— 跟 0-1 背包装法几乎一样，只是正序！
def knapsack_complete(weights, values, capacity):
    n = len(weights)
    dp = [0] * (capacity + 1)
    for i in range(n):
        for w in range(weights[i], capacity + 1):       # ★ 正序！
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[capacity]

# 零钱兑换（最少硬币数）
def coin_change(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for coin in coins:
        for x in range(coin, amount + 1):
            dp[x] = min(dp[x], dp[x - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```

### 3.6 贪心 (Greedy)

```
适用条件：局部最优能推出全局最优 / 排序后能贪心
常见题型：区间调度、跳跃游戏、分发糖果
```

**实例：区间调度（最多不重叠区间）**

```python
def max_non_overlapping(intervals):
    """选择最多的不重叠区间"""
    intervals.sort(key=lambda x: x[1])  # ★ 按结束时间排序
    count = 0
    last_end = float('-inf')
    for start, end in intervals:
        if start >= last_end:            # 不重叠
            count += 1
            last_end = end
    return count
```

**实例：跳跃游戏**

```python
def can_jump(nums):
    """是否能跳到最后一个位置"""
    max_reach = 0
    for i in range(len(nums)):
        if i > max_reach:           # 当前位置不可达
            return False
        max_reach = max(max_reach, i + nums[i])
    return True

def jump_game_2(nums):
    """跳到最后一个位置的最少跳跃次数"""
    jumps = 0
    cur_end = 0                     # 当前跳跃能达到的最远位置
    cur_farthest = 0                # 下一步能达到的最远位置
    for i in range(len(nums) - 1):  # 最后一个位置不用跳
        cur_farthest = max(cur_farthest, i + nums[i])
        if i == cur_end:            # 到达当前边界，必须跳一步
            jumps += 1
            cur_end = cur_farthest
    return jumps
```

### 3.7 BFS（广度优先搜索）

```
适用条件：最短路径/最少步数/层序遍历
核心思想：队列 + visited 集合
```

**通用模板：**

```python
from collections import deque

def bfs(start, target):
    q = deque([start])
    visited = set([start])          # ★ 入队就标记 visited，不是出队时标记！
    step = 0

    while q:
        size = len(q)               # ★ 当前层的节点数
        for _ in range(size):
            cur = q.popleft()
            if cur == target:       # 到达目标
                return step
            for next_node in get_neighbors(cur):
                if next_node not in visited:
                    visited.add(next_node)
                    q.append(next_node)
        step += 1

    return -1                       # 不可达
```

**实例：打开转盘锁**

```python
def open_lock(deadends, target):
    dead = set(deadends)
    if '0000' in dead: return -1
    from collections import deque
    q = deque(['0000'])
    visited = set(['0000'])
    step = 0

    while q:
        for _ in range(len(q)):
            cur = q.popleft()
            if cur == target:
                return step
            # 每个位置向上或向下拨动
            for i in range(4):
                for d in [1, -1]:
                    nxt = cur[:i] + str((int(cur[i]) + d) % 10) + cur[i+1:]
                    if nxt not in dead and nxt not in visited:
                        visited.add(nxt)
                        q.append(nxt)
        step += 1
    return -1
```

### 3.8 DFS（深度优先搜索）

```
适用条件：连通分量 / 岛屿问题 / 树的路径 / 排列组合
核心思想：递归 + visited 数组或直接修改原数据标记
```

**实例：岛屿数量**

```python
def num_islands(grid):
    m, n = len(grid), len(grid[0])
    count = 0

    def dfs(i, j):
        if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] != '1':
            return
        grid[i][j] = '0'            # ★ 直接修改原数组表示已访问
        dfs(i+1, j)                 # 四个方向
        dfs(i-1, j)
        dfs(i, j+1)
        dfs(i, j-1)

    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                count += 1
                dfs(i, j)
    return count
```

**实例：岛屿最大面积**

```python
def max_area_of_island(grid):
    m, n = len(grid), len(grid[0])

    def dfs(i, j):
        if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] != 1:
            return 0
        grid[i][j] = 0
        return 1 + dfs(i+1, j) + dfs(i-1, j) + dfs(i, j+1) + dfs(i, j-1)

    res = 0
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                res = max(res, dfs(i, j))
    return res
```

---

## 4. Python 内置函数与技巧

### 4.1 高频内置函数速览

```python
# ========== 数学 ==========
abs(x)                            # 绝对值
pow(x, y)                         # x^y
round(x, 2)                       # 四舍五入保留 2 位小数
divmod(a, b)                      # (商, 余数) = (a//b, a%b)

# ========== 容器转换 ==========
list(iterable)                    # 转列表
tuple(iterable)                   # 转元组
set(iterable)                     # 转集合（去重）
dict(iterable)                    # 转字典

# ========== 枚举 ==========
enumerate(arr)                    # (0, arr[0]), (1, arr[1]), ...
enumerate(arr, start=1)           # (1, arr[0]), (2, arr[1]), ...

# ========== 拉链 ==========
zip([1,2,3], [4,5,6])             # [(1,4), (2,5), (3,6)]
zip(*matrix)                      # 解压 / 矩阵转置

# ========== 序列操作 ==========
all(iterable)                     # 全真 → True
any(iterable)                     # 有真 → True
max(x, y, z) / min(x, y, z)       # 多参数取最值
sorted(arr, key=fn, reverse=True) # 排序（返回新列表）
reversed(arr)                     # 反转（返回迭代器）
filter(fn, arr)                   # 过滤
map(fn, arr)                      # 映射

# ========== 类型判断 ==========
isinstance(x, int)                # 类型判断
type(x)                           # 获取类型
```

### 4.2 列表/字典推导式

```python
# 列表推导
squares = [x**2 for x in range(10)]
evens = [x for x in arr if x % 2 == 0]
pairs = [(i, j) for i in range(3) for j in range(3)]

# 字典推导
freq = {x: arr.count(x) for x in set(arr)}
d = {k: v for k, v in zip(keys, values)}

# 集合推导
unique_squares = {x**2 for x in range(-5, 5)}
```

### 4.3 位运算

```python
x & 1                             # 判断奇偶（1=奇，0=偶）
x & (x - 1)                       # 清除最低位的 1
x & -x                            # 获取最低位的 1
1 << k                            # 2^k
n >> k & 1                        # 判断 n 的第 k 位是否为 1
n ^ (1 << k)                      # 翻转 n 的第 k 位
x ^ x                             # = 0（自异或为 0）
x ^ 0                             # = x（异或 0 为自身）
a ^ b ^ a                         # = b（异或满足交换律）
```

### 4.4 二分查找模块

```python
import bisect
i = bisect.bisect_left(arr, x)    # 第一个 >= x 的位置
i = bisect.bisect_right(arr, x)   # 第一个 > x 的位置
bisect.insort(arr, x)             # 插入并保持有序
```

### 4.5 排列组合模块

```python
from itertools import permutations, combinations, product

list(permutations(arr, 2))        # 排列：[(a,b), (a,c), (b,a), ...]
list(combinations(arr, 2))        # 组合：[(a,b), (a,c), (b,c), ...]
list(product(arr1, arr2))         # 笛卡尔积
list(product(range(3), repeat=2)) # 自己 × 自己
```

### 4.6 functools 高频用法

```python
from functools import lru_cache, reduce

# 递归记忆化（一行搞定 DP！）
@lru_cache(None)
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

# reduce 聚合
from functools import reduce
product = reduce(lambda x, y: x * y, arr)  # 连乘
```

### 4.7 一行代码解决问题

```python
# 判断回文
is_palindrome = s == s[::-1]

# 字符串倒序
reversed_s = s[::-1]

# 找数组中出现次数最多的元素
most_common = max(set(arr), key=arr.count)
# 更好的方法（O(n)）：
from collections import Counter
most_common = Counter(arr).most_common(1)[0][0]

# 判断字符串是否全为字母/数字
s.isalnum()

# Python 大整数天然支持（不用管溢出）
big_num = 2 ** 1000               # 完全没问题

# 快速交换
a, b = b, a

# 三元表达式
res = a if condition else b

# 取二维数组的某一列
col = [row[i] for row in matrix]

# 扁平化列表
flat = [x for row in matrix for x in row]
```

---

## 5. 高频题型逐类拆解

### 5.1 两数之和 / N 数之和

```
思考路径：
1. 无序 → 哈希表（存 target-num，遍历时间 O(n)）
2. 有序 → 双指针（左右夹逼）
3. 去重 → 排序 + 跳过相同值
```

### 5.2 子数组问题

```
思考路径：
1. 连续子数组 → 滑动窗口 或 前缀和
2. 前缀和 + 哈希表 → "和为 K 的子数组"（高频！！！）
3. 最大子数组和 → Kadane（dp[i] = max(dp[i-1]+x, x)）
```

```python
# 和为 K 的子数组（必背！前缀和 + 哈希表）
def subarray_sum(nums, k):
    prefix_count = {0: 1}          # 前缀和为 0 出现了 1 次
    prefix_sum = 0
    count = 0
    for x in nums:
        prefix_sum += x
        # 如果 prefix_sum - k 出现过，说明存在子数组和为 k
        count += prefix_count.get(prefix_sum - k, 0)
        prefix_count[prefix_sum] = prefix_count.get(prefix_sum, 0) + 1
    return count
```

### 5.3 链表题

```
思考路径：
- 找中点 → 快慢指针
- 反转 → 三指针翻转
- 环 → 快慢指针 + 数学
- 合并 → 归并
- 倒数第 K → 双指针
```

### 5.4 二叉树题

```
思考路径：
- 遍历 → 前/中/后序选一个（中序=有序通常是 BST）
- 深度/直径/路径 → 后序遍历（自底向上）
- 层序 → BFS
- BST → 利用左小右大缩小范围
```

### 5.5 回文问题

```
思考路径：
1. 判断回文 → 双指针
2. 最长回文子串 → 中心扩展法 O(n²)
3. 最长回文子序列 → DP
4. 回文分割 → 回溯 + 预计算回文表
```

```python
# 中心扩展法（最长回文子串）
def longest_palindrome(s):
    res = ''
    for i in range(len(s)):
        odd = expand(s, i, i)     # 奇数长度回文
        even = expand(s, i, i+1)  # 偶数长度回文
        res = max(res, odd, even, key=len)
    return res

def expand(s, l, r):
    while l >= 0 and r < len(s) and s[l] == s[r]:
        l -= 1; r += 1
    return s[l+1:r]               # 注意：l 和 r 已经越界
```

### 5.6 股票问题

```
思考路径：全部是 DP 变种，核心是记录"之前的最低价"
```

```python
# 只能买卖一次
def max_profit_1(prices):
    min_price = float('inf')
    max_profit = 0
    for p in prices:
        min_price = min(min_price, p)
        max_profit = max(max_profit, p - min_price)
    return max_profit

# 可以买卖无限次
def max_profit_2(prices):
    profit = 0
    for i in range(1, len(prices)):
        if prices[i] > prices[i-1]:
            profit += prices[i] - prices[i-1]  # 吃掉所有上涨
    return profit
```

### 5.7 括号问题

```
思考路径：
- 有效括号 → 栈（左括号入栈，右括号匹配弹出）
- 最长有效括号 → DP 或 栈存下标
- 生成括号 → 回溯
```

```python
# 最长有效括号
def longest_valid_parentheses(s):
    stack = [-1]                   # 栈底放 -1 作为基准
    max_len = 0
    for i, c in enumerate(s):
        if c == '(':
            stack.append(i)
        else:
            stack.pop()            # 弹出匹配的 '('
            if not stack:          # 栈空了，说明这个 ')' 是断点
                stack.append(i)    # 断点入栈
            else:
                max_len = max(max_len, i - stack[-1])
    return max_len
```

### 5.8 拓扑排序

```
场景：课程安排 / 任务调度 / 依赖关系
核心：BFS + 入度表
```

---

## 6. 边界与调试检查清单

### 6.1 提交前的 8 条检查

```
□ 空输入：[] / "" / None / 0
□ 单元素：长度为 1 的输入
□ 重复值：有重复元素的数组
□ 全相同：所有元素都一样
□ 正负混合：有正数也有负数
□ 大输入：极端大的 n
□ 整数溢出：Python 不担心，但要知道
□ 索引越界：while 循环里的条件是否安全
```

### 6.2 常见bug速查

| 症状 | 常见原因 |
|------|---------|
| 死循环 | while l < r 写成 while l <= r；二分边界没更新 |
| 结果多/少一个 | 索引从 0 还是 1 开始搞混了 |
| 结果全错 | dp 初始值设错了；visited 没在入队时标记 |
| 引用问题 | path.append() 后没 copy，回溯没恢复状态 |
| 超时 | O(n²) 解法投了大数据；应该用哈希表优化 |
| 二维数组出了问题 | 用了 [[0]*m]*n（所有行共享引用！）|

### 6.3 面试现场话术

```
"我先说一下思路..."              ← 别闷头写，先说思路
"我举几个例子验证一下..."         ← 写完主动测试
"这个解法的时间复杂度是...空间复杂度是..."  ← 主动分析
"这个边界情况我处理一下..."       ← 展示边界意识
"有没有更优解？我分析一下..."     ← 如果卡住了，分析限制条件
```

---

> **最后一条建议**：每道题做完后，用一句话总结"这题为什么用这个算法"。积累 50 道题的一句话总结，你就有思路了。
