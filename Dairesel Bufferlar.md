[[Bellek Yönetimi]]
Dairesel buffer, sabit boyutlu bir dizinin **son elemanından sonra tekrar başa dönen** veri yapısıdır. Sanki dizi bir halka gibi uç uca bağlıdır.
```c
Normal Dizi:    [0] [1] [2] [3] [4]  → son gelince bitti
Dairesel:       [0] [1] [2] [3] [4]  → son gelince başa döner
                 ↑___________________↓
```
## Neden İhtiyaç Duyulur?
Normal kuyruk (queue) ile şu problem yaşanır
```c
Ekle: [A] [B] [C] [D] [E]
Sil:  [ ] [ ] [C] [D] [E]   ← baş boşta kalır, yer israf
```

```
→ Veri gelir → sıraya girer → okunur → yer boşalır → başa döner →
     ↑___________________________________________________|
```

## Kullanım Alanları

### 1. Ses / Video Streaming

Spotify, YouTube gibi uygulamalar veriyi dairesel buffer'da tutar. Ağ hızlı gelirse buffer dolar, yavaşlarsa buffer'dan tüketilir. **Takılma (buffering)** tam olarak budur.

### 2. Klavye / Mouse Girişi

İşletim sistemi tuş basışlarını dairesel buffer'a yazar. İşlemci meşgulken buffer'dan okur. Hızlı yazınca tuşlar kaybolmaz, sıraya girer.

### 3. UART / Seri Haberleşme (Embedded)

Mikrodenetleyiciler (Arduino, STM32) seri porttan gelen veriyi dairesel buffer'a alır. İşlemci hazır olunca buffer'dan okur. Veri kaybolmaz.

### 4. Log Sistemi

Sunucular son N log kaydını dairesel buffer'da tutar. Buffer dolunca en eski log silinir, yenisi yazılır. Sonsuz bellek kullanılmaz.

### 5. Oyun Motorları

Oyuncunun son X hareketini buffer'da tutar. **Geri alma (undo)** veya **replay** özelliği böyle çalışır.

---

## Normal Kuyruk vs Dairesel Buffer

|Özellik|Normal Kuyruk|Dairesel Buffer|
|---|---|---|
|Bellek kullanımı|İsraflı|Verimli|
|Hız|O(n) kaydırma|O(1)|
|Sabit boyut|Hayır|Evet|
|Kullanım alanı|Genel|Gerçek zamanlı sistemler|
### Yapı 
```c
#define BOYUT 5

typedef struct {
    int veri[BOYUT];
    int bas;    // okuma noktası
    int son;    // yazma noktası
    int sayi;   // içindeki eleman sayısı
} DaireselBuffer;
```

## Temel Operasyonlar 

#### Başlatma
```c
void baslat(DaireselBuffer* db) {
    db->bas  = 0;
    db->son  = 0;
    db->sayi = 0;
}
```


#### Dolu Boş Kontrolü
```c
int doluMu(DaireselBuffer* db) {
    return db->sayi == BOYUT;
}

int bosMu(DaireselBuffer* db) {
    return db->sayi == 0;
}
```

### Ekleme 
```c
int ekle(DaireselBuffer* db, int deger) {
    if (doluMu(db)) {
        printf("Buffer dolu!\n");
        return -1;
    }

    db->veri[db->son] = deger;
    db->son = (db->son + 1) % BOYUT;  // ← dairesel hareket
    db->sayi++;
    return 0;
}
```

```c
int cikar(DaireselBuffer* db, int* deger) {
    if (bosMu(db)) {
        printf("Buffer boş!\n");
        return -1;
    }

    *deger = db->veri[db->bas];
    db->bas = (db->bas + 1) % BOYUT;  // ← dairesel hareket
    db->sayi--;
    return 0;
}
```

#### Görsel Olarak 
```c
Başlangıç:   [ ][ ][ ][ ][ ]   bas=0, son=0
              ↑
             bas/son

Ekle A,B,C:  [A][B][C][ ][ ]   bas=0, son=3
              ↑        ↑
             bas       son

Çıkar A,B:   [ ][ ][C][ ][ ]   bas=2, son=3
                    ↑  ↑
                   bas son

Ekle D,E,F:  [F][ ][C][D][E]   bas=2, son=1
              ↑  ↑  ↑
             son    bas         ← son başı geçti, dairesel döndü!
```


### Gerçek Hayat Örneği 
```c
## Kullanım Alanları

### 1. Ses / Video Streaming

Spotify, YouTube gibi uygulamalar veriyi dairesel buffer'da tutar. Ağ hızlı gelirse buffer dolar, yavaşlarsa buffer'dan tüketilir. **Takılma (buffering)** tam olarak budur.

### 2. Klavye / Mouse Girişi

İşletim sistemi tuş basışlarını dairesel buffer'a yazar. İşlemci meşgulken buffer'dan okur. Hızlı yazınca tuşlar kaybolmaz, sıraya girer.

### 3. UART / Seri Haberleşme (Embedded)

Mikrodenetleyiciler (Arduino, STM32) seri porttan gelen veriyi dairesel buffer'a alır. İşlemci hazır olunca buffer'dan okur. Veri kaybolmaz.

### 4. Log Sistemi

Sunucular son N log kaydını dairesel buffer'da tutar. Buffer dolunca en eski log silinir, yenisi yazılır. Sonsuz bellek kullanılmaz.

### 5. Oyun Motorları

Oyuncunun son X hareketini buffer'da tutar. **Geri alma (undo)** veya **replay** özelliği böyle çalışır.

---

## Normal Kuyruk vs Dairesel Buffer

|Özellik|Normal Kuyruk|Dairesel Buffer|
|---|---|---|
|Bellek kullanımı|İsraflı|Verimli|
|Hız|O(n) kaydırma|O(1)|
|Sabit boyut|Hayır|Evet|
|Kullanım alanı|Genel|Gerçek zamanlı sistemler|

#include <stdio.h>
#define BOYUT 5

// ... (yukarıdaki tüm kodlar)

int main() {
    DaireselBuffer db;
    baslat(&db);

    ekle(&db, 10);
    ekle(&db, 20);
    ekle(&db, 30);

    int deger;
    cikar(&db, &deger);
    printf("Çıkan: %d\n", deger);  // 10

    ekle(&db, 40);
    ekle(&db, 50);
    ekle(&db, 60);

    // Tüm elemanları çıkar
    while (!bosMu(&db)) {
        cikar(&db, &deger);
        printf("%d ", deger);  // 20 30 40 50 60
    }

    return 0;
}
```

### Anlatmadıklarımız

**1. Overwrite Modu** Buffer dolunca hata vermek yerine **en eski veriyi sil, yenisini yaz.** Log sistemleri böyle çalışır.

**2. Producer-Consumer Problemi** Bir taraf yazar, diğer taraf okur. İkisi farklı hızda çalışırsa ne olur? Nasıl yönetilir?

**3. Thread-Safe Buffer** İki farklı thread aynı anda yazıp okursa **veri bozulur.** Mutex ve semafor ile nasıl korunur?

**4. Dinamik Boyutlu Buffer** Sabit 5 yerine **ihtiyaca göre büyüyen** buffer nasıl yapılır?

---

### Senin İçin Tavsiyem

Şu an için **temel buffer yeterli.** Sadece şunu not haritana ekle:

```
Temel Buffer        ✅ tamamlandı
Overwrite Modu      ← öğrenmeye değer
Producer-Consumer   ← thread konusuna girince bak
Thread-Safe         ← işletim sistemi konusuna girince bak
```