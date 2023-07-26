# Binary Search

- Search space must be monotonically increasing or decreasing.
- **Time Complexity:** `O(log n)`

Algorithm:

1. Initialize `low` and `high` to minimum and maximum values respectively.
2. Find middle element of the search space `mid`.
3. If `mid` is the required `target`, stop.
4. If `target` is greater than `mid`, search the right half; else search the left
   half of the search space.
5. Repeat until `target` is found or search space is exhausted.

Two variants for changing search space:

1. Set `high = mid` or `low = mid + 1`: Useful when `target` is unknown such as
   finding the minimum time at which some event occurs.
2. Set `high = mid - 1` or `low = mid + 1`: Can be used when `target` is known
   or unknown. For unknown target, it requires an additional variable `ans` to
   store potential intermediate results.

**Do not use the combination of `low = mid` and `high = mid - 1` because it
may lead to infinite loop when using integer division to find `mid`**

```python
def valid(mid):
    """ return True if `mid` is an exceptable value """

def search():
    low = lower_bound
    high = upper_bound
    
    while low < high:
        mid = (low + high) >> 1
        if valid(mid):
            high = mid
        else:
            low = mid + 1
    
    return high
```

## Patterns

- Search in a given array.
- Search on the answer space.

