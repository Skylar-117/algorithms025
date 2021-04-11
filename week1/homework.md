作业题 - 移动零（LC283 - easy）
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """Do not return anything, modify nums in-place instead."""
        # while loop 写法：
        fast = slow = 0
        while (fast <= len(nums) - 1):
            if (nums[fast] != 0): # !=0, nums[fast]与nums[slow] 左右交换
                nums[fast], nums[slow] = nums[slow], nums[fast]
                slow += 1
            fast += 1 # ==0，fast++(即快指针前进一个index)，slow不动
```
思路：同向快慢指针 & 元素交换(swap)  
时间复杂度：O(n)  
Edge cases：空数组；全是0的数组  
执行时间：40 ms / 80 ms / 98 ms 都有  
占用内存：15.5 MB  
[图解](https://pic.leetcode-cn.com/36d1ac5d689101cbf9947465e94753c626eab7fcb736ae2175f5d87ebc85fdf0-283_2.gif)  
一次遍历。若!=0，则 fast++，slow 不动；若==0，则左右互换，且 fast++、slow++。直到 fast 到了 len(nums)-1。  
slow即中间点0的位置。只要是0，slow就不动；只要非0，slow++  
上面的写法，不用考虑边界条件也能通过。   

**【背住】“左右互换”的代码写成：x, y = y, x（python3不使用第三方变量来交换两变量的数值）**  

==========  

作业题 - 两数之和（LC1 - easy)  
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashtable = {}
        for i in range(len(nums)):
            diff = target - nums[i]
            if (diff not in hashtable):
                hashtable[nums[i]] = i
            else:
                return [i, hashtable[diff]]
```
思路：hashtable  
时间复杂度：O(n)  
执行时间：44 ms  
占用内存：14.5 MB  
**反向思维找差值 “target - x” 是否存在与 list 中**，而非正向思维找求和 “x + y” 是否等于target。   
正向思维的复杂度是 O(n^2)，而反向思维的复杂度是 O(n)  

==========  

作业题 - 删除排序数组中的重复项 (LC26 - easy)  
```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        slow, fast = 0, 0
        while (fast <= len(nums) - 1):
            if (nums[fast] == nums[slow]):
                fast += 1
            else:
                slow += 1
                nums[slow] = nums[fast]
        return slow + 1
```
思路：同向快慢指针  
时间复杂度：O(n)  
执行时间 116 ms  
占用内存：15.7 MB  
注意这道题不是移动零的 “前后互换” 思想，而是 “把后替代成前”，还是有点儿区别的。  

==========

作业题 - 合并两个有序链表 (LC21 - easy)  
```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        start = ListNode(0, None)
        move = start
        while (l1 and l2):
            if (l1.val <= l2.val):
                move.next = l1
                move, l1 = move.next, l1.next
            else:
                move.next = l2
                move, l2 = move.next, l2.next
        move.next = l1 if (l1) else l2
        return start.next
```
思路：哑节点 dummy（start）  
时间复杂度：O(n)  
最后，别忘了连接剩余未合并的链表！  
Q&A：  
（1）”move = start” 能不能写成 “move = ListNode(0, None)”？  
    不能。这段代码的意思是 move 和 start 两个指针**同时**指向了同一个节点 ListNode(0, None)——而这个 dummy 节点的作用，是为了指向我们**即将要创建的新链表（即题目的要求）**的首节点。  
（2）接下来的语句中，move 有所移动，那 start 会也跟着 move 的移动而移动吗？  
    不会。“move = start” 是属于赋值引用，Python的变量只不过是对于一块指定内存地址的引用，也即对对象的引用，读取对象所存储的信息（比如整数值18）。（就简单理解成这里的 “对象” == “内存地址”）  
知识点：  
Python中，万物皆对象。Python中不存在 “传值调用”，一切传递的都是对象（或理解为内存地址）的引用，而这些引用说白了就是指向视例的指针，故Python里全都是指针，只不过你看不出来而已。  
[Python变量的理解与内存管理](https://blog.csdn.net/baidu_35812706/article/details/82021400?utm_medium=distribute.pc_relevant_bbs_down.none-task--2~all~first_rank_v2~rank_v29-9.nonecase&depth_1-utm_source=distribute.pc_relevant_bbs_down.none-task--2~all~first_rank_v2~rank_v29-9.nonecase)   
[Python对变量直接赋值，可变对象 OR 不可变对象，修改之后的不同之处](https://blog.csdn.net/qq_37189082/article/details/96451081)  
[Python可变对象和不可变对象](https://blog.csdn.net/taohuaxinmu123/article/details/39008281)  



_这周实在是太忙，花了大量时间在链表理解和Python变量赋值理解上，下次作业一定所有题都写上完整思路和解答。_  
