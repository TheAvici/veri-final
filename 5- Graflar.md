1. Temel Graf Yapısı ve Gezinme (BFS & DFS)

Bu bölüm sunumdaki temel gezinme algoritmalarını içerir.

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <list>
#include <map>
#include <algorithm>

using namespace std;

// Kenar (Edge) yapısı: Ağırlıklı graflar için hedef ve ağırlık tutar
struct Edge {
    int dest;
    int weight;
};

class Graph {
    int V; // Düğüm sayısı
    // Komşuluk Listesi: Her düğüm için bir Edge vektörü
    vector<vector<Edge>> adj; 
    bool isDirected; // Yönlü mü yönsüz mü?

public:
    Graph(int V, bool directed = false) : V(V), isDirected(directed) {
        adj.resize(V);
    }

    // Kenar Ekleme Fonksiyonu [cite: 2350, 2382]
    void addEdge(int u, int v, int weight = 1) {
        adj[u].push_back({v, weight});
        if (!isDirected) {
            adj[v].push_back({u, weight}); // Yönsüz ise tersini de ekle
        }
    }

    // --- 1. Genişlik Öncelikli Arama (BFS) ---
    // Sunumda Queue kullanılarak anlatılan yapı [cite: 1358, 2341]
    void BFS(int startNode) {
        vector<bool> visited(V, false);
        queue<int> q;

        visited[startNode] = true;
        q.push(startNode);

        cout << "BFS Dolasimi: ";
        while(!q.empty()) {
            int u = q.front();
            q.pop();
            cout << u << " ";

            for (auto& edge : adj[u]) {
                if (!visited[edge.dest]) {
                    visited[edge.dest] = true;
                    q.push(edge.dest);
                }
            }
        }
        cout << endl;
    }

    // --- 2. Derinlik Öncelikli Arama (DFS - Recursive) ---
    // Sunumda Stack veya Recursion ile anlatılan yapı [cite: 1463]
    void DFSHelper(int v, vector<bool>& visited) {
        visited[v] = true;
        cout << v << " ";

        for (auto& edge : adj[v]) {
            if (!visited[edge.dest]) {
                DFSHelper(edge.dest, visited);
            }
        }
    }

