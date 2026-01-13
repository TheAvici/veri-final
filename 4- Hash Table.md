Hash Table - Open Addressing

```cpp
// Kaynak: Sunum 9-10, Sayfa 42-46
#include <iostream>
#include <vector>
#include <string>

using namespace std;

const int TABLE_SIZE = 11; // Sunumdaki örnek boyut

void printTable(const vector<int>& table, string method) {
    cout << "\n--- " << method << " Sonuc Tablosu ---" << endl;
    for (int i = 0; i < TABLE_SIZE; i++) {
        if (table[i] != -1)
            cout << "[" << i << "]: " << table[i] << endl;
        else
            cout << "[" << i << "]: Bos" << endl;
    }
}

// Yöntem 1: Doğrusal Araştırma (Linear Probing)
void linearProbing(const vector<int>& data) {
    vector<int> table(TABLE_SIZE, -1); // Tabloyu -1 (boş) ile başlat

    for (int key : data) {
        int index = key % TABLE_SIZE;
        // Çakışma varsa bir sonrakine bak: (i + 1)
        while (table[index] != -1) {
            index = (index + 1) % TABLE_SIZE;
        }
        table[index] = key;
    }
    printTable(table, "Linear Probing");
}

// Yöntem 2: Karesel Araştırma (Quadratic Probing)
void quadraticProbing(const vector<int>& data) {
    vector<int> table(TABLE_SIZE, -1);

    for (int key : data) {
        int hashVal = key % TABLE_SIZE;
        int i = 0;
        int index = hashVal;

        // Çakışma varsa: index + i^2 formülünü uygula
        while (table[index] != -1) {
            i++;
            index = (hashVal + i * i) % TABLE_SIZE;
        }
        table[index] = key;
    }
    printTable(table, "Quadratic Probing");
}

int main() {
    // Sunumdaki örnek veri seti
    vector<int> inputs = {12, 25, 45, 14, 1};
    
    cout << "Veriler: {12, 25, 45, 14, 1}" << endl;
    
    linearProbing(inputs);
    quadraticProbing(inputs);

    return 0;
}
```


Hash Table - Separate Chaining

Sunumda Sayfa 15-20 arasında anlatılan "Ayrı Zincirleme" yönteminin C++ kodudur.

Bu yöntem çakışma durumunda aynı indekste bir Linked List (Bağlı Liste) oluşturur.

```cpp
// Standart Separate Chaining (Ayrı Zincirleme) Implementasyonu
#include <iostream>
#include <vector>
#include <list> // Bağlı liste için gerekli kütüphane

using namespace std;

class HashTableChaining {
    int capacity;
    // Her slot bir integer listesi tutar (std::list)
    vector<list<int>> table; 

public:
    HashTableChaining(int size) {
        capacity = size;
        table.resize(capacity);
    }

    int hashFunction(int key) {
        return key % capacity;
    }

    void insert(int key) {
        int index = hashFunction(key);
        // İlgili indeksteki listenin sonuna ekle (Push Back)
        table[index].push_back(key);
    }

    void display() {
        cout << "\n--- Separate Chaining Sonuc Tablosu ---" << endl;
        for (int i = 0; i < capacity; i++) {
            cout << "[" << i << "]: ";
            // O indisteki listeyi gez
            for (int val : table[i]) {
                cout << val << " -> ";
            }
            cout << "NULL" << endl;
        }
    }
};

int main() {
    // Sunumdaki mod alma mantığına göre boyut 5 olsun (Sayfa 16-20 örneği)
    HashTableChaining ht(5);
    
    // Sunum sayfa 18-20'deki çakışma örneği: 12, 22, 15, 25
    vector<int> data = {12, 22, 15, 25};

    for (int key : data) {
        ht.insert(key);
    }

    ht.display();
    // Beklenen çıktı:
    // Index 0: 15 -> 25 -> NULL (ikisi de 5'e tam bölünür)
    // Index 2: 12 -> 22 -> NULL (ikisi de 5'e bölümden 2 kalır)

    return 0;
}
```





