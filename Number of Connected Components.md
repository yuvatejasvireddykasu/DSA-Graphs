# 🧩 Number of Connected Components

This note covers how to count connected components in both:

1. **General Graph (Adjacency List)**
2. **Grid / Matrix Problems**

We will use:
- BFS
- DFS
- Union-Find (Disjoint Set Union)

---

# 1️⃣ Core Idea

A connected component is a maximal set of nodes where each node is reachable from any other node in that set.

To count components:
- Start from every unvisited node.
- Run a traversal (BFS/DFS) to mark everything in that component.
- Increase answer by 1 for each new traversal start.

---

# 2️⃣ Graph Version (Undirected) — DFS

## 🔹 Intuition

- If node `i` is unvisited, it belongs to a new component.
- Run DFS from `i`, mark all reachable nodes.
- Repeat for all nodes.

## 💻 Code (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

void dfs(int node, vector<int> adj[], vector<int>& vis) {
    vis[node] = 1;
    for (int nbr : adj[node]) {
        if (!vis[nbr]) dfs(nbr, adj, vis);
    }
}

int countComponents(int V, vector<int> adj[]) {
    vector<int> vis(V, 0);
    int components = 0;

    for (int i = 0; i < V; i++) {
        if (!vis[i]) {
            components++;
            dfs(i, adj, vis);
        }
    }

    return components;
}
```

---

# 3️⃣ Graph Version (Undirected) — BFS

## 🔹 Intuition

Same logic as DFS, but explore layer by layer using a queue.

## 💻 Code (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

void bfs(int start, vector<int> adj[], vector<int>& vis) {
    queue<int> q;
    q.push(start);
    vis[start] = 1;

    while (!q.empty()) {
        int node = q.front();
        q.pop();

        for (int nbr : adj[node]) {
            if (!vis[nbr]) {
                vis[nbr] = 1;
                q.push(nbr);
            }
        }
    }
}

int countComponents(int V, vector<int> adj[]) {
    vector<int> vis(V, 0);
    int components = 0;

    for (int i = 0; i < V; i++) {
        if (!vis[i]) {
            components++;
            bfs(i, adj, vis);
        }
    }

    return components;
}
```

---

# 4️⃣ Grid Version ("Number of Islands" pattern) — DFS

## 🔹 Intuition

Treat each land cell as a node.
- `grid[r][c] == '1'` means land.
- 4-direction movement: up, right, down, left.
- Every DFS/BFS start from an unvisited land cell adds one island/component.

## 💻 Code (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

void dfs(int r, int c, vector<vector<char>>& grid, vector<vector<int>>& vis) {
    int n = grid.size(), m = grid[0].size();
    vis[r][c] = 1;

    int dr[] = {-1, 0, 1, 0};
    int dc[] = {0, 1, 0, -1};

    for (int k = 0; k < 4; k++) {
        int nr = r + dr[k], nc = c + dc[k];

        if (nr >= 0 && nr < n && nc >= 0 && nc < m &&
            !vis[nr][nc] && grid[nr][nc] == '1') {
            dfs(nr, nc, grid, vis);
        }
    }
}

int numIslands(vector<vector<char>>& grid) {
    int n = grid.size(), m = grid[0].size();
    vector<vector<int>> vis(n, vector<int>(m, 0));
    int islands = 0;

    for (int r = 0; r < n; r++) {
        for (int c = 0; c < m; c++) {
            if (!vis[r][c] && grid[r][c] == '1') {
                islands++;
                dfs(r, c, grid, vis);
            }
        }
    }

    return islands;
}
```

---

# 5️⃣ Graph Version — Union-Find (DSU)

Useful when edges are given directly (like LeetCode 323).

## 🔹 Intuition

- Initially every node is its own parent (`V` components).
- For each edge `(u, v)`, union their sets.
- If union succeeds, components decrease by 1.

## 💻 Code (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct DSU {
    vector<int> parent, size;

    DSU(int n) {
        parent.resize(n);
        size.assign(n, 1);
        iota(parent.begin(), parent.end(), 0);
    }

    int findUPar(int x) {
        if (x == parent[x]) return x;
        return parent[x] = findUPar(parent[x]);
    }

    bool unite(int a, int b) {
        a = findUPar(a);
        b = findUPar(b);
        if (a == b) return false;

        if (size[a] < size[b]) swap(a, b);
        parent[b] = a;
        size[a] += size[b];
        return true;
    }
};

int countComponents(int n, vector<vector<int>>& edges) {
    DSU dsu(n);
    int components = n;

    for (auto &e : edges) {
        if (dsu.unite(e[0], e[1])) components--;
    }

    return components;
}
```

---

# 6️⃣ Complexity

For DFS/BFS (adjacency list):
- Time: **O(V + E)**
- Space: **O(V)** (visited + recursion stack / queue)

For grid traversal:
- Time: **O(N × M)**
- Space: **O(N × M)**

For DSU:
- Time: **O(E · α(V))** (almost linear)
- Space: **O(V)**

---

# 7️⃣ LeetCode Variations (Connected Components Family)

## Core Component Counting

- Number of Connected Components in an Undirected Graph (Premium):
  https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/
- Number of Provinces:
  https://leetcode.com/problems/number-of-provinces/

## Grid / Island Variants

- Number of Islands:
  https://leetcode.com/problems/number-of-islands/
- Max Area of Island:
  https://leetcode.com/problems/max-area-of-island/
- Number of Closed Islands:
  https://leetcode.com/problems/number-of-closed-islands/
- Number of Enclaves:
  https://leetcode.com/problems/number-of-enclaves/
- Surrounded Regions:
  https://leetcode.com/problems/surrounded-regions/
- Island Perimeter:
  https://leetcode.com/problems/island-perimeter/
- Count Sub Islands:
  https://leetcode.com/problems/count-sub-islands/
- Number of Distinct Islands (Premium):
  https://leetcode.com/problems/number-of-distinct-islands/
- Number of Islands II (Premium, DSU):
  https://leetcode.com/problems/number-of-islands-ii/

## DSU / Connectivity Query Variants

- Redundant Connection:
  https://leetcode.com/problems/redundant-connection/
- Accounts Merge:
  https://leetcode.com/problems/accounts-merge/
- Most Stones Removed with Same Row or Column:
  https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/
- Satisfiability of Equality Equations:
  https://leetcode.com/problems/satisfiability-of-equality-equations/
- Number of Operations to Make Network Connected:
  https://leetcode.com/problems/number-of-operations-to-make-network-connected/
- Graph Valid Tree (Premium):
  https://leetcode.com/problems/graph-valid-tree/

---

# 🎯 Interview One-Liner

> Counting connected components means: “How many times do I need to start DFS/BFS from an unvisited node to cover all nodes?”
