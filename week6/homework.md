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

作业题 - 最大正方形 (LC221 - medium)  
```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        maxedge = 0
        for r in range(len(matrix)):
            for c in range(len(matrix[0])):
                if (matrix[r][c] == "1"):
                    if (r == 0 or c == 0):
                        matrix[r][c] = 1
                    else:
                        matrix[r][c] = min(int(matrix[r - 1][c]), int(matrix[r - 1][c - 1]), int(matrix[r][c - 1])) + 1
                else:
                    matrix[r][c] = 0
                maxedge = max(maxedge, matrix[r][c])
        return maxedge * maxedge
```
【思路】  
1. 找重复性  
2. 定义中间状态  
    dp[i,j] - [0,0] ~ [i,j]能组成的最大的全是1的正方形的边长。或者，理解成以 [i,j] 为右下角的全是 1 的最大正方形边长。  
3. DP方程  
	if (dp[i,j] == "0"): continue  
	if (dp[i,j] == "1"): dp[i,j] = min(dp[i-1][j], dp[i-1][j-1], dp[i][j-1]) + 1  
【复杂度】  
    时间：O(MN)，需要遍历数组每个位置的元素来计算/更新当前位置的 dp 值。  
    空间：O(1)，直接在原数组上更改当前位置的 dp 值。  
    每个元素的值就是 dp 值。  
【详解】  
- 若当前位置为0，则 dp[i,j] = 0，因为 0 不能构成有效的正方形  
- 若当前位置为1，则 dp[i,j] 由其 左边、上边、左上边 这三个相邻位置的 dp值 决定。找三个位置的最小值再 + 1 即得 dp[i,j] 值，原因的话看这里。  
边界条件，i == 0 和 j == 0，即 [0,0]位置、第一行和第一列，因为不存在上边和左边，所以这三个位置如果本来值就是 0 则不用管、如果值是 1 则就是本身值（即1）。  
此外，还要注意，题中给的 matrix 本身元素是 字符形式的 “0” 和 “1” 而不是 整数形式的 0 和 1。  

==========  

作业题 - 回文子串 (LC647 - medium)  
```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        res = 0
        for i in range(len(s)):
            res += self.spread(i, i, s)
        for j in range(len(s)):
            res += self.spread(j, j + 1, s)
        return res
            
    def spread(self, l, r, s):
        c = 0
        while (l >= 0 and r <= len(s) - 1 and s[l] == s[r]):
            c += 1
            l -= 1
            r += 1
        return c
```
【思路】DP（中心扩展法）		其实这题有点儿不像常规的DP三步走思路  
【复杂度】时间：O(n^2)，空间：O(1)  
暴力解法：遍历数组 - O(n)，每次都看当前元素和之后元素的所有组合是否是palindrome - O(n^2)，总共时间复杂度 - O(n^3)。  
中心扩展法：遍历数组 - O(n)，每次都从当前元素往两边看 - O(n)，看以当前元素为中心的这个范围内的元素是否是palindrome，总共时间复杂度 - O(n^2)。  
**edge case：when we take one character and expand outwards from it, we are getting palindromes that are of odd length**，即此时丢失了那些 even length 的 palindromes，故要考虑到这点。  

==========

