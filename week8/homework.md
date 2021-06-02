作业题 - 位 1 的个数 (LC191 - easy)  
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        # method I: 位运算
        c = 0
        while (n):
            n &= (n - 1) # n = n & (n - 1)
            c += 1

        # method II: python 内置 bin() 函数
        binary = bin(n)
        c = 0
        for i in binary:
            if (i == "1"):
                c += 1
        return c
        
        # method III: python 内置 bin() 函数 + count() 方法
        return bin(n).count("1")
```
【思路】位运算（清零二进制最低位的 1）  
【复杂度】  
时间：O(logn)，循环次数等于 n 的二进制位中 1 的个数，最坏情况下 n 的二进制位全为 1   
空间：O(1)  
【详解】简言之，不断地让当前的 n 与 n−1 做与运算，直到 n 变为 0 即可。  
如何想到 “清零二进制的最低位的 1”？因为清零操作是去掉二进制中最低位的1，所以每次去掉最右边的1、每次去掉最右边的1、...，当二进制中就剩下最后一个（最低位）1了，再去掉的话就是完全的 0 了（无论是十进制还是二进制均为 0 了），故此法可以用来统计 1 的出现次数。  
【Python知识点】  
1. Python bin() 内置函数，返回一个整数 int 或长整数 long int 的二进制表示（字符串）  
2. Python count() 方法，用于统计字符串里某个字符出现的次数。  
    str.count(sub, start= 0, end=len(string))  
    - sub -- 搜索的子字符串  
    - start -- 字符串开始搜索的位置。默认为第一个字符,第一个字符索引值为 0。  
    - end -- 字符串中结束搜索的位置。字符中第一个字符的索引为 0。默认为字符串的最后一个位置。  

==========  

作业题 - 2 的幂 (LC231 - easy)  
```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        return (n > 0) and (n & (n - 1)) == 0
```
【思路】位运算（清零二进制表示中的最低位的 1）  
【复杂度】时间 & 空间：O(1)，O(1)  
仔细观察几个 2 的幂：2（10）、4（100）、8（1000）、16（10000）... 能看出 “二进制表示只有一个 1”。因此，一个数 n 是 2 的幂的条件: (1) n 是正整数 (2) n 的二进制表示中只包含一个 1  

==========  

作业题 - 颠倒二进制位 (LC190 - easy)
```python
class Solution:
    def reverseBits(self, n: int) -> int:
        res = 0
        for i in range(32): # 因为题中说了是32bits，所以只需要循环32次即可
            res = res << 1 # res其实是个32位的0，即有32个0的二进制表示
            res = res + (n & 1) # 拼接用“加法”，虽然第一反应是append，但int无法做到
            n = n >> 1 # 最后别忘了把 n 右移一位
        return res
```
【思路】位运算（左右移、取二进制最后一位）  
【复杂度】时间 & 空间：O(1)，O(1)  
取二进制的最后一位数字，想到用 “判断奇偶” 来实现（因为二进制只有0/1），即截取二进制最后一位数字的代码就是 (n & 1)  

==========  

