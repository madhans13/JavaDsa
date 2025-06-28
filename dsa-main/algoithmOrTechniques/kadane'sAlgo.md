# Kadane's Algorithm – Maximum Subarray Sum

My implementation and understanding of **Kadane's Algorithm**, a dynamic programming technique to find the **maximum sum of a contiguous subarray** in O(n) time.

---

## 📌 What is Kadane's Algorithm?

Solves the **Maximum Subarray Problem** by iteratively computing the maximum subarray sum ending at each position while tracking the global maximum.

**Key Insight:** At each element, decide whether to extend the current subarray or start fresh.

---

## 📚 Problem Statement

> Given an array `arr[]` of integers, find the **contiguous subarray** with the largest sum and return its sum.

**Example:**
```
Input: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Output: 6 (subarray [4, -1, 2, 1])
```

---

## ✅ Algorithm Steps

1. Initialize:
   - `currSum = nums[0]` (current running sum)
   - `maxSum = nums[0]` (best sum found so far)

2. For each element from index 1 to n-1:
   - If `currSum < 0`: Reset `currSum = nums[i]`
   - Else: Extend `currSum += nums[i]`
   - Update `maxSum = max(maxSum, currSum)`

3. Return `maxSum`

---

## 💡 Intuition

- **Reset on Negative:** If `currSum < 0`, it will only drag down future elements, so start fresh with current element
- **Accumulate Positive:** If `currSum ≥ 0`, it helps boost the next elements, so keep adding
- **Track Best:** Always remember the maximum sum encountered so far

**Core Logic:** A negative running sum is useless - discard it and restart from current position.

---

## 🚀 Implementation

### Java Solution
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int currSum = nums[0];
        int maxSum = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            // Start fresh if current sum is negative
            if (currSum < 0) {
                currSum = nums[i];
            } else {
                currSum += nums[i];
            }
            // Update global maximum
            maxSum = Math.max(maxSum, currSum);
        }
        return maxSum;
    }
}
```

### Alternative Approach
```java
public int maxSubArray(int[] nums) {
    int maxSoFar = nums[0];
    int maxEndingHere = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}
```

---

## 🧪 Trace Example

Array: `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`

| i | nums[i] | currSum | maxSum | Decision |
|---|---------|---------|--------|----------|
| 0 | -2      | -2      | -2     | Initialize |
| 1 | 1       | 1       | 1      | Reset (currSum < 0) |
| 2 | -3      | -2      | 1      | Extend (1 + (-3)) |
| 3 | 4       | 4       | 4      | Reset (currSum < 0) |
| 4 | -1      | 3       | 4      | Extend (4 + (-1)) |
| 5 | 2       | 5       | 5      | Extend (3 + 2) |
| 6 | 1       | 6       | 6      | Extend (5 + 1) |
| 7 | -5      | 1       | 6      | Extend (6 + (-5)) |
| 8 | 4       | 5       | 6      | Extend (1 + 4) |

**Result:** 6

---

## ⚡ Complexity Analysis

- **Time:** O(n) - Single pass through array
- **Space:** O(1) - Only two variables needed

---

## 🎯 Key Takeaways

1. **Dynamic Programming:** Build solution using previous results
2. **Greedy Choice:** At each step, make locally optimal decision
3. **State Transition:** `maxEndingHere` represents subproblem solution
4. **Edge Cases:** Handle single element and all-negative arrays

---

## 🔄 Variations to Practice

- **Find actual subarray indices** (not just sum)
- **2D version** (maximum sum rectangle)
- **Circular array** version
- **At least K elements** constraint

---

## 📝 Common Mistakes to Avoid

- Forgetting to handle negative numbers correctly
- Not initializing variables with first element
- Confusing local vs global maximum tracking
