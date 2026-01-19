# KMP Algorithm (Knuth-Morris-Pratt) - Complete Guide 🚀

A comprehensive, doubt-free explanation of the KMP string matching algorithm with detailed examples, visual aids, and working code.

---

## 📚 Table of Contents

1. [What is KMP Algorithm?](#what-is-kmp-algorithm)
2. [Why Do We Need KMP?](#why-do-we-need-kmp)
3. [The Core Concept](#the-core-concept)
4. [Understanding the LPS Array](#understanding-the-lps-array)
5. [Building the LPS Array - Step by Step](#building-the-lps-array---step-by-step)
6. [The Tricky Part: Handling Mismatches in LPS](#the-tricky-part-handling-mismatches-in-lps)
7. [Using LPS Array for Pattern Matching](#using-lps-array-for-pattern-matching)
8. [Complete Java Implementation](#complete-java-implementation)
9. [Common Mistakes to Avoid](#common-mistakes-to-avoid)
10. [Practice Examples](#practice-examples)

---

## What is KMP Algorithm?

KMP is an **efficient string matching algorithm** that finds all occurrences of a pattern in a text.

**Time Complexity:** O(n + m) where:
- n = length of text
- m = length of pattern

**Key Feature:** Never goes backward in the text - only moves forward!

---

## Why Do We Need KMP?

### ❌ Naive Approach Problem

```
Text:    "ABABDABABCABAB"
Pattern: "ABABC"

Step 1: A B A B D...
        A B A B C
        ✓ ✓ ✓ ✓ ✗ (mismatch at D vs C)

Step 2: A B A B D...  (shift by 1)
          A B A B C
          ✗ (start comparing from beginning again - WASTE!)
```

**Problem:** We already know "ABAB" matched, but we're rechecking everything!

**Time Complexity:** O(n × m) - Very slow!

### ✅ KMP Solution

KMP remembers what was matched and **skips unnecessary comparisons**.

```
Step 1: A B A B D...
        A B A B C
        ✓ ✓ ✓ ✓ ✗ (mismatch)

Step 2: A B A B D...  (intelligent shift using LPS)
            A B A B C
            ↑ ↑ (already know these match - skip checking!)
```

**Time Complexity:** O(n + m) - Much faster!

---

## The Core Concept

### Main Idea: "Don't Re-check What You Already Know"

When a mismatch occurs, KMP uses the **LPS array** to determine:
- How many characters can be skipped
- Where to continue matching from

**The Magic Question:** "How much of what I just matched can I reuse?"

---

## Understanding the LPS Array

### What is LPS?

**LPS** = **Longest Proper Prefix which is also Suffix**

For each position `i` in the pattern:
- `LPS[i]` = length of longest proper prefix of `pattern[0...i]` that is also a suffix

**Proper Prefix:** A prefix that is NOT the entire string
- For "ABA": proper prefixes are "A", "AB" (NOT "ABA")

**Suffix:** End part of string
- For "ABA": suffixes are "A", "BA", "ABA"

### Why LPS Array?

LPS tells us: **"When mismatch happens, how much can I skip?"**

### LPS Array Examples

#### Example 1: Pattern = "ABABC"

```
Position 0: "A"
  Proper prefixes: (none)
  Suffixes: "A"
  Match: none
  LPS[0] = 0

Position 1: "AB"
  Proper prefixes: "A"
  Suffixes: "B", "AB"
  Match: none
  LPS[1] = 0

Position 2: "ABA"
  Proper prefixes: "A", "AB"
  Suffixes: "A", "BA", "ABA"
  Match: "A" = "A" ✓ (length 1)
  LPS[2] = 1

Position 3: "ABAB"
  Proper prefixes: "A", "AB", "ABA"
  Suffixes: "B", "AB", "BAB", "ABAB"
  Match: "AB" = "AB" ✓ (length 2)
  LPS[3] = 2

Position 4: "ABABC"
  Proper prefixes: "A", "AB", "ABA", "ABAB"
  Suffixes: "C", "BC", "ABC", "BABC", "ABABC"
  Match: none
  LPS[4] = 0
```

**Final LPS Array:**
```
Pattern: A B A B C
Index:   0 1 2 3 4
LPS:     0 0 1 2 0
```

#### Example 2: Pattern = "AAAA"

```
Pattern: A A A A
Index:   0 1 2 3
LPS:     0 1 2 3

Explanation:
- "AA" → "A" matches "A" → LPS[1] = 1
- "AAA" → "AA" matches "AA" → LPS[2] = 2
- "AAAA" → "AAA" matches "AAA" → LPS[3] = 3
```

#### Example 3: Pattern = "ABCDE"

```
Pattern: A B C D E
Index:   0 1 2 3 4
LPS:     0 0 0 0 0

Explanation: No repeating patterns
```

---

## Building the LPS Array - Step by Step

### The Algorithm

```java
int[] lps = new int[pattern.length()];
int length = 0;  // Length of previous longest prefix suffix
int i = 1;       // Start from index 1 (lps[0] is always 0)

lps[0] = 0;  // Base case

while (i < pattern.length()) {
    if (pattern[i] == pattern[length]) {
        // Characters match → extend the match
        length++;
        lps[i] = length;
        i++;
    } else {
        // Characters don't match
        if (length != 0) {
            // Try shorter prefix (THIS IS THE TRICKY PART!)
            length = lps[length - 1];
            // Don't increment i - retry with new length
        } else {
            // No match at all
            lps[i] = 0;
            i++;
        }
    }
}
```

### Detailed Example: Building LPS for "ABABCABAB"

```
Pattern: A B A B C A B A B
Index:   0 1 2 3 4 5 6 7 8
LPS:     0 ? ? ? ? ? ? ? ?

Initial: length = 0, i = 1
```

**Iteration 1:**
```
i=1, length=0
Compare: pattern[1]='B' with pattern[0]='A'
Result: 'B' != 'A' ✗
Action: length == 0, so lps[1] = 0, i++
```
```
Pattern: A B A B C A B A B
LPS:     0 0 ? ? ? ? ? ? ?
```

**Iteration 2:**
```
i=2, length=0
Compare: pattern[2]='A' with pattern[0]='A'
Result: 'A' == 'A' ✓
Action: length++, lps[2]=1, i++
```
```
Pattern: A B A B C A B A B
LPS:     0 0 1 ? ? ? ? ? ?
         ↑     ↑ (matched)
```

**Iteration 3:**
```
i=3, length=1
Compare: pattern[3]='B' with pattern[1]='B'
Result: 'B' == 'B' ✓
Action: length++, lps[3]=2, i++
```
```
Pattern: A B A B C A B A B
LPS:     0 0 1 2 ? ? ? ? ?
         ↑ ↑   ↑ ↑ (matched "AB")
```

**Iteration 4:**
```
i=4, length=2
Compare: pattern[4]='C' with pattern[2]='A'
Result: 'C' != 'A' ✗
Action: length != 0, so length = lps[1] = 0
Retry: pattern[4]='C' with pattern[0]='A'
Result: 'C' != 'A' ✗
Action: length == 0, so lps[4] = 0, i++
```
```
Pattern: A B A B C A B A B
LPS:     0 0 1 2 0 ? ? ? ?
```

**Iteration 5:**
```
i=5, length=0
Compare: pattern[5]='A' with pattern[0]='A'
Result: 'A' == 'A' ✓
Action: length++, lps[5]=1, i++
```
```
Pattern: A B A B C A B A B
LPS:     0 0 1 2 0 1 ? ? ?
```

**Iteration 6:**
```
i=6, length=1
Compare: pattern[6]='B' with pattern[1]='B'
Result: 'B' == 'B' ✓
Action: length++, lps[6]=2, i++
```
```
Pattern: A B A B C A B A B
LPS:     0 0 1 2 0 1 2 ? ?
```

**Iteration 7:**
```
i=7, length=2
Compare: pattern[7]='A' with pattern[2]='A'
Result: 'A' == 'A' ✓
Action: length++, lps[7]=3, i++
```
```
Pattern: A B A B C A B A B
LPS:     0 0 1 2 0 1 2 3 ?
```

**Iteration 8:**
```
i=8, length=3
Compare: pattern[8]='B' with pattern[3]='B'
Result: 'B' == 'B' ✓
Action: length++, lps[8]=4, i++
```
```
Pattern: A B A B C A B A B
LPS:     0 0 1 2 0 1 2 3 4
         ↑ ↑ ↑ ↑       ↑ ↑ ↑ ↑
         "ABAB" matches "ABAB"
```

**Final LPS: [0, 0, 1, 2, 0, 1, 2, 3, 4]**

---

## The Tricky Part: Handling Mismatches in LPS

### Why `length = lps[length - 1]`?

This is the **MOST CONFUSING** part of KMP! Let me explain clearly.

### The Problem

When we have a mismatch, we ask: **"Is there a SHORTER prefix that still matches?"**

### Visual Explanation

#### Example: Pattern = "AAACAAAA" at i=7

```
Pattern: A A A C A A A A
Index:   0 1 2 3 4 5 6 7
LPS:     0 1 2 0 1 2 3 ?

Current state: i=7, length=3
```

**Step 1: Try longest prefix (length=3)**
```
Compare: pattern[7]='A' with pattern[3]='C'
Result: Mismatch! ✗

We were trying:
Pattern: A A A C A A A A
         ↑ ↑ ↑ ✗       ↑
         0 1 2 3       7
         "AAAC" doesn't match "AAAA"
```

**Step 2: Find shorter prefix**
```
Question: "Can I use a shorter prefix?"
Check: lps[length-1] = lps[2] = 2

This tells us: "Inside 'AAA' (0-2), the longest prefix-suffix match is 2"

Meaning:
A A A
↑ ↑     ← prefix "AA" (length 2)
  ↑ ↑   ← suffix "AA" (length 2)
They match!
```

**Step 3: Try with shorter prefix**
```
length = lps[2] = 2

Now compare: pattern[7]='A' with pattern[2]='A'
Result: Match! ✓

Pattern: A A A C A A A A
           ↑ ↑       ↑ ↑ ↑
           0 1 2     5 6 7
           "AA" matches "AA" (and we can extend!)
```

### The Key Insight

```
If a long prefix doesn't match:
  → Check if that prefix has internal repetition (using LPS)
  → If yes, try the shorter repeating part
  → Keep trying shorter and shorter until it works or length becomes 0
```

### Another Example: "AAAABAAA" at i=7

```
Pattern: A A A A B A A A
Index:   0 1 2 3 4 5 6 7
LPS:     0 1 2 3 0 1 2 ?

i=7, length=2
Compare: pattern[7]='A' with pattern[2]='A'
Match! ✓
lps[7] = 3
```

But what if it was a mismatch?

```
Mismatch at i=7, length=2
Jump 1: length = lps[2-1] = lps[1] = 1
        Compare pattern[7] with pattern[1]
        
If still mismatch:
Jump 2: length = lps[1-1] = lps[0] = 0
        Compare pattern[7] with pattern[0]

If still mismatch:
        length = 0, so lps[7] = 0
```

### Visual: Cascading Jumps

```
Matched "AAA" but next character doesn't match

Try "AAA" (length 3) → Fail
  ↓
Use lps[2]=2
  ↓
Try "AA" (length 2) → Fail
  ↓
Use lps[1]=1
  ↓
Try "A" (length 1) → Fail
  ↓
Use lps[0]=0
  ↓
No prefix works, lps[i] = 0
```

---

## Using LPS Array for Pattern Matching

### The Search Algorithm

```java
int i = 0;  // Index for text
int j = 0;  // Index for pattern

while (i < text.length()) {
    if (text[i] == pattern[j]) {
        // Match! Move both pointers
        i++;
        j++;
    } else {
        // Mismatch!
        if (j != 0) {
            // Use LPS to skip characters
            j = lps[j - 1];
            // Don't increment i - retry same text character
        } else {
            // j is already 0, move to next text character
            i++;
        }
    }
    
    // Found complete pattern?
    if (j == pattern.length()) {
        // Match found at index (i - j)
        j = lps[j - 1];  // Continue searching
    }
}
```

### How LPS Helps in Searching

#### Example: Find "ABABC" in "ABABDABABC"

```
Text:    A B A B D A B A B C
Pattern: A B A B C
LPS:     0 0 1 2 0
```

**Step 1: Match and Mismatch**
```
i=0-3, j=0-3: All match
i=4, j=4: 'D' != 'C' → MISMATCH!

Text:    A B A B D A B A B C
Pattern: A B A B C
         ✓ ✓ ✓ ✓ ✗
```

**Step 2: Use LPS**
```
j != 0, so j = lps[j-1] = lps[3] = 2

This means: "The last 2 characters 'AB' match the first 2 'AB'"
So we can skip re-checking them!

Text:    A B A B D A B A B C
Pattern:     A B A B C
             ↑ ↑ (already matched, skip!)
```

**Step 3: Continue from where we left off**
```
i=4 (stays), j=2
Compare: text[4]='D' with pattern[2]='A'
Mismatch again!

j = lps[2-1] = lps[1] = 0
Now j=0

i=4, j=0: 'D' != 'A'
j==0, so i++
```

**Step 4: Continue searching**
```
i=5, j=0: 'A' == 'A' ✓ → i=6, j=1
i=6, j=1: 'B' == 'B' ✓ → i=7, j=2
i=7, j=2: 'A' == 'A' ✓ → i=8, j=3
i=8, j=3: 'B' == 'B' ✓ → i=9, j=4
i=9, j=4: 'C' == 'C' ✓ → i=10, j=5

j == pattern.length() → MATCH FOUND at index (10-5) = 5!

Text:    A B A B D A B A B C
                   ↑ ↑ ↑ ↑ ↑
                   Match found!
```

---

## Complete Java Implementation

```java
import java.util.ArrayList;
import java.util.Arrays;

public class KMPAlgorithm {
    
    /**
     * Build LPS (Longest Proper Prefix which is also Suffix) array
     * @param pattern The pattern string
     * @return LPS array
     */
    public static int[] buildLPS(String pattern) {
        int m = pattern.length();
        int[] lps = new int[m];
        
        int length = 0;  // Length of previous longest prefix suffix
        int i = 1;       // lps[0] is always 0
        
        lps[0] = 0;
        
        while (i < m) {
            if (pattern.charAt(i) == pattern.charAt(length)) {
                // Characters match - extend the length
                length++;
                lps[i] = length;
                i++;
            } else {
                // Characters don't match
                if (length != 0) {
                    // Try shorter prefix
                    // IMPORTANT: Don't increment i, retry with new length
                    length = lps[length - 1];
                } else {
                    // No prefix match at all
                    lps[i] = 0;
                    i++;
                }
            }
        }
        
        return lps;
    }
    
    /**
     * Search for all occurrences of pattern in text using KMP algorithm
     * @param text The text to search in
     * @param pattern The pattern to search for
     * @return List of starting indices where pattern is found
     */
    public static ArrayList<Integer> search(String text, String pattern) {
        ArrayList<Integer> result = new ArrayList<>();
        
        if (pattern.length() == 0 || text.length() == 0) {
            return result;
        }
        
        // Build LPS array
        int[] lps = buildLPS(pattern);
        
        int i = 0;  // Index for text
        int j = 0;  // Index for pattern
        
        while (i < text.length()) {
            if (text.charAt(i) == pattern.charAt(j)) {
                // Characters match
                i++;
                j++;
            } else {
                // Mismatch after j matches
                if (j != 0) {
                    // Use LPS array to skip characters
                    j = lps[j - 1];
                } else {
                    // j is 0, just move to next character in text
                    i++;
                }
            }
            
            // Pattern found
            if (j == pattern.length()) {
                result.add(i - j);  // Starting index of match
                j = lps[j - 1];     // Look for next match
            }
        }
        
        return result;
    }
    
    /**
     * Helper method to print results nicely
     */
    public static void printResults(String text, String pattern, 
                                   ArrayList<Integer> matches, int[] lps) {
        System.out.println("Text:    \"" + text + "\"");
        System.out.println("Pattern: \"" + pattern + "\"");
        System.out.println("LPS:     " + Arrays.toString(lps));
        System.out.println("Matches found at indices: " + matches);
        
        if (matches.isEmpty()) {
            System.out.println("No matches found!");
        } else {
            System.out.println("\nMatch details:");
            for (int idx : matches) {
                System.out.println("  Index " + idx + ": \"" + 
                    text.substring(idx, idx + pattern.length()) + "\"");
            }
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        // Test Case 1: Multiple matches
        String text1 = "ABABDABABCABAB";
        String pattern1 = "ABAB";
        int[] lps1 = buildLPS(pattern1);
        ArrayList<Integer> matches1 = search(text1, pattern1);
        printResults(text1, pattern1, matches1, lps1);
        
        // Test Case 2: Pattern with repetition
        String text2 = "AABAACAADAABAABA";
        String pattern2 = "AABA";
        int[] lps2 = buildLPS(pattern2);
        ArrayList<Integer> matches2 = search(text2, pattern2);
        printResults(text2, pattern2, matches2, lps2);
        
        // Test Case 3: No match
        String text3 = "ABCDEFGH";
        String pattern3 = "XYZ";
        int[] lps3 = buildLPS(pattern3);
        ArrayList<Integer> matches3 = search(text3, pattern3);
        printResults(text3, pattern3, matches3, lps3);
        
        // Test Case 4: Overlapping matches
        String text4 = "AAAAAAA";
        String pattern4 = "AAA";
        int[] lps4 = buildLPS(pattern4);
        ArrayList<Integer> matches4 = search(text4, pattern4);
        printResults(text4, pattern4, matches4, lps4);
    }
}
```

---

## Common Mistakes to Avoid

### ❌ Mistake 1: Accessing `lps[j-1]` when `j=0`

```java
// WRONG - Will cause ArrayIndexOutOfBoundsException!
else {
    j = lps[j - 1];  // What if j = 0?
}

// CORRECT
else {
    if (j != 0) {
        j = lps[j - 1];
    } else {
        i++;
    }
}
```

### ❌ Mistake 2: Incrementing `i` when doing LPS jump

```java
// WRONG - Don't increment i when jumping!
if (length != 0) {
    length = lps[length - 1];
    i++;  // ❌ Wrong! We need to retry same position
}

// CORRECT
if (length != 0) {
    length = lps[length - 1];
    // Don't increment i - use continue or while loop
}
```

### ❌ Mistake 3: Forgetting to set `lps[0] = 0`

```java
// Always set base case
lps[0] = 0;  // First position always 0
```

### ❌ Mistake 4: Not continuing search after finding match

```java
// WRONG - Stops after first match
if (j == pattern.length()) {
    result.add(i - j);
    break;  // ❌ Will miss other matches
}

// CORRECT - Continue searching
if (j == pattern.length()) {
    result.add(i - j);
    j = lps[j - 1];  // ✓ Continue searching
}
```

---

## Practice Examples

### Example 1: Build LPS for "AAACAAAA"

**Do it yourself first, then check:**

<details>
<summary>Click to see answer</summary>

```
Pattern: A A A C A A A A
Index:   0 1 2 3 4 5 6 7
LPS:     0 1 2 0 1 2 3 3

Explanation:
- lps[0] = 0 (base case)
- lps[1] = 1 ("AA" → "A" = "A")
- lps[2] = 2 ("AAA" → "AA" = "AA")
- lps[3] = 0 ("AAAC" → no match)
- lps[4] = 1 ("AAACA" → "A" = "A")
- lps[5] = 2 ("AAACAA" → "AA" = "AA")
- lps[6] = 3 ("AAACAAA" → "AAA" = "AAA")
- lps[7] = 3 ("AAACAAAA" → mismatch at 'C' vs 'A', jump to length=2, 
             then "AA" matches, but next is 'A', so extends to 3)
```
</details>

### Example 2: Find "ABA" in "ABABABA"

**Try to trace the algorithm:**

<details>
<summary>Click to see answer</summary>

```
Text:    A B A B A B A
Pattern: A B A
LPS:     0 0 1

Matches found at: [0, 2, 4]

Trace:
i=0,j=0: 'A'='A' ✓ → i=1,j=1
i=1,j=1: 'B'='B' ✓ → i=2,j=2
i=2,j=2: 'A'='A' ✓ → i=3,j=3 → Match at 0! j=lps[2]=1
i=3,j=1: 'B'='B' ✓ → i=4,j=2
i=4,j=2: 'A'='A' ✓ → i=5,j=3 → Match at 2! j=lps[2]=1
i=5,j=1: 'B'='B' ✓ → i=6,j=2
i=6,j=2: 'A'='A' ✓ → i=7,j=3 → Match at 4! j=lps[2]=1
i=7: end
```
</details>

### Example 3: Build LPS for "ABABCABAB"

**Your turn:**

<details>
<summary>Click to see answer</summary>

```
Pattern: A B A B C A B A B
Index:   0 1 2 3 4 5 6 7 8
LPS:     0 0 1 2 0 1 2 3 4

The last 4 characters "ABAB" match the first 4 characters "ABAB"!
```
</details>

---

## Quick Reference Card

### LPS Building Rules

| Condition | Action | Move i? |
|-----------|--------|---------|
| `pattern[i] == pattern[length]` | `length++; lps[i] = length; i++` | ✓ Yes |
| `pattern[i] != pattern[length]` AND `length != 0` | `length = lps[length-1]` | ✗ No (retry) |
| `pattern[i] != pattern[length]` AND `length == 0` | `lps[i] = 0; i++` | ✓ Yes |

### Search Rules

| Condition | Action | Move i? |
|-----------|--------|---------|
| `text[i] == pattern[j]` | `i++; j++` | ✓ Yes |
| `text[i] != pattern[j]` AND `j != 0` | `j = lps[j-1]` | ✗ No (retry) |
| `text[i] != pattern[j]` AND `j == 0` | `i++` | ✓ Yes |
| `j == pattern.length()` | Match found! `j = lps[j-1]` | Continue |

---

## Summary

### Key Takeaways

1. **LPS Array** tells you "how much can I reuse" when a mismatch occurs
2. **`length = lps[length-1]`** jumps to a shorter prefix that might match
3. **Never go backward in text** - only move forward in the text
4. **Time complexity O(n+m)** because each pointer only moves forward
5. **Check `j != 0`** before accessing `lps[j-1]` to avoid errors

### The Two Main Questions

When building LPS:
> "Does this prefix have a smaller repeating pattern inside it?"

When searching:
> "How much of what I matched can I reuse when there's a mismatch?"

---

## Resources

- [Original Paper by Knuth, Morris, and Pratt](https://epubs.siam.org/doi/10.1137/0206024)
- [Visualizer Tool](https://www.cs.usfca.edu/~galles/visualization/KMP.html)
- [Practice Problems on LeetCode](https://leetcode.com/tag/string-matching/)

---

**Created with ❤️ for better understanding of KMP Algorithm**

Last Updated: January 2026