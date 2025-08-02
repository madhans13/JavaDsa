# 🚀 Sliding Window Pattern (Left-Right Pointers) + HashSet

## ✅ Problem Type
Find the **longest/shortest subarray or substring** with certain constraints — like **no repeating characters**, sum within a range, or matching a pattern.

## 🔧 When to Use
- Need to process **continuous sequences (substrings/subarrays)**
- Constraint: **No duplicates**, or **at most k unique**, or **sum < target**
- Need to maintain a **"window"** of valid elements and **shrink/grow** it dynamically

## 🧠 Core Idea
Use two pointers `left` and `right` to represent the current window. Use a **HashSet** (or HashMap/array based on problem) to track window state.

### Basic Template
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

## 📌 Example: Longest Substring Without Repeating Characters

### 🧱 Tools Used
- `HashSet<Character>` to track unique characters
- `left`, `right` pointers as window boundaries

### 💡 Algorithm Logic
1. Move `right` to expand the window
2. If duplicate is found (`set.contains(char)`), shrink window by removing `s.charAt(left++)`
3. Track `maxLen` at each step

### ✅ Java Implementation
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

## ⏱️ Time & Space Complexity
- **Time:** O(n) – each character is visited at most twice (once by `right`, once by `left`)
- **Space:** O(n) – in worst case, all unique characters stored in the set

## 🔄 Template Variations

| Constraint Type | Tool Used | Use Case |
|----------------|-----------|----------|
| No duplicates | `HashSet` | Longest substring without repeating chars |
| At most K distinct | `HashMap + count` | Longest substring with at most K unique chars |
| Target sum | `Prefix Sum` | Subarray sum equals target |
| Matching pattern | `Char frequency` | Find anagram substrings |

## 🧩 Key Tips & Best Practices

### Window Management
- **Shrink** the window **only when invalid** (e.g., duplicate found)
- **Expand** the window by moving `right` one step at a time
- **Shrink** by moving `left` to restore validity

### Result Tracking
- Always update result (`maxLen`, etc.) **after expanding**, not shrinking
- Calculate window size as `right - left + 1`

### Common Pitfalls
- Don't forget to update the data structure when shrinking the window
- Remember to handle edge cases (empty string, single character)
- Ensure pointers don't go out of bounds

## 🎯 Problem Variations
- **Minimum Window Substring**
- **Longest Substring with At Most K Distinct Characters**
- **Sliding Window Maximum**
- **Find All Anagrams in a String**

## 📝 Implementation Notes
- Use `HashSet` for simple presence/absence tracking
- Use `HashMap` when you need to count frequencies
- Consider using arrays for character counting when dealing with limited character sets (e.g., lowercase letters only)  