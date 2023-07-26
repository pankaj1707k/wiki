# Binary Trees

- **Depth**: measured from the root.
- **Height**: measured from the deepest leaf.

## Node definition

```python
class Node:
    def __init__(self, val, left, right):
        self.val = val
        self.left = left
        self.right = right
```

## Inorder traversal

```python
def inorder(root):
    if root == None: return
    inorder(root.left)
    print(root.val)
    inorder(root.right)
```

**Time Complexity**: `O(n)`  
**Space Complexity**: `O(h)`

## Preorder traversal

```python
def preorder(root):
    if root == None: return
    print(root.val)
    preorder(root.left)
    preorder(root.right)
```

**Time Complexity**: `O(n)`  
**Space Complexity**: `O(h)`

## Postorder traversal

```python
def postorder(root):
    if root == None: return
    postorder(root.left)
    postorder(root.right)
    print(root.val)
```

**Time Complexity**: `O(n)`  
**Space Complexity**: `O(h)`

## Level order traversal

**Time Complexity**: `O(n)`  
**Space Complexity**: `O(2**h)`

### Variant 1

Print each node on a new line, i.e., no segregation of levels.

```python
que = deque()
que.append(root)

while que:
    node = que.popleft()
    print(node.val)
    if node.left: que.append(node.left)
    if node.right: que.append(node.right)
```

### Variant 2

Print nodes of a level on a single line and each level on a new line.

```python
que = deque()
que.append(root)

while que:
    level_length = len(que)
    for _ in range(level_length):
        node = que.popleft()
        print(node, end=" ")
        if node.left: que.append(node.left)
        if node.right: que.append(node.right)
    print()
```
