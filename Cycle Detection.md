# 🔁 Cycle Detection in Graphs

This file covers all 4 major cycle detection patterns:

1. Undirected Graph – BFS  
2. Undirected Graph – DFS  
3. Directed Graph – DFS  
4. Directed Graph – BFS (Kahn’s Algorithm)  

---

# 1️⃣ Undirected Graph – Using DFS

## 🔹 Intuition

In an undirected graph, if during DFS we visit a node that is already visited
and it is NOT the parent, then a cycle exists.

We ignore the parent because edges are bidirectional.

---

## 🖼 Dry Run Example

Graph:
0 — 1  
|    |  
3 — 2  

Cycle: 0 → 1 → 2 → 3 → 0

---

## 💻 Code

```cpp
#include <bits/stdc++.h>
using namespace std;

bool dfs(int node, int parent, vector<int> adj[], vector<int>& vis){
    vis[node] = 1;
    for(auto it : adj[node]){
        if(!vis[it]){
            if(dfs(it, node, adj, vis)) return true;
        }
        else if(it != parent) return true;
    }
    return false;
}

bool isCycle(int V, vector<int> adj[]){
    vector<int> vis(V, 0);
    for(int i = 0; i < V; i++){
        if(!vis[i]){
            if(dfs(i, -1, adj, vis)) return true;
        }
    }
    return false;
}
```

---

# 2️⃣ Undirected Graph – Using BFS

## 🔹 Intuition

Store (node, parent) in queue.
If we encounter a visited neighbor that is not the parent → cycle exists.

---

## 🖼 Dry Run Example

Same graph as above.
While processing node 2, we see neighbor 0 already visited and not parent.
Cycle detected.

---

## 💻 Code

```cpp
#include <bits/stdc++.h>
using namespace std;

bool isCycle(int V, vector<int> adj[]){
    vector<int> vis(V, 0);

    for(int i = 0; i < V; i++){
        if(!vis[i]){
            queue<pair<int,int>> q;
            q.push({i, -1});
            vis[i] = 1;

            while(!q.empty()){
                int node = q.front().first;
                int parent = q.front().second;
                q.pop();

                for(auto it : adj[node]){
                    if(!vis[it]){
                        vis[it] = 1;
                        q.push({it, node});
                    }
                    else if(it != parent) return true;
                }
            }
        }
    }
    return false;
}
```

---

# 3️⃣ Directed Graph – Using DFS

## 🔹 Intuition

If during DFS we reach a node that is already in the current recursion stack,
then there is a back edge → cycle exists.

We maintain:
- vis[]
- pathVis[] (recursion stack)

---

## 🖼 Dry Run Example

0 → 1 → 2 → 3  
        ↑     ↓  
        └─────┘  

At node 3, we see node 1 already in recursion stack.
Cycle detected.

---

## 💻 Code

```cpp
#include <bits/stdc++.h>
using namespace std;

bool dfs(int node, vector<int> adj[], vector<int>& vis, vector<int>& pathVis){
    vis[node] = 1;
    pathVis[node] = 1;

    for(auto it : adj[node]){
        if(!vis[it]){
            if(dfs(it, adj, vis, pathVis)) return true;
        }
        else if(pathVis[it]) return true;
    }

    pathVis[node] = 0;
    return false;
}

bool isCycle(int V, vector<int> adj[]){
    vector<int> vis(V, 0), pathVis(V, 0);

    for(int i = 0; i < V; i++){
        if(!vis[i]){
            if(dfs(i, adj, vis, pathVis)) return true;
        }
    }
    return false;
}
```

---

# 4️⃣ Directed Graph – Using BFS (Kahn’s Algorithm)

## 🔹 Intuition

If topological sorting does not process all nodes,
then a cycle exists.

Why?
Because cycle blocks zero indegree formation.

---

## 🖼 Dry Run Example

0 → 1 → 2 → 3  
        ↑     ↓  
        └─────┘  

No node gets indegree 0 after processing.
Processed count < V → cycle exists.

---

## 💻 Code

```cpp
#include <bits/stdc++.h>
using namespace std;

bool isCycle(int V, vector<int> adj[]){
    vector<int> indegree(V, 0);

    for(int i = 0; i < V; i++){
        for(auto it : adj[i]){
            indegree[it]++;
        }
    }

    queue<int> q;
    for(int i = 0; i < V; i++){
        if(indegree[i] == 0) q.push(i);
    }

    int count = 0;

    while(!q.empty()){
        int node = q.front();
        q.pop();
        count++;

        for(auto it : adj[node]){
            indegree[it]--;
            if(indegree[it] == 0) q.push(it);
        }
    }

    return count != V;
}
```

---

# 🎯 Final Interview Summary

| Graph Type | Method | Detection Condition |
|------------|--------|--------------------|
| Undirected | DFS | Visited ≠ Parent |
| Undirected | BFS | Visited ≠ Parent |
| Directed | DFS | Node in recursion stack |
| Directed | BFS | Topo count < V |

---

# ⏱ Time Complexity

All four methods run in:

O(V + E)

