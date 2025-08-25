# 🚀 Sliding Window Pattern (Left-Right Pointers) + HashSet

## ✅ Problem Type
Find the **longest/shortest subarray or substring** with certain constraints — like **no repeating characters**, sum within a range, or matching a pattern.

## 🔧 When to Use
- Need to process **continuous sequences (substrings/subarrays)**
- Constraint: **No duplicates**, or **at most k unique**, or **sum < target**
- Need to maintain a **"window"** of valid elements and **shrink/grow** it dynamically

## 🧠 Core Idea
Use two pointers `left` and `right` to represent the current window. Use a **HashSet** (or HashMap/array based on problem) to track window state.

---

## 🔄 Two Main Approaches

### 1️⃣ Basic Sliding Window Template
```java
int left = 0, right = 0;
while (right < s.length()) {
    if (window is valid) {
        // Expand window
        right++;
    } else {
        // Shrink window
        left++;
    }
}
```

### 2️⃣ Dynamic Sliding Window (More Efficient) ⭐
```java
int left = 0, maxLen = 0;
for (int right = 0; right < s.length(); right++) {
    // Add current element to window
    addToWindow(s.charAt(right));
    
    // Shrink window while invalid
    while (!isWindowValid()) {
        removeFromWindow(s.charAt(left));
        left++;
    }
    
    // Update result with current valid window
    maxLen = Math.max(maxLen, right - left + 1);
}
```

---

## 📌 Example: Longest Substring Without Repeating Characters

### 🧱 Tools Used
- `HashSet<Character>` to track unique characters
- `left`, `right` pointers as window boundaries

### 💡 Algorithm Logic
1. Move `right` to expand the window
2. If duplicate is found (`set.contains(char)`), shrink window by removing `s.charAt(left++)`
3. Track `maxLen` at each step

### ✅ Basic Implementation
```java
public int lengthOfLongestSubstring(String s) {
    Set<Character> set = new HashSet<>();
    int left = 0, right = 0, maxLen = 0;
    
    while (right < s.length()) {
        char ch = s.charAt(right);
        
        if (!set.contains(ch)) {
            set.add(ch);
            maxLen = Math.max(maxLen, right - left + 1);
            right++;
        } else {
            set.remove(s.charAt(left));
            left++;
        }
    }
    
    return maxLen;
}
```

### 🚀 Dynamic Sliding Window Implementation (Optimized)
```java
public int lengthOfLongestSubstring(String s) {
    Set<Character> window = new HashSet<>();
    int left = 0, maxLen = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char ch = s.charAt(right);
        
        // Shrink window while we have duplicate
        while (window.contains(ch)) {
            window.remove(s.charAt(left));
            left++;
        }
        
        // Add current character and update result
        window.add(ch);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    
    return maxLen;
}
```

### 🏆 Ultra-Optimized with HashMap (Jump Strategy)
```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> lastIndex = new HashMap<>();
    int left = 0, maxLen = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char ch = s.charAt(right);
        
        // Jump left pointer to avoid duplicate
        if (lastIndex.containsKey(ch)) {
            left = Math.max(left, lastIndex.get(ch) + 1);
        }
        
        lastIndex.put(ch, right);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    
    return maxLen;
}
```

---

## ⏱️ Time & Space Complexity Comparison

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Basic Template | O(2n) | O(k) | Each char visited twice in worst case |
| Dynamic Sliding | O(n) | O(k) | Each char visited once by right pointer |
| HashMap Jump | O(n) | O(k) | Optimal - direct jumping to valid position |

*k = size of character set or window constraint*

---

## 🔄 Dynamic Template Variations

### 1️⃣ For "At Most K" Problems
```java
public int atMostK(String s, int k) {
    Map<Character, Integer> count = new HashMap<>();
    int left = 0, maxLen = 0;
    
    for (int right = 0; right < s.length(); right++) {
        // Expand window
        char ch = s.charAt(right);
        count.put(ch, count.getOrDefault(ch, 0) + 1);
        
        // Shrink window while invalid
        while (count.size() > k) {
            char leftChar = s.charAt(left);
            count.put(leftChar, count.get(leftChar) - 1);
            if (count.get(leftChar) == 0) {
                count.remove(leftChar);
            }
            left++;
        }
        
        maxLen = Math.max(maxLen, right - left + 1);
    }
    
    return maxLen;
}
```

