-1. Temel Yapı (Node Struct)

#include <iostream>
using namespace std;

// Tek Yönlü Bağlı Liste Düğümü
struct Node {
    int data;
    Node* next;
    
    // Kurucu (Constructor) - Hızlı node oluşturmak için
    Node(int val) {
        data = val;
        next = nullptr;
    }
};

-2. Ekleme Operasyonları (Insertion)
--A. Başa Ekleme (Push Front) - O(1)

void pushFront(Node*& head, int newData) {
    Node* newNode = new Node(newData);
    newNode->next = head; // Yeni düğümün next'i eski head'i gösterir
    head = newNode;       // Head artık yeni düğümdür
}

--B. Sona Ekleme (Push Back) - O(N)
Eğer tail pointer tutmuyorsan en sona kadar gitmen gerekir.

void pushBack(Node*& head, int newData) {
    Node* newNode = new Node(newData);
    
    // 1. Liste boşsa head direkt bu olur
    if (head == nullptr) {
        head = newNode;
        return;
    }
    
    // 2. Son düğüme kadar git
    Node* temp = head;
    while (temp->next != nullptr) {
        temp = temp->next;
    }
    
    // 3. Son düğümün next'ine yeniyi bağla
    temp->next = newNode;
}

--C. Araya Ekleme (Belirli bir düğümden sonraya)

void insertAfter(Node* prevNode, int newData) {
    if (prevNode == nullptr) {
        cout << "Onceki dugum NULL olamaz!" << endl;
        return;
    }
    
    Node* newNode = new Node(newData);
    
    // KRİTİK SIRALAMA:
    // 1. Yeninin next'i, öncekinin next'ini göstersin
    newNode->next = prevNode->next;
    
    // 2. Öncekinin next'i, yeniyi göstersin
    prevNode->next = newNode;
}

-3. Silme Operasyonları (Deletion)

void deleteNode(Node*& head, int key) {
    Node* temp = head;
    Node* prev = nullptr;
    
    // DURUM 1: Silinecek eleman HEAD ise
    if (temp != nullptr && temp->data == key) {
        head = temp->next; // Head'i bir yana kaydır
        delete temp;       // Eski head'i sil
        return;
    }
    
    // DURUM 2: Elemanı aramak için gezinme
    while (temp != nullptr && temp->data != key) {
        prev = temp;
        temp = temp->next;
    }
    
    // DURUM 3: Eleman bulunamadıysa
    if (temp == nullptr) return;
    
    // DURUM 4: Bağlantıyı kopar ve sil
    // Öncekinin next'i, silineceğin next'ine bağlansın (Atlama)
    prev->next = temp->next;
    delete temp;
}


-4. Sınavda Çıkabilecek "Zor" Algoritmalar
--A. Listeyi Ters Çevirme (Reverse Linked List)
void reverseList(Node*& head) {
    Node* prev = nullptr;
    Node* current = head;
    Node* next = nullptr;
    
    while (current != nullptr) {
        next = current->next;  // 1. Bağlantıyı kaybetmemek için sıradakini tut
        current->next = prev;  // 2. Oku terse çevir (Arkanı göster)
        
        // 3. Pointerları bir adım ileri kaydır
        prev = current;
        current = next;
    }
    head = prev; // Yeni baş, en son işlem gören düğüm olur
}

-5. Çift Yönlü Bağlı Liste (Doubly Linked List) Farkı
struct DNode {
    int data;
    DNode* next;
    DNode* prev; // Geriye gidiş
    
    DNode(int val) {
        data = val;
        next = nullptr;
        prev = nullptr;
    }
};

// Çift Yönlüye Ekleme Örneği (Başa)
void pushFrontDoubly(DNode*& head, int newData) {
    DNode* newNode = new DNode(newData);
    
    newNode->next = head;
    newNode->prev = nullptr;
    
    if (head != nullptr) {
        head->prev = newNode; // Eski head'in gerisi yeni düğüm olur
    }
    head = newNode;
}