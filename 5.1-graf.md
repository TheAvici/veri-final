-1. Temel Yapı (Komşuluk Listesi)
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <limits.h> // INT_MAX için

using namespace std;

// Kenar (Edge) yapısı - Dijkstra için ağırlık (weight) gerekli
struct Edge {
    int dest;   // Gidilen düğüm
    int weight; // Ağırlık (Maliyet)
};

// Graf Sınıfı
class Graph {
    int V; // Düğüm sayısı (Vertices)
    vector<vector<Edge>> adj; // Komşuluk listesi

public:
    Graph(int v) {
        V = v;
        adj.resize(V);
    }

    // Yönlü mü Yönsüz mü? (Soruya göre değiştir)
    // undirected = true ise yönsüz (A-B varsa B-A da var)
    void addEdge(int src, int dest, int weight = 1, bool undirected = true) {
        adj[src].push_back({dest, weight});
        if (undirected) {
            adj[dest].push_back({src, weight});
        }
    }
    
    // Getter
    int getV() { return V; }
    vector<vector<Edge>>& getAdj() { return adj; }
};
```

-2. Gezinme Algoritmaları (Traversal)

--A. BFS (Breadth-First Search) - Sığ Öncelikli Arama
```cpp
void BFS(Graph& g, int startNode) {
    int V = g.getV();
    vector<bool> visited(V, false);
    queue<int> q;

    // Başlangıç düğümünü ekle
    visited[startNode] = true;
    q.push(startNode);

    cout << "BFS Dolasimi: ";

    while (!q.empty()) {
        int u = q.front();
        q.pop();
        cout << u << " ";

        // Komşuları gez
        for (auto& edge : g.getAdj()[u]) {
            if (!visited[edge.dest]) {
                visited[edge.dest] = true;
                q.push(edge.dest);
            }
        }
    }
    cout << endl;
}
```
--B. DFS (Depth-First Search) - Derin Öncelikli Arama
```cpp
// Yardımcı rekürsif fonksiyon
void DFSUtil(int u, vector<bool>& visited, vector<vector<Edge>>& adj) {
    visited[u] = true;
    cout << u << " ";

    for (auto& edge : adj[u]) {
        if (!visited[edge.dest]) {
            DFSUtil(edge.dest, visited, adj);
        }
    }
}

// Ana çağırma fonksiyonu
void DFS(Graph& g, int startNode) {
    vector<bool> visited(g.getV(), false);
    cout << "DFS Dolasimi: ";
    DFSUtil(startNode, visited, g.getAdj());
    cout << endl;
}
```

-3. Dijkstra Algoritması (En Kısa Yol)

"A'dan diğer tüm düğümlere en kısa mesafeyi bulun"
```cpp
typedef pair<int, int> pii; // {mesafe, düğüm_id}

void Dijkstra(Graph& g, int src) {
    int V = g.getV();
    vector<int> dist(V, INT_MAX); // Mesafeleri sonsuz yap
    
    // Min-Heap (En küçük mesafe tepede)
    priority_queue<pii, vector<pii>, greater<pii>> pq;

    // Başlangıç ayarları
    dist[src] = 0;
    pq.push({0, src}); // {mesafe, düğüm}

    cout << "\n--- Dijkstra Trace ---" << endl;
    
    while (!pq.empty()) {
        int u = pq.top().second;
        int d = pq.top().first;
        pq.pop();

        // Daha kısa bir yol zaten bulunduysa bunu atla
        if (d > dist[u]) continue;

        // Komşuları kontrol et (Relaxation)
        for (auto& edge : g.getAdj()[u]) {
            int v = edge.dest;
            int weight = edge.weight;

            // Eğer u üzerinden v'ye gitmek daha kısaysa
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
                
                // Trace (Hoca adım adım görmek isteyebilir)
                cout << "Guncelle: " << v << " dugumunun yeni mesafesi: " << dist[v] 
                     << " (Gelinen: " << u << ")" << endl;
            }
        }
    }

    // Sonuçları Yazdır
    cout << "\nDugum \t Mesafe" << endl;
    for (int i = 0; i < V; i++) {
        cout << i << " \t " << (dist[i] == INT_MAX ? -1 : dist[i]) << endl;
    }
}
```
-4. Bağlantılı Bileşen Sayısı (Connected Components)
```cpp
	int countConnectedComponents(Graph& g) {
    int V = g.getV();
    vector<bool> visited(V, false);
    int count = 0;

    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            // Ziyaret edilmemiş bir düğüm bulduk, yeni bir bileşen demektir.
            // Bu bileşenin tamamını gezmek için DFS/BFS çağırıyoruz.
            DFSUtil(i, visited, g.getAdj()); 
            count++;
            cout << " -> Bilesen " << count << " bulundu." << endl;
        }
    }
    return count;
}
```

-5. Döngü Tespiti (Cycle Detection)
```cpp
bool isCyclicUtil(int u, vector<bool>& visited, int parent, vector<vector<Edge>>& adj) {
    visited[u] = true;

    for (auto& edge : adj[u]) {
        int v = edge.dest;

        // Ziyaret edilmemişse ilerle
        if (!visited[v]) {
            if (isCyclicUtil(v, visited, u, adj))
                return true;
        }
        // Ziyaret edilmiş VE ebeveynimiz DEĞİLSE -> Döngü vardır!
        else if (v != parent) {
            return true;
        }
    }
    return false;
}

bool isCyclic(Graph& g) {
    vector<bool> visited(g.getV(), false);
    for (int i = 0; i < g.getV(); i++) {
        if (!visited[i]) {
            if (isCyclicUtil(i, visited, -1, g.getAdj()))
                return true;
        }
    }
    return false;
}

```