### 2️⃣ For "Exactly K" Problems
```java
public int exactlyK(String s, int k) {
    return atMostK(s, k) - atMostK(s, k - 1);
}
```

### 3️⃣ For Sum-Based Problems
```java
public int maxSubarraySum(int[] nums, int target) {
    int left = 0, sum = 0, maxLen = 0;
    
    for (int right = 0; right < nums.length; right++) {
        // Expand window
        sum += nums[right];
        
        // Shrink window while invalid
        while (sum > target && left <= right) {
            sum -= nums[left];
            left++;
        }
        
        // Update result if valid
        if (sum == target) {
            maxLen = Math.max(maxLen, right - left + 1);
        }
    }
    
    return maxLen;
}
```

---

## 🎯 Pattern Recognition Guide

| Problem Description | Approach | Key Data Structure |
|-------------------|----------|-------------------|
| "Longest substring without repeating" | Dynamic Window | HashSet |
| "At most K distinct characters" | Dynamic Window | HashMap (count) |
| "Exactly K distinct characters" | atMostK(k) - atMostK(k-1) | HashMap (count) |
| "Subarray sum equals target" | Dynamic Window | Running sum |
| "Find all anagrams" | Fixed Window | HashMap (frequency) |
| "Minimum window substring" | Dynamic Window | HashMap (need count) |

---

## 🧩 Key Tips & Best Practices

### Dynamic Window Advantages
- **Single pass**: Right pointer never goes backward
- **Cleaner logic**: Always expand, shrink when needed
- **Better performance**: Each element processed once

### Window Management
- **Always expand** by moving `right` in the main loop
- **Shrink conditionally** using while loop when invalid
- **Update result** after ensuring window is valid

### Result Tracking
- Calculate window size as `right - left + 1`
- Update result **after** shrinking (when window is valid)
- Handle edge cases (empty arrays, single elements)

### Common Pitfalls
- Forgetting to update data structure when shrinking
- Not handling the case when `left > right`
- Updating result before ensuring window validity

---

## 🎯 Problem Variations with Solutions

### 1️⃣ Minimum Window Substring
```java
public String minWindow(String s, String t) {
    Map<Character, Integer> need = new HashMap<>();
    for (char c : t.toCharArray()) {
        need.put(c, need.getOrDefault(c, 0) + 1);
    }
    
    Map<Character, Integer> window = new HashMap<>();
    int left = 0, right = 0, valid = 0;
    int start = 0, len = Integer.MAX_VALUE;
    
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;
        
        if (need.containsKey(c)) {
            window.put(c, window.getOrDefault(c, 0) + 1);
            if (window.get(c).equals(need.get(c))) {
                valid++;
            }
        }
        
        while (valid == need.size()) {
            if (right - left < len) {
                start = left;
                len = right - left;
            }
            
            char d = s.charAt(left);
            left++;
            
            if (need.containsKey(d)) {
                if (window.get(d).equals(need.get(d))) {
                    valid--;
                }
                window.put(d, window.get(d) - 1);
            }
        }
    }
    
    return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);
}
```

### 2️⃣ Sliding Window Maximum
```java
public int[] maxSlidingWindow(int[] nums, int k) {
    Deque<Integer> deque = new ArrayDeque<>();
    int[] result = new int[nums.length - k + 1];
    
    for (int right = 0; right < nums.length; right++) {
        // Remove indices outside window
        while (!deque.isEmpty() && deque.peekFirst() < right - k + 1) {
            deque.pollFirst();
        }
        
        // Remove smaller elements (they'll never be maximum)
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[right]) {
            deque.pollLast();
        }
        
        deque.offerLast(right);
        
        // Window is complete, record result
        if (right >= k - 1) {
            result[right - k + 1] = nums[deque.peekFirst()];
        }
    }
    
    return result;
}
```

---

## 📝 Implementation Decision Tree

```
Problem involves contiguous subarray/substring?
├─ YES → Consider Sliding Window
│   ├─ Fixed size window? → Use for-loop with fixed k
│   ├─ Variable size window? → Use dynamic approach
│   │   ├─ Need unique elements? → HashSet
│   │   ├─ Need element counts? → HashMap
│   │   ├─ Sum-based constraint? → Running sum
│   │   └─ Order matters? → Deque
│   └─ Multiple constraints? → Combine data structures
└─ NO → Consider other patterns
```

## 🎪 Advanced Patterns

