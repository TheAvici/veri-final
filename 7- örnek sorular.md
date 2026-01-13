Soru-1)

![1](https://github.com/user-attachments/assets/842a0537-9945-4c82-ac13-b870151cea43)

Teorik Analiz ve Cevap

Soruda bizden "olmayan bir ürünü" aramamız isteniyor. Bu, algoritmada En Kötü Senaryo (Worst Case) durumudur.

Kod A: Doğrusal Arama (Linear Search)

-Mantık: Liste rastgele olduğu için, aradığımız ürünün "olmadığını" anlamak için listenin en başından en sonuna kadar her bir ürüne tek tek bakmamız gerekir.

-Karmaşıklık: O(N)

-İşlem Sayısı: N = 1.000.000 (1 Milyon) ise, 1.000.000 karşılaştırma işlemi yapılır.

Kod B: İkili Arama (Binary Search)

-Mantık: Liste sıralı olduğu için, algoritma her adımda listeyi ikiye böler. "Aradığım ürün ortadakinden büyük mü küçük mü?" diye bakar ve listenin yarısını eler.

-Karmaşıklık: O(\log_2 N) (Logaritma 2 tabanında N)

-İşlem Sayısı: 2^{10} = 1.000, 2^{20} \approx 1.000.000. Yani log_2(1.000.000) = 19.9.

-Bu durumda yaklaşık 20 karşılaştırma işlemi yapılır.

Sınav kağıdına yazılacak en net ve teknik yorum şudur:

Kod A (Lineer Search): Verim düşüktür. Veri seti büyüdükçe (1 Milyon, 10 Milyon...), arama süresi de doğru orantılı olarak artar. Eğer 1 milyon kullanıcı aynı anda arama yaparsa, sunucu 1 trilyon işlem yapmak zorunda kalır ve sistem kilitlenir.

Kod B (Binary Search): Çok daha verimlidir. Veri seti 1 milyondan 2 milyona çıksa bile işlem sayısı sadece 1 artar (20'den 21'e çıkar). Bu algoritma "Scalable" (Ölçeklenebilir) bir sistemdir.

Maliyet Dengesi: Kod B'de ürünleri "her gece sıralama" maliyeti vardır. Ancak arama işlemi (kullanıcının beklediği süre) çok daha kritik olduğu için, sıralama işleminin gece sunucu boşken yapılması ve arama anında Binary Search kullanılması mühendislik açısından doğru yaklaşımdır.

Özet Cevap:

Kod A: 1.000.000 İşlem

Kod B: ~20 İşlem

İkili Arama (Kod B), devasa veri setlerinde Doğrusal Aramaya (Kod A) göre inanılmaz derecede (yaklaşık 50.000 kat) daha performanslıdır.


Soru-2

![2](https://github.com/user-attachments/assets/5cca6aff-34ff-4b3f-b4e2-ebea4ced0d11)

































![3](https://github.com/user-attachments/assets/fe3b1926-dd08-4859-9666-71b03ce8b492)


![4](https://github.com/user-attachments/assets/c964a53f-88db-461a-8b9a-27c2296a5118)





