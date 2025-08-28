# 2D Matrix Traversal - Complete Guide 🧭

A comprehensive guide to mastering 2D matrix traversal patterns with practical examples and solutions.

## 📋 Table of Contents

- [Overview](#overview)
- [Key Concepts](#key-concepts)
- [Common Traversal Patterns](#common-traversal-patterns)
- [Problem-Solving Framework](#problem-solving-framework)
- [Solved Examples](#solved-examples)
- [Practice Problems](#practice-problems)
- [Tips and Tricks](#tips-and-tricks)

## Overview

Matrix traversal is a fundamental skill in coding interviews and competitive programming. This guide covers essential patterns and provides a systematic approach to solve matrix problems efficiently.

## Key Concepts

### Matrix Basics
- **Dimensions**: `m × n` where `m` = rows, `n` = columns
- **Indexing**: `matrix[row][col]` where `0 ≤ row < m` and `0 ≤ col < n`
- **Total Elements**: `m * n`

### Coordinate System
```
(0,0) (0,1) (0,2) ... (0,n-1)
(1,0) (1,1) (1,2) ... (1,n-1)
...
(m-1,0) (m-1,1) ... (m-1,n-1)
```

## Common Traversal Patterns

### 1. Basic Traversals
- **Row-wise**: Left to right, top to bottom
- **Column-wise**: Top to bottom, left to right
- **Reverse Row-wise**: Right to left, bottom to top
- **Reverse Column-wise**: Bottom to top, right to left

### 2. Diagonal Traversals
- **Main Diagonal**: Elements where `row == col`
- **Anti-diagonal**: Elements where `row + col == n - 1`
- **All Diagonals**: Parallel diagonal lines

### 3. Special Patterns
- **Spiral**: Clockwise/counterclockwise traversal
- **Zigzag**: Alternating direction per row/column
- **Layer-wise**: Concentric rectangular layers

## Problem-Solving Framework

### Step 1: Understand the Pattern
1. Identify the traversal direction
2. Determine starting and ending points
3. Figure out the increment/decrement logic

### Step 2: Handle Boundaries
1. Check matrix bounds: `0 ≤ row < m` and `0 ≤ col < n`
2. Handle edge cases: empty matrix, single row/column
3. Consider wraparound or direction changes

### Step 3: Implement the Logic
1. Use appropriate loop structures
2. Track current position and direction
3. Store results in the required format

### Step 4: Optimize
1. Minimize space complexity when possible
2. Avoid unnecessary data structures
3. Consider in-place modifications

## Solved Examples

### Example 1: Diagonal Order Traversal

**Problem**: Given a matrix, return all elements in diagonal order.

```java
class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat[0].length;  // columns
        int n = mat.length;     // rows
        int[] ans = new int[m * n];
        int index = 0;
        ArrayList<Integer> curr = new ArrayList<>();
        
        // Traverse each diagonal
        for(int i = 0; i < m + n - 1; i++) {
            curr.clear();
            
            // Calculate starting position for diagonal i
            int r = i < m ? 0 : i - m + 1;
            int c = i < m ? i : m - 1;
            
            // Collect elements in current diagonal
            while(r < n && c >= 0) {
                curr.add(mat[r][c]);
                r += 1;
                c -= 1;
            }
            
            // Reverse every alternate diagonal
            if(i % 2 == 0) {
                Collections.reverse(curr);
            }
            
            // Add to result array
            for(int j = 0; j < curr.size(); j++) {
                ans[index++] = curr.get(j);
            }
        }
        return ans;
    }
}
```

**Key Insights**:
- Total diagonals = `m + n - 1`
- Starting position depends on diagonal index
- Alternate diagonals need reversal

### Example 2: Sort Matrix Diagonally

**Problem**: Sort each diagonal of the matrix independently.

```java
class Solution {
    public int[][] sortMatrix(int[][] grid) {
        int M = grid.length;
        int N = grid[0].length;
        ArrayList<Integer> curr = new ArrayList<>();
        
        for(int i = 0; i < M + N - 1; i++) {
            // Calculate starting position for diagonal i
            int r = i < M ? M - i - 1 : 0;
            int c = i >= M ? i - N + 1 : 0;
            
            curr.clear();
            
            // Collect diagonal elements
            while(r < M && c < N) {
                curr.add(grid[r][c]);
                r++;
                c++;
            }
            
            // Sort based on diagonal type
            if(i >= M) {
                Collections.sort(curr);  // Ascending
            } else {
                Collections.sort(curr);
                Collections.reverse(curr);  // Descending
            }
            
            // Put back sorted elements
            r = i < M ? M - i - 1 : 0;
            c = i >= M ? i - N + 1 : 0;
            int index = 0;
            
            while(r < M && c < N) {
                grid[r][c] = curr.get(index++);
                r++;
                c++;
            }
        }
        return grid;
    }
}
```

**Key Insights**:
- Each diagonal can be processed independently
- Starting position calculation is crucial
- In-place modification saves space

## Practice Problems

### Beginner Level
1. **Matrix Rotation**: Rotate matrix 90 degrees clockwise/counterclockwise
2. **Transpose Matrix**: Convert rows to columns
3. **Boundary Traversal**: Print matrix boundary elements

### Intermediate Level
1. **Spiral Matrix**: Traverse matrix in spiral order
2. **Zigzag Traversal**: Traverse in zigzag pattern
3. **Search in Sorted Matrix**: Find element in row/column sorted matrix

### Advanced Level
1. **Island Problems**: Count/find connected components
2. **Path Finding**: Find paths with specific constraints
3. **Dynamic Programming on Grid**: Minimum/maximum path problems

## Tips and Tricks

### 🎯 Pattern Recognition
- **Diagonal traversals**: Look for patterns where `row + col` or `row - col` is constant
- **Spiral patterns**: Use direction vectors and boundary tracking
- **Layer-wise processing**: Work from outside to inside or vice versa

### 🚀 Optimization Techniques
- **In-place operations**: Modify the original matrix when possible
- **Direction vectors**: Use arrays like `dx[] = {-1, 0, 1, 0}` for 4-directional movement
- **Boundary handling**: Check bounds before accessing elements

### 🐛 Common Pitfalls
- **Index out of bounds**: Always validate coordinates
- **Off-by-one errors**: Carefully handle loop boundaries
- **Direction changes**: Track current direction in spiral/zigzag patterns

### 📝 Code Templates

#### Basic Traversal Template
```java
for(int i = 0; i < rows; i++) {
    for(int j = 0; j < cols; j++) {
        // Process matrix[i][j]
    }
}
```

#### Diagonal Traversal Template
```java
// For diagonals parallel to main diagonal
for(int d = 0; d < rows + cols - 1; d++) {
    int startRow = d < cols ? 0 : d - cols + 1;
    int startCol = d < cols ? d : cols - 1;
    
    while(startRow < rows && startCol >= 0) {
        // Process matrix[startRow][startCol]
        startRow++;
        startCol--;
    }
}
```

#### Direction Vector Template
```java
int[] dx = {-1, 0, 1, 0};  // up, right, down, left
int[] dy = {0, 1, 0, -1};

for(int dir = 0; dir < 4; dir++) {
    int newX = x + dx[dir];
    int newY = y + dy[dir];
    
    if(isValid(newX, newY)) {
        // Process valid neighbor
    }
}
```

## Advanced Problem-Solving Strategies

### 🔍 Problem Classification Guide

#### 1. **Traversal Problems**
**When to use**: Need to visit elements in specific order
- Diagonal traversal, spiral traversal, zigzag patterns
- **Approach**: Identify pattern → Calculate positions → Handle boundaries

#### 2. **Search Problems** 
**When to use**: Finding elements or paths in matrix
- Binary search in sorted matrix, pathfinding, target sum
- **Approach**: Exploit sorted properties → Use appropriate search algorithm

#### 3. **Transformation Problems**
**When to use**: Modifying matrix structure or values
- Rotation, transpose, in-place modifications
- **Approach**: Identify transformation rule → Apply systematically

#### 4. **Dynamic Programming on Grid**
**When to use**: Optimization problems with overlapping subproblems
- Min/max path sum, unique paths, coin change on grid
- **Approach**: Define state → Identify recurrence → Optimize space

#### 5. **Graph Problems on Matrix**
**When to use**: Connectivity, components, shortest paths
- Islands, flood fill, shortest path in grid
- **Approach**: Treat cells as nodes → Apply graph algorithms

### 🧠 Step-by-Step Problem Analysis

#### Phase 1: Problem Understanding
```
1. What is the input format? (dimensions, constraints, data types)
2. What is the expected output? (modified matrix, array, single value)
3. Are there any special conditions? (sorted, binary, obstacles)
4. What are the edge cases? (empty matrix, single element, boundaries)
```

#### Phase 2: Pattern Recognition
```
1. Movement Pattern:
   - Linear (row/column wise)
   - Diagonal (main, anti, parallel diagonals)
   - Spiral (clockwise/counterclockwise)
   - Custom (L-shape, knight moves)

2. Processing Type:
   - Read-only traversal
   - In-place modification
   - Generate new structure
   - Accumulate results
```

#### Phase 3: Algorithm Selection
```
1. Simple Iteration: Basic row/column traversal
2. Two Pointers: Boundary manipulation, meeting in middle
3. Recursion/DFS: Tree-like exploration, backtracking
4. BFS: Level-by-level exploration, shortest paths
5. Dynamic Programming: Optimization problems
6. Divide & Conquer: Breaking into subproblems
```

### 🛠️ Common Techniques & Patterns

#### Technique 1: Direction Vectors
```java
// 4-directional movement
int[][] directions = {{-1,0}, {1,0}, {0,-1}, {0,1}}; // up, down, left, right

// 8-directional movement  
int[][] directions = {{-1,-1}, {-1,0}, {-1,1}, {0,-1}, {0,1}, {1,-1}, {1,0}, {1,1}};

// Custom movements (Knight moves)
int[][] knightMoves = {{-2,-1}, {-2,1}, {-1,-2}, {-1,2}, {1,-2}, {1,2}, {2,-1}, {2,1}};

// Usage
for(int[] dir : directions) {
    int newRow = row + dir[0];
    int newCol = col + dir[1];
    if(isValid(newRow, newCol)) {
        // Process neighbor
    }
}
```

#### Technique 2: Boundary Management
```java
// Spiral traversal boundaries
int top = 0, bottom = rows - 1;
int left = 0, right = cols - 1;

while(top <= bottom && left <= right) {
    // Traverse right
    for(int i = left; i <= right; i++) result.add(matrix[top][i]);
    top++;
    
    // Traverse down  
    for(int i = top; i <= bottom; i++) result.add(matrix[i][right]);
    right--;
    
    // Traverse left (if still valid)
    if(top <= bottom) {
        for(int i = right; i >= left; i--) result.add(matrix[bottom][i]);
        bottom--;
    }
    
    // Traverse up (if still valid)
    if(left <= right) {
        for(int i = bottom; i >= top; i--) result.add(matrix[i][left]);
        left++;
    }
}
```

#### Technique 3: Layer Processing
```java
// Process matrix layer by layer (for rotation, spiral, etc.)
int layers = Math.min(rows, cols) / 2;

for(int layer = 0; layer < layers; layer++) {
    int first = layer;
    int last = rows - 1 - layer;
    
    for(int i = first; i < last; i++) {
        int offset = i - first;
        
        // Save top element
        int top = matrix[first][i];
        
        // Left -> Top
        matrix[first][i] = matrix[last - offset][first];
        
        // Bottom -> Left  
        matrix[last - offset][first] = matrix[last][last - offset];
        
        // Right -> Bottom
        matrix[last][last - offset] = matrix[i][last];
        
        // Top -> Right
        matrix[i][last] = top;
    }
}
```

#### Technique 4: State Management for DP
```java
// 2D DP on grid - space optimized
int[] prev = new int[cols];
int[] curr = new int[cols];

for(int i = 0; i < rows; i++) {
    for(int j = 0; j < cols; j++) {
        if(i == 0 && j == 0) {
            curr[j] = matrix[i][j];
        } else if(i == 0) {
            curr[j] = curr[j-1] + matrix[i][j];
        } else if(j == 0) {
            curr[j] = prev[j] + matrix[i][j];
        } else {
            curr[j] = Math.min(prev[j], curr[j-1]) + matrix[i][j];
        }
    }
    // Swap arrays
    int[] temp = prev;
    prev = curr;
    curr = temp;
}
```

### 📊 Complexity Analysis Guide

#### Time Complexity Patterns
- **Simple Traversal**: O(m × n) - visit each cell once
- **Nested Operations**: O(m × n × k) - k operations per cell
- **Binary Search on Matrix**: O(log(m × n)) or O(m + n)
- **BFS/DFS**: O(m × n) for visiting + O(edges) for processing
- **Dynamic Programming**: O(m × n) with O(states per cell)

#### Space Complexity Optimization
- **In-place**: O(1) extra space - modify original matrix
- **Single Array**: O(min(m,n)) - use 1D array for 2D DP
- **Rolling Arrays**: O(n) - keep only necessary previous states
- **Recursive**: O(max(m,n)) - due to call stack depth

### 🎯 Interview Strategy

#### Before Coding
1. **Clarify requirements**: Ask about edge cases, constraints
2. **Trace through examples**: Walk through 2-3 test cases manually  
3. **Discuss approach**: Explain your strategy before coding
4. **Consider edge cases**: Empty matrix, single cell, boundaries

#### During Coding  
1. **Write helper functions**: `isValid()`, `getNeighbors()`, etc.
2. **Handle boundaries**: Check indices before accessing
3. **Use meaningful names**: `row`, `col` instead of `i`, `j`
4. **Comment complex logic**: Especially coordinate calculations

#### After Coding
1. **Trace through code**: Walk through with an example
2. **Check edge cases**: Test boundary conditions  
3. **Analyze complexity**: Time and space complexity
4. **Suggest optimizations**: Discuss alternative approaches

### 🔧 Debugging Checklist

#### Common Issues
- ✅ **Index bounds**: `0 ≤ row < m` and `0 ≤ col < n`
- ✅ **Direction vectors**: Correct dx/dy values
- ✅ **Loop conditions**: Off-by-one errors in boundaries
- ✅ **Coordinate calculations**: Starting positions for diagonals
- ✅ **State updates**: Proper state transitions in DP
- ✅ **Base cases**: Handle empty/single element matrices

#### Testing Strategy
```java
// Test with different matrix sizes
int[][] test1 = {{1}};                    // 1x1
int[][] test2 = {{1,2,3}};               // 1x3  
int[][] test3 = {{1},{2},{3}};           // 3x1
int[][] test4 = {{1,2},{3,4}};           // 2x2
int[][] test5 = {{1,2,3},{4,5,6},{7,8,9}}; // 3x3
```

### 📚 Problem Categories & Solutions

#### **Matrix Rotation & Transformation**
```java
// 90-degree clockwise rotation
public void rotate(int[][] matrix) {
    // Transpose
    for(int i = 0; i < matrix.length; i++) {
        for(int j = i; j < matrix[0].length; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    // Reverse rows
    for(int i = 0; i < matrix.length; i++) {
        reverse(matrix[i]);
    }
}
```

#### **Search in Sorted Matrix**
```java
// Search in row and column sorted matrix
public boolean searchMatrix(int[][] matrix, int target) {
    int row = 0, col = matrix[0].length - 1;
    
    while(row < matrix.length && col >= 0) {
        if(matrix[row][col] == target) return true;
        else if(matrix[row][col] > target) col--;
        else row++;
    }
    return false;
}
```

#### **Island Problems (Connected Components)**
```java
public int numIslands(char[][] grid) {
    int count = 0;
    for(int i = 0; i < grid.length; i++) {
        for(int j = 0; j < grid[0].length; j++) {
            if(grid[i][j] == '1') {
                dfs(grid, i, j);
                count++;
            }
        }
    }
    return count;
}

private void dfs(char[][] grid, int i, int j) {
    if(i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != '1') {
        return;
    }
    grid[i][j] = '0'; // Mark as visited
    dfs(grid, i+1, j);
    dfs(grid, i-1, j);  
    dfs(grid, i, j+1);
    dfs(grid, i, j-1);
}
```

#### **Dynamic Programming Paths**
```java
// Minimum path sum
public int minPathSum(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(i == 0 && j == 0) continue;
            else if(i == 0) grid[i][j] += grid[i][j-1];
            else if(j == 0) grid[i][j] += grid[i-1][j];
            else grid[i][j] += Math.min(grid[i-1][j], grid[i][j-1]);
        }
    }
    return grid[m-1][n-1];
}
```

## Contributing

Feel free to add more problems, solutions, or improvements to this guide. Matrix traversal patterns are fundamental to many algorithmic problems!

---

*Happy Coding! 🚀* 