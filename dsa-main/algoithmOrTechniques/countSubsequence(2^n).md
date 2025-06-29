# 🧠 LeetCode 1498: 2^n Subsequence Counting Technique Recall

## 📋 Problem Statement
**Number of Subsequences That Satisfy the Given Sum Condition**

Given an array `nums` and integer `target`, return the number of **non-empty subsequences** where `min + max ≤ target`.

## 🎯 Key Insight: Why 2^n Works Here

The brilliant insight is that **once we sort the array**, for any valid window `[left, right]` where `nums[left] + nums[right] ≤ target`, we can use the **2^n formula**!

### The Magic Behind It:
- **Minimum** = `nums[left]` (leftmost element)
- **Maximum** = `nums[right]` (rightmost element) 
- **Condition**: `nums[left] + nums[right] ≤ target`

For any subsequence in this window:
- The **minimum** will always be `nums[left]` (since array is sorted)
- The **maximum** will be ≤ `nums[right]` (since array is sorted)
- So `min + max ≤ nums[left] + nums[right] ≤ target` ✓

## 🔥 The 2^n Application

For window `[left, right]` with `n = right - left + 1` elements:
- We **MUST include** `nums[left]` (to ensure it's the minimum)
- We can **choose or skip** each of the remaining `n-1` elements
- Total subsequences = **2^(n-1) = 2^(right-left)**

## 💡 Complete Solution Template

### Java Solution for LeetCode 1498 (Count Subsequences)

```java
class Solution {
    public int numSubseq(int[] nums, int target) {
        Arrays.sort(nums);  // Key: Sort first!
        int left = 0, right = nums.length - 1;
        int result = 0;
        int MOD = 1_000_000_007;
        
        // Precompute powers of 2 to avoid TLE
        int[] powers = new int[nums.length];
        powers[0] = 1;
        for (int i = 1; i < nums.length; i++) {
            powers[i] = (powers[i-1] * 2) % MOD;
        }
        
        while (left <= right) {
            if (nums[left] + nums[right] <= target) {
                // All subsequences starting with nums[left]
                result = (result + powers[right - left]) % MOD;
                left++;  // Move to next starting point
            } else {
                right--;  // Shrink window from right
            }
        }
        
        return result;
    }
}
```

## 🧩 Step-by-Step Example

**Input**: `nums = [3,5,6,7]`, `target = 9`

1. **Sort**: `[3,5,6,7]` (already sorted)
2. **Two pointers**: `left=0, right=3`

| Step | left | right | nums[left] | nums[right] | sum | Valid? | Count | Formula |
|------|------|-------|------------|-------------|-----|--------|-------|---------|
| 1 | 0 | 3 | 3 | 7 | 10 | ❌ | 0 | - |
| 2 | 0 | 2 | 3 | 6 | 9 | ✅ | 4 | 2^(2-0) = 4 |
| 3 | 1 | 2 | 5 | 6 | 11 | ❌ | 0 | - |

**Total**: 4 subsequences

**Which 4 subsequences?**
- When `left=0, right=2`: subsequences from `[3,5,6]` that include `3`
  - `[3]` ✓
  - `[3,5]` ✓  
  - `[3,6]` ✓
  - `[3,5,6]` ✓

## 🎪 Why This Works: The Mathematical Beauty

## 🔑 Key Recognition Patterns

**When to use 2^n in subsequence problems:**

1. **After sorting** - min/max positions become predictable
2. **Two pointers** - can define valid windows efficiently  
3. **Counting not generating** - we need count, not actual subsequences
4. **Window property** - if condition holds for window, it holds for all subsequences in that window

## ⚡ Complexity Analysis

- **Time**: O(n log n) for sorting + O(n) for two pointers = **O(n log n)**
- **Space**: O(n) for powers array = **O(n)**

Compare to brute force: O(n × 2^n) time! 🤯

## 🧠 Mental Model for Recall

**Think of it as**: 
> "Sort first, then use two pointers to find valid windows. For each window, the 2^n formula counts all subsequences that maintain the min/max property!"

**Key insight**: 
> "Sorting transforms the problem from 'check all 2^n subsequences' to 'count valid subsequences in O(n) windows'"

## 🎯 Similar Problems Where This Applies

1. **Subsequence sum problems** with min/max constraints
2. **Window-based counting** after sorting
3. **Two-pointer + combinatorics** combinations

## 🚀 Pro Tips

1. **Always precompute powers of 2** to avoid TLE
2. **Remember the modulo operation** for large results
3. **Sort first** - this is usually the key insight
4. **Use 2^(right-left)** not 2^(right-left+1) - we fix the leftmost element

---

**🎉 Congratulations on solving LeetCode 1498!** 

This problem beautifully demonstrates how mathematical insights (2^n formula) combined with algorithmic techniques (sorting + two pointers) can transform an exponential problem into a polynomial one!

*The real magic: We count without generating - that's the power of combinatorics!* ✨