作业题 - 从前序与中序遍历序列构造二叉树 (LC105 - medium)
```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if (not preorder and not inorder): return None
        root = TreeNode(preorder[0])
        mid = inorder.index(root.val)
        root.left = self.buildTree(preorder[1:mid + 1], inorder[:mid])
        root.right = self.buildTree(preorder[mid + 1:], inorder[mid + 1:])
        return root
```
【思路】递归算法  
前序遍历：We take care of the root node, then the entire left subtree first, then the entire right subtree.  
中序遍历：We take care of the entire left subtree first, then the root node, then the entire right subtree.  
【两个重要切入点】  
(1) In the list of pre-order traversal, the first value is always going to be the root (of the left/right subtree), e.g. 3 is the root of the entire tree, 9 is the root of left subtree … (remember we always process the root first when doing pre-order traversal)  
[here one question pops up] When looking at the sublist after done looking at the very first value in the pre-order list, which values are going to go in the left subtree, and which values are going to go in the right subtree?  
(2) After done (1) (taking the first value from pre-order array each time and create a root node using that first value), we then look that value up in in-order array to get the index, which helps us get the length of the left and the right subarray. Those counts (length of left/right subarray) tell us how to partition the pre-order array. 记住这个partition，之后，From the left/right subarray, we are going to recursively run the algorithm to create the left/right subtree 这里就用到了递归. We are going to continuously repeat the process until we get to base cases and until we’ve finished with every single node that we need to create.  
【递归部分】  
不管值多少，接下来要做的是找到它在 in-order 数组中的索引，这个索引是用来分割当前 root 节点的左右子树的。然后，现在轮到看从刚才 root 节点分出来的 ”左 subarray “了，这个 ”左 subarray “ 来自 pre-order 数组（这里别乱，所有的 subarray 都来自于题中给的 pre-order 数组），到这儿之后我们又要重复刚才的操作，即找到这个 subarray 中的头数值 —— 当前左子树的 root 节点值，然后找到这个值在 in-order 中的索引，然后再次根据这个索引分割这个 subarray 成左右子树，然后再看向左边的 sub subarray…… 这里就形成了递归 —— 不断的寻找索引，分割成左右子数组。注意，上面只详细针对了左子树，右子树跟左子树是完全一样的操作。  
