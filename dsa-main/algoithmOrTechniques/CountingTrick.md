# 🔢 Complete Guide to Counting Patterns in Arrays

A comprehensive pattern library for solving count-type problems efficiently. This guide covers the most common counting techniques with exact formulas, detailed explanations, and working examples.

## 📚 Table of Contents

- [1. Sorted Two-Pointers (Pairs / Triplets)](#1-sorted-two-pointers-pairs--triplets)
- [2. Sliding Window (Subarrays)](#2-sliding-window-subarrays)
- [3. Prefix-Sum / Hash (Equalities)](#3-prefix-sum--hash-equalities)
- [4. Binary Search on Boundary](#4-binary-search-on-boundary)
- [5. How to Derive Formulas](#5-how-to-derive-formulas)
- [6. Quick Reference Cheat Sheet](#6-quick-reference-cheat-sheet)
- [7. Common Pitfalls](#7-common-pitfalls)

---

## 1. Sorted Two-Pointers (Pairs / Triplets)

**When to use:** Problems involving pairs/triplets where sorting preserves the property and the constraint is monotonic.

### A) Count Pairs with Sum ≤ Target

**Problem:** Given an array, count pairs `(i, j)` where `i < j` and `a[i] + a[j] ≤ target`.

**Why this works:** After sorting, if `a[i] + a[j] ≤ target`, then all pairs `(i, i+1)`, `(i, i+2)`, ..., `(i, j)` are also valid because the array is sorted.

**Example:** `arr = [1, 3, 4, 6, 8]`, `target = 7`
- After sorting: `[1, 3, 4, 6, 8]`
- `i=0, j=4`: `1+8=9 > 7`, move `j--`
- `i=0, j=3`: `1+6=7 ≤ 7`, add `3-0=3` pairs: `(0,1), (0,2), (0,3)`, move `i++`
- `i=1, j=3`: `3+6=9 > 7`, move `j--`
- `i=1, j=2`: `3+4=7 ≤ 7`, add `2-1=1` pair: `(1,2)`, move `i++`
- Total: 4 pairs

```java
public int countPairsWithSumLE(int[] arr, int target) {
    Arrays.sort(arr);
    int left = 0, right = arr.length - 1, count = 0;
    
    while (left < right) {
        if (arr[left] + arr[right] <= target) {
            count += (right - left);  // Key formula!
            left++;
        } else {
            right--;
        }
    }
    return count;
}
```

### B) Count Pairs with Difference ≤ D

**Problem:** Count pairs `(i, j)` where `i < j` and `arr[j] - arr[i] ≤ D`.

**Why this works:** For a fixed `j`, all indices `i` where `arr[j] - arr[i] ≤ D` form a contiguous range `[leftmost_valid_i, j]`.

**Example:** `arr = [1, 3, 7, 9]`, `D = 3`
- `j=0`: all pairs ending at index 0: count = `0-0+1 = 1`
- `j=1`: `3-1=2 ≤ 3`, all valid from i=0: count = `1-0+1 = 2`
- `j=2`: `7-1=6 > 3`, move left until `7-3=4 > 3`, then `i=2`: count = `2-2+1 = 1`
- `j=3`: `9-7=2 ≤ 3`, valid from i=2: count = `3-2+1 = 2`
- Total: 6 pairs

```java
public int countPairsWithDiffLE(int[] arr, int D) {
    Arrays.sort(arr);
    int left = 0, count = 0;
    
    for (int right = 0; right < arr.length; right++) {
        while (arr[right] - arr[left] > D) {
            left++;
        }
        count += (right - left + 1);  // All pairs ending at 'right'
    }
    return count;
}
```

### C) Count Valid Triangles

**Problem:** Count triplets that can form valid triangles (sum of any two sides > third side).

**Why this works:** After sorting, for triangle inequality `a + b > c`, we only need to check `a[i] + a[j] > a[k]` where `k` is the largest side. If true for `(i, j, k)`, it's true for all `(i', j, k)` where `i ≤ i' < j`.

**Example:** `arr = [4, 6, 3, 7]`
- After sorting: `[3, 4, 6, 7]`
- `k=3` (largest=7): `i=0, j=2`: `3+6=9 > 7`, add `2-0=2` triangles, `j--`
- `k=3`: `i=0, j=1`: `3+4=7 = 7`, not valid, `i++`
- `k=2` (largest=6): `i=0, j=1`: `3+4=7 > 6`, add `1-0=1` triangle, `j--`
- Total: 3 triangles

```java
public int countTriangles(int[] arr) {
    Arrays.sort(arr);
    int count = 0;
    
    for (int k = arr.length - 1; k >= 2; k--) {
        int left = 0, right = k - 1;
        while (left < right) {
            if (arr[left] + arr[right] > arr[k]) {
                count += (right - left);  // All triangles with right side
                right--;
            } else {
                left++;
            }
        }
    }
    return count;
}
```

### D) Count Triplets with Sum < Target

**Problem:** Count triplets `(i, j, k)` where `i < j < k` and `arr[i] + arr[j] + arr[k] < target`.

**Why this works:** Fix the first element `i`, then use two pointers on the remaining array. If `arr[i] + arr[j] + arr[k] < target`, then all triplets `(i, j, j+1)`, `(i, j, j+2)`, ..., `(i, j, k)` are valid.

**Example:** `arr = [1, 2, 3, 4]`, `target = 6`
- `i=0`: `j=1, k=3`: `1+2+4=7 ≥ 6`, `k--`
- `i=0`: `j=1, k=2`: `1+2+3=6 ≥ 6`, `k--`, now `j ≥ k`, done with i=0
- `i=1`: `j=2, k=3`: `2+3+4=9 ≥ 6`, `k--`, now `j ≥ k`, done
- Total: 0 triplets

```java
public int countTripletsWithSumLT(int[] arr, int target) {
    Arrays.sort(arr);
    int count = 0;
    
    for (int i = 0; i < arr.length - 2; i++) {
        int left = i + 1, right = arr.length - 1;
        while (left < right) {
            if (arr[i] + arr[left] + arr[right] < target) {
                count += (right - left);  // Range collapse
                left++;
            } else {
                right--;
            }
        }
    }
    return count;
}
```

---

## 2. Sliding Window (Subarrays)

**When to use:** Counting subarrays where the property is monotonic when expanding/shrinking the window.

### A) Count Subarrays with Sum ≤ S

**Problem:** Count subarrays with sum ≤ S (non-negative elements).

**Why this works:** For each right boundary `r`, find the leftmost valid left boundary `l`. All subarrays `[l,r], [l+1,r], [l+2,r], ..., [r,r]` are valid, giving us `r-l+1` subarrays.

**Example:** `arr = [1, 2, 1, 3]`, `S = 4`
- `r=0`: sum=1, all subarrays ending at 0: `[0,0]` → count = 1
- `r=1`: sum=3, all subarrays ending at 1: `[0,1], [1,1]` → count = 2
- `r=2`: sum=4, all subarrays ending at 2: `[0,2], [1,2], [2,2]` → count = 3
- `r=3`: sum=7 > 4, shrink: sum=6 > 4, shrink: sum=4, subarrays: `[2,3], [3,3]` → count = 2
- Total: 8 subarrays

```java
public int countSubarraysWithSumLE(int[] arr, int S) {
    int left = 0, sum = 0, count = 0;
    
    for (int right = 0; right < arr.length; right++) {
        sum += arr[right];
        while (sum > S) {
            sum -= arr[left++];
        }
        count += (right - left + 1);  // All subarrays ending at 'right'
    }
    return count;
}
```

### B) Count Subarrays with Product < K

**Problem:** Count subarrays with product < K (positive elements only).

**Why this works:** Same logic as sum - maintain a valid window and count all subarrays ending at each position.

**Example:** `arr = [2, 3, 1, 4]`, `K = 10`
- `r=0`: prod=2, count = 1
- `r=1`: prod=6, count = 2
- `r=2`: prod=6, count = 3
- `r=3`: prod=24 ≥ 10, shrink until prod=12 ≥ 10, shrink until prod=4, count = 2
- Total: 8 subarrays

```java
public int countSubarraysWithProductLT(int[] arr, int K) {
    int left = 0, count = 0;
    long product = 1;
    
    for (int right = 0; right < arr.length; right++) {
        product *= arr[right];
        while (left <= right && product >= K) {
            product /= arr[left++];
        }
        count += (right - left + 1);  // All subarrays ending at 'right'
    }
    return count;
}
```

### C) Count Subarrays with At Most K Distinct Elements

**Problem:** Count subarrays with at most K distinct characters/elements.

**Why this works:** Maintain a frequency map and ensure distinct count ≤ K by shrinking the window when needed.

**Example:** `arr = [1, 2, 1, 3]`, `K = 2`
- `r=0`: distinct=1, count = 1
- `r=1`: distinct=2, count = 2
- `r=2`: distinct=2, count = 3
- `r=3`: distinct=3 > 2, shrink until distinct=2, count = 2
- Total: 8 subarrays

**For exactly K distinct:** Use the formula `atMost(K) - atMost(K-1)`

```java
public int countSubarraysWithAtMostKDistinct(int[] arr, int K) {
    Map<Integer, Integer> freq = new HashMap<>();
    int left = 0, distinct = 0, count = 0;
    
    for (int right = 0; right < arr.length; right++) {
        if (freq.getOrDefault(arr[right], 0) == 0) distinct++;
        freq.put(arr[right], freq.getOrDefault(arr[right], 0) + 1);
        
        while (distinct > K) {
            freq.put(arr[left], freq.get(arr[left]) - 1);
            if (freq.get(arr[left]) == 0) {
                freq.remove(arr[left]);
                distinct--;
            }
            left++;
        }
        count += (right - left + 1);  // All subarrays ending at 'right'
    }
    return count;
}

public int countSubarraysWithExactlyKDistinct(int[] arr, int K) {
    return countSubarraysWithAtMostKDistinct(arr, K) - 
           countSubarraysWithAtMostKDistinct(arr, K - 1);
}
```

### D) Count Subarrays with Max - Min ≤ K

**Problem:** Count subarrays where the difference between maximum and minimum elements is ≤ K.

**Why this works:** Use deques to efficiently track window maximum and minimum. When the difference exceeds K, shrink the window.

**Example:** `arr = [1, 3, 2, 4]`, `K = 2`
- Maintain max and min using deques
- For each valid window, count all subarrays ending at current position

```java
public int countSubarraysWithMaxMinDiffLE(int[] arr, int K) {
    Deque<Integer> maxDeque = new ArrayDeque<>();
    Deque<Integer> minDeque = new ArrayDeque<>();
    int left = 0, count = 0;
    
    for (int right = 0; right < arr.length; right++) {
        // Maintain max deque
        while (!maxDeque.isEmpty() && arr[maxDeque.peekLast()] <= arr[right]) {
            maxDeque.pollLast();
        }
        maxDeque.addLast(right);
        
        // Maintain min deque
        while (!minDeque.isEmpty() && arr[minDeque.peekLast()] >= arr[right]) {
            minDeque.pollLast();
        }
        minDeque.addLast(right);
        
        // Shrink window if difference > K
        while (arr[maxDeque.peekFirst()] - arr[minDeque.peekFirst()] > K) {
            if (maxDeque.peekFirst() == left) maxDeque.pollFirst();
            if (minDeque.peekFirst() == left) minDeque.pollFirst();
            left++;
        }
        
        count += (right - left + 1);  // All subarrays ending at 'right'
    }
    return count;
}
```

---

## 3. Prefix-Sum / Hash (Equalities)

**When to use:** Problems involving exact equality constraints, especially with negative numbers.

### A) Count Subarrays with Sum = K

**Problem:** Count subarrays with exact sum K.

**Why this works:** For each position, if `prefixSum[r] - prefixSum[l-1] = K`, then `prefixSum[l-1] = prefixSum[r] - K`. Count how many times this target prefix sum occurred before.

**Example:** `arr = [1, -1, 1, 1]`, `K = 1`
- Prefix sums: `[0, 1, 0, 1, 2]`
- At index 0: sum=1, need sum-K=0, freq[0]=1, add 1
- At index 1: sum=0, need sum-K=-1, freq[-1]=0, add 0
- At index 2: sum=1, need sum-K=0, freq[0]=1, add 1  
- At index 3: sum=2, need sum-K=1, freq[1]=1, add 1
- Total: 3 subarrays

```java
public int countSubarraysWithSumK(int[] arr, int K) {
    Map<Long, Integer> prefixSumCount = new HashMap<>();
    prefixSumCount.put(0L, 1);  // Empty prefix
    long sum = 0, count = 0;
    
    for (int num : arr) {
        sum += num;
        count += prefixSumCount.getOrDefault(sum - K, 0);
        prefixSumCount.put(sum, prefixSumCount.getOrDefault(sum, 0) + 1);
    }
    return (int) count;
}
```

### B) Count Subarrays with Sum Divisible by M

**Problem:** Count subarrays whose sum is divisible by M.

**Why this works:** If `sum[l..r] % M = 0`, then `prefixSum[r] % M = prefixSum[l-1] % M`. Count occurrences of each remainder.

```java
public int countSubarraysDivisibleByM(int[] arr, int M) {
    int[] remainderCount = new int[M];
    remainderCount[0] = 1;  // Empty prefix
    long sum = 0, count = 0;
    
    for (int num : arr) {
        sum += num;
        int remainder = (int) ((sum % M + M) % M);  // Handle negative numbers
        count += remainderCount[remainder];
        remainderCount[remainder]++;
    }
    return (int) count;
}
```

---

## 4. Binary Search on Boundary

**When to use:** When the constraint is monotonic but two-pointer movement is complex.

### Count Pairs with Constraint Using Binary Search

**Why this works:** For each left element, binary search finds the rightmost valid right element, giving us the count of valid pairs.

```java
public int countPairsBS(int[] arr, int target) {
    Arrays.sort(arr);
    int count = 0;
    
    for (int i = 0; i < arr.length; i++) {
        int j = upperBound(arr, i + 1, arr.length - 1, target - arr[i]);
        if (j > i) count += (j - i);
    }
    return count;
}

private int upperBound(int[] arr, int left, int right, int target) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] <= target) left = mid + 1;
        else right = mid - 1;
    }
    return right;
}
```

---

## 5. How to Derive Formulas

### Step-by-Step Process:

1. **Identify the Structure:** Are we counting pairs, triplets, or subarrays?

2. **Find Monotonicity:** If the condition holds for one element, does it hold for a range of adjacent elements?

3. **Choose the Anchor:** Fix one boundary and move the other(s).

4. **Count the Range:** When condition is satisfied, count all valid combinations in the range.

5. **Avoid Double Counting:** Ensure each valid combination is counted exactly once.

### Formula Patterns:
- **Pairs with one anchor:** `count += (other_boundary - anchor)`
- **Subarrays ending at position:** `count += (right - left + 1)`
- **Prefix sum equality:** `count += frequency[target_prefix]`

---

## 6. Quick Reference Cheat Sheet

| Problem Type | Precondition | Formula | When Valid Action |
|-------------|--------------|---------|-------------------|
| Pairs sum ≤ T | Sorted array | `j - i` | `i++` |
| Pairs diff ≤ D | Sorted array | `j - i + 1` | Expand `j` |
| Valid triangles | Sorted, fix largest | `j - i` | `j--` |
| Triplets sum < T | Sorted, fix first | `k - j` | `j++` |
| Subarray sum ≤ S | Non-negative | `r - l + 1` | Expand `r` |
| Subarray product < K | Positive | `r - l + 1` | Expand `r` |
| At most K distinct | Any | `r - l + 1` | Expand `r` |
| Exactly K distinct | Any | `atMost(K) - atMost(K-1)` | - |
| Sum = K | Any integers | `freq[prefix-K]` | - |
| Sum % M = 0 | Any integers | `freq[remainder]` | - |

---

## 7. Common Pitfalls

⚠️ **Don't use sliding window with negative numbers** for sum/product constraints
⚠️ **Always sort** before using two-pointer for pairs/triplets  
⚠️ **Choose the right anchor** (largest for triangles, smallest for sum constraints)
⚠️ **Use `long`** to prevent integer overflow
⚠️ **Handle edge cases** like empty arrays or single elements
⚠️ **Be careful with modulo** of negative numbers: use `((x % M) + M) % M`

---

## 🎯 Practice Problems

Try these problems to master the patterns:
- [3Sum Smaller (LeetCode 259)](https://leetcode.com/problems/3sum-smaller/)
- [Subarray Product Less Than K (LeetCode 713)](https://leetcode.com/problems/subarray-product-less-than-k/)
- [Subarrays with K Different Integers (LeetCode 992)](https://leetcode.com/problems/subarrays-with-k-different-integers/)
- [Subarray Sums Divisible by K (LeetCode 974)](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

---

## 📝 Contributing

Feel free to submit issues and enhancement requests! This guide aims to be the most comprehensive resource for counting patterns in competitive programming.

## 📄 License

This guide is open source and available under the [MIT License](LICENSE).