### Multi-constraint Windows
```java
// Example: Substring with at most 2 distinct chars AND length >= 3
public int complexConstraint(String s) {
    Map<Character, Integer> count = new HashMap<>();
    int left = 0, result = 0;
    
    for (int right = 0; right < s.length(); right++) {
        count.put(s.charAt(right), count.getOrDefault(s.charAt(right), 0) + 1);
        
        while (count.size() > 2) {
            char leftChar = s.charAt(left);
            count.put(leftChar, count.get(leftChar) - 1);
            if (count.get(leftChar) == 0) count.remove(leftChar);
            left++;
        }
        
        if (right - left + 1 >= 3) {  // Additional constraint
            result = Math.max(result, right - left + 1);
        }
    }
    
    return result;
}
```

The dynamic sliding window approach is generally preferred for its clarity, efficiency, and easier debugging. It follows the principle of "expand optimistically, shrink when necessary" which maps well to most sliding window problems.   
# Sliding Window for Strings

## Pattern: Fixed-Size Window with Character Frequency

**Use Case:** Finding permutations, anagrams, or character pattern matches within strings.

### Key Steps:
1. **Initialize frequency map** of target pattern
2. **Expand window** by adding characters from the right
3. **Maintain fixed size** by removing characters from the left when window exceeds target size
4. **Compare frequencies** at each valid window position

### Template:
```java
public boolean slidingWindowString(String pattern, String text) {
    Map<Character, Integer> targetFreq = new HashMap<>();
    // Build frequency map for pattern
    for (char c : pattern.toCharArray()) {
        targetFreq.put(c, targetFreq.getOrDefault(c, 0) + 1);
    }
    
    Map<Character, Integer> windowFreq = new HashMap<>();
    int left = 0, windowSize = pattern.length();
    
    for (int right = 0; right < text.length(); right++) {
        // Expand window
        char rightChar = text.charAt(right);
        windowFreq.put(rightChar, windowFreq.getOrDefault(rightChar, 0) + 1);
        
        // Maintain fixed window size
        if (right >= windowSize) {
            char leftChar = text.charAt(left);
            windowFreq.put(leftChar, windowFreq.get(leftChar) - 1);
            if (windowFreq.get(leftChar) == 0) {
                windowFreq.remove(leftChar);
            }
            left++;
        }
        
        // Check condition (frequencies match)
        if (targetFreq.equals(windowFreq)) {
            return true; // or collect result
        }
    }
    return false;
}
```

### Time Complexity: O(n)
### Space Complexity: O(k) where k is the number of unique characters

---

## String Pattern Matching with Fixed Window

### Pattern: Finding Permutations/Anagrams in Text

**Problem Example:** Check if any permutation of `s1` exists as a substring in `s2`.

### Approach:
1. **Fixed window size** = length of pattern string
2. **Character frequency comparison** using HashMap.equals()
3. **Slide window** one character at a time, maintaining frequency counts

### Implementation:
```java
public boolean checkInclusion(String s1, String s2) {
    int left = 0;
    HashMap<Character, Integer> targetFreq = new HashMap<>();
    
    // Build frequency map for pattern
    for (char ch : s1.toCharArray()) {
        targetFreq.put(ch, targetFreq.getOrDefault(ch, 0) + 1);
    }
    
    int windowLength = s1.length();
    HashMap<Character, Integer> windowFreq = new HashMap<>();
    
    for (int right = 0; right < s2.length(); right++) {
        char ch = s2.charAt(right);
        windowFreq.put(ch, windowFreq.getOrDefault(ch, 0) + 1);
        
        // Maintain fixed window size
        if (right >= windowLength) {
            char leftChar = s2.charAt(left);
            windowFreq.put(leftChar, windowFreq.get(leftChar) - 1);
            if (windowFreq.get(leftChar) == 0) {
                windowFreq.remove(leftChar);
            }
            left++;
        }
        
        // Check if current window matches pattern
        if (targetFreq.equals(windowFreq)) {
            return true;
        }
    }
    
    return false;
}
```

### Key Points:
- **Window boundary:** `right >= windowLength` ensures we maintain exact window size
- **Frequency cleanup:** Remove zero-count entries for accurate `equals()` comparison
- **Efficient matching:** HashMap.equals() compares both keys and values
- **Pattern agnostic:** Works for any permutation of the pattern

### Use Cases:
- Find all anagrams in a string
- Check substring permutation existence  
- Pattern matching with character rearrangement
- Fixed-length substring problems with frequency constraints