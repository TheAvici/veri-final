Bu kod şunları içerir:

1- Temel Yapı: Node sınıfı.

2- Dolaşma (Traversal): Preorder, Inorder, Postorder, Level Order (BFS) .

3- Analiz: Yükseklik (Height), Eleman Sayısı, Yaprak Kontrolü.

4- Tür Kontrolleri: Full, Complete, Perfect .

5- BST İşlemleri: Ekleme (Insert), Arama (Search), Min/Max Bulma .

6- AVL (Dengeleme): Rotasyonlar ve Dengeli Ekleme .


```cpp
#include <iostream>
#include <queue>
#include <algorithm>
#include <cmath>

using namespace std;

// --- 1. DÜĞÜM YAPISI (Node Structure) ---
// Hem BST hem AVL için uyumlu yapı [cite: 3, 11]
struct Node {
    int data;
    Node* left;
    Node* right;
    int height; // AVL için gerekli [cite: 62]

    Node(int val) {
        data = val;
        left = right = nullptr;
        height = 1; // Yeni düğüm yüksekliği 1 başlar
    }
};

class TreeManager {
public:
    // --- YARDIMCI FONKSİYONLAR ---
    
    // Yükseklik hesaplama (Null güvenli) [cite: 9]
    int getHeight(Node* node) {
        if (node == nullptr) return 0;
        return node->height;
    }

    // Balance Factor (Denge Faktörü) Hesaplama [cite: 62]
    int getBalance(Node* node) {
        if (node == nullptr) return 0;
        return getHeight(node->left) - getHeight(node->right);
    }

    // --- 2. TRAVERSAL (DOLAŞMA) YÖNTEMLERİ [cite: 44] ---

    // Preorder: Root -> Left -> Right [cite: 46]
    void preorder(Node* root) {
        if (root != nullptr) {
            cout << root->data << " ";
            preorder(root->left);
            preorder(root->right);
        }
    }

    // Inorder: Left -> Root -> Right [cite: 47]
    // Not: BST ise sıralı çıktı verir [cite: 51]
    void inorder(Node* root) {
        if (root != nullptr) {
            inorder(root->left);
            cout << root->data << " ";
            inorder(root->right);
        }
    }

    // Postorder: Left -> Right -> Root [cite: 48]
    void postorder(Node* root) {
        if (root != nullptr) {
            postorder(root->left);
            postorder(root->right);
            cout << root->data << " ";
        }
    }

    // Level Order (BFS): Kuyruk (Queue) kullanarak [cite: 49]
    void levelOrder(Node* root) {
        if (root == nullptr) return;
        queue<Node*> q;
        q.push(root);

        while (!q.empty()) {
            Node* current = q.front();
            q.pop();
            cout << current->data << " ";

            if (current->left != nullptr) q.push(current->left);
            if (current->right != nullptr) q.push(current->right);
        }
    }

    // --- 3. AĞAÇ TÜRÜ KONTROLLERİ ---

    // Full Binary Tree Kontrolü: 0 veya 2 çocuk kuralı [cite: 17-19]
    bool isFullTree(Node* root) {
        if (root == nullptr) return true;
        
        // Yaprak düğüm
        if (root->left == nullptr && root->right == nullptr) return true;

        // İki çocuk da varsa altlara bak
        if (root->left && root->right)
            return (isFullTree(root->left) && isFullTree(root->right));

        return false; // Biri var biri yoksa false
    }

    // Complete Binary Tree Kontrolü: Soldan sağa doluluk [cite: 21-23]
    bool isCompleteTree(Node* root) {
        if (root == nullptr) return true;
        
        queue<Node*> q;
        q.push(root);
        bool nullSeen = false;

        while(!q.empty()) {
            Node* curr = q.front();
            q.pop();

            if (curr->left) {
                if (nullSeen) return false; // Null gördükten sonra dolu düğüm gelemez
                q.push(curr->left);
            } else {
                nullSeen = true;
            }

            if (curr->right) {
                if (nullSeen) return false;
                q.push(curr->right);
            } else {
                nullSeen = true;
            }
        }
        return true;
    }

    // Perfect Binary Tree Kontrolü: Derinlik hesapla + düğüm sayısı formülü (2^h - 1) [cite: 25-29]
    int countNodes(Node* root) {
        if (root == nullptr) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }

    bool isPerfectTree(Node* root) {
        int h = getHeight(root); // Bu fonksiyonda AVL yüksekliği değil, gerçek derinlik lazım olabilir
        int totalNodes = countNodes(root);
        return totalNodes == (pow(2, h) - 1); // [cite: 29]
    }

    // --- 4. BST İŞLEMLERİ (SEARCH & INSERT) ---

    // BST Arama [cite: 58]
    Node* searchBST(Node* root, int key) {
        if (root == nullptr || root->data == key) return root;
        
        if (key < root->data) return searchBST(root->left, key);
        return searchBST(root->right, key);
    }

    // Standart BST Ekleme (Dengesiz) [cite: 53-56]
    Node* insertBST(Node* root, int key) {
        if (root == nullptr) return new Node(key);

        if (key < root->data)
            root->left = insertBST(root->left, key);
        else if (key > root->data)
            root->right = insertBST(root->right, key);

        return root;
    }

    // --- 5. AVL AĞACI İŞLEMLERİ (ROTATIONS & BALANCED INSERT) ---

    // Sağa Rotasyon (Right Rotate) - LL Durumu [cite: 65]
    Node* rightRotate(Node* y) {
        Node* x = y->left;
        Node* T2 = x->right;

        // Dönme işlemi
        x->right = y;
        y->left = T2;

        // Yükseklik güncelleme
        y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
        x->height = max(getHeight(x->left), getHeight(x->right)) + 1;

        return x; // Yeni kök
    }

    // Sola Rotasyon (Left Rotate) - RR Durumu [cite: 65]
    Node* leftRotate(Node* x) {
        Node* y = x->right;
        Node* T2 = y->left;

        // Dönme işlemi
        y->left = x;
        x->right = T2;

        // Yükseklik güncelleme
        x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
        y->height = max(getHeight(y->left), getHeight(y->right)) + 1;

        return y; // Yeni kök
    }

    // AVL Ekleme (Otomatik Dengelemeli) 
    Node* insertAVL(Node* node, int key) {
        // 1. Standart BST Ekleme
        if (node == nullptr) return new Node(key);

        if (key < node->data)
            node->left = insertAVL(node->left, key);
        else if (key > node->data)
            node->right = insertAVL(node->right, key);
        else
            return node; // Eşit anahtarlar yok

        // 2. Yükseklik Güncelleme
        node->height = 1 + max(getHeight(node->left), getHeight(node->right));

        // 3. Denge Kontrolü (Balance Factor)
        int balance = getBalance(node);

        // 4. Rotasyon Durumları

        // Left Left (LL) -> Sağa Döndür
        if (balance > 1 && key < node->left->data)
            return rightRotate(node);

        // Right Right (RR) -> Sola Döndür
        if (balance < -1 && key > node->right->data)
            return leftRotate(node);

        // Left Right (LR) -> Sola, Sonra Sağa Döndür
        if (balance > 1 && key > node->left->data) {
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }

        // Right Left (RL) -> Sağa, Sonra Sola Döndür
        if (balance < -1 && key < node->right->data) {
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }

        return node;
    }
};

// --- TEST MAIN FONKSİYONU ---
int main() {
    TreeManager treeMgr;
    Node* root = nullptr;

    // AVL Test Verisi: 10, 20, 30 eklersek normal BST'de sağa yatık (skewed) olur.
    // AVL bunu otomatik olarak dengeler ve 20'yi kök yapar.
    
    cout << "--- AVL Agacina Ekleme (10, 20, 30, 40, 50, 25) ---" << endl;
    root = treeMgr.insertAVL(root, 10);
    root = treeMgr.insertAVL(root, 20);
    root = treeMgr.insertAVL(root, 30); // Burada rotasyon olur
    root = treeMgr.insertAVL(root, 40);
    root = treeMgr.insertAVL(root, 50);
    root = treeMgr.insertAVL(root, 25);

    cout << "Inorder (Sirali): ";
    treeMgr.inorder(root); // Çıktı: 10 20 25 30 40 50
    cout << endl;

    cout << "Preorder (Kok Basta): "; 
    treeMgr.preorder(root); // Kökün kim olduğunu görmek için
    cout << endl;

    cout << "Level Order (BFS): ";
    treeMgr.levelOrder(root);
    cout << endl;

    cout << "--- Agac Turu Kontrolleri ---" << endl;
    cout << "Full Tree mi? " << (treeMgr.isFullTree(root) ? "Evet" : "Hayir") << endl;
    cout << "Complete Tree mi? " << (treeMgr.isCompleteTree(root) ? "Evet" : "Hayir") << endl;
    
    // Arama Testi
    int aranan = 25;
    if (treeMgr.searchBST(root, aranan))
        cout << aranan << " agacta bulundu." << endl;
    else
        cout << aranan << " agacta yok." << endl;

    return 0;
}
```
