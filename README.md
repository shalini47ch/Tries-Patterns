# Tries-Patterns-and-code-
Master tries with patterns 

## Pattern 1: Basic Trie (Insert / Search / Prefix)

### Problem Statement Identification
Look for keywords demanding standard dictionary mechanics or exact prefix matches:
* *"Implement a data structure supporting `insert`, `search`, and `startsWith` methods."*
* Keys are processed character by character from a fixed alphabet size (e.g., lowercase English letters `a-z`).

---

### Core Tactic & Intuition
Instead of searching an entire array or hash map of strings in $O(N \times L)$ time, a Trie structures strings as paths down a tree. 
* Each character represents an edge leading to a child node.
* We use a dedicated `Node` class containing an array of size 26 (`links`) to store pointers to subsequent characters.
* A boolean flag (`flag` or `isEnd`) marks whether a specific node signifies the completion of a valid inserted word.

---

### 🔗 LeetCode Practice
* [LeetCode 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

---

### Python3 Implementation (Striver Template)

```python
class Node:
    def __init__(self):
        self.links=[None for i in range(26)]
        self.flag=False 
    
    def containsKey(self,ch):
        return self.links[ord(ch)-ord("a")]!=None 
    
    def get(self,ch):
        return self.links[ord(ch)-ord("a")]
    
    def put(self,ch):
        self.links[ord(ch)-ord("a")]=Node()
    
    def setEnd(self):
        self.flag=True 
    
    def isEnd(self):
        return self.flag

class Trie:

    def __init__(self):
        self.root=Node()
        
    def insert(self, word: str) -> None:
        node=self.root
        for i in range(0,len(word)):
            if not node.containsKey(word[i]):
                node.put(word[i])
            node=node.get(word[i])
        node.setEnd()
        
    def search(self, word: str) -> bool:
        node=self.root
        for i in range(0,len(word)):
            if not node.containsKey(word[i]):
                return False
            node=node.get(word[i])
        return node.isEnd()
        
    def startsWith(self, prefix: str) -> bool:
        #here we need to return True or False
        node=self.root
        for i in range(0,len(prefix)):
            if not node.containsKey(prefix[i]):
                return False
            node=node.get(prefix[i])
        return True
        


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)

       
