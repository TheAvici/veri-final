-1. Temel Ağaç Yapısı (Start Here)

#include <iostream>
#include <algorithm> // max fonksiyonu için
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    int height; // AVL için gerekli

    Node(int val) {
        data = val;
        left = nullptr;
        right = nullptr;
        height = 1; // Yeni düğümün yüksekliği başlangıçta 1'dir
    }
};

-2. Binary Search Tree (BST) Operasyonları
--A. Ekleme (Insert) - Rekürsif

Node* insertBST(Node* root, int val) {
    // 1. Ağaç boşsa veya yaprağa geldiysek yeni düğümü oluştur
    if (root == nullptr) {
        return new Node(val);
    }
    
    // 2. Değer küçükse sola, büyükse sağa git
    if (val < root->data) {
        root->left = insertBST(root->left, val);
    } else if (val > root->data) {
        root->right = insertBST(root->right, val);
    }
    
    // 3. Değişmiş root'u geri döndür
    return root;
}

--B. Silme (Delete)
// En küçük değeri bulan yardımcı fonksiyon (Sağ alt ağacın en küçüğü lazım olacak)
Node* findMin(Node* root) {
    while (root->left != nullptr) root = root->left;
    return root;
}

Node* deleteNode(Node* root, int key) {
    if (root == nullptr) return root;

    // Önce silinecek düğümü bul
    if (key < root->data) 
        root->left = deleteNode(root->left, key);
    else if (key > root->data) 
        root->right = deleteNode(root->right, key);
    else {
        // Düğüm bulundu! Şimdi 3 durumu kontrol et:
        
        // Durum 1 & 2: Tek çocuklu veya çocuksuz
        if (root->left == nullptr) {
            Node* temp = root->right;
            delete root;
            return temp;
        } else if (root->right == nullptr) {
            Node* temp = root->left;
            delete root;
            return temp;
        }

        // Durum 3: İki çocuklu (Sağın en küçüğünü al, yerine koy)
        Node* temp = findMin(root->right);
        root->data = temp->data; // Değeri kopyala
        root->right = deleteNode(root->right, temp->data); // Kopya düğümü sil
    }
    return root;
}

-3. Gezinme (Traversals)
// 1. Preorder (Önce Kök): Kök -> Sol -> Sağ
void preOrder(Node* root) {
    if (root != nullptr) {
        cout << root->data << " ";
        preOrder(root->left);
        preOrder(root->right);
    }
}

// 2. Inorder (Ortada Kök): Sol -> Kök -> Sağ (Sayıları SIRALI verir)
void inOrder(Node* root) {
    if (root != nullptr) {
        inOrder(root->left);
        cout << root->data << " ";
        inOrder(root->right);
    }
}

// 3. Postorder (Sonda Kök): Sol -> Sağ -> Kök
void postOrder(Node* root) {
    if (root != nullptr) {
        postOrder(root->left);
        postOrder(root->right);
        cout << root->data << " ";
    }
}

-4. Hoca Tarzı "Analiz" Kodları (Rekürsif)
--A. Ağacın Yüksekliğini Bulma (Height)

int height(Node* root) {
    if (root == nullptr) return 0;
    
    int lHeight = height(root->left);
    int rHeight = height(root->right);
    
    // Hangi taraf daha derinse onu al + 1 (kendisi)
    return max(lHeight, rHeight) + 1;
}

--B. Belirli Bir Değerden Büyük Düğümleri Toplama

int sumGreater(Node* root, int x) {
    if (root == nullptr) return 0;
    
    int sum = 0;
    if (root->data > x) sum += root->data; // Şartı sağlıyorsa ekle
    
    sum += sumGreater(root->left, x);
    sum += sumGreater(root->right, x);
    
    return sum;
}


-5. AVL Ağacı (AVL Tree)

// Yardımcı: Yükseklik alma (NULL güvenli)
int getHeight(Node* n) {
    if (n == nullptr) return 0;
    return n->height;
}

// Yardımcı: Denge Faktörü (Balance Factor)
int getBalance(Node* n) {
    if (n == nullptr) return 0;
    return getHeight(n->left) - getHeight(n->right);
}

// SAĞA ROTASYON (Right Rotate) - Sol taraf ağırlaştığında (LL)
Node* rightRotate(Node* y) {
    Node* x = y->left;
    Node* T2 = x->right;

    // Döndürme
    x->right = y;
    y->left = T2;

    // Yükseklikleri güncelle
    y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
    x->height = max(getHeight(x->left), getHeight(x->right)) + 1;

    return x; // Yeni kök
}

// SOLA ROTASYON (Left Rotate) - Sağ taraf ağırlaştığında (RR)
Node* leftRotate(Node* x) {
    Node* y = x->right;
    Node* T2 = y->left;

    // Döndürme
    y->left = x;
    x->right = T2;

    // Yükseklikleri güncelle
    x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
    y->height = max(getHeight(y->left), getHeight(y->right)) + 1;

    return y; // Yeni kök
}

// AVL EKLEME (INSERT)
Node* insertAVL(Node* node, int key) {
    // 1. Normal BST Eklemesi
    if (node == nullptr) return new Node(key);

    if (key < node->data)
        node->left = insertAVL(node->left, key);
    else if (key > node->data)
        node->right = insertAVL(node->right, key);
    else 
        return node; // Eşit anahtarlara izin yok

    // 2. Yüksekliği Güncelle
    node->height = 1 + max(getHeight(node->left), getHeight(node->right));

    // 3. Dengeyi Kontrol Et
    int balance = getBalance(node);

    // 4. Dengesizlik Varsa 4 Durumu Kontrol Et (Rotasyonlar)

    // Left Left (LL) Durumu
    if (balance > 1 && key < node->left->data)
        return rightRotate(node);

    // Right Right (RR) Durumu
    if (balance < -1 && key > node->right->data)
        return leftRotate(node);

    // Left Right (LR) Durumu
    if (balance > 1 && key > node->left->data) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }

    // Right Left (RL) Durumu
    if (balance < -1 && key < node->right->data) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }

    return node;
}


