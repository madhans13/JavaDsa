# Binary Search Patterns - Complete Guide (Java)

A comprehensive reference for mastering binary search patterns and their applications in problem-solving using Java.

## Table of Contents
- [Introduction](#introduction)
- [Core Binary Search Template](#core-binary-search-template)
- [Pattern 1: Basic Binary Search](#pattern-1-basic-binary-search)
- [Pattern 2: Finding Boundaries](#pattern-2-finding-boundaries)
- [Pattern 3: Search in Rotated Arrays](#pattern-3-search-in-rotated-arrays)
- [Pattern 4: Binary Search on Answer Space](#pattern-4-binary-search-on-answer-space)
- [Pattern 5: Peak Finding](#pattern-5-peak-finding)
- [Pattern 6: Matrix Search](#pattern-6-matrix-search)
- [Common Pitfalls](#common-pitfalls)
- [Practice Problems](#practice-problems)

## Introduction

Binary search is a fundamental algorithm that operates on the principle of "divide and conquer" to search in sorted arrays with O(log n) time complexity. However, its applications extend far beyond simple searching - it can be used to find boundaries, search in rotated arrays, and even solve optimization problems.

### Key Insight
Binary search works on any **monotonic** function - not just sorted arrays. If you can define a condition that changes from `false` to `true` (or vice versa) at some point, you can use binary search.

## Core Binary Search Template

```java
public class BinarySearchTemplate {
    
    // Generic binary search template
    public static int binarySearchTemplate(int left, int right, Predicate<Integer> condition) {
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (condition.test(mid)) {
                right = mid;  // condition is true, search left half
            } else {
                left = mid + 1;  // condition is false, search right half
            }
        }
        return left;
    }
    
    // Functional interface for condition checking
    @FunctionalInterface
    interface Predicate<T> {
        boolean test(T t);
    }
}
```

## Pattern 1: Basic Binary Search

**Use Case**: Find exact target in sorted array

### Implementation
```java
public class BasicBinarySearch {
    
    public static int binarySearch(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
    
    // Example: Integer Square Root
    public static int mySqrt(int x) {
        if (x < 2) return x;
        
        int left = 1, right = x / 2;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            long square = (long) mid * mid;
            
            if (square == x) {
                return mid;
            } else if (square < x) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return right;
    }
}
```

### Example Problems
- Search in sorted array
- Square root of integer
- Guess number higher or lower

## Pattern 2: Finding Boundaries

**Use Case**: Find first/last occurrence, insertion point

### Find First Occurrence
```java
public class BoundarySearch {
    
    public static int findFirstOccurrence(int[] nums, int target) {
        int left = 0, right = nums.length;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] >= target) {  // Found target or greater
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        return (left < nums.length && nums[left] == target) ? left : -1;
    }
    
    public static int findLastOccurrence(int[] nums, int target) {
        int left = 0, right = nums.length;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] <= target) {  // Found target or smaller
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return (left > 0 && nums[left - 1] == target) ? left - 1 : -1;
    }
    
    // Search Insert Position
    public static int searchInsert(int[] nums, int target) {
        int left = 0, right = nums.length;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return left;
    }
    
    // First Bad Version
    public static int firstBadVersion(int n) {
        int left = 1, right = n;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (isBadVersion(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        return left;
    }
    
    // Mock API for First Bad Version
    private static boolean isBadVersion(int version) {
        // This would be provided by the problem
        return false;
    }
}
```

### Example Problems
- First bad version
- Find first and last position of element
- Search insert position

## Pattern 3: Search in Rotated Arrays

**Use Case**: Search in rotated sorted arrays

### Find Pivot Point
```java
public class RotatedArraySearch {
    
    public static int findPivot(int[] nums) {
        int left = 0, right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > nums[right]) {
                // Pivot is in right half
                left = mid + 1;
            } else {
                // Pivot is in left half or mid is pivot
                right = mid;
            }
        }
        
        return left;
    }
    
    public static int searchRotated(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;
            }
            
            // Determine which half is sorted
            if (nums[left] <= nums[mid]) {  // Left half is sorted
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else {  // Right half is sorted
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        
        return -1;
    }
    
    public static int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return nums[left];
    }
    
    // With duplicates
    public static int findMinWithDuplicates(int[] nums) {
        int left = 0, right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else if (nums[mid] < nums[right]) {
                right = mid;
            } else {
                right--;  // Handle duplicates
            }
        }
        
        return nums[left];
    }
}
```

### Example Problems
- Search in rotated sorted array
- Find minimum in rotated sorted array
- Search in rotated sorted array II (with duplicates)

## Pattern 4: Binary Search on Answer Space

**Use Case**: Optimization problems where we search for the optimal answer

### Template for Answer Space Search
```java
public class AnswerSpaceSearch {
    
    // Generic template for answer space search
    public static int binarySearchAnswer(int minVal, int maxVal, Predicate<Integer> feasible) {
        int left = minVal, right = maxVal;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (feasible.test(mid)) {
                right = mid;  // This answer works, try smaller
            } else {
                left = mid + 1;  // This answer doesn't work, need larger
            }
        }
        
        return left;
    }
    
    // Koko Eating Bananas
    public static int minEatingSpeed(int[] piles, int h) {
        int left = 1, right = Arrays.stream(piles).max().getAsInt();
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (canFinish(piles, mid, h)) {
                right = mid;  // This speed works, try slower
            } else {
                left = mid + 1;  // Too slow, need faster
            }
        }
        
        return left;
    }
    
    private static boolean canFinish(int[] piles, int speed, int h) {
        int hoursNeeded = 0;
        for (int pile : piles) {
            hoursNeeded += (pile + speed - 1) / speed;  // Ceiling division
        }
        return hoursNeeded <= h;
    }
    
    // Capacity to Ship Packages
    public static int shipWithinDays(int[] weights, int days) {
        int left = Arrays.stream(weights).max().getAsInt();
        int right = Arrays.stream(weights).sum();
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (canShip(weights, mid, days)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        return left;
    }
    
    private static boolean canShip(int[] weights, int capacity, int days) {
        int currentWeight = 0;
        int daysNeeded = 1;
        
        for (int weight : weights) {
            if (currentWeight + weight > capacity) {
                daysNeeded++;
                currentWeight = weight;
            } else {
                currentWeight += weight;
            }
        }
        
        return daysNeeded <= days;
    }
    
    // Split Array Largest Sum
    public static int splitArray(int[] nums, int m) {
        int left = Arrays.stream(nums).max().getAsInt();
        int right = Arrays.stream(nums).sum();
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (canSplit(nums, mid, m)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        return left;
    }
    
    private static boolean canSplit(int[] nums, int maxSum, int m) {
        int currentSum = 0;
        int groups = 1;
        
        for (int num : nums) {
            if (currentSum + num > maxSum) {
                groups++;
                currentSum = num;
            } else {
                currentSum += num;
            }
        }
        
        return groups <= m;
    }
    
    @FunctionalInterface
    interface Predicate<T> {
        boolean test(T t);
    }
}
```

### Example Problems
- Koko eating bananas
- Capacity to ship packages within D days
- Split array largest sum
- Minimum time to complete trips
- Magnetic force between two balls

## Pattern 5: Peak Finding

**Use Case**: Find peak elements in arrays

### Find Peak Element
```java
public class PeakFinding {
    
    public static int findPeakElement(int[] nums) {
        int left = 0, right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > nums[mid + 1]) {
                // Peak is in left half (including mid)
                right = mid;
            } else {
                // Peak is in right half
                left = mid + 1;
            }
        }
        
        return left;
    }
    
    // Peak Index in Mountain Array
    public static int peakIndexInMountainArray(int[] arr) {
        int left = 0, right = arr.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] < arr[mid + 1]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return left;
    }
    
    // Find Peak in 2D Array
    public static int[] findPeakGrid(int[][] mat) {
        int left = 0, right = mat[0].length - 1;
        
        while (left <= right) {
            int midCol = left + (right - left) / 2;
            int maxRow = 0;
            
            // Find maximum element in current column
            for (int i = 0; i < mat.length; i++) {
                if (mat[i][midCol] > mat[maxRow][midCol]) {
                    maxRow = i;
                }
            }
            
            // Check if this is a peak
            int leftVal = (midCol > 0) ? mat[maxRow][midCol - 1] : -1;
            int rightVal = (midCol < mat[0].length - 1) ? mat[maxRow][midCol + 1] : -1;
            
            if (mat[maxRow][midCol] > leftVal && mat[maxRow][midCol] > rightVal) {
                return new int[]{maxRow, midCol};
            } else if (leftVal > mat[maxRow][midCol]) {
                right = midCol - 1;
            } else {
                left = midCol + 1;
            }
        }
        
        return new int[]{-1, -1};
    }
}
```

### Example Problems
- Find peak element
- Find a peak element II (2D)
- Peak index in mountain array

## Pattern 6: Matrix Search

**Use Case**: Search in 2D matrices with specific properties

### Matrix Search Implementation
```java
public class MatrixSearch {
    
    // Search in row-wise and column-wise sorted matrix
    public static boolean searchMatrixII(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        
        int row = 0, col = matrix[0].length - 1;
        
        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] > target) {
                col--;  // Move left
            } else {
                row++;  // Move down
            }
        }
        
        return false;
    }
    
    // Search in fully sorted matrix
    public static boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        
        int rows = matrix.length, cols = matrix[0].length;
        int left = 0, right = rows * cols - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int midVal = matrix[mid / cols][mid % cols];
            
            if (midVal == target) {
                return true;
            } else if (midVal < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return false;
    }
    
    // Kth Smallest Element in Sorted Matrix
    public static int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int left = matrix[0][0], right = matrix[n - 1][n - 1];
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            int count = countLessEqual(matrix, mid);
            
            if (count < k) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return left;
    }
    
    private static int countLessEqual(int[][] matrix, int target) {
        int count = 0;
        int n = matrix.length;
        int row = n - 1, col = 0;
        
        while (row >= 0 && col < n) {
            if (matrix[row][col] <= target) {
                count += row + 1;
                col++;
            } else {
                row--;
            }
        }
        
        return count;
    }
}
```

### Example Problems
- Search a 2D matrix
- Search a 2D matrix II
- Kth smallest element in sorted matrix

## Common Pitfalls

### 1. Integer Overflow
```java
// Wrong: Can cause overflow
int mid = (left + right) / 2;

// Correct: Prevents overflow
int mid = left + (right - left) / 2;
```

### 2. Infinite Loop
```java
// Wrong: Can cause infinite loop when left = right - 1
while (left < right) {
    int mid = (left + right) / 2;  // This can equal left
    if (condition(mid)) {
        right = mid;
    } else {
        left = mid;  // This doesn't change left when mid == left
    }
}

// Correct: Always make progress
while (left < right) {
    int mid = left + (right - left) / 2;
    if (condition(mid)) {
        right = mid;
    } else {
        left = mid + 1;  // Always increment left
    }
}
```

### 3. Array Bounds
```java
// Always check array bounds
if (nums == null || nums.length == 0) {
    return -1; // or appropriate default
}

// Be careful with mid+1 and mid-1 operations
if (mid < nums.length - 1) {
    // Safe to access nums[mid + 1]
}
```

### 4. Search Space Definition
```java
// Make sure search space is well-defined
// left and right should represent valid bounds
// For inclusive search: left <= right
// For exclusive search: left < right
```

## Practice Problems

### Beginner Level
1. **Binary Search** - Basic search in sorted array
2. **First Bad Version** - Find first occurrence pattern
3. **Search Insert Position** - Find insertion point
4. **Sqrt(x)** - Integer square root
5. **Guess Number Higher or Lower** - Interactive binary search

### Intermediate Level
6. **Find First and Last Position** - Find boundaries
7. **Search in Rotated Sorted Array** - Rotated array search
8. **Find Minimum in Rotated Sorted Array** - Find pivot
9. **Peak Index in Mountain Array** - Peak finding
10. **Koko Eating Bananas** - Binary search on answer space
11. **Capacity to Ship Packages** - Optimization problem
12. **Find Peak Element** - General peak finding

### Advanced Level
13. **Search in Rotated Sorted Array II** - With duplicates
14. **Split Array Largest Sum** - Complex optimization
15. **Kth Smallest Element in Sorted Matrix** - Matrix search
16. **Find Peak Element II** - 2D peak finding
17. **Magnetic Force Between Two Balls** - Advanced optimization
18. **Minimum Time to Complete Trips** - Complex feasibility check

## Key Takeaways

1. **Identify the Pattern**: Recognize when binary search can be applied
2. **Define Search Space**: Clearly define what you're searching for
3. **Monotonic Condition**: Ensure your condition function is monotonic
4. **Choose the Right Template**: Use appropriate template for the problem type
5. **Handle Edge Cases**: Consider empty arrays, single elements, etc.
6. **Avoid Common Pitfalls**: Prevent overflow, infinite loops, and boundary errors

### Java-Specific Tips
- Use `Arrays.stream(arr).max().getAsInt()` for finding maximum
- Use `Arrays.stream(arr).sum()` for array sum
- Handle integer overflow with `left + (right - left) / 2`
- Use functional interfaces for cleaner code organization
- Always check for null arrays and empty arrays

Remember: Binary search is not just about searching in arrays - it's about efficiently finding the boundary where a condition changes from false to true (or vice versa) in any monotonic space.

---

*Happy coding! 🚀*