# 🔁 Backtracking in Java — Guide & Examples

Backtracking is a powerful algorithmic technique used to solve problems incrementally by trying partial solutions and then abandoning them ("backtracking") if they do not lead to a valid complete solution.

This repository contains examples and explanations on how to approach **backtracking problems in Java**, especially focusing on **balanced parentheses generation**.

---

## 📌 What is Backtracking?

Backtracking is a **depth-first search (DFS)** approach used for problems that involve:
- Generating all combinations or permutations
- Solving constraint satisfaction problems
- Exploring decision trees

You explore all possible options, and whenever you hit a dead-end or invalid state, you **undo the last step (backtrack)** and try a different option.

---

## 🧠 General Backtracking Approach

1. **Identify the Decision Tree**  
   What choices do I have at each step?

2. **Define the Base Case**  
   When should I stop recursion? (e.g., a valid solution is found)

3. **Explore the Choices (Recursive Case)**  
   Try each option, recursively proceed, and backtrack after exploring.

4. **Backtrack (Undo the Move)**  
   Revert the change made before the recursive call.

---

## 🧪 Example: Generate Parentheses

### Problem Statement

Given `n` pairs of parentheses, generate all combinations of **well-formed parentheses**.

### Constraints:
- Never close more `)` than you’ve opened `(`
- Maximum `n` opening and closing brackets each

---

## ✅ Java Solution Using Backtracking

```java
class Solution {
    List<String> ans = new ArrayList<>();

    public List<String> generateParenthesis(int n) {
        generate(0, 0, n, new StringBuilder());
        return ans;
    }

    private void generate(int openC, int closeC, int n, StringBuilder sb) {
        if (openC == n && closeC == n) {
            ans.add(sb.toString());
            return;
        }

        if (openC < n) {
            sb.append('(');
            generate(openC + 1, closeC, n, sb);
            sb.deleteCharAt(sb.length() - 1); // backtrack
        }

        if (closeC < openC) {
            sb.append(')');
            generate(openC, closeC + 1, n, sb);
            sb.deleteCharAt(sb.length() - 1); // backtrack
        }
    }
}
