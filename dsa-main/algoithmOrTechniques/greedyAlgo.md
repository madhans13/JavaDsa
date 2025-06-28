# Greedy Algorithms: A Comprehensive Guide

## Table of Contents
- [Introduction](#introduction)
- [What is a Greedy Algorithm?](#what-is-a-greedy-algorithm)
- [When to Use Greedy Algorithms](#when-to-use-greedy-algorithms)
- [Key Characteristics](#key-characteristics)
- [Problem Examples](#problem-examples)
  - [1. Lemonade Change](#1-lemonade-change)
  - [2. Assign Cookies](#2-assign-cookies)
- [Step-by-Step Problem Solving](#step-by-step-problem-solving)
- [Common Greedy Patterns](#common-greedy-patterns)
- [Time & Space Complexity](#time--space-complexity)
- [Practice Problems](#practice-problems)
- [Tips and Tricks](#tips-and-tricks)

## Introduction

Greedy algorithms are a class of algorithms that make locally optimal choices at each step, hoping to find a global optimum. They are particularly useful for optimization problems where making the best choice at each moment leads to an overall optimal solution.

## What is a Greedy Algorithm?

A greedy algorithm builds up a solution piece by piece, always choosing the next piece that offers the most immediate benefit. The key insight is that **local optimization leads to global optimization** for certain types of problems.

### Core Principle
> "Make the choice that looks best right now, without worrying about future consequences."

## When to Use Greedy Algorithms

Greedy algorithms work best when:
- The problem has **optimal substructure** (optimal solution contains optimal solutions to subproblems)
- The problem exhibits the **greedy choice property** (locally optimal choices lead to globally optimal solution)
- You need an efficient solution (usually O(n) or O(n log n))

## Key Characteristics

1. **Irrevocable Decisions**: Once a choice is made, it's never reconsidered
2. **Local Optimization**: Each step chooses the best available option
3. **No Backtracking**: Never undoes previous decisions
4. **Efficient**: Usually faster than dynamic programming or exhaustive search

---

## Problem Examples

### 1. Lemonade Change

**Problem Statement**: At a lemonade stand, each lemonade costs $5. Customers pay with $5, $10, or $20 bills. You start with no money. Return `true` if you can provide correct change to every customer, `false` otherwise.

#### Example
```
Input: bills = [5,5,10,10,20]
Output: true

Explanation:
- Customer 1: pays $5 → no change needed
- Customer 2: pays $5 → no change needed  
- Customer 3: pays $10 → give $5 change
- Customer 4: pays $10 → give $5 change
- Customer 5: pays $20 → give $15 change ($10 + $5)
```

#### Greedy Strategy
Always give the largest denomination possible as change to preserve smaller bills for future transactions.

**For $20 payment**: Prefer giving one $10 + one $5 over three $5 bills
**For $10 payment**: Give one $5 bill

#### Solution

```java
public class LemonadeChange {
    /**
     * Greedy approach: Always use largest denomination first
     * Time: O(n), Space: O(1)
     */
    public boolean lemonadeChange(int[] bills) {
        int fiveCount = 0;
        int tenCount = 0;
        
        for (int bill : bills) {
            if (bill == 5) {
                fiveCount++;
                
            } else if (bill == 10) {
                if (fiveCount == 0) {
                    return false;
                }
                fiveCount--;
                tenCount++;
                
            } else { // bill == 20
                // Greedy: prefer $10 + $5 over three $5s
                if (tenCount > 0 && fiveCount > 0) {
                    tenCount--;
                    fiveCount--;
                } else if (fiveCount >= 3) {
                    fiveCount -= 3;
                } else {
                    return false;
                }
            }
        }
        
        return true;
    }
}
```

#### Why Greedy Works Here
- **Greedy Choice**: Always use larger denominations first
- **Optimal Substructure**: If we can make change for customers 1 to i, and can make change for customer i+1, then we can make change for customers 1 to i+1
- **Key Insight**: $5 bills are more "valuable" than $10 bills because they can be used for both $10 and $20 change

---

### 2. Assign Cookies

**Problem Statement**: You have `g` children and `s` cookies. Each child has a greed factor `g[i]` (minimum cookie size to satisfy them). Each cookie has size `s[j]`. Maximize the number of satisfied children.

#### Example
```
Input: g = [1,2,3], s = [1,1]
Output: 1

Explanation:
- Child 0 (greed=1): can be satisfied with cookie of size 1
- Child 1 (greed=2): needs cookie of size ≥2 (not available)  
- Child 2 (greed=3): needs cookie of size ≥3 (not available)
- Maximum satisfied: 1 child
```

#### Greedy Strategy
1. Sort children by greed factor (ascending)
2. Sort cookies by size (ascending)
3. Try to satisfy the least greedy child first with the smallest sufficient cookie

#### Solution

```java
import java.util.Arrays;

public class AssignCookies {
    /**
     * Greedy approach: Satisfy least greedy children first
     * Time: O(n log n + m log m), Space: O(1)
     */
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);  // Sort children by greed
        Arrays.sort(s);  // Sort cookies by size
        
        int childI = 0;
        int cookieJ = 0;
        int satisfied = 0;
        
        while (childI < g.length && cookieJ < s.length) {
            if (s[cookieJ] >= g[childI]) {
                // Cookie can satisfy this child
                satisfied++;
                childI++;
            }
            cookieJ++;
        }
        
        return satisfied;
    }
}
```

#### Alternative Approach (Greedy from largest)
```java
import java.util.Arrays;
import java.util.Collections;

public class AssignCookiesV2 {
    /**
     * Alternative: Start from most greedy child
     * Time: O(n log n + m log m), Space: O(1)
     */
    public int findContentChildren(int[] g, int[] s) {
        // Sort in descending order
        Arrays.sort(g);
        Arrays.sort(s);
        reverseArray(g);
        reverseArray(s);
        
        int satisfied = 0;
        int cookieJ = 0;
        
        for (int childGreed : g) {
            if (cookieJ < s.length && s[cookieJ] >= childGreed) {
                satisfied++;
                cookieJ++;
            }
        }
        
        return satisfied;
    }
    
    private void reverseArray(int[] arr) {
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }
}
```

#### Why Greedy Works Here
- **Greedy Choice**: Either satisfy the least greedy child with smallest possible cookie, or start with most greedy child
- **Optimal Substructure**: If we optimally assign cookies to a subset of children, we can extend this to the full set
- **Key Insight**: No benefit in giving a larger cookie to a child who would be satisfied with a smaller one

---

## Step-by-Step Problem Solving

### 1. Identify if Greedy Applies
- Can you make locally optimal choices?
- Does the problem have optimal substructure?
- Will local optimization lead to global optimization?

### 2. Define the Greedy Choice
- What criteria determines the "best" choice at each step?
- Should you sort the input first?

### 3. Prove Correctness (Optional but Recommended)
- Show that greedy choice property holds
- Prove optimal substructure exists

### 4. Implement and Optimize
- Code the solution
- Analyze time and space complexity

---

## Common Greedy Patterns

### Pattern 1: Interval Scheduling
**Examples**: Meeting rooms, activity selection
**Strategy**: Sort by end time, select non-overlapping intervals

```java
// Example: Activity Selection
public int activitySelection(int[] start, int[] end) {
    int n = start.length;
    int[][] activities = new int[n][2];
    
    for (int i = 0; i < n; i++) {
        activities[i][0] = start[i];
        activities[i][1] = end[i];
    }
    
    // Sort by end time
    Arrays.sort(activities, (a, b) -> a[1] - b[1]);
    
    int count = 1;
    int lastEnd = activities[0][1];
    
    for (int i = 1; i < n; i++) {
        if (activities[i][0] >= lastEnd) {
            count++;
            lastEnd = activities[i][1];
        }
    }
    
    return count;
}
```

### Pattern 2: Fractional/Exchange Problems  
**Examples**: Coin change (specific denominations), gas stations
**Strategy**: Use largest denomination/most efficient choice first

```java
// Example: Gas Station Problem
public int canCompleteCircuit(int[] gas, int[] cost) {
    int totalGas = 0, totalCost = 0;
    int currentGas = 0, start = 0;
    
    for (int i = 0; i < gas.length; i++) {
        totalGas += gas[i];
        totalCost += cost[i];
        currentGas += gas[i] - cost[i];
        
        // If current gas becomes negative, reset start point
        if (currentGas < 0) {
            start = i + 1;
            currentGas = 0;
        }
    }
    
    return totalGas >= totalCost ? start : -1;
}
```

### Pattern 3: Sorting + Two Pointers
**Examples**: Assign cookies, boats to save people
**Strategy**: Sort both arrays, use two pointers to find optimal pairing

```java
// Example: Boats to Save People
public int numRescueBoats(int[] people, int limit) {
    Arrays.sort(people);
    int left = 0, right = people.length - 1;
    int boats = 0;
    
    while (left <= right) {
        if (people[left] + people[right] <= limit) {
            left++;
        }
        right--;
        boats++;
    }
    
    return boats;
}
```

### Pattern 4: Priority Queue/Heap
**Examples**: Huffman coding, merge k sorted lists
**Strategy**: Always process the highest/lowest priority element

```java
import java.util.PriorityQueue;

// Example: Meeting Rooms II
public int minMeetingRooms(int[][] intervals) {
    if (intervals.length == 0) return 0;
    
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    for (int[] interval : intervals) {
        if (!minHeap.isEmpty() && minHeap.peek() <= interval[0]) {
            minHeap.poll();
        }
        minHeap.offer(interval[1]);
    }
    
    return minHeap.size();
}
```

---

## Time & Space Complexity

| Problem | Time Complexity | Space Complexity | Notes |
|---------|----------------|------------------|-------|
| Lemonade Change | O(n) | O(1) | Single pass, constant extra space |
| Assign Cookies | O(n log n + m log m) | O(1) | Sorting dominates, constant extra space |

**General Complexity Patterns**:
- **No sorting needed**: O(n) time
- **Sorting required**: O(n log n) time  
- **With priority queue**: O(n log n) time
- **Space**: Usually O(1) or O(n) for auxiliary structures

---

## Practice Problems

### Beginner Level
1. **Best Time to Buy and Sell Stock** - Single pass greedy
```java
public int maxProfit(int[] prices) {
    int minPrice = Integer.MAX_VALUE;
    int maxProfit = 0;
    
    for (int price : prices) {
        if (price < minPrice) {
            minPrice = price;
        } else if (price - minPrice > maxProfit) {
            maxProfit = price - minPrice;
        }
    }
    
    return maxProfit;
}
```

2. **Jump Game** - Can you reach the end?
```java
public boolean canJump(int[] nums) {
    int maxReach = 0;
    
    for (int i = 0; i < nums.length; i++) {
        if (i > maxReach) return false;
        maxReach = Math.max(maxReach, i + nums[i]);
    }
    
    return true;
}
```

3. **Gas Station** - Circular array problem (see Pattern 2 above)

### Intermediate Level
4. **Meeting Rooms II** - Minimum rooms needed (see Pattern 4 above)

5. **Non-overlapping Intervals** - Remove minimum intervals
```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) return 0;
    
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
    int end = intervals[0][1];
    int count = 0;
    
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] < end) {
            count++;
        } else {
            end = intervals[i][1];
        }
    }
    
    return count;
}
```

6. **Partition Labels** - Partition string into maximum parts
```java
public List<Integer> partitionLabels(String s) {
    int[] lastIndex = new int[26];
    
    // Record last occurrence of each character
    for (int i = 0; i < s.length(); i++) {
        lastIndex[s.charAt(i) - 'a'] = i;
    }
    
    List<Integer> result = new ArrayList<>();
    int start = 0, end = 0;
    
    for (int i = 0; i < s.length(); i++) {
        end = Math.max(end, lastIndex[s.charAt(i) - 'a']);
        if (i == end) {
            result.add(end - start + 1);
            start = end + 1;
        }
    }
    
    return result;
}
```

### Advanced Level
7. **Candy** - Distribute candy with constraints
```java
public int candy(int[] ratings) {
    int n = ratings.length;
    int[] candies = new int[n];
    Arrays.fill(candies, 1);
    
    // Left to right pass
    for (int i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }
    
    // Right to left pass
    for (int i = n - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            candies[i] = Math.max(candies[i], candies[i + 1] + 1);
        }
    }
    
    return Arrays.stream(candies).sum();
}
```

8. **Jump Game II** - Minimum jumps to reach end
```java
public int jump(int[] nums) {
    int jumps = 0, currentEnd = 0, farthest = 0;
    
    for (int i = 0; i < nums.length - 1; i++) {
        farthest = Math.max(farthest, i + nums[i]);
        
        if (i == currentEnd) {
            jumps++;
            currentEnd = farthest;
        }
    }
    
    return jumps;
}
```

9. **Task Scheduler** - Schedule tasks with cooldown
```java
public int leastInterval(char[] tasks, int n) {
    int[] freq = new int[26];
    for (char task : tasks) {
        freq[task - 'A']++;
    }
    
    Arrays.sort(freq);
    int maxFreq = freq[25];
    int idleSlots = (maxFreq - 1) * n;
    
    for (int i = 24; i >= 0 && freq[i] > 0; i--) {
        idleSlots -= Math.min(freq[i], maxFreq - 1);
    }
    
    return idleSlots > 0 ? tasks.length + idleSlots : tasks.length;
}
```

---

## Tips and Tricks

### 🎯 Problem Recognition
- Look for keywords: "maximum", "minimum", "optimal"
- Check if local choices don't affect future options negatively
- See if sorting helps reveal the greedy strategy

### 🧠 Strategy Selection
- **When to sort**: If order matters for making optimal choices
- **What to prioritize**: End times, ratios, deadlines, sizes
- **Edge cases**: Empty inputs, single elements, all same values

### 🐛 Common Pitfalls
- Not proving that greedy works (some problems need DP)
- Wrong sorting criteria
- Forgetting edge cases
- Not considering all possible greedy strategies

### 💡 Optimization Tips
- Avoid sorting if input has special properties
- Use counting sort for limited range values
- Consider space-time tradeoffs

---

## Conclusion

Greedy algorithms offer elegant and efficient solutions to many optimization problems. The key is recognizing when the greedy choice property holds and implementing the correct local optimization strategy.

Remember: **Not every problem can be solved greedily**, but when applicable, greedy algorithms provide some of the most intuitive and efficient solutions in competitive programming and real-world applications.

---

**Happy Coding! 🚀**

*Feel free to contribute additional examples, optimizations, or corrections to this guide.*
