1. Min-Heap Kodu

```cpp
// Kaynak: Sunum 9-10, Sayfa 37-40
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class MinHeap {
public:
    vector<int> heap;

    // Ekleme İşlemi (Insert) - Percolate Up
    void insert(int val) {
        heap.push_back(val);
        int index = heap.size() - 1;
        // Yukarı Sızdırma (Percolate Up): Çocuk ebeveyninden küçükse yer değiştir
        while (index > 0 && heap[(index - 1) / 2] > heap[index]) {
            swap(heap[index], heap[(index - 1) / 2]);
            index = (index - 1) / 2;
        }
    }

    // En Küçüğü Silme (DeleteMin) - Percolate Down
    void deleteMin() {
        if (heap.empty()) return;
        
        // Kökü silip son elemanı köke taşıyoruz
        heap[0] = heap.back();
        heap.pop_back();

        int index = 0;
        int size = heap.size();

        // Aşağı Sızdırma (Percolate Down)
        while (true) {
            int leftChild = 2 * index + 1;
            int rightChild = 2 * index + 2;
            int smallest = index;

            if (leftChild < size && heap[leftChild] < heap[smallest])
                smallest = leftChild;
            if (rightChild < size && heap[rightChild] < heap[smallest])
                smallest = rightChild;

            if (smallest == index) break;

            swap(heap[index], heap[smallest]);
            index = smallest;
        }
    }

    void printHeap() {
        cout << "[ ";
        for (int i : heap) cout << i << " ";
        cout << "]" << endl;
    }
};

int main() {
    MinHeap h;
    vector<int> inputs = {15, 8, 24, 3, 10};

    cout << "--- EKLEME ADIMLARI ---" << endl;
    for (int val : inputs) {
        h.insert(val);
        cout << "Insert(" << val << ") -> ";
        h.printHeap();
    }

    cout << "\n--- SILME ADIMI ---" << endl;
    cout << "DeleteMin() -> ";
    h.deleteMin();
    h.printHeap();

    return 0;
}
```

2. Max-Heap Kodu

```cpp
// Standart Max-Heap Implementasyonu
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class MaxHeap {
public:
    vector<int> heap;

    // Ekleme: Yeni eleman ekle ve yukarı taşı (Percolate Up)
    void insert(int val) {
        heap.push_back(val);
        int index = heap.size() - 1;
        
        // FARK: Çocuk ebeveyninden BÜYÜKSE yer değiştir (Max-Heap kuralı)
        while (index > 0 && heap[(index - 1) / 2] < heap[index]) {
            swap(heap[index], heap[(index - 1) / 2]);
            index = (index - 1) / 2;
        }
    }

    // En Büyüğü Silme: Kökü sil ve aşağı taşı (Percolate Down)
    void deleteMax() {
        if (heap.empty()) return;

        heap[0] = heap.back();
        heap.pop_back();

        int index = 0;
        int size = heap.size();

        while (true) {
            int leftChild = 2 * index + 1;
            int rightChild = 2 * index + 2;
            int largest = index;

            // FARK: Çocuk ebeveynden BÜYÜKSE index'i güncelle
            if (leftChild < size && heap[leftChild] > heap[largest])
                largest = leftChild;
            if (rightChild < size && heap[rightChild] > heap[largest])
                largest = rightChild;

            if (largest == index) break;

            swap(heap[index], heap[largest]);
            index = largest;
        }
    }

    void printHeap() {
        cout << "[ ";
        for (int i : heap) cout << i << " ";
        cout << "]" << endl;
    }
};

int main() {
    MaxHeap h;
    // MinHeap ile aynı verileri ekleyelim farkı görelim
    vector<int> inputs = {15, 8, 24, 3, 10};

    cout << "--- MAX HEAP EKLEME ---" << endl;
    for (int val : inputs) {
        h.insert(val);
        cout << "Insert(" << val << ") -> ";
        h.printHeap();
    }

    cout << "\n--- MAX HEAP SILME ---" << endl;
    cout << "DeleteMax() -> ";
    h.deleteMax(); // En büyük eleman (24) silinmeli
    h.printHeap();

    return 0;
}
```






