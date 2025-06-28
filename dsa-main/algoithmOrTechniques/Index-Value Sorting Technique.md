# Index-Value Sorting Technique

A powerful sorting approach for maintaining element positions while sorting by values, particularly useful when dealing with duplicate elements and subsequence preservation.

## 🎯 Problem Statement

Given an array with potentially duplicate elements, how do you sort by values while preserving the ability to restore original ordering? This technique solves the classic "Find Subsequence of Length K With the Largest Sum" problem elegantly.

## 💡 Core Technique

The **Index-Value Pairing** technique uses a two-dimensional array to store both the original index and value of each element, enabling flexible sorting operations while maintaining positional information.

### Key Components:
- **Pairing**: Store `[original_index, value]` for each element
- **Dual Sorting**: Sort by different criteria in sequence
- **Order Restoration**: Maintain subsequence properties

## 🚀 Implementation

```java
class Solution {
    public int[] maxSubsequence(int[] nums, int k) {
        int n = nums.length;
        
        // Step 1: Create index-value pairs
        int[][] arr = new int[nums.length][2];
        for(int i = 0; i < nums.length; i++){
            arr[i][0] = i;        // Original index
            arr[i][1] = nums[i];  // Value
        }
        
        // Step 2: Sort by value (descending) to find k largest
        Arrays.sort(arr, (a,b) -> Integer.compare(b[1], a[1]));
        
        // Step 3: Sort first k elements by original index to restore order
        Arrays.sort(arr, 0, k, (a,b) -> Integer.compare(a[0], b[0]));
        
        // Step 4: Extract the values
        int[] ans = new int[k];
        for(int i = 0; i < k; i++){
            ans[i] = arr[i][1];
        }
        return ans;
    }
}
```

## 🔧 How It Works

### Phase 1: Value-Based Selection
```java
// Sort by value (descending) - finds k largest elements
Arrays.sort(arr, (a,b) -> Integer.compare(b[1], a[1]));
```

### Phase 2: Index-Based Restoration
```java
// Sort only first k elements by original index - preserves subsequence order
Arrays.sort(arr, 0, k, (a,b) -> Integer.compare(a[0], b[0]));
```

## 📊 Example Walkthrough

**Input**: `nums = [2,1,3,3], k = 2`

| Step | Array State | Description |
|------|-------------|-------------|
| Initial | `[[0,2], [1,1], [2,3], [3,3]]` | Index-value pairs |
| After Phase 1 | `[[2,3], [3,3], [0,2], [1,1]]` | Sorted by value (desc) |
| After Phase 2 | `[[2,3], [3,3]]` | First k=2 elements sorted by index |
| Result | `[3,3]` | Values in original subsequence order |

## ⚡ Advantages

- **Handles Duplicates**: Naturally manages duplicate values while preserving positions
- **Efficient**: O(n log n) time complexity with partial sorting optimization
- **Flexible**: Can be adapted for various sorting criteria
- **Memory Efficient**: Uses O(n) extra space for index tracking

## 🎯 Use Cases

- **Subsequence Problems**: When you need largest/smallest k elements in original order
- **Stable Sorting Variants**: When built-in stable sort isn't available
- **Multi-Criteria Sorting**: Sort by one criterion, then restore by another
- **Range Queries**: Selecting top elements within specific ranges

## 🔄 Variations

### For Smallest K Elements
```java
// Sort by value (ascending)
Arrays.sort(arr, (a,b) -> Integer.compare(a[1], b[1]));
```

### For Custom Comparators
```java
// Example: Sort by absolute value, then by original index
Arrays.sort(arr, (a,b) -> {
    int absCompare = Integer.compare(Math.abs(a[1]), Math.abs(b[1]));
    return absCompare != 0 ? absCompare : Integer.compare(a[0], b[0]);
});
```

## 📈 Complexity Analysis

- **Time Complexity**: O(n log n) for sorting operations
- **Space Complexity**: O(n) for the index-value array
- **Optimization**: Partial sorting in phase 2 reduces constants

## 🏷️ Tags

`sorting` `arrays` `subsequence` `two-pointer` `greedy` `java` `algorithms` `data-structures`

## 📝 Related Problems

- [Find Subsequence of Length K With the Largest Sum](https://leetcode.com/problems/find-subsequence-of-length-k-with-the-largest-sum/)
- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
- [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

---

⭐ **Star this repository** if you found this technique helpful!

📢 **Contribute** by adding more variations or optimizations of this pattern.