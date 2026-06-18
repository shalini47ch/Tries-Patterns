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


```

## Pattern 2: Trie + DFS (Wildcard Search)

### Problem Statement Identification
Look for keywords demanding flexible string matching where characters can be substituted by a wildcard placeholder:
* *"Implement a data structure that supports adding words and searching for words where letters can be matched with a wildcard character `.`"*
* The wildcard `.` means any letter from `a-z` can match this specific position.

---

### Core Tactic & Intuition
When traversing a standard Trie, matching a character is deterministic—you simply look at the index `ord(ch) - ord('a')`. However, when you encounter a wildcard `.` character, **you no longer have a single deterministic path**. 

The fundamental shift here is moving from simple iterative traversal to **Depth-First Search (DFS) / Backtracking**:
* The `.` character acts as a universal key. It says: *"Try every single valid child node that exists at this current level."*
* You must iterate through all 26 possible references in the `links` array of the current node. 
* For every non-null child node found, you recursively launch a DFS to see if the *remainder* of the word matches that branch.
* If any of those recursive paths returns `True`, the entire search returns `True`. If all 26 options are exhausted and none lead to a valid word match, it returns `False`.

---

### 🔗 LeetCode Practice
* [LeetCode 211: Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

---

### Python3 Implementation 

```python
#this is classic trie problem but with a twist of dfs as we need to match .
class Node:
    def __init__(self):
        self.links=[None for i in range(26)]
        self.flag=False 
    
    def containsKey(self,ch):
        return self.links[ord(ch)-ord("a")]!=None 
    
    def put(self,ch):
        self.links[ord(ch)-ord("a")]=Node()
    
    def get(self,ch):
        return self.links[ord(ch)-ord("a")]
    
    def setEnd(self):
        self.flag=True 
    
    def isEnd(self):
        return self.flag

class WordDictionary:

    def __init__(self):
        self.root=Node()
        
    def addWord(self, word: str) -> None:
        node=self.root
        for i in range(0,len(word)):
            if not node.containsKey(word[i]):
                node.put(word[i])
            node=node.get(word[i])
        node.setEnd()
        

    def search(self, word: str) -> bool:
        return self.dfs(self.root,word,0)
    
    #now in searching we also have a dot so we need to perform dfs here 
    def dfs(self,node,word,ind):
        if(ind==len(word)):
            return node.isEnd()
        ch=word[ind]
        if(ch=="."):
            #we will perform dfs on all the children of it
            for child in node.links:
                if child is not None:
                    if(self.dfs(child,word,ind+1)):
                        return True
            return False
        
        if not node.containsKey(ch):
            return False
        node=node.get(word[ind])
        return self.dfs(node,word,ind+1)

                


        


# Your WordDictionary object will be instantiated and called as such:
# obj = WordDictionary()
# obj.addWord(word)
# param_2 = obj.search(word)
```

# Pattern 3: Trie + DFS (Search Suggestions / Autocomplete)

## Problem Statement Identification

Look for keywords:

* Search Suggestions
* Autocomplete
* Phone Directory
* Contacts Suggestions
* Prefix Recommendations

Typical statement:

> Given a list of words and a prefix, return matching words in lexicographical order.

---

## Core Tactic & Intuition

A Trie naturally groups words by prefixes.

Instead of scanning all words:

```text
Reach Prefix Node
+
DFS Subtree
=
All Suggestions
```

Example:

```text
Products:
mobile
mouse
mousepad
monitor

Prefix:
mou

Output:
mouse
mousepad
```

---

### LeetCode

* LC 1268 — Search Suggestions System

### GFG

* Phone Directory
* Contacts Suggestions

---

## Recognition Rule

```text
Prefix + Suggestions
=
Trie + DFS
```

## Python Implementation

```
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
    
    def insert(self,word):
        node=self.root 
        for i in range(0,len(word)):
            if not node.containsKey(word[i]):
                node.put(word[i])
            node=node.get(word[i])
        node.setEnd()

class Solution:
    def dfs(self,node,path,res):
        if(len(res)==3):
            return 
        if node.isEnd():
            res.append(path)
        for i in range(26):
            if(node.links[i]):
                self.dfs(node.links[i],path+chr(i+97),res)
    
    def getSuggestions(self,trie,prefix):
        node=trie.root 
        for ch in prefix:
            if not node.containsKey(ch):
                return []
            node=node.get(ch) #this is the one from where we will do dfs
        res=[]
        self.dfs(node,prefix,res)
        return res

    def suggestedProducts(self, products: List[str], searchWord: str) -> List[List[str]]:
        #now build trie and perform the search suggstions 
        trie=Trie()
        ans=[]
        #insert in the trie all the products
        for ch in products:
            trie.insert(ch)
        prefix=""
        for ch in searchWord:
            prefix+=ch
            ans.append(self.getSuggestions(trie,prefix))
        return ans


```

## Question Phone Directory (Same patterns trie+prefix dfs)
Python Implementation

```
#User function Template for python3
class Node:
    def __init__(self):
        self.links=[None for i in range(26)]
        self.flag=False 
    
    def containsKey(self,ch):
        return self.links[ord(ch)-ord("a")]!=None 
    
    def put(self,ch):
        self.links[ord(ch)-ord("a")]=Node()
    
    def get(self,ch):
        return self.links[ord(ch)-ord("a")]
    
    def setEnd(self):
        self.flag=True 
    
    def isEnd(self):
        return self.flag

#similarly create a trie
class Trie:
    def __init__(self):
        self.root=Node()
    
    def insert(self,word):
        node=self.root 
        for i in range(0,len(word)):
            if not node.containsKey(word[i]):
                node.put(word[i])
            node=node.get(word[i])
        node.setEnd()
        
class Solution:
    def dfs(self,node,path,res):
        if(node.isEnd()):
            res.append(path)
        for i in range(0,26):
            if(node.links[i]):
                self.dfs(node.links[i],path+chr(i+97),res)
        
    #now similarly create a helper to form the search suggestions 
    def getWords(self,trie,prefix):
        node=trie.root 
        res=[]
        for ch in prefix:
            if not node.containsKey(ch):
                return []
            node=node.get(ch)
        #here we are perfoming autosuggestions logic from using dfs from prefix
        self.dfs(node,prefix,res)
        return sorted(res) #as we need it in lexicographically increasing order 
    
    def displayContacts(self, n, contact, s):
        # code here
        #this is exactly the same question as that of search suggestion system
        #based on the concept of trie+prefix dfs
        trie=Trie()
        #insert the elements of the contact in trie
        for ele in contact:
            trie.insert(ele)
        prefix=""
        ans=[]
        for ch in s:
            prefix+=ch
            res=self.getWords(trie,prefix)
            if(len(res)==0):
                ans.append(["0"])
            else:
                ans.append(res)
        return ans
        
```


       
