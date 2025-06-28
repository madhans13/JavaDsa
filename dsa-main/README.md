# Prefix Sum Range Update Technique

## Overview

The prefix sum technique is a powerful algorithmic approach for efficiently handling range queries and updates on arrays. This README explains the technique with a focus on range updates, using the "Zero Array" problem as a practical example.

## The Technique Explained

The prefix sum range update technique allows us to perform multiple range updates in O(n + q) time, where n is the array length and q is the number of updates, rather than O(n * q) which would be required if we updated each element individually.

### Key Concepts

1. **Difference Array**: Instead of directly modifying the original array, we create a difference array to track changes.

2. **Marking Boundaries**: For each range [start, end] that needs to be updated:
   - Increment `diff[start]` by the update value
   - Decrement `diff[end+1]` by the update value (if `end+1` is within bounds)

3. **Cumulative Effect**: To get the final state of each element, calculate the running sum of the difference array and apply it to the original array.

### Step-by-Step Process

1. Initialize a difference array of the same length as the original array, filled with zeros.
2. For each range update [start, end], with update value v:
   - Add v to diff[start]
   - Subtract v from diff[end+1] (if end+1 is valid)
3. Compute the running sum of the difference array.
4. Apply the running sum to the original array elements.

## Example Problem: Zero Array

### Problem Statement

Given an array `nums` and a set of queries where each query [start, end] decrements all elements in the range by 1, determine if it's possible to make all elements in the array non-positive (less than or equal to 0).

### Example

**Input:** 
- nums = [1, 0, 1]
- queries = [[0, 2]]

**Expected Output:** 
- true

**Explanation:**
- After applying the query [0, 2], all elements are decremented by 1.
- The resulting array becomes [0, -1, 0].
- Since all elements are now less than or equal to 0, the result is true.

### Solution Using Prefix Sum

```java
public boolean isZeroArray(int[] nums, int[][] queries) {
    // Initialize difference array
    int[] diff = new int[nums.length];
    
    // Process each query to mark range updates
    for (int[] query : queries) {
        int start = query[0];
        int end = query[1];
        
        // Mark the start of range with +1 (indicating one decrement)
        diff[start]++;
        
        // Mark the end of range (if within bounds)
        if (end + 1 < diff.length) {
            diff[end + 1]--;
        }
    }
    
    // Calculate cumulative effect at each position
    int operations = 0;
    for (int i = 0; i < nums.length; i++) {
        operations += diff[i];
        
        // Check if the element will still be positive after operations
        if (nums[i] - operations > 0) {
            return false;
        }
    }
    
    return true;
}
```

### Visualization of the Process

Let's walk through the example:

**Original Array**: `[1, 0, 1]`
**Query**: `[0, 2]` (decrement elements at indices 0, 1, and 2)

1. **Initialize diff array**: `[0, 0, 0]`

2. **Process the query [0, 2]**:
   - Increment diff[0] → `[1, 0, 0]`
   - If there was an index 3, we would decrement diff[3], but it's out of bounds.

3. **Calculate cumulative effect**:
   - At index 0: operations = 0 + 1 = 1
     - nums[0] - operations = 1 - 1 = 0 (not positive, continue)
   - At index 1: operations = 1 + 0 = 1
     - nums[1] - operations = 0 - 1 = -1 (not positive, continue)
   - At index 2: operations = 1 + 0 = 1
     - nums[2] - operations = 1 - 1 = 0 (not positive, continue)

4. **Result**: Since no element remains positive after applying operations, return true.

## Time and Space Complexity

- **Time Complexity**: O(n + q), where n is the array length and q is the number of queries.
- **Space Complexity**: O(n) for the difference array.

## When to Use This Technique

The prefix sum range update technique is especially useful when:

1. You have multiple range updates to perform
2. You need to efficiently find the final state of the array after all updates
3. You want to avoid repeatedly iterating through the same elements
4. The updates follow a consistent pattern (like increment/decrement by a fixed value)

## Implementation Tips

1. Be careful with array bounds when marking the end of ranges.
2. Remember that the difference array tracks changes, not absolute values.
3. When checking conditions after updates, calculate the running sum properly.
4. This technique can be extended to 2D arrays for rectangle updates.

## Related Techniques

- Standard Prefix Sum for Range Queries
- Fenwick Tree / Binary Indexed Tree
- Segment Tree for more complex range operations
