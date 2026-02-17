# 🗺️ Complete Roadmap – Shortest Path

This roadmap gives you a practical sequence to master shortest-path problems for interviews and contests.

---

## 🟢 Step 1: Basics (Must Know First)

### 1) BFS (Unweighted Graph)

✅ **Use when:**
- Graph is unweighted
- Or all edges have weight `1`

⏱ **Time:** `O(V + E)`

**Practice types:**
- Shortest path in unweighted graph
- Word Ladder pattern
- Grid shortest path (binary matrix)

### 2) DFS (Generally not for shortest path ❌)

- DFS does **not** guarantee shortest path
- Mostly used for path existence, traversal, and backtracking

---

## 🟡 Step 2: Weighted Graph Basics

### 3) Dijkstra’s Algorithm

✅ **Use when:**
- Graph has **non-negative** weights
- Need **single-source shortest path**

⏱ **Time:**
- With min-heap / priority queue: `O(E log V)`

**Core idea:**
- Greedy relaxation using a priority queue

**Practice types:**
- Network Delay Time
- Minimum cost to reach destination
- Cheapest Flights Within K Stops (state-based variation)

### 4) Bellman–Ford

✅ **Use when:**
- Graph can have **negative** weights
- Need to detect a **negative cycle**

⏱ **Time:** `O(V × E)`

**Practice types:**
- Negative cycle detection
- Currency exchange / arbitrage style problems
- Graphs with possible negative edges

---

## 🔵 Step 3: DAG Shortest Path

### 5) Shortest Path in DAG

✅ **Use when:**
- Graph is a **Directed Acyclic Graph (DAG)**

**Method:**
1. Topological sort
2. Relax edges in topological order

⏱ **Time:** `O(V + E)`

✅ Often faster than Dijkstra for DAGs.

---

## 🟣 Step 4: Advanced Patterns

### 6) Floyd–Warshall (All-Pairs Shortest Path)

✅ **Use when:**
- Need shortest path between **every pair** of nodes
- Graph size is small (typically `n ≤ 400`)

⏱ **Time:** `O(n³)`

### 7) 0-1 BFS

✅ **Use when:**
- Edge weights are only `0` or `1`

**Method:**
- Use a deque instead of a priority queue

⏱ **Time:** `O(V + E)`

🔥 Very common interview pattern.

### 8) Multi-Source BFS

✅ **Use when:**
- There are multiple starting points
- “Spread/rot/expansion” style problems

**Examples:**
- Rotten Oranges
- Walls and Gates

---

## 🔴 Step 5: Hard / Pattern-Based

### 9) K Stops / State-Based Dijkstra

Used in:
- Cheapest Flights Within K Stops
- Shortest path with constraints

**Need:**
- State in PQ like `(node, cost, stops_used)`

### 10) Shortest Path in Grid Variants

Variants include:
- Obstacles
- Weighted cells
- Directional cost
- Teleport/portal mechanics

Common techniques:
- BFS
- Dijkstra
- 0-1 BFS
- A* (advanced)

---

## 🧠 Decision Tree (Most Important)

When you see a shortest-path question, ask:

1. **Is graph weighted?**  
   - ❌ No → Use **BFS**  
   - ✅ Yes → Go next

2. **Any negative weight?**  
   - ❌ No → Use **Dijkstra**  
   - ✅ Yes → Use **Bellman–Ford**

3. **Is it a DAG?**  
   - ✅ Yes → Use **Topological sort + relax**  
   - ❌ No → Continue

4. **Are weights only 0 or 1?**  
   - ✅ Yes → Use **0-1 BFS**

5. **Need all-pairs shortest paths?**  
   - ✅ Yes → Use **Floyd–Warshall**
