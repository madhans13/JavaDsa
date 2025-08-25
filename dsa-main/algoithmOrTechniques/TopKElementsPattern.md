# 🏆 Top K Elements Pattern

## ✅ Problem Type
Find the **K largest, smallest, or most/least frequent** elements from a collection. Essential for ranking, recommendation systems, and statistical analysis.

## 🔧 When to Use
- Need **K best/worst** elements from a dataset
- **Frequency-based** problems (most/least frequent)
- **Ranking** or **leaderboard** type questions
- **Streaming data** where you need to maintain top K
- Problems asking for **Kth largest/smallest** element

## 🎯 Core Pattern Recognition

| Problem Description | Approach | Key Insight |
|-------------------|----------|-------------|
| "Top K Frequent..." | Count + Sort/Heap | Frequency mapping first |
| "K Largest Elements" | Min-Heap of size K | Keep smallest in heap, larger replaces |
| "Kth Largest Element" | QuickSelect/Heap | Single element, not K elements |
| "K Closest Points" | Distance + Heap | Custom distance function |
| "Top K in Stream" | Sliding Heap | Dynamic maintenance |

---

## 🔄 Two Main Approaches

### 1️⃣ Sorting Approach ⭐ (Simple & Reliable)
```java
// Time: O(n log n), Space: O(n)
public List<Type> topKWithSorting(Collection data, int k) {
    // 1. Process/Count data
    Map<Type, Integer> frequency = countFrequency(data);
    
    // 2. Convert to sortable list
    List<Map.Entry<Type, Integer>> entries = new ArrayList<>(frequency.entrySet());
    
    // 3. Custom sort
    entries.sort(comparator);
    
    // 4. Extract top K
    return entries.stream().limit(k).map(Map.Entry::getKey).collect(toList());
}
```

### 2️⃣ Heap Approach 🚀 (Memory Efficient)
```java
// Time: O(n log k), Space: O(k) - Better for large datasets with small K
public List<Type> topKWithHeap(Collection data, int k) {
    // Use Min-Heap to keep K largest elements
    PriorityQueue<Type> minHeap = new PriorityQueue<>(k, comparator);
    
    for (Type element : data) {
        if (minHeap.size() < k) {
            minHeap.offer(element);
        } else if (compare(element, minHeap.peek()) > 0) {
            minHeap.poll();
            minHeap.offer(element);
        }
    }
    
    return new ArrayList<>(minHeap);
}
```

---

## 📌 Example 1: Top K Frequent Words

### 🧱 Problem Statement
Given an array of strings `words` and an integer `k`, return the `k` most frequent strings. If frequencies are equal, return lexicographically smaller strings first.

### ✅ Sorting Solution (Your Code Enhanced)
```java
public List<String> topKFrequent(String[] words, int k) {
    // Step 1: Count word frequencies
    Map<String, Integer> freqMap = new HashMap<>();
    for (String word : words) {
        freqMap.put(word, freqMap.getOrDefault(word, 0) + 1);
    }
    
    // Step 2: Convert to list of entries for sorting
    List<Map.Entry<String, Integer>> entries = new ArrayList<>(freqMap.entrySet());
    
    // Step 3: Custom comparator - frequency desc, then lexicographical asc
    entries.sort((a, b) -> {
        if (!a.getValue().equals(b.getValue())) {
            return b.getValue() - a.getValue(); // Higher frequency first
        }
        return a.getKey().compareTo(b.getKey()); // Alphabetical order for ties
    });
    
    // Step 4: Extract top K words
    List<String> result = new ArrayList<>();
    for (int i = 0; i < Math.min(k, entries.size()); i++) {
        result.add(entries.get(i).getKey());
    }
    
    return result;
}
```

### 🚀 Heap Solution (Memory Optimized)
```java
public List<String> topKFrequentHeap(String[] words, int k) {
    // Step 1: Count frequencies
    Map<String, Integer> freqMap = new HashMap<>();
    for (String word : words) {
        freqMap.put(word, freqMap.getOrDefault(word, 0) + 1);
    }
    
    // Step 2: Min-heap with reverse logic
    // Keep k elements, smallest frequency at top
    PriorityQueue<String> minHeap = new PriorityQueue<>((a, b) -> {
        int freqCompare = freqMap.get(a) - freqMap.get(b);
        if (freqCompare != 0) return freqCompare;
        return b.compareTo(a); // Reverse lexicographical for min-heap
    });
    
    // Step 3: Maintain heap of size k
    for (String word : freqMap.keySet()) {
        minHeap.offer(word);
        if (minHeap.size() > k) {
            minHeap.poll(); // Remove least frequent
        }
    }
    
    // Step 4: Extract results in correct order
    List<String> result = new ArrayList<>();
    while (!minHeap.isEmpty()) {
        result.add(0, minHeap.poll()); // Add to front to reverse order
    }
    
    return result;
}
```

### 🧠 Key Insights
- **Frequency counting** is almost always the first step
- **Tie-breaking rules** are crucial - read problem carefully
- **Sorting** is simpler to implement and debug
- **Heap** is more efficient for small K values

