1. Sunumda Yer Alan Temel Sıralama Algoritmaları

Bu kodlar sunumdaki sözde kod (pseudocode) ve C++ parçalarının derlenebilir halidir.

A. Kabarcık Sıralaması (Bubble Sort)

Sunumda belirtildiği üzere swapped bayrağı (flag) kullanılarak optimize edilmiş halidir. 
Dizi zaten sıralıysa $O(N)$ sürede biter1.

```cpp
#include <vector>
#include <algorithm> // swap için

void bubbleSort(std::vector<int>& arr) {
    int n = arr.size();
    bool swapped;
    for (int i = 0; i < n - 1; i++) {
        swapped = false;
        // Her adımda en büyük eleman sona taşındığı için j < n - i - 1
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                std::swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        // Eğer iç döngüde hiç takas olmadıysa dizi sıralıdır
        if (!swapped)
            break;
    }
}
// Kaynak: Sayfa 8 [cite: 40, 41, 42]
```

B. Seçmeli Sıralama (Selection Sort)

Her döngüde en küçük elemanı bulup başa koyar.

```cpp
void selectionSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        int min_idx = i;
        // Sırasız kısımdaki en küçük elemanı bul
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        // Bulunan en küçük elemanı, sırasız kısmın başındaki ile değiştir
        std::swap(arr[i], arr[min_idx]);
    }
}
// Kaynak: Sayfa 13 [cite: 134, 136, 155]
```

C. Eklemeli Sıralama (Insertion Sort)

Özellikle "neredeyse sıralı" dizilerde çok hızlı çalışır (O(N)).

Sunumdaki while döngüsü yapısına sadık kalınmıştır.

```cpp
void insertionSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;

        // Key'den büyük olan elemanları bir sağa kaydır
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}
// Kaynak: Sayfa 21 [cite: 200, 206, 215, 224]
```

D. Kabuk Sıralaması (Shell Sort)

Insertion Sort'un gap (aralık) kullanılarak genelleştirilmiş halidir. 
Sunumda gap dizisi $N/2, N/4...$ şeklinde verilmiştir3.

```
void shellSort(std::vector<int>& arr) {
    int n = arr.size();
    // Aralığı (gap) yarıya indirerek ilerle
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // Gap'li insertion sort uygula
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j;
            for (j = i; j >= gap && arr[j - gap] > temp; j -= gap) {
                arr[j] = arr[j - gap];
            }
            arr[j] = temp;
        }
    }
}
// Kaynak: Sayfa 29 [cite: 329, 331, 333]
```

E. Birleştirmeli Sıralama (Merge Sort)

"Böl ve Yönet" algoritmasıdır. Sunumda L ve R geçici vektörleri kullanılarak implemente edilmiştir.

```cpp
void merge(std::vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Geçici vektörler oluştur
    std::vector<int> L(n1), R(n2);

    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];

    int i = 0, j = 0, k = left;
    
    // Küçük olanı asıl diziye geri yaz
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Kalan elemanları kopyala
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(std::vector<int>& arr, int left, int right) {
    if (left >= right)
        return;

    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}
// Kaynak: Sayfa 36 [cite: 465, 492]
```


F. Hızlı Sıralama (Quick Sort) - Standart (Son Eleman Pivot)

Sunumda pivot olarak son eleman (arr[high]) seçilmiştir.

```cpp
int partition(std::vector<int>& arr, int low, int high) {
    int pivot = arr[high]; // Pivot seçimi (Son eleman)
    int i = (low - 1); // Küçük elemanların indeksi

    for (int j = low; j <= high - 1; j++) {
        // Mevcut eleman pivottan küçükse
        if (arr[j] < pivot) {
            i++;
            std::swap(arr[i], arr[j]);
        }
    }
    std::swap(arr[i + 1], arr[high]);
    return (i + 1);
}

void quickSort(std::vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
// Kaynak: Sayfa 42 [cite: 600, 619]
```

------------------------------------------------------------------------

2. Sınavda Çıkabilecek "Zor" ve Sunumda Kodu Olmayanlar

A. Quick Sort: Median-of-Three (Üçün Ortancası) Optimizasyonu

