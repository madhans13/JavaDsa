# Product of Array Except Self - Two-Pass Technique

## 🎯 Problem Statement

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

**Constraints:**
- The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer
- You must write an algorithm that runs in O(n) time
- **Without using the division operator**

## 💡 The Two-Pass Technique

This technique breaks down the problem into two phases:

### Phase 1: Left Products (Prefix)
Calculate the product of all elements to the **left** of each index.

### Phase 2: Right Products (Suffix) 
Calculate the product of all elements to the **right** of each index and multiply with left products.

## 🔍 Algorithm Walkthrough

Let's trace through `nums = [1, 2, 3, 4]`:

### Step 1: Initialize Result Array with Left Products
```
Index:     0   1   2   3
nums:      1   2   3   4
ans:       1   1   2   6
           ↑   ↑   ↑   ↑
          1  1×1 1×2 2×3
```

### Step 2: Traverse Right-to-Left, Multiply by Right Products
```
post = 1 (running product from right)

i=3: ans[3] = 6 × 1 = 6,  post = 1 × 4 = 4
i=2: ans[2] = 2 × 4 = 8,  post = 4 × 3 = 12  
i=1: ans[1] = 1 × 12 = 12, post = 12 × 2 = 24
i=0: ans[0] = 1 × 24 = 24, post = 24 × 1 = 24

Final: [24, 12, 8, 6]
```

## 🚀 Complete Solution

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] ans = new int[nums.length];
        
        // Phase 1: Fill with left products
        ans[0] = 1;
        for (int i = 1; i < nums.length; i++) {
            ans[i] = ans[i-1] * nums[i-1];
        }
        
        // Phase 2: Multiply with right products
        int post = 1;  // Running product from right
        for (int i = nums.length - 1; i >= 0; i--) {
            ans[i] = ans[i] * post;
            post *= nums[i];
        }
        
        return ans;
    }
}
```

## 📊 Complexity Analysis

- **Time Complexity:** O(n) - Two passes through the array
- **Space Complexity:** O(1) - Only using the output array (doesn't count toward space complexity)

## 🎨 Technique Variations

### 1. Three-Array Approach (Less Optimal)
```java
// Create separate left and right arrays
int[] left = new int[n];
int[] right = new int[n];
// Then multiply left[i] * right[i]
```

### 2. Division Approach (Not Allowed)
```java
// Calculate total product, then divide by nums[i]
// Problem: Fails with zeros and division restriction
```

## 🔧 How to Apply This Technique to Similar Problems

### Pattern Recognition
This technique works when you need to:
1. **Combine information from both sides** of each element
2. **Avoid nested loops** for better performance
3. **Process arrays where each element depends on others**

### Similar Problems

#### 1. **Trapping Rain Water**
```java
// Left max heights + Right max heights
int[] leftMax = new int[n];
int[] rightMax = new int[n];
```

#### 2. **Maximum Product Subarray**
```java
// Track max/min products from left and right
int leftMax = 1, leftMin = 1;
int rightMax = 1, rightMin = 1;
```

#### 3. **Best Time to Buy and Sell Stock**
```java
// Max profit combining left mins and right maxes
int[] minPrices = new int[n];  // Left pass
int[] maxProfits = new int[n]; // Right pass
```

#### 4. **Candy Distribution**
```java
// Left pass: increasing sequence
// Right pass: decreasing sequence
int[] candies = new int[n];
```

## 🎯 Problem-Solving Template

```java
public int[] twoPassTechnique(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    
    // Phase 1: Left-to-Right Pass
    result[0] = /* base case */;
    for (int i = 1; i < n; i++) {
        result[i] = /* combine with previous + current logic */;
    }
    
    // Phase 2: Right-to-Left Pass  
    int rightVar = /* initialize */;
    for (int i = n - 1; i >= 0; i--) {
        result[i] = /* combine left result with right info */;
        rightVar = /* update running variable */;
    }
    
    return result;
}
```

## 🧪 Test Cases

```java
// Test Case 1: Basic case
Input: [1,2,3,4]
Output: [24,12,8,6]

// Test Case 2: With zeros
Input: [0,1,2,3]
Output: [6,0,0,0]

// Test Case 3: Multiple zeros
Input: [0,0,2,3]
Output: [0,0,0,0]

// Test Case 4: Negative numbers
Input: [-1,2,-3,4]
Output: [-24,12,-8,6]

// Test Case 5: Single element
Input: [1]
Output: [1]
```

## ⚡ Key Insights

1. **Space Optimization:** Use the result array itself to store intermediate results
2. **Edge Cases:** Handle zeros and negative numbers naturally
3. **No Division:** Eliminates issues with zero division and integer overflow
4. **Scalability:** Works efficiently for large arrays

## 🔗 Related Concepts

- **Prefix Sums:** Similar concept but with addition
- **Sliding Window:** Different approach but related optimization technique  
- **Dynamic Programming:** State building from previous computations
- **Two Pointers:** Processing from both ends simultaneously

## 💪 Practice Problems

1. [LeetCode 238 - Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
2. [LeetCode 42 - Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
3. [LeetCode 135 - Candy](https://leetcode.com/problems/candy/)
4. [LeetCode 152 - Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

---

**Remember:** The two-pass technique is powerful for problems where you need to combine information from both directions. Master this pattern, and you'll solve many array problems efficiently! 🎉
