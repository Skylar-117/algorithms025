作业题 - 搜索二维矩阵 (LC240 - medium)  
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if (not matrix): return False # 注意，matrix为空的情况(但其实这一行也可以没有)
        row, col = 0, len(matrix[0]) - 1
        while (row <= len(matrix) - 1 and col >= 0):
            # if target > top right corner, row++
            if (target > matrix[row][col]):
                row += 1
            # if target < top right corner, col--
            elif (target < matrix[row][col]):
                col -= 1
            # if target == top right corner, we found it!
            else:
                return True
        return False
```
【思路】（类似移动双指针）缩小领域法，左下角or右上角，这种查询方式，记住就好了！因为这种方法，有效的利用了“升序”这一条件。  
【复杂度】时间：O(m+n)，因为每一行递增，每一列递增；空间：O(1)  
所以可从右上角往左下角找或者从左下角往右上角找，每次比较可以排除一行或者一列，故总共m+n次。  
法一，如果从右上角开始：if (target>右上角值)，则row++；if (target<右上角值)，则col--  
法二，如果从左下角开始：if (target>左下角值)，则col++；if (target<左下角值)，则row--  

==========