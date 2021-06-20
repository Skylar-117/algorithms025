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

作业题 - 反转字符串 II (LC541 - easy)
```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        # O(N/2k) ≈ O(N)
        res = ""
        for i in range(0, len(s), 2 * k):
            reverse_str = s[i:i + k]
            tmp_str = reverse_str[::-1] + s[i + k:i + 2 * k]
            res += tmp_str
        return res
```
【思路】字符串切片 slice  
【详解】  
本题核心：每间隔 2k 个索引长度，反转前 k 个字符。  
剩余字符长度0≤lengh<2k，将其再分为两部分：  
    - 若 0≤lengh<k，还不够满足 “前k个字符”，因此剩余字符全部反转；  
    - 若 k≤lengh<2k，“前k个字符” 是 “完整” 的，直接反转即可。  

==========  

