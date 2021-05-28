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

作业题 - 解码方式 (LC91 - medium)  
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        if (s[0] == "0"): return 0 # 如果一上来就是"0"，则直接退出，返回 0（无法解码）                 

        dp = [0] * len(s)
        # 初始化dp[0]
        dp[0] = 1
        # 初始化dp[1]
        if (len(s) >= 2):
            if (s[1] == "0" and s[0] > "2"): return 0
            elif ((s[:2] >= "27") or (s[:2] in ["10", "20"])): dp[1] = 1
            else: dp[1] = 2

        for i in range(2, len(s)):
            if (s[i] == "0"):
                if (s[i - 1] + s[i] not in ["10", "20"]):
                    return 0
                else:
                    dp[i] = dp[i - 2]
            else:
                if ("10" <= s[i - 1] + s[i] <= "26"): # 注意这里不包含"10"和"20"!!! 也可以写成 "10" < s[i-1:i+1] < "20" or "20" < s[i-1:i+1] <= "26"
                    dp[i] = dp[i - 1] + dp[i - 2]
                else:
                    dp[i] = dp[i - 1]
        return dp[-1]
```
【思路】DP  
【复杂度】时间：O(n)；空间：O(n)  
【详解】本题是 “有限制条件的” 爬楼梯 或者 “有限制条件的” 斐波那契 问题（爬楼梯 == 斐波那契）  
1. 找重复性  
2. 定义中间状态：dp[i] - 解码字符串前 i 个字符（即 s[:i+1]，包含第 i 个）的所有不同组合  
3. DP方程：  
    回顾 “爬楼梯” 问题，dp[n] 表示爬完 n 个台阶的总方法数，即 dp[n] = dp[n-1] + dp[n-2]，如果还是纠结为什么 dp[n-1] + dp[n-2] 不用再 + 1 的话，看下面这个例子：  
	    假设 s = “2113”，正确答案 dp[-1] 是 5  
        详解：假设 i 当前位于 “3” 的位置，截止 i - 1 即 “211” 的所有组合共有 3 种（”2 1 1”、”21 1”、”2 11”），截止 i - 2 即 “21” 的所有组合共有 2 种（”2 1”、”21”），所以正确答案的 5 是 3 + 2 得来的，所以为什么 DP 方程 dp[i] - dp[i - 1] + dp[i - 2] 不用再 + 1 ？  
        “2 1 1”     ”3“	   dp[i - 1]  
        “21  1”	    ”3“  
        “2  11”     ”3“  
        ”2   1“     ”3“    dp[i - 2]  
        “21”        “3”  
        在原先 dp[i - 1] 和 dp[i - 2] 的基础上加上了 “3”，没有 ”3” 的情况下组成 ”211” 共有 3 种，加上 ”3“ 的情况下组成 “2113” 共有 dp[i - 1] + dp[i - 2] = 3 + 2 = 5 种，所以不用再 + 1 直接 dp[i - 1] 和 dp[i - 2] 的组合和就能得出当前组合数，这是个自然界规律，不用纠结为什么 dp[i] 仅由临近的 dp[i - 1] 和 dp[i - 2] 就能得到，自己体会一下.  
    在看本题，每次解码 一个数字字符 或 两个数字字符，dp[i] 表示解码完 i 个字符的总方法数，即解码 s[:i+1] 的可能情况为：  
        （1）s[:i+1] 符合解码条件（解码一个数字字符），此时解码 s[:i+1] 可以通过解码 s[:i] 和 最后一个字符 s[i] 得到。  
        （2）s[i-1:i+1] 符合解码条件（解码两个数字字符），此时解码 s[:i+1] 可以通过解码 s[:i-1] 和 最后两个字符 s[i-1:i+1] 得到。  
	综合(1)(2)，DP方程可以写成：
        (a) dp[i] = dp[i-1] + dp[i-2]. if (s[i] != “0” and (s[i-1:i+1] >= “10” or s[i-1:i+1] <= “26”))  
        (b) dp[i] = dp[i-1]. if (s[i] != “0” and (s[i-1:i+1] < “10” or s[i-1:i+1] > “26”))  
        (c) dp[i] = dp[i-2]. if (s[i] == “0” and (s[i-1:i+1] >= “10” or s[i-1:i+1] <= “26”))  
        (d) dp[i] = 0. if (s[i] == “0” and (s[i-1:i+1] < “10” or s[i-1:i+1] > “26”))  
    本题的另一个难点是 ，对 dp[0] 和 dp[1] 的初始化考虑（动态规划题常用的padding或初始化），如果必须初始化的话，一般都是 dp[0] = 1。  

==========  
