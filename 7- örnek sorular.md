Soru-1)

![1](https://github.com/user-attachments/assets/842a0537-9945-4c82-ac13-b870151cea43)

### 1. Kod A: Doğrusal Arama (Linear Search)
* **Yöntem:** Liste rastgele (sırasız) olduğu için, aranan ürünün sistemde "olmadığını" kesinleştirmek adına listenin başından sonuna kadar **tüm** elemanlara tek tek bakılmalıdır.
* **İşlem Sayısı:** 1 Milyon ($N = 10^6$) karşılaştırma işlemi yapılır.
* **Karmaşıklık:** **$O(N)$**

### 2. Kod B: İkili Arama (Binary Search)
* **Yöntem:** Liste sıralı olduğu için algoritma "Böl ve Yönet" mantığıyla çalışır. Her adımda veri setini tam ortadan ikiye böler ve yarısını eler.
* **İşlem Sayısı:** $\log_2(1.000.000) \approx$ **20** işlem yapılır.
* **Karmaşıklık:** **$O(\log N)$**

### 3. Farkın Yorumu
* **Performans Uçurumu:** Kod B (İkili Arama), en kötü senaryoda Kod A'ya göre **50.000 kat** daha az işlem yapar.
* **Ölçeklenebilirlik:** Eğer ürün sayısı 1 Milyon'dan 2 Milyon'a çıkarsa; Kod A'nın işi iki katına (2 Milyon işlem) çıkarken, Kod B'nin işi sadece **1 adım** artar (21 işlem). Büyük sistemler için Kod B zorunluluktur.
----------------------------------------------------------------------------------------------------------------------------
Sınav kağıdına yazılacak en net ve teknik yorum şudur:

Kod A (Lineer Search): Verim düşüktür. Veri seti büyüdükçe (1 Milyon, 10 Milyon...), arama süresi de doğru orantılı olarak artar. Eğer 1 milyon kullanıcı aynı anda arama yaparsa, sunucu 1 trilyon işlem yapmak zorunda kalır ve sistem kilitlenir.

Kod B (Binary Search): Çok daha verimlidir. Veri seti 1 milyondan 2 milyona çıksa bile işlem sayısı sadece 1 artar (20'den 21'e çıkar). Bu algoritma "Scalable" (Ölçeklenebilir) bir sistemdir.

Maliyet Dengesi: Kod B'de ürünleri "her gece sıralama" maliyeti vardır. Ancak arama işlemi (kullanıcının beklediği süre) çok daha kritik olduğu için, sıralama işleminin gece sunucu boşken yapılması ve arama anında Binary Search kullanılması mühendislik açısından doğru yaklaşımdır.
----------------------------------------------------------------------------------------------------------------------------
Özet Cevap:

Kod A: 1.000.000 İşlem

Kod B: ~20 İşlem

İkili Arama (Kod B), devasa veri setlerinde Doğrusal Aramaya (Kod A) göre inanılmaz derecede (yaklaşık 50.000 kat) daha performanslıdır.

---------------------------------------------------------------------------------------------------------------------------
Soru-2

![2](https://github.com/user-attachments/assets/5cca6aff-34ff-4b3f-b4e2-ebea4ced0d11)

### 🔹 Senaryo X: "Geri Al" (Undo) Butonu
* **Veri Yapısı:** **Stack (Yığın)**
* **Prensip:** `LIFO` (Last In, First Out - Son Giren İlk Çıkar)
* **Neden:** Kelime işlemcide yapılan son hamle, geri al denildiğinde ilk iptal edilmesi gereken işlemdir. Üst üste binen verilerde en üsttekini (sonuncuyu) almak için Stack kullanılır.

### 🔹 Senaryo Y: Ağ Yazıcısı (Network Printer)
* **Veri Yapısı:** **Queue (Kuyruk)**
* **Prensip:** `FIFO` (First In, First Out - İlk Giren İlk Çıkar)
* **Neden:** Yazıcıya gönderilen belgeler geliş sırasına göre basılmalıdır. İlk gönderen kişinin belgesi ilk basılmalıdır (Adil sıralama).

---------------------------------------------------------------------------------------------------------------------------
Stack (Undo) Örneği

```cpp
#include <queue>
#include <iostream>
using namespace std;

int main() {
    queue<string> yazici;
    yazici.push("Ahmet_Rapor.pdf"); // İlk gelen
    yazici.push("Ayse_Odev.docx");
    
    // Yazdırma İşlemi
    cout << "Basiliyor: " << yazici.front(); // Ahmet_Rapor.pdf basılır
    yazici.pop(); // Kuyruktan çıkarılır
}
```

Queue (Printer) Örneği

```cpp
#include <queue>
#include <iostream>
using namespace std;

int main() {
    queue<string> yazici;
    yazici.push("Ahmet_Rapor.pdf"); // İlk gelen
    yazici.push("Ayse_Odev.docx");
    
    // Yazdırma İşlemi
    cout << "Basiliyor: " << yazici.front(); // Ahmet_Rapor.pdf basılır
    yazici.pop(); // Kuyruktan çıkarılır
}
```
---------------------------------------------------------------------------------------------------------------------------
Soru-3)

![3](https://github.com/user-attachments/assets/fe3b1926-dd08-4859-9666-71b03ce8b492)

### Cevap:

#### 1. Ağacın Şekli Nasıl Olur?
* **Oluşan Yapı:** Sayılar sıralı eklendiği için ağaç dengesini kaybeder ve **Sağa Çarpık Ağaç (Right Skewed Tree)** halini alır.
* **Neden:** Her yeni gelen sayı bir öncekinden büyük olduğu için sürekli sağ çocuğa eklenir. Ağaçta hiç dallanma (sol çocuk) oluşmaz.
* **Görünüm:** Düz bir zincir şeklinde olur: `1 ➔ 2 ➔ 3 ➔ 4 ➔ 5`

#### 2. Bağlı Liste (Linked List) ile Performans Farkı Kalır mı?
* **Sonuç:** **Hayır, performans farkı kalmaz.**
* **Analiz:** Normalde dengeli bir BST'de arama işlemi **O(log N)** hızındadır. Ancak veriler sıralı girildiği için bu BST, yapısal olarak **Tek Yönlü Bağlı Liste (Singly Linked List)** ile aynı forma dönüşmüştür.
* **Karmaşıklık:** 5 sayısını bulmak için baştan sona tüm düğümleri gezmek gerekir. Bu durum BST için **En Kötü Senaryo (Worst Case)** dur ve karmaşıklığı **O(N)** olur. Bu da Bağlı Liste'deki arama maliyetiyle birebir aynıdır.

---------------------------------------------------------------------------------------------------------------------------

Soru-4)

![4](https://github.com/user-attachments/assets/c964a53f-88db-461a-8b9a-27c2296a5118)

C**Cevap:**

### 1. Temel Sebep: Ortanca Değer (Median Value) Kuralı
* **Mantık:** Elimizde 10, 20 ve 30 sayıları vardır. Bir İkili Arama Ağacının (BST) dengeli olabilmesi için, kök düğümün sağında ve solunda eşit sayıda (veya birbirine yakın) eleman olmalıdır.
* **Analiz:** Bu üç sayının matematiksel ortancası **20**'dir. Bu yüzden 20 kök olmalı ki; 10 onun solunda, 30 ise sağında kalabilsin. Böylece ağaç mükemmel dengeye ulaşır.

### 2. Mekanik Sebep: Sağa Döndürme (Right Rotation) İşlemi
* **Durum (Sol-Sol):** Şu anki yapı `30 -> 20 -> 10` şeklindedir. Ağaç sola doğru devrilmek üzeredir.
* **Hareket:** Sağa döndürme işlemi uygulandığında, ortadaki düğüm olan **20 yukarı çekilir**.
* **Yer Değiştirme:** Eski kök olan 30, 20'den büyük olduğu için BST kuralı gereği 20'nin **sağ çocuğu** haline gelir. 10 ise 20'nin solunda kalmaya devam eder.

### 3. Sonuç Yapısı
İşlem sonunda denge faktörü sıfırlanır ve ağaç şu hali alır:

```text
      20 (Yeni Kök)
     /  \
   10    30
```
---------------------------------------------------------------------------------------------------------------------------

Soru-5) 

![5](https://github.com/user-attachments/assets/a8227409-bf2f-4d9d-9816-a4aca8700b60)

**Soru:** Min-Heap'ten kökü sildiğimizde, neden hemen 2. en küçük sayıyı değil de, **dizinin en sonundaki** sayıyı tepeye getiriyoruz?

### 1. Temel Sebep: Ağaç Yapısını (Shape Property) Korumak
Heap veri yapısı **"Tam İkili Ağaç" (Complete Binary Tree)** olmak zorundadır. Yani arada boşluk olamaz, tüm düğümler soldan sağa dolu olmalıdır.
* Eğer aradan bir elemanı (çocuğu) yukarı çekersek, aşağıda bir **boşluk (gap/hole)** oluşur.
* Bu boşluk, dizi indeksleme mantığını (`2i+1`, `2i+2`) bozar.

### 2. Çözüm Stratejisi
1.  **Silme:** Kök silinir.
2.  **Taşıma:** Oluşan boşluğa dizinin **en sonundaki eleman** getirilir. (Çünkü silindiğinde ağaç yapısını bozmayan tek eleman odur).
3.  **Düzeltme (Heapify):** Yeni kök muhtemelen büyüktür. `Heapify Down` işlemi ile aşağı doğru kaydırılarak doğru yerine oturtulur.

> **Özet:** Önce ağacın **Şekli (Shape)** korunur, sonra **Sıralaması (Order)** düzeltilir.

---------------------------------------------------------------------------------------------------------------------------

Soru-6) 

![6](https://github.com/user-attachments/assets/65fc8f38-566d-42c4-816e-42c607928ad0)

**Cevap:**

### 1. Hatalı Olan Fonksiyon: Fonksiyon A
* **Seçim:** `h(x) = x.length()` (Kitap isminin harf uzunluğu)
* **Sonuç:** Bu fonksiyon seçilirse sistem **Linked List hızına ($O(N)$)** düşer ve performans sorunu yaşar.

### 2. Neden Çöker? (Teknik Analiz)

* **Sorun: Kümelenme (Clustering) ve Aşırı Çakışma (Massive Collision)**
    * İngilizce kitap isimlerinin uzunlukları genellikle belli bir aralıktadır (Örneğin: 2 karakter ile 100 karakter arası).
    * Hash tablomuzun boyutu ($M$) **10.000** olsa bile, `x.length()` fonksiyonu sadece **2 ile 100 arasındaki** değerleri üretecektir.
    
* **Tablonun Durumu:**
    * Tablonun **0-100** arasındaki indeksleri aşırı dolarken, **101 ile 9.999** arasındaki indeksler tamamen **BOŞ** kalacaktır.
    * 10.000 adet kitap, sadece 50-60 tane kutuya (bucket) doluşur.

* **Performans Kaybı ($O(1) \rightarrow O(N)$):**
    * Bir indekse yüzlerce kitap düştüğü için, "Çakışma Çözme" (Collision Resolution) mekanizması (genelde Chaining/Zincirleme) devreye girer.
    * O kutudaki yüzlerce kitap, arka arkaya bağlı bir **Linked List** oluşturur.
    * Aradığımız kitabı bulmak için bu uzun listeyi tek tek gezmek zorunda kalırız. Bu da Hash Tablosunun $O(1)$ avantajını yok eder ve sistemi yavaşlatır.

> **Not:** Fonksiyon B (Polinom yaklaşımı), harf kodlarını (ASCII) işin içine kattığı için çok daha geniş bir dağılım sağlar ve tabloyu daha verimli kullanır.

---------------------------------------------------------------------------------------------------------------------------

Soru-7) 

![7](https://github.com/user-attachments/assets/1cf4b59d-e900-492c-a5f4-10c13e2b45fe)

**Cevap:**

### 1. Özellik: Tanıyor Olabileceğin Kişiler (Yakın Çevre)
* **Seçim:** **BFS (Breadth-First Search - Genişlik Öncelikli Arama)**
* **Neden:**
    * Amacımız kaynağa **en yakın** (en kısa mesafedeki) kişileri bulmaktır.
    * BFS, bir havuza atılan taşın yaydığı dalgalar gibi çalışır; önce 1. derece arkadaşlarını (merkez), sonra 2. derece arkadaşlarını (arkadaşının arkadaşı) tarar.
    * Eğer DFS kullansaydık, algoritma bir daldan girip ağın en ucundaki (belki 50. dereceden) alakasız bir kişiye kadar derinlemesine giderdi. Bu da "yakın çevre" önerisi için yanlış ve verimsiz olurdu.

### 2. Özellik: Network Analizi (Tüm Erişim Ağı)
* **Seçim:** **DFS (Depth-First Search - Derinlik Öncelikli Arama)**
* **Neden:**
    * Amacımız uzaklık değil, "bu ağda toplam kaç tekil kişi var" sorusuna cevap bulmak, yani tüm ağı (Connected Component) sonuna kadar gezmektir.
    * DFS, bir düğümden başlayıp gidebildiği en son noktaya kadar gitmeye (derinlemesine inmeye) programlıdır.
    * Tüm ağı dolaşıp sayım yapmak (reachability) veya ağdaki kopuk parçaları analiz etmek için DFS'in özyinelemeli (recursive) yapısı daha doğal bir çözüm sunar.

---------------------------------------------------------------------------------------------------------------------------

Soru-8) 

![8](https://github.com/user-attachments/assets/c9788a63-0fc8-4bb9-9693-41abd515494a)

**Cevap:**

### 1. Durum A: IoT Cihazı (Kısıtlı RAM)
* **Seçim:** **Quick Sort (Hızlı Sıralama)**
* **Neden (Bellek Verimliliği):**
    * Akıllı termostat gibi cihazlarda RAM (Bellek) en kritik kaynaktır.
    * Merge Sort, çalışmak için dizinin boyutu kadar (**$O(N)$**) **ekstra bellek alanı (Auxiliary Space)** talep eder.
    * Quick Sort ise **"In-Place" (Yerinde)** çalışan bir algoritmadır. Ekstra bir dizi oluşturmaz, sıralamayı mevcut dizi üzerinde yapar. Bu yüzden bellek kısıtı olan donanımlarda standart tercihtir.

### 2. Durum B: Büyük Veritabanı (Kararlılık/Stability)
* **Seçim:** **Merge Sort (Birleştirmeli Sıralama)**
* **Neden (Kararlılık):**
    * **Kararlılık (Stability):** Soruda "Verinin aslı bozulmamalı" ifadesi geçmektedir. Merge Sort **Kararlı (Stable)** bir algoritmadır; yani değeri eşit olan kayıtların (örneğin aynı fiyattaki ürünlerin) birbirine göre olan orijinal sırasını **korur**.
    * **Quick Sort Kararsızdır:** Standart Quick Sort, elemanları takas ederken (swap) orijinal sırayı bozabilir (Unstable). Veritabanı sıralamalarında bu istenmeyen bir durumdur.

---------------------------------------------------------------------------------------------------------------------------

Joker Soru-)

![joker](https://github.com/user-attachments/assets/8bf9859b-f661-4905-bdb6-36aa6455b07e)

**Cevap:**

### 1. Yöntem 1: Diziyi Tamamen Sıralamak (Sorting)
* **Mantık:** Tüm diziyi küçükten büyüğe sıralarız (Örn: QuickSort veya MergeSort) ve ardından `k.` sıradaki elemanı alırız.
* **Karmaşıklık:** **$O(N \log N)$**
* **Neden Kötü?** Eğer 1 Milyon ($N$) elemanımız varsa ve biz sadece en küçük 10. ($k$) elemanı arıyorsak; geri kalan 999.990 elemanı da boşu boşuna sıralamış oluruz. Bu işlemci gücü israfıdır.

### 2. Yöntem 2: Heap Kullanmak (Heap Approach)
* **Mantık:** `k` boyutunda bir **Max-Heap** oluştururuz. Diziyi tararken elimizdeki sayıyı Heap'in tepesindeki (o anki $k$. en küçük aday) ile kıyaslarız. Eğer yeni sayı daha küçükse Heap'i güncelleriz.
* **Karmaşıklık:** **$O(N \log k)$**
* **Neden Tercih Edilir?**
    * $\log k$, $\log N$'den çok daha küçüktür (Özellikle $k \ll N$ durumunda).
    * Algoritma tüm veriyi sıraya dizmekle uğraşmaz, sadece "En Küçük k" elemanlık **küçük bir havuzu** yönetir.
    * Bu yöntem, verinin tamamını (Streaming Data) bellekte tutamadiğımız durumlarda bile çalışabilir.

> **Metafor:** Kayıp bir anahtarı bulmak için tüm evi (Sorting) temizlemek yerine, sadece anahtarın olabileceği çekmeceyi (Heap) düzenlemek gibidir.

---------------------------------------------------------------------------------------------------------------------------

Hızlı Sıralama (Quick Sort)

```cpp
void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}
```
---------------------------------------------------------------------------------------------------------------------------

Kabuk Sıralaması (Shell Sort)

```cpp
void shellSort(vector<int>& arr) {
    int n = arr.size();

    for (int gap = n / 2; gap > 0; gap /= 2) {
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            arr[j] = temp;
        }
    }
}
```

---------------------------------------------------------------------------------------------------------------------------
uygulama- Heap Trace:

![uyg heap trace](https://github.com/user-attachments/assets/12c69320-d6f6-4111-a41e-33c2b5e4faed)

### Adım Adım Çözüm:

#### 1. Ekleme Adımları (Insert)
* `Insert(15)` -> `[15]`
* `Insert(8)`  -> `[15, 8]` -> Swap(8,15) -> **`[8, 15]`**
* `Insert(24)` -> `[8, 15, 24]` (Düzgün)
* `Insert(3)`  -> `[8, 15, 24, 3]`
    * 3 < 15 (Parent) -> Swap -> `[8, 3, 24, 15]`
    * 3 < 8 (Parent) -> Swap -> **`[3, 8, 24, 15]`** (Min tepede)
* `Insert(10)` -> `[3, 8, 24, 15, 10]`
    * Parent(10) = 8.
    * 10 > 8 (Sorun yok, Swap gerekmez).
    * **Ekleme Sonucu:** **`[3, 8, 24, 15, 10]`**

#### 2. Silme Adımı (DeleteMin)
* **Kök Silinir (3):** Yerine en sondaki eleman (10) gelir.
    * Geçici Durum: `[10, 8, 24, 15]`
* **Heapify Down (Aşağı Düzeltme):**
    * 10'un çocukları: 8 ve 24.
    * En küçük çocuk: 8.
    * 10 > 8 olduğu için **Swap(10, 8)** yapılır.
    * Yeni Durum: `[8, 10, 24, 15]`
    * Kontrol: 10'un çocuğu 15. 10 < 15 (Sorun yok).
* **Final Sonuç:** **`[8, 10, 24, 15]`**
---------------------------------------------------------------------------------------------------------------------------

1. Uygulama: Heap Insert ve Print Fonksiyonları

```cpp
void insert(int val) {
    heap.push_back(val);
    int index = heap.size() - 1;

    while (index > 0 && heap[(index - 1) / 2] > heap[index]) {
        swap(heap[index], heap[(index - 1) / 2]);
        index = (index - 1) / 2;
    }
}

void printHeap() {
    cout << "[ ";
    for (int i : heap) cout << i << " ";
    cout << "]" << endl;
}
```

---------------------------------------------------------------------------------------------------------------------------

2. Uygulama: Heap Main Fonksiyonu

```cpp
int main() {
    MinHeap h;
    vector<int> inputs = {15, 8, 24, 3, 10};

    cout << "-------EKLEME ADIMLARI-------" << endl;
    for (int val : inputs) {
        h.insert(val);
        cout << "Insert(" << val << ") -> ";
        h.printHeap();
    }

    cout << "\n-------SILME ADIMI-------" << endl;
    h.deleteMin();
    cout << "DeleteMin() -> ";
    h.printHeap();

    return 0;
}
```