    void DFS(int startNode) {
        vector<bool> visited(V, false);
        cout << "DFS Dolasimi (Recursive): ";
        DFSHelper(startNode, visited);
        cout << endl;
    }
};
```

2. En Kısa Yol Algoritmaları (Shortest Path)

Sunumda Dijkstra detaylı işlenmiş ancak Bellman-Ford sadece isim olarak geçmiştir.

Negatif ağırlıklı kenarlar için Bellman-Ford gereklidir.

```cpp
// --- 3. Dijkstra Algoritması (Greedy) ---
// Pozitif ağırlıklı graflarda en kısa yolu bulur. Priority Queue kullanır[cite: 1596, 2429].
void Dijkstra(int src, int V, const vector<vector<Edge>>& adj) {
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    vector<int> dist(V, 1e9); // Sonsuz uzaklık

    dist[src] = 0;
    pq.push({0, src}); // {mesafe, düğüm}

    while (!pq.empty()) {
        int u = pq.top().second;
        int d = pq.top().first;
        pq.pop();

        if (d > dist[u]) continue;

        for (auto& edge : adj[u]) {
            int v = edge.dest;
            int weight = edge.weight;

            // Relaxation (Gevşetme) Adımı [cite: 1601]
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }

    cout << "Dijkstra Sonuclari (Kaynak: " << src << "):\n";
    for (int i = 0; i < V; ++i)
        cout << i << " -> " << (dist[i] == 1e9 ? -1 : dist[i]) << endl;
}

// --- 4. Bellman-Ford Algoritması (Zor/Ekstra) ---
// Sunumda "Negatif kenarlar varsa Dijkstra çalışmaz, Bellman-Ford gerekir" denilen kısım.
// O(V*E) karmaşıklığındadır.
void BellmanFord(int src, int V, const vector<vector<Edge>>& adj) {
    vector<int> dist(V, 1e9);
    dist[src] = 0;

    // V-1 kez tüm kenarları gevşet
    for (int i = 1; i <= V - 1; i++) {
        for (int u = 0; u < V; u++) {
            for (auto& edge : adj[u]) {
                int v = edge.dest;
                int weight = edge.weight;
                if (dist[u] != 1e9 && dist[u] + weight < dist[v])
                    dist[v] = dist[u] + weight;
            }
        }
    }

    // Negatif Döngü Kontrolü
    for (int u = 0; u < V; u++) {
        for (auto& edge : adj[u]) {
            if (dist[u] != 1e9 && dist[u] + edge.weight < edge.dest) {
                cout << "Hata: Grafik negatif ağırlıklı döngü içeriyor!" << endl;
                return;
            }
        }
    }
    
    cout << "Bellman-Ford Sonuclari:\n";
    for (int i = 0; i < V; ++i) cout << i << " -> " << dist[i] << endl;
}
```

3. Minimum Kapsayan Ağaç (MST) Algoritmaları


```cpp
// --- 5. Prim Algoritması ---
// Dijkstra'ya çok benzer, ağaca en yakın düğümü seçer[cite: 1802].
void PrimMST(int V, const vector<vector<Edge>>& adj) {
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    vector<int> key(V, 1e9);
    vector<int> parent(V, -1);
    vector<bool> inMST(V, false);

    int src = 0; // 0'dan başla
    pq.push({0, src});
    key[src] = 0;

    int mstWeight = 0;

    while (!pq.empty()) {
        int u = pq.top().second;
        pq.pop();

        if(inMST[u]) continue;
        inMST[u] = true;
        mstWeight += key[u];

        for (auto& edge : adj[u]) {
            int v = edge.dest;
            int weight = edge.weight;
            if (!inMST[v] && weight < key[v]) {
                key[v] = weight;
                pq.push({key[v], v});
                parent[v] = u;
            }
        }
    }
    cout << "Prim MST Toplam Agirlik: " << mstWeight << endl;
}

// --- 6. Kruskal Algoritması (Zor/Ekstra) ---
// Sunumda Disjoint Set / Union-Find kullanıldığı belirtilmiş[cite: 2088].
// İşte o yapı:

struct DSU {
    vector<int> parent, rank;
    DSU(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        for(int i=0; i<n; i++) parent[i] = i;
    }
    // Find with Path Compression
    int find(int i) {
        if (parent[i] != i)
            parent[i] = find(parent[i]);
        return parent[i];
    }
    // Union by Rank
    void unite(int i, int j) {
        int root_i = find(i);
        int root_j = find(j);
        if (root_i != root_j) {
            if (rank[root_i] < rank[root_j]) parent[root_i] = root_j;
            else if (rank[root_i] > rank[root_j]) parent[root_j] = root_i;
            else { parent[root_i] = root_j; rank[root_j]++; }
        }
    }
};

struct EdgeTuple {
    int u, v, weight;
    bool operator<(const EdgeTuple& other) const {
        return weight < other.weight;
    }
};

void KruskalMST(int V, vector<EdgeTuple>& edges) {
    sort(edges.begin(), edges.end()); // Kenarları ağırlığa göre sırala [cite: 2086]
    DSU dsu(V);
    int mstWeight = 0;
    vector<EdgeTuple> result;

    for (auto& e : edges) {
        if (dsu.find(e.u) != dsu.find(e.v)) { // Döngü oluşturmuyor mu? [cite: 2088]
            dsu.unite(e.u, e.v);
            mstWeight += e.weight;
            result.push_back(e);
        }
    }

    cout << "Kruskal MST Toplam Agirlik: " << mstWeight << endl;
}

```





