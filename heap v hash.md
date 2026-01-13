1. HEAP (Yığın Ağacı) - Dizi Üzerinde
Heap sorularında ağaç çizmek kolaydır ama kod yazarken indeks formüllerini bilmen gerekir.

Formüller (Ezberle):

-i düğümünün Sol Çocuğu: 2*i + 1
-i düğümünün Sağ Çocuğu: 2*i + 2
-i düğümünün Ebeveyni: (i - 1) / 2

Tam Teşekküllü Max-Heap Kodu (C++)
Bu kod, bir diziyi Max-Heap kuralına göre düzenler. (Kök en büyük olur).
Eğer Min-Heap istenirse büyüktür (>) işaretlerini küçüktür (<) yapman yeterli.

#include <iostream>
#include <vector>
#include <algorithm> // swap için

using namespace std;

// MAX-HEAP Sınıfı
class MaxHeap {
    vector<int> heap;

    // Ebeveyn ve Çocuk İndeksleri
    int parent(int i) { return (i - 1) / 2; }
    int left(int i) { return (2 * i + 1); }
    int right(int i) { return (2 * i + 2); }

    // Yukarı Taşıma (Ekleme sonrası düzenleme)
    void heapifyUp(int i) {
        // Kök değilse VE ebeveyninden büyükse takas et
        while (i > 0 && heap[parent(i)] < heap[i]) {
            swap(heap[parent(i)], heap[i]);
            i = parent(i); // Yukarı çıkmaya devam et
        }
    }

    // Aşağı Taşıma (Silme sonrası düzenleme)
    void heapifyDown(int i) {
        int maxIndex = i;
        int l = left(i);
        int r = right(i);
        int n = heap.size();

        // Sol çocuk var ve current'tan büyükse
        if (l < n && heap[l] > heap[maxIndex])
            maxIndex = l;

        // Sağ çocuk var ve şu ana kadarki en büyükten de büyükse
        if (r < n && heap[r] > heap[maxIndex])
            maxIndex = r;

        // Eğer en büyük kendisi değilse, takas et ve aşağı in
        if (i != maxIndex) {
            swap(heap[i], heap[maxIndex]);
            heapifyDown(maxIndex);
        }
    }

public:
    // Eleman Ekleme
    void insert(int val) {
        heap.push_back(val); // Sona ekle
        heapifyUp(heap.size() - 1); // Yukarı taşı
    }

    // En Büyüğü (Kökü) Silme
    void extractMax() {
        if (heap.size() == 0) return;

        // 1. Kök ile son elemanı yer değiştir
        heap[0] = heap.back();
        // 2. Son elemanı (eski kökü) at
        heap.pop_back();
        // 3. Yeni kökü aşağı taşı (Heap kuralını sağla)
        heapifyDown(0);
    }
    
    // Yazdırma (Dizi olarak)
    void printHeap() {
        for (int val : heap) cout << val << " ";
        cout << endl;
    }
};


-2. HASH TABLE (Karma Tablo) - Çakışma Çözme
--A. Linear Probing (Doğrusal Arama) - Adım Adım Tablo Doldurma

#include <iostream>
#include <vector>

using namespace std;

const int TABLE_SIZE = 11; // Hoca genelde asal sayı verir (örn: 7, 11, 13)

void linearProbing(const vector<int>& data) {
    // Tabloyu -1 ile doldur (Boş anlamında)
    vector<int> table(TABLE_SIZE, -1);

    for (int key : data) {
        int index = key % TABLE_SIZE; // Hash Fonksiyonu: Mod Alma

        // Çakışma varsa (Doluysa) bir yana kay
        // Hoca buradaki döngüyü sorabilir!
        while (table[index] != -1) {
            cout << "Cakisman: " << key << " indeks " << index << "'e giremedi." << endl;
            index = (index + 1) % TABLE_SIZE; // Dairesel olarak sonrakine bak
        }
        
        table[index] = key; // Boş yeri buldu, yerleştir
        cout << "Yerlesti: " << key << " -> [" << index << "]" << endl;
    }

    // SONUCU YAZDIR
    cout << "\n--- Linear Probing Sonuc ---" << endl;
    for (int i = 0; i < TABLE_SIZE; i++) {
        if (table[i] != -1) cout << "[" << i << "]: " << table[i] << endl;
        else cout << "[" << i << "]: BOS" << endl;
    }
}

--B. Quadratic Probing (Karesel Arama)
void quadraticProbing(const vector<int>& data) {
    vector<int> table(TABLE_SIZE, -1);

    for (int key : data) {
        int hashVal = key % TABLE_SIZE;
        int i = 0;
        int index = hashVal;

        // Dolu olduğu sürece karesel artırarak bak
        while (table[index] != -1) {
            i++;
            // index = (İlkHash + i*i) % Boyut
            index = (hashVal + i * i) % TABLE_SIZE;
            
            // Sonsuz döngüden kaçınmak için basit bir koruma (Sınavda gerekmez ama iyi olur)
            if (i > TABLE_SIZE) { 
                cout << key << " yerlestirilemedi!" << endl; 
                break; 
            }
        }

        if (table[index] == -1) { // Boş yer bulunduysa
            table[index] = key;
        }
    }
    
    // Tabloyu yazdır (Yukarıdaki ile aynı)
    // ...
}

--C. Chaining (Zincirleme) - Linked List Kullanarak
#include <list>

class HashTableChaining {
    int size;
    list<int>* table; // Array of lists

public:
    HashTableChaining(int s) {
        size = s;
        table = new list<int>[size];
    }

    void insert(int key) {
        int index = key % size;
        table[index].push_back(key); // O indeksteki listenin sonuna ekle
    }

    void printTable() {
        for (int i = 0; i < size; i++) {
            cout << "[" << i << "]";
            for (int x : table[i]) {
                cout << " -> " << x;
            }
            cout << " -> NULL" << endl;
        }
    }
};

Sınav Taktikleri (Kopyalayıp Not Defterine At)
Dijkstra Trace (Vurgulu Konu): 
Vize sorularında ve graf sunumunda (Page 30, Sunum 11-12) "Trace" tablosu var. Hash Table için de aynısını yapabilir.

Sütunlar: Eklenen Sayı | Hash(x) | Çakışma Oldu mu? | Son İndeks
Bu tabloyu çizmeyi unutma.
Heap Array İndeksi: Soru: " [10, 20, 5, 30] dizisini Min-Heap'e çevirin."
Kağıda önce ağaç olarak çiz, yerleştir, heapifyUp yap.
Sonra ağacı level-order (yukarıdan aşağı, soldan sağa) okuyarak diziye geri yaz.
Hash Fonksiyonu:
Hoca sınavda h(x) = (2x + 5) % 11 gibi farklı bir formül verebilir.
Kodda key % TABLE_SIZE yazan yeri hocanın formülüyle değiştirmeyi unutma!