```cpp
// Median-of-Three yardımcı fonksiyonu
int medianOfThree(std::vector<int>& arr, int low, int high) {
    int mid = low + (high - low) / 2;
    
    // Başı, ortayı ve sonu sırala ki ortanca (median) sonda dursun veya pivot olarak kullanılsın
    if (arr[mid] < arr[low]) std::swap(arr[low], arr[mid]);
    if (arr[high] < arr[low]) std::swap(arr[low], arr[high]);
    if (arr[high] < arr[mid]) std::swap(arr[mid], arr[high]);
    
    // Şu an: arr[low] <= arr[mid] <= arr[high]
    // Median arr[mid] oldu. Pivot olarak kullanmak için son elemanla yer değiştirebiliriz
    // veya partition mantığını buna göre güncelleyebiliriz.
    // Basitlik için median'ı sona atıp standart partition çağırıyoruz:
    std::swap(arr[mid], arr[high]); 
    return arr[high];
}

// Optimize edilmiş Partition
int partitionOptimized(std::vector<int>& arr, int low, int high) {
    // Pivotu rastgele veya son eleman seçmek yerine Median ile seçiyoruz:
    medianOfThree(arr, low, high); 
    // Artık 'high' indeksinde median değer var, standart mantık devam eder:
    
    int pivot = arr[high];
    int i = (low - 1);
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            std::swap(arr[i], arr[j]);
        }
    }
    std::swap(arr[i + 1], arr[high]);
    return (i + 1);
}

void quickSortOptimized(std::vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partitionOptimized(arr, low, high);
        quickSortOptimized(arr, low, pi - 1);
        quickSortOptimized(arr, pi + 1, high);
    }
}
```


B. Heap Sort (Yığın Sıralaması)

Sunumdaki karşılaştırma tablosunda (Sayfa 50) yer alıyor 7 ancak kodu yok. O(N \log N)
garantisi verdiği ve "in-place" olduğu için sınavlarda Merge Sort ile kıyaslatılır.

```cpp
// Bir alt ağacı heapify (yığınlama) yapan fonksiyon
// n: yığının boyutu, i: kök düğümün indeksi
void heapify(std::vector<int>& arr, int n, int i) {
    int largest = i;   // En büyüğü kök olarak başlat
    int left = 2 * i + 1; 
    int right = 2 * i + 2; 

    // Sol çocuk kökten büyükse
    if (left < n && arr[left] > arr[largest])
        largest = left;

    // Sağ çocuk şu ana kadarki en büyükten büyükse
    if (right < n && arr[right] > arr[largest])
        largest = right;

    // Eğer en büyük kök değilse
    if (largest != i) {
        std::swap(arr[i], arr[largest]);
        // Etkilenen alt ağacı tekrar heapify yap
        heapify(arr, n, largest);
    }
}

void heapSort(std::vector<int>& arr) {
    int n = arr.size();

    // 1. Diziyi Max-Heap haline getir (Yeniden düzenle)
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    // 2. Yığından elemanları teker teker çıkar
    for (int i = n - 1; i > 0; i--) {
        // Mevcut kökü (en büyüğü) sona taşı
        std::swap(arr[0], arr[i]);

        // Azaltılmış yığın üzerinde kökü tekrar heapify yap
        heapify(arr, i, 0);
    }
}
```

C. Radix Sort (Taban Sıralaması)

Sayfa 51'de "Karşılaştırma yapmayan algoritmalar" başlığı altında geçiyor. O(N)$karmaşıklığına sahip
olduğu içinteorik sorularda "Lineer zamanda sıralama mümkün mü?" sorusunun cevabıdır.

```cpp
// Radix sort için yardımcı fonksiyon: Basamağa göre Counting Sort
void countingSortForRadix(std::vector<int>& arr, int exp) {
    int n = arr.size();
    std::vector<int> output(n); 
    int count[10] = {0};

    // Belirtilen basamaktaki (exp) sayıların frekansını say
    for (int i = 0; i < n; i++)
        count[(arr[i] / exp) % 10]++;

    // Count dizisini kümülatif hale getir (pozisyonları belirle)
    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];

    // Output dizisini oluştur (Sondan başa doğru kararlılık için)
    for (int i = n - 1; i >= 0; i--) {
        output[count[(arr[i] / exp) % 10] - 1] = arr[i];
        count[(arr[i] / exp) % 10]--;
    }

    // Sıralı output'u ana diziye kopyala
    for (int i = 0; i < n; i++)
        arr[i] = output[i];
}

void radixSort(std::vector<int>& arr) {
    if (arr.empty()) return;

    // En büyük sayıyı bul (basamak sayısını bilmek için)
    int maxVal = arr[0];
    for (int x : arr)
        if (x > maxVal) maxVal = x;

    // Her basamak için (1ler, 10lar, 100ler...) counting sort uygula
    for (int exp = 1; maxVal / exp > 0; exp *= 10)
        countingSortForRadix(arr, exp);
}
```


