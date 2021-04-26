作业题 - 搜索旋转排序数组 (LC33 - medium)
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1
        while (l <= r):
            mid = (l + r) // 2
            if (target == nums[mid]): return mid
            # left sorted portion, [l, mid], 统一把>=的值归为left sorted portion
            if (nums[mid] >= nums[l]): # 注意这里有=
                if (target < nums[l] or target > nums[mid]):
                    l = mid + 1
                else:
                    r = mid - 1
            # right sorted portion, [mid, r]
            else: # 这里就是nums[mid]<nums[l]了，注意只有<没有=
                if (target < nums[mid] or target > nums[r]):
                    r = mid - 1
                else:
                    l = mid + 1
        return -1
```
【思路】二分查找  
（1）找出有序区间；（2）然后根据target是否在有序区间舍弃一半元素  
【复杂度】时间：O(logn)；空间：O(1)  
【详解】  
Anytime you’re looking for a solution with time complexity O(logn) or better than linear, then this is a binary search type of question. 因为体重要求时间复杂度是O(logn)，所以本题解法就是 “二分查找” 。  
二分查找通常都有3个指针 — left/mid/right，并且总有左指针对应的值<=右指针对应的值(leftright)。题中，原本有序的一个数组被拆成了两部分，所以一个想法是看 mid 在左边有序部分还是右边有序部分。  
* 首先，找到 mid 位于左侧有序部分还是右侧有序部分，确定 mid 的当前位置：  
    - 如果mid<left，则当前的 mid 在 right sorted portion（注意是 <，没有 =）  
    - 如果midleft，则当前的 mid 在 left sorted portion（注意是 >=，必须有 =）  
* 然后，继续判断 target 在当前左/右侧有序部分中和 mid 的大小，从而再舍弃一部分，只看另一部分：  
    - 若 mid 在 left sorted portion，则：（注意左边有序部分的边界是 leftmost ~ mid）  
        - 当 target<leftmost 时，说明 target 比 leftmost ~ mid 范围内的所有元素都小，故 mid 左侧的全都不用看了，往 right sorted portion 中去看 —— 更新 left = mid + 1；  
        - 当 target >mid 时，说明 target 比 leftmost ~ mid 范围内的所有元素都大，故 mid 左侧的全都不用看了，往 mid+1 ~ rightmost 中去看 —— 更新 left = mid + 1；  
        - 当 leftmosttargetmid 时，说明 target 正好在 leftmost ~ mid 范围内，故 mid 右侧的全都不用看了 —— 更新 right = mid - 1  
    - 若 mid 在 right sorted portion，则：（注意右侧有序部分的边界是 mid ~ rightmost）  
        - 当 target<mid 时，说明 target 比 mid ~ rightmost 中的所有元素都小，故 mid 右侧的都不用看了，更新 right = mid - 1；  
        - 当 target>rightmost 时，说明 target 比 mid ~ rightmost 范围内的所有元素都大，故 mid 右侧的全都不用看了，转向 left sorted portion 去看 —— 更新 right = mid - 1；  
        - 当 midtargetrightmost 时，说明 target 正好在 mid ~ rightmost 范围内，故 mid 左侧的全都不用看了，更新 left  = mid + 1。  
【背住】对于二分查找类的题，上来就先写好左右指针和 while 循环体：  
```python
left, right = 0, len(nums) - 1
while (left <= right):
    mid = (left + right) // 2
    ...
```

==========

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

作业题 - 寻找旋转排序数组中的最小值 (LC153 - medium)
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        # method I: mid 和 left 比较，需要记录当前 minv
        l, r, minv = 0, len(nums) - 1, float("inf")
        while (l <= r):
            mid = (l + r) // 2
            # left sorted portion:
            if (nums[mid] >= nums[l]):
                minv = nums[l] if (nums[l] < minv) else minv
                l = mid + 1
            # right sorted portion
            else:
                minv = nums[mid] if (nums[mid] < minv) else minv
                r = mid - 1
        return minv
        
        # method II：mid 和 right 比较，不用担心记录当前 minv 的问题
        l, r = 0, len(nums) - 1
        while (l < r): # 注意，这么写会使[l,r]范围内最终留下唯一一个元素，且这个元素是全局最小值
            mid = (l + r) // 2
            # left sorted portion:
            if (nums[mid] > nums[r]):
                l = mid + 1
            # right sorted portion:
            else:
                r = mid # 不能是 mid - 1，因为你不知道nums[mid]是否已经是right sorted portion中的最后一个元素了
        return nums[l]
```
【思路】二分查找  
这题有两种最优方法：（1） mid 和 left 比较（2）mid 和 right 比较。方法（1）是因为受到了 LC33 题的影响，方法二在返回结果的时候更巧妙一些。  
断掉的左右两段有序部分有如下关系：左侧有序部分的所有元素都大于右侧有序部分的所有元素，也即，左侧有序部分的最小值都大于右侧有序部分的最大值，但如果拿 left 和 mid 比较的话，需要额外记录当前的 minv，否则 if (nums[mid] >= nums[l]) 这条判断会将完全升序部分的真正的最小值更新掉（因为 l = mid + 1）。  
* 为什么，法一中更新左右指针都用到了 +/-1，而法二中更新右指针没有+1？  
    - 因为法一用了 minv 来记录当前 mid 所在的 [l,mid] 范围内的最小值，已经记录了自然就没必要再看了，所以才有的 l = mid + 1 和 r = mid - 1。如果没有记录 minv 就如方法二，则需要 r = mid。  
* 注意，法一和法二的 while 循环体判断条件不同！！！  
【技巧】只要 “排过序的、有上下界的、可通过索引访问元素的” 就往二分查找上想！  

==========
