作业题 - 数组的相对排序 (LC1122 - easy)
```python
class Solution:
    def relativeSortArray(self, arr1: List[int], arr2: List[int]) -> List[int]:
        bins = [0 for _ in range(1001)] # 由于题目说 arr1 的范围在 0-1000，所以生成一个 1001 大小的数组用来存放每个数出现的次数。
        res = []
        
        # 遍历arr1，计算频次并保存在bins中(≈index(true integer value)-frequency pair)
        for i in arr1:
            bins[i] += 1 # 数值i的出现频次
            
        # 遍历arr2，将arr1中出现在arr2里的乘上频次输出答案
        for j in arr2:
            res.extend([j] * bins[j])
            bins[j] = 0
        
        # 遍历bins，arr1中未出现于arr2里的接在res后面(注意已经是ascending了)
        for k in range(len(bins)):
            if (bins[k] != 0):
                res.extend([k] * bins[k])
        return res
```
【思路】计数排序  
【复杂度】  
时间：O(m+n)，m 为 arr1 数组的长度，n 为 arr2 数组的长度  
空间：O(m)，因为 bins 是 arr1数组 的长度  
【巧妙之处】  
bins 的 index 对应 arr1 每个位置的数值，即 “数值以index的形式存储在bins中”。由于 index 本身就是 ascending 的，每个数值即 index、每个 index 位置的 bins 值即数值出现频次（相当于是 key-value pair 了，只不过这里是 index-frequency pair）的话，直接保证了 arr1 中不存在于 arr2 里的数直接就被 ascending排序了！！！  

==========  

作业题 - 有效的字母异位词 (LC242 - easy)
```python
from collections import Counter
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        # method I: 高性能库collections中的Counter计数器
        return Counter(s) == Counter(t)
    
        # method II: 直接使用 hashtable 计数；计数排序
        d1, d2 = {}, {}
        for i in s:
            d1[i] = d1.get(i, 0) + 1
        for j in t:
            d2[j] = d2.get(j, 0) + 1
        return d1 == d2
```
【思路】计数排序；hashtable，使用dict.get(key[, default])来计数（或者用collections.Counter）  
【复杂度】  
时间：O(n)，n 为 s 的长度  
空间：O(s)，s 为字符集大小（即有多少个独特的字符出现在 s 中）  
【Python 知识点】  
dict.get(key[, default])：返回字典中 key 对应的 value，如果 key 不存在于字典中则返回 default 值，default 默认是None，这里可以设置成0。  

==========  

作业题 - 反转字符串里的单词 (LC151 - medium)
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        string = ""
        stack = []
        for i in range(len(s)):
            if (s[i] != " "):
                string += s[i]
            if (s[i] == " " or i == len(s) - 1):
                if (string):
                    stack.append(string)
                    string = ""
        res = " ".join([stack.pop() for _ in range(len(stack))])
        return res
```
【思路】栈  
【复杂度】  
时间：O(n)，n 为 s 的长度  
空间：O(k)，k 为 s 中含有的独特单词的数量  
利用了栈的 LIFO 性质，将字符串中的每个词倒序输出。  

==========  

作业题 - 同构字符串 (LC205 - easy)
```python
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        # 如果字符频次>=2的话，若s和t中对应的index序列不等，则False；若s和t中对应的index序列等，True
        # method I: O(n^2)      这个是自己想出来的
        for i in range(len(s)):
            if (s.index(s[i]) != t.index(t[i])):
                return False
        return True
    
        # method II: O(n)
        d1, d2 = {}, {}
        for i in range(len(s)):
            if (s[i] in d1 and d1[s[i]] != t[i] or t[i] in d2 and d2[t[i]] != s[i]):
                return False
            d1[s[i]] = t[i]
            d2[t[i]] = s[i]
        return True
```
【思路】hashtable；list.index(x)  
【复杂度】  
时间：method I O(n^2)；method II O(n)  
空间：method I O(1)；method II O(||)，即 s 和 t 总共的独特字符数量  
本题本质就是：找到两个字符串中不同字符从左到右出现的全部位置的 index 列表是否完全一致。  
需要注意的是 Python 内置函数 list.index(x) 的复杂度是 O(n)。第二种方法有点儿绕，不过 if 判断那里有点儿像 LC647 回文子串中的 “奇偶两次 spread”。  

==========  

