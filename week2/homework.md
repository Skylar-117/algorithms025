作业题 - 有效的字母异位词 (LC242- easy)
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        d1, d2 = {}, {}
        for i in s:
            d1[i] = d1.get(i, 0) + 1
        for i in t:
            d2[i] = d2.get(i, 0) + 1
        return d1 == d2
```
【思路】hashtable，使用dict.get(key[, default])来计数（或者用collections.Counter）  

==========

作业题 - 两数之和 (L1 - easy)
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # method I:
        d = {}
        for i, val in enumerate(nums):
            diff = target - val
            if (d.get(diff) == None):
                d[val] = i
            else:
                return [i, d.get(diff)]
        
        # method II:
        d = {}
        for i, val in enumerate(nums):
            diff = target - val
            if (diff not in d):
                d[val] = i
            else:
                return [i, d[diff]]
```
【思路】hashtable，反向思维找差值 “target - x” 是否存在于 list 中，而非正向思维找求和 “x + y” 是否等于target。正向思维的复杂度是 O(n^2)，而反向思维的复杂度是 O(n)  

==========

作业题 - N 叉树的前序遍历 (LC589 - easy)
```python
# method I: recursive
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        res = []
        self.dfs(res, root)
        return res
    
    def dfs(self, res, root):
        if (root is None): return
        res.append(root.val)
        for child in root.children:
            self.dfs(res, child) # 每次都往下移动一个孩子，看以当前孩子节点为跟的子树
# method II: iterative
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        res, stack = [], [root]
        while (stack):
            root = stack.pop()
            if (root):
                res.append(root.val)
                # add children of the root into the stack in reverse order
                stack.extend(root.children[::-1])
        return res
```
【思路】N叉树前序遍历：递归；迭代（需要自行调用栈stack)  
二叉树的遍历是：  
    遍历左子树... 遍历右子树...  
N叉树的遍历是：  
    对于每个子节点：  
        通过递归地调用遍历函数来遍历以该子节点为根的子树  
【定义】N叉树：二叉树中每个结点有一个数据项，最多有两个子节点，如果允许树的每个节点可以有两个以上的子节点，那么这个树就称为N阶的多叉树——N叉树。  
【疑问】为什么要将当前节点的所有一阶子节点 “逆序” 压入栈中？ stack.extend(root.children[::-1])？  
假设当前 root = 1 的所有一阶子节点是 [3,2,4]，如果直接这么顺序入栈中，则子节点 4 将会第一个被弹出，但正确的应该是子节点 3 最先被弹出，所以逆序入栈就变成 [4,2,3]，这样子节点 3 就被第一个弹出。  
【注意】明确 “子节点” 的定义，可以理解成 “一阶子节点” 更好。  
【详解】[链接](https://blog.csdn.net/weixin_43314519/article/details/106981900)  

==========

作业题 - 字母异位词分组 (LC49 - medium)
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # method I: hashtable + 创建字符串列表
        d = {}
        for s in strs:
            k = "".join(sorted(s))
            if (k not in d):
                d[k] = [s]
            else:
                d[k].append(s)
        return list(d.values())
    
        # method II: hashtable + 用dict.get(key[, default])
        d = {}
        for s in strs:
            k = "".join(sorted(s))
            d[k] = d.get(k, []) + [s]
        return list(d.values())
```
【思路】hashtable，dict.get(key[, default])  
【注意】在创建字符串列表时，要用到方括号即 [ 和 ]，不要用 list(string) 这个会把 string 中的成员（单个字母）全部分开放在列表中，即 list(“abc”) 的结果是 [“a”, “b”, “c”]。  
Python中的 iterable 是什么？  
    - 其被认为是一类对象，这类对象能够一次返回它的一个成员（也就是元素）。抽象一点就是，适合迭代的对象。实际上，任何具有__iter__()或__getitem__()方法的对象，Python就认为它是一个iterable。Python里有大量内置的 iterable 类型，如： list，str，tuple，dict，file，xrange等。使用内置的 iter() 函数来生成。  
Python常用函数：  
    - join()：  
        将序列中的每一个元素以指定的字符连接，然后生成一个新的字符串。正确写法是：str.join(sequence)。如代码 ”-”.join(“abc”) 返回的结果即是 “a-b-c”，含义是 “将字符串序列 abc 中的每个成员（字母元素）以字符 ”-“ 分隔开再拼接成一个新字符串”。  
    - sorted(iterable, key=None, reverse=False)：  
        当iterable只是一个字符串时，返回的是 a list of individual characters of that string，比如sorted(“machine”)返回的是 ['a', 'c', 'e', 'h', 'i', 'm', 'n']。[需要注意的几点](https://realpython.com/python-sort/)：The original variable is unchanged because sorted() provides sorted output and does not change the original value in place；Lists With Non-Comparable Data Types Can’t Be sorted()

==========
作业题 - 二叉树的中序遍历 (LC94 - medium)
```python
# method I: recursive
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        self.preorder(res, root)
        return res
        
    def preorder(self, res, root):
        if (root is None): return
        self.preorder(res, root.left)
        res.append(root.val)
        self.preorder(res, root.right)
# method II: iterative
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        # method II: iterative
        stack = []
        res = []
        while (stack or root):
            if (root):
                stack.append(root)
                root = root.left
            else:
                cur = stack.pop()
                res.append(cur.val)
                root = cur.right
        return res
```
【思路】二叉树中序遍历：递归；迭代（需要自行调用 栈stack）  
【时空复杂度】递归 O(n) O(h)；迭代 O(n) O(h)，h为输的高度  
【知识点】中序遍历是 “left - root - right”，从 root 开始要一直往左走，直到左边 node 的下一个左边节点是空（即左边走到底了）就打印当前 node —— 这个节点就是左边最底的节点，打印完后跳向该最左最底的节点的右节点，跳到右节点之后继续往左边一直走下去……  
**本体难点在于 “如何用迭代的方式来模拟或说实现 “函数自己调用自己” 的这种层层嵌套”，伪代码如下：**  
```
dfs(root.left)
	dfs(root.left)
		dfs(root.left)
			为null返回
		打印节点
		dfs(root.right)
			dfs(root.left)
				dfs(root.left)
				........
```
[非常详细的递归+迭代题解](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/dong-hua-yan-shi-94-er-cha-shu-de-zhong-xu-bian-li/)

==========

作业题 - 二叉树的前序遍历 (LC144 - medium)
```python
# method I: recursive
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        self.preorder(res, root)
        return res
        
    def preorder(self, res, root):
        if (root is None): return
        res.append(root.val)
        self.preorder(res, root.left)
        self.preorder(res, root.right)
# method II: iterative
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        stack = []
        while (stack or root):
            if (root):
                res.append(root.val)
                stack.append(root)
                root = root.left
            else:
                cur = stack.pop()
                root = cur.right
        return res
```
【思路】二叉树前序遍历：递归；迭代（需要自行调用 栈stack）  
【时空复杂度】递归 O(n) O(h)；迭代 O(n) O(h)，h为输的高度  
【易混淆点】为什么是 while (stack or root) ？  
看下图，当root为7时，stack = [1, 2, 4, 8, 9, 5, 3, 6, 7] 因为stack.append(root)，然后root = root.left也就变成None空节点。然后轮回到while，此时stack = [7]且root = None，故符合while循环条件，跳到 if(root) .. else: ...，跳到else之后stack变为空且cur = 节点7且root = root.right即None空节点，此时 stack = [] & root = None，故结束循环。  
[前序遍历](https://img-blog.csdnimg.cn/20190310152155319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dvbmdqaWFuYm8xOTky,size_16,color_FFFFFF,t_70)
