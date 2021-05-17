作业题 - 最小路径和 (LC64 - medium)
```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        # method I: 时间、空间复杂度：O(MN)、O(MN)	自己想出来的
        dp = grid
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if (i == 0 and j != 0):
                    dp[i][j] = dp[i][j - 1] + dp[i][j]
                elif (i != 0 and j == 0):
                    dp[i][j] = dp[i - 1][j] + dp[i][j]
                elif (i == 0 and j == 0):
                    dp[i][j] = dp[i][j]
                else:
                    dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + dp[i][j]
        return dp[-1][-1]

        # method II: 时间、空间复杂度：O(MN)、O(1)（直接改动原矩阵 grid）
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if (i == 0 and j != 0):
                    grid[i][j] += grid[i][j - 1]
                elif (i != 0 and j == 0):
                    grid[i][j] += grid[i - 1][j]
                elif (i == 0 and j == 0):
                    continue
                else:
                    grid[i][j] += min(grid[i-1][j], grid[i][j-1])
        return grid[-1][-1]
```
【思路】DP  
【详解】  
1. 找重复性  
2. 定义中间状态：`dp[i][j]` 从`(0, 0)`开始，截止位置`(i, j)`，所走过的总和（包含走完`(i, j)`)
3. DP方程：`dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + dp[i][j]`  
    但由于只能 **往下** 和 **往右** 走，所以此处还要细分4种情况，以防止 `i/j` 出边界：
* 当位于第一行（i == 0 and j != 0）  
    所有的 `dp[i - 1][j]` 都在第一行上边，出了数组上边界，故此时的 DP方程 简化为：`dp[i][j] = dp[i][j - 1] + dp[i][j]`  
* 当位于第一列（i != 0 and j == 0）  
    所有的 `dp[i][j - 1]` 都在第一列左边，出了数组左边界，故此时的 DP方程 简化为：`dp[i][j] = dp[i - 1][j] + dp[i][j]`  
* 当位于第一行第一列（i == 0 and j == 0）  
    `dp[i - 1][j] 和 dp[i][j - 1]` 都出了边界，故此时的 DP方程 只是：`dp[i][j] = dp[i][j]`  
* 当位于数组里面某一位置（i != 0 and j != 0）  
    此时就是正常的 DP 方程了：`dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + dp[i][j]`  
4. 空间优化（Optional）  
    空间优化的原理是，不需要额外创建一个 DP数组 来储存每一个中间状态，而是直接改动原数组（题目中给的）  
最后，遍历完全部行和列之后，返回 `dp[-1][-1]` 即可。  

==========

