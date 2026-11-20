### 1. Kuyruk (Queue) Nedir ve Nasıl Çalışır?

Kuyruk, verilerin bir uçtan eklendiği, diğer uçtan çıkarıldığı doğrusal bir veri yapısıdır.

- **FIFO Mantığı:** "İlk Giren İlk Çıkar" (First In First Out) veya "Son giren son çıkar" (LILO) prensibiyle çalışır.
    
- **Gerçek Yaşam ve Sistemlerdeki Yeri:** Banka kuyrukları veya duraklardaki sıralar nasıl çalışıyorsa bilgisayardaki kuyruklar da aynıdır. İşletim sistemlerinde işlemciye gönderilen görevler, yazıcıya gönderilen belgeler veya iki sistem arası tampon (buffer) bellekler bu yapıyı kullanır.
    
[[Bellek Yönetimi]]
### 2. Temel Kuyruk İşlemleri

Yığıttaki `push` ve `pop` işlemlerinin yerini burada şunlar alır:

- **`enqueue` (Ekle):** Yeni bir nesneyi kuyruğun en arkasına (sonuna) ekler.
    
- **`dequeue` (Çıkart):** Kuyruğun en başındaki (önündeki) nesneyi alır ve kuyruktan siler.
    
- **`front` / `getHead`:** Kuyruğun başındaki elemanı silmeden sadece okumanı sağlar.
    
- **İstisnai Durumlar:** Tamamen boş bir kuyruktan eleman çıkarmaya veya okumaya çalışırsan **`EmptyQueueException`** hatası fırlatılır.
    

---

### 3. Bellek Üzerinde Kuyruk Tasarımları

Kuyruk veri yapısını algoritmik olarak inşa etmenin üç temel yolu vardır:

#### A. Dizi Üzerinde Kaydırmalı Kuyruk (Linear Array)

Sabit boyutlu standart bir dizi kullanılır. Ekleme işlemleri dizinin son dolu elemanının bir ilerisine kolayca yapılır.

- **Sorun:** Bir eleman kuyruktan çıkarıldığında (çıkartma işlemi her zaman 0. indisten yapılır), dizideki diğer tüm verilerin birer adım öne kaydırılması gerekir ($K[1]$ -> $K[0]$ gibi). Bu kaydırma işlemi büyük verilerde performans açısından çok maliyetlidir.
    

#### B. Dizi Üzerinde Çevrimsel Kuyruk (Circular Array)

Kaydırma maliyetinden kurtulmak için geliştirilmiş çok zekice bir yöntemdir. Dizi doğrusal değil, kendi etrafında dönen bir halka gibi düşünülür.

- `front` (ön) ve `rear` (arka) adında iki işaretçi (pointer) kullanılır.
    
- Dizinin en sonuna ulaşıldığında yer kalmamışsa, sistem hata vermek yerine dizinin başına (0. indise) bakar. Eğer orada boşluk varsa `rear` işaretçisi başa döner ve ekleme işlemine oradan devam eder.
    
- Sınırları modüler aritmetik ile hesaplandığı için gereksiz bellek kaydırmalarının önüne geçer.
    

#### C. Bağlantılı Liste ile Kuyruk Tasarımı (Linked List)

Tıpkı stack'te olduğu gibi bellek sınırlarına (dizi boyutlarına) takılmamak için düğümlerden (node) oluşan liste yapısı kullanılır. Bellekte yer olduğu sürece kuyruk dinamik olarak uzayabilir.

---

### 4. Öncelikli Kuyruklar (Priority Queues)

Bazen sistemlerde standart FIFO kuralını esnetmek gerekir. Öncelikli kuyruklarda kuyruğa giren nesnelerin bir "öncelik derecesi" vardır.

- Normalde ilk gelen ilk işlenir, ancak sonradan gelen bir verinin öncelik değeri çok yüksekse (örneğin işletim sisteminde donanımı kilitleyen acil bir durum prosesi veya oyunda acil render edilmesi gereken bir frame), bu işlem kuyrukta diğerlerinin önüne geçer.
    
- Sistem, kuyruktan eleman alırken ilk geleni değil, "en yüksek önceliğe sahip grubun ilk gelenini" işleme alır.



n=5 1
m=9 1 4 4
for(i=0 ; n=m; n++);1 6 7 8 9
toplam+= 4 15 t(n)=15