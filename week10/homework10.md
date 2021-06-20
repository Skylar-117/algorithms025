作业题 - 字符串中的第一个唯一字符 (LC386 - easy)
```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        # method I-1: O(N) O(|Σ|)
        d = {}
        for char in s:
            d[char] = d.get(char, 0) + 1
        for i, char in enumerate(s):
            if (d[char] == 1):
                return i
        return -1
    
        # method I-2: O(N) O(|Σ|)
        d = collections.Counter(s)
        for k, v in d.items():
            if (v == 1):
                return s.index(k)
        return -1
    
        # method II: O(N) O(|Σ|)
        d = collections.Counter(s)
        for i in range(len(s)):
            if (d.get(s[i]) == 1):
                return i
        return -1
```
【思路】hashtable 哈希表  
遍历字符串创建哈希表，然后找第一个哈希表的值为 1 的就行了。最重要的是搞清 enumerate 的用法。  
【复杂度】  
时间：O(N)，N 为字符串 s 的长度  
空间：O(|Σ|)，Σ 为字符串 s 中独特的字符，因为题中说仅包含字母，故 |Σ| <= 26  
【Python知识点】  
注意，内置函数 .index() 时间复杂度是 O(N)；以及像这种需要哈希表统计频次的，最快的方法是用高性能库 collections 的 Counter 函数。  

==========  

