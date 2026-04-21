# BFS-3

## Problem1 Remove Invalid Parentheses(https://leetcode.com/problems/remove-invalid-parentheses/)

Time: O(2^n)
Space: O(2^(n^2))

class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:        
        self.out = []
        self.visited = set()
        self.max = 0
        self.dfs(s)
        return self.out

    def dfs(self,s):
        # base
        if len(s) < self.max:
            return

        if self.isValid(s):
            if len(s) > self.max:
                print(s, self.out)
                self.max = len(s)
                self.out.clear()
            self.out.append(s)
            return

        # logic
        for i in range(len(s)):
            baby = s[:i] + s[i+1:]
            if baby not in self.visited:
                self.visited.add(baby)
                self.dfs(baby)
        
    # check if valid parenthesis
    def isValid(self,s):
        count = 0
        for i in s:
            if i == '(':
                count += 1
            elif i == ')':
                if count == 0: 
                    return False
                count -= 1
            elif i != '(' or i != ')':
                continue
        return count == 0

Alternate solution using BFS:

Time: O(2^n) - cz its choose or not to choose so base 2
Space: O(2^(n^2))

class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:        
        res = []
        visited = set([s])
        queue = deque([s])
        found = False

        while queue and not found:
            for _ in range(len(queue)):
                curr = queue.popleft()
                if self.isValid(curr):
                    res.append(curr)
                    found = True
                else:
                    for i in range(len(curr)):
                        baby = curr[:i] + curr[i+1:]
                        if baby not in visited:
                            visited.add(baby)
                            queue.append(baby)
        return res
        
        
    # check if valid parenthesis
    def isValid(self,s):
        count = 0
        for i in s:
            if i.isalpha():
                continue
            if i == '(':
                count += 1
            elif i == ')':
                count -= 1
            if count < 0: 
                return False
        return count == 0


## Problem2 Clone graph (https://leetcode.com/problems/clone-graph/)

Using DFS -

Time = O(V+E)
Space = O(V)

"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

from typing import Optional
class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        if node is None:
            return node
            
        self.hashmap = {}
        self.dfs(node)
        return self.hashmap[node]
        
    def dfs(self, node):
        currNode = self.clone(node)

        for ne in node.neighbors:
            if ne not in self.hashmap:
                self.dfs(ne)
            currNode.neighbors.append(self.clone(ne))
    
    def clone(self,node):
        if node is None:
            return None
        if node in self.hashmap:
            return self.hashmap[node]
        self.hashmap[node] = Node(node.val)
        return self.hashmap[node]


Using BFS -

Time = O(V+E)
Space = O(V)

"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

from typing import Optional
class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        if node is None:
            return node

        self.hashmap = {}
        q = [node]
        while q:
            curr = q.pop(0)
            newNode = self.clone(curr)

            for ne in curr.neighbors:
                if ne not in self.hashmap:
                    q.append(ne)
                newNode.neighbors.append(self.clone(ne))

        return self.hashmap[node]
    
    def clone(self,node):
        if node is None:
            return None
        if node in self.hashmap:
            return self.hashmap[node]
        self.hashmap[node] = Node(node.val)
        return self.hashmap[node]
