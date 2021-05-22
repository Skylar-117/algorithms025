作业题 - 括号生成(LC22 - medium)
```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        res = []
        
        def backtrack(stack, openN, closeN):
            if (openN == closeN == n):
                res.append("".join(stack))
                return
            if (openN < n):
                stack.append("(")
                backtrack(stack, openN + 1, closeN)
                stack.pop()
            if (closeN < openN):
                stack.append(")")
                backtrack(stack, openN, closeN + 1)
                stack.pop()
        
        backtrack([], 0, 0)
        return res
```
【思路】回溯（backtracking）   
【复杂度】时间：O(4^n/sqrt(n))；空间：O(n) - 因为res
只在序列仍然保持有效时才添加 '(' or ')'，我们可以通过跟踪到目前为止放置的左括号和右括号的数目来做到这一点：  
    valid if # of open paranthesis == # of close paranthesis == n  
    (1) only add ( if # of open paranthesis < n  
    (2) only add ) if # of close paranthesis < open paranthesis AND # of close paranthesis < n  
其实这里的(1)和(2)算是状态树剪枝了   

==========

作业题 - 有效的数独(LC36 - medium)
```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        row, col, block = set(), set(), set()
        for i in range(9):
            for j in range(9):
                if (board[i][j] != "."):
                    r_val = (i, board[i][j])
                    c_val = (j, board[i][j])
                    b_val = (i//3, j//3, board[i][j])
                    if (r_val in row or c_val in col or b_val in block):
                        return False
                    row.add(r_val)
                    col.add(c_val)
                    block.add(b_val)
        return True
```
【思路】set，一次迭代  
这题没什么诀窍，就是一次迭代找是否已经出现过重复值。分别创建三个 set 针对：行、列、3x3子数独块，set 里面记录的是（当前格子指针，当前格子值）。对于 3x3子数独块 的指针标记有诀窍，即使用了 i//3 和 j//3，这样第一个 3x3子数独块 就会有指针（0, 0, ...），右边的第二个 3x3子数独块 有指针（0, 1, ...），下面的第一个 3x3子数独块 有指针（1, 1, ...），中间的 3x3子数独块 有指针（1, 1, ...），以此类推就会有如下针对 3x3子数独块 的指针：  
（0，0，...） （0，1，...） （0，2，...）  
（1，0，...） （1，1，...） （1，2，...）  
（2，0，...） （2，1，...） （2，2，...）  

==========