---

## 📊 Performance Comparison

| Approach | Time Complexity | Space Complexity | Best When |
|----------|----------------|------------------|-----------|
| **Sorting** | O(n log n) | O(n) | K is large, simple logic needed |
| **Min-Heap** | O(n log k) | O(k) | K is small, memory constrained |
| **Max-Heap** | O(n log k) | O(k) | Finding K smallest elements |
| **QuickSelect** | O(n) average | O(1) | Single Kth element needed |

---

## 🎯 Problem Variations & Templates

### 1️⃣ K Largest Elements
```java
public int[] topKLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }
    
    return minHeap.stream().mapToInt(i -> i).toArray();
}
```

### 2️⃣ Kth Largest Element (Single Element)
```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }
    
    return minHeap.peek(); // Kth largest is at the top
}
```

### 3️⃣ Top K Frequent Numbers
```java
public int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> count = new HashMap<>();
    for (int num : nums) {
        count.put(num, count.getOrDefault(num, 0) + 1);
    }
    
    PriorityQueue<Integer> heap = new PriorityQueue<>(
        (a, b) -> count.get(a) - count.get(b)
    );
    
    for (int num : count.keySet()) {
        heap.offer(num);
        if (heap.size() > k) {
            heap.poll();
        }
    }
    
    return heap.stream().mapToInt(i -> i).toArray();
}
```

### 4️⃣ K Closest Points to Origin
```java
public int[][] kClosest(int[][] points, int k) {
    PriorityQueue<int[]> maxHeap = new PriorityQueue<>(
        (a, b) -> Integer.compare(
            b[0] * b[0] + b[1] * b[1],  // Distance squared for point b
            a[0] * a[0] + a[1] * a[1]   // Distance squared for point a
        )
    );
    
    for (int[] point : points) {
        maxHeap.offer(point);
        if (maxHeap.size() > k) {
            maxHeap.poll();
        }
    }
    
    return maxHeap.toArray(new int[k][]);
}
```

---

## 🔧 Implementation Decision Tree

```
Need Top K Elements?
├─ Single Kth element? → Use QuickSelect or Single-pass heap
├─ K << n (small K)? → Use Heap approach
├─ K ≈ n (large K)? → Use Sorting approach
├─ Streaming data? → Maintain sliding heap
├─ Frequency-based? → Count first, then apply Top K
├─ Custom sorting? → Define comparator carefully
└─ Memory critical? → Prefer heap over sorting
```

---

## 🧩 Common Patterns & Tips

### Comparator Design
```java
// Template for complex sorting
entries.sort((a, b) -> {
    // Primary criteria (most important)
    if (primaryDifference != 0) return primaryDifference;
    
    // Secondary criteria (tie breaker)
    return secondaryCriteria;
});
```

### Heap Direction Logic
```java
// For Top K Largest → Use Min-Heap (keep smallest at top, replace with larger)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// For Top K Smallest → Use Max-Heap (keep largest at top, replace with smaller) 
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

### Frequency Counting Pattern
```java
// Standard frequency map building
Map<T, Integer> freq = new HashMap<>();
for (T item : collection) {
    freq.put(item, freq.getOrDefault(item, 0) + 1);
}
```

---

## 🎪 Advanced Variations

### 1️⃣ Top K in Sliding Window
```java
public List<List<Integer>> topKInSlidingWindow(int[] nums, int k, int windowSize) {
    // Will be implemented with sliding window + heap
    // Coming in next update...
}
```

### 2️⃣ Top K with Custom Objects
```java
class Frequency {
    String word;
    int count;
    
    public Frequency(String word, int count) {
        this.word = word;
        this.count = count;
    }
}

// Use custom comparator with the class
```

### 3️⃣ Online Top K (Stream Processing)
```java
class TopKStream {
    PriorityQueue<Integer> minHeap;
    int k;
    
    public TopKStream(int k) {
        this.k = k;
        this.minHeap = new PriorityQueue<>();
    }
    
    public int add(int val) {
        minHeap.offer(val);
        if (minHeap.size() > k) {
            minHeap.poll();
        }
        return minHeap.peek(); // Kth largest so far
    }
}
```

---

## 📝 Debugging Checklist

- [ ] **Comparator logic** is correct (especially for tie-breaking)
- [ ] **Heap direction** matches the problem (min-heap vs max-heap)
- [ ] **Edge cases**: k > array length, k = 0, empty input
- [ ] **Result extraction** maintains correct order
- [ ] **Frequency counting** handles duplicates properly
- [ ] **Memory usage** is acceptable for given constraints

---

## 🚀 Next Patterns to Add
- [ ] Top K in Sliding Window
- [ ] Top K with Multiple Criteria
- [ ] Top K in 2D Arrays/Matrices
- [ ] Top K with Custom Distance Functions
- [ ] Distributed Top K (System Design)

---

*The Top K Elements pattern is fundamental in many real-world applications like search rankings, recommendation systems, and data analysis. Master the heap approach for optimal performance!*