# Heaps

- Tree-based data structure
- Types:
  - Min Heap: Value of a node is less than the values of its children
  - Max Heap: Value of a node is greater than the values of its children
- Variants:
  - Binary heap (binary tree structure)
  - Fibonacci heap (n-ary forest)
  - Binomial heap, etc.

## Binary heap

- Represented in an array.
- Each index `i` has children: `2*i + 1` and `2*i + 2`.
- Parent of index `i` is `floor(i / 2)`.

## Heapify algorithm

Given an array `A` representing a binary heap and an index `i`. `A` follows
the heap property except at index `i`, we need to perform some rearrangement
to make `A` a max heap.

Algorithm:

Compare `A[i]` with its child nodes `A[2*i+1]` and `A[2*i+2]`.
- If `A[2*i+1] > A[2*i+2]`: Swap `A[i]` and `A[2*i+1]`, and move to `2*i+1`
- Else swap `A[i]` and `A[2*i+2]`, move to `2*i+2`

Repeat this process until we reach a leaf node.

```python
def heapify(heap, index):
    if 2 * index + 1 >= len(heap):
        return
    if 2 * index + 2 >= len(heap):
        if heap[index] < heap[2 * index + 1]:
            heap[index], heap[2 * index + 1] = heap[2 * index + 1], heap[index]
        return heapify(heap, 2 * index + 1)
    if heap[2 * index + 1] > heap[2 * index + 2]:
        heap[index], heap[2 * index + 1] = heap[2 * index + 1], heap[index]
        return heapify(heap, 2 * index + 1)
    heap[index], heap[2 * index + 2] = heap[2 * index + 2], heap[index]
    return heapify(heap, 2 * index + 2)
```

**Time complexity:** `O(log n)`
**Space complexity:** `O(log n)` (function call stack)

## Insert

1. Append the new `key` to the end of the array. Now, this may not follow
   the heap property.
2. Move up the heap from the newly inserted `key` by moving to its parent nodes
   using `floor(index / 2)`.
3. For each ancestor, check if it follows the heap property.
4. If an ancestor is found which violates the heap property, execute `heapify()`
   on that ancestor.
5. Perform this procedure until the root node.

**Time complexity:** `O(log n)`
**Space complexity:** `O(log n)` (function call stack)

## Delete the minimum element

1. Read the value of the root node.
2. Remove the last number from the array, and replace the root value with it.
3. Execute `heapify()` on the new root.

**Time complexity:** `O(log n)`
**Space complexity:** `O(log n)` (function call stack)

## Heapify an arbitrary array

1. Execute `heapify()` on each non-leaf index, starting from the bottom and
   moving up the tree.
2. This ensures that before heapifying an index the tree below it already
   follows the heap property.

**Time complexity:** `O(n)` (amortized)
**Space complexity:** `O(log n)` (function call stack)

