- Bellek aslında RAM'in kendisidir. Ram bilgisayarın geçici hafızasıdır belli bir veri alır işler ve bilgisayar kapandığında siler. Yani bilgisayarımızın ram kapasitesi onun ne kadar bilgiyi aynı anda işleyebileceğini gösterir aslında 
[[Bellek Yönetimi]]
  ## 1. Bellek Bölgeleri

C  çalışırken bellek 4 ana bölgeye ayrılır:

```
┌─────────────────────┐  Yüksek Adres
│       STACK         │  ← Yerel değişkenler, fonksiyon çağrıları
├─────────────────────┤
│         ↓           │
│      (boş alan)     │
│         ↑           │
├─────────────────────┤
│        HEAP         │  ← Dinamik bellek (malloc, calloc...)
├─────────────────────┤
│    BSS Segment      │  ← Başlatılmamış global/static değişkenler
├─────────────────────┤
│    Data Segment     │  ← Başlatılmış global/static değişkenler
├─────────────────────┤
│    Text Segment     │  ← Program kodu (read-only)
└─────────────────────┘  Düşük Adres
```

### Stack Belleği 
Bu kısım tamamen otomatik yönetilir fonksiyon çalışmayı durdurduğu an temizlenir 
- Siz bir değişken tanımladığınızda bu stackte doğar 
```c
#include <stdio.h>

void fonksiyon() {
    int x = 10;        // Stack'te oluşturulur
    int y = 20;        // Stack'te oluşturulur
    int z = x + y;     // Stack'te oluşturulur

    printf("z = %d\n", z);
} // ← Fonksiyon bitince x, y, z otomatik silinir

int main() {
    fonksiyon();
    // Burada x, y, z artık erişilemez
    return 0;
}
```

### => Stack ne işe yarar 
- Çok hızlıdır pointer burda daha hızlı işlem yapabilir 
- Boyutu 1 ve 8 mb ile sınırlıdır 
- Veriler düzenli değildir sürekli çökebilen bir yapıdır

### Heap Belleği 
- Bu kısım manuel yönetilir. Algoritmalarda alan ayırdığımız ve ekleme yaptığımız yapılar burda oluşturulur. 
- Burda genellikle `malloc`, `calloc`, `realloc`, `free` gibi metodlar kullanılır. Şimdi bu metodların ne işe yaradığına bakalım. 
### Malloc: Bellek Ayırma 
```c
include<stdio.h> 
include<stdlib.h>

int main(){
int n=5;
int*dizi=(int*)malloc(n*sizeof(int));

if(dizi==NULL){
printf("Bellek ayırma başarısız");
}
return 1;

for (int i =0;i<n;i++)
{
	dizi[i]=i*10;
}
free(dizi); //Belleği temizle 
dizi=NULL; //Pointerı temizle 

return 0;


}
```
![[Pasted image 20260412104818.png]]
Kodda  ```int*dizi=(int*)malloc(n*sizeof(int));``` satırı karışık gelebilir şöyle açıklayalım 

**`malloc(n * sizeof(int))`** — Önce içerideki hesap yapılır. `n = 5` olduğu için `sizeof(int)` genellikle 4 byte'tır, yani `5 * 4 = 20 byte` talep edilir. `malloc` bu kadar boş alan bulup heap'te ayırır ve o bloğun başlangıç adresini döndürür.

**`malloc`'un döndürdüğü değer `void *` türündedir** — yani "bir yerin adresi var ama ne tür veri olduğunu bilmiyorum" anlamına gelir. C++ bunu kabul etmez, saf C'de ise cast olmadan da çalışır ama açıkça yazmak iyi alışkanlıktır.

**`(int *)`** — Bu bir **tür dönüşümü (cast)**. `malloc`'un döndürdüğü `void *` türündeki adresi, "bu adres `int` dizisinin başlangıcıdır" diye işaretler. Artık derleyici bu pointer üzerinden `int` okuma/yazma yapacağını bilir.

**`int *dizi`** — `dizi` değişkeni bir `int` pointer'ıdır. Heap'teki bloğun adresini burada saklar. Köşeli parantez ile `dizi[0]`, `dizi[1]` şeklinde kullanılabilmesinin sebebi budur.

Yani tek cümlede: **"Heap'te 20 byte ayır, o bloğun adresini `int` dizisi olarak işaretle ve `dizi` değişkenine ata."**


### Calloc : Sıfırlanmış Bellek Ayırma

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n = 5;

    // malloc'tan farkı: tüm baytları 0 ile doldurur
    int *dizi = (int *)calloc(n, sizeof(int));

    if (dizi == NULL) {
        printf("Bellek ayırma başarısız!\n");
        return 1;
    }

    // Tüm elemanlar zaten 0
    for (int i = 0; i < n; i++) {
        printf("dizi[%d] = %d\n", i, dizi[i]); // hepsi 0 çıkar
    }

    free(dizi);
    dizi = NULL;

    return 0;
}
```

### `realloc` — Belleği Yeniden Boyutlandırma

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *dizi = (int *)malloc(3 * sizeof(int));

    if (dizi == NULL) return 1;

    dizi[0] = 1;
    dizi[1] = 2;
    dizi[2] = 3;

    // Diziyi 3'ten 6 elemana genişlet
    int *yeni_dizi = (int *)realloc(dizi, 6 * sizeof(int));

    if (yeni_dizi == NULL) {
        printf("realloc başarısız!\n");
        free(dizi); // eski belleği serbest bırak
        return 1;
    }

    dizi = yeni_dizi; // güvenli atama

    dizi[3] = 4;
    dizi[4] = 5;
    dizi[5] = 6;

    for (int i = 0; i < 6; i++) {
        printf("dizi[%d] = %d\n", i, dizi[i]);
    }

    free(dizi);
    dizi = NULL;

    return 0;
}
```

---
## 4. Yaygın Bellek Hataları

### 4.1 Memory Leak (Bellek Sızıntısı)

```c
#include <stdlib.h>

void sizan_fonksiyon() {
    int *ptr = (int *)malloc(100 * sizeof(int));
    // free(ptr) unutulmuş! → Bellek sızıntısı
}

int main() {
    for (int i = 0; i < 1000; i++) {
        sizan_fonksiyon(); // Her çağrıda bellek sızar
    }
    return 0;
}
```

✅ **Doğrusu:**
```c
void dogru_fonksiyon() {
    int *ptr = (int *)malloc(100 * sizeof(int));
    if (ptr == NULL) return;
    // ... kullan ...
    free(ptr);  // Mutlaka serbest bırakn
    ptr = NULL;
}
```

--- 
### 4.2 Dangling Pointer 
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr = (int *)malloc(sizeof(int));
    *ptr = 42;

    free(ptr); // Bellek serbest bırakıldı

    // HATA: ptr hâlâ eski adresi gösteriyor
    printf("%d\n", *ptr); // Tanımsız davranış

    // ÇÖZÜM: free'den sonra NULL atayın
    ptr = NULL;

    return 0;
}
```
---
### 4.3 Double Free 
- Bu programı çökertmek içindir
```c
#include <stdlib.h>

int main() {
    int *ptr = (int *)malloc(sizeof(int));

    free(ptr);
    free(ptr); // HATA: Çift free → program çöker!

    // ÇÖZÜM:
    // free(ptr);
    // ptr = NULL;  → NULL pointer'ı free etmek güvenlidir

    return 0;
}
```
---

### 4.4 Buffer Overflow (Taşma)

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *dizi = (int *)malloc(5 * sizeof(int));

    for (int i = 0; i <= 5; i++) { // ← i <= 5 HATALI! (0-4 olmalı)
        dizi[i] = i; // dizi[5] sınır dışı! → Undefined Behavior
    }

    free(dizi);
    return 0;
}
```
---

## 5. Struct ile Dinamik Bellek

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char *isim;
    int yas;
} Kisi;

Kisi *kisi_olustur(const char *isim, int yas) {
    Kisi *k = (Kisi *)malloc(sizeof(Kisi));
    if (k == NULL) return NULL;

    k->isim = (char *)malloc(strlen(isim) + 1); // +1 null terminator için
    if (k->isim == NULL) {
        free(k);
        return NULL;
    }

    strcpy(k->isim, isim);
    k->yas = yas;

    return k;
}

void kisi_sil(Kisi *k) {
    if (k == NULL) return;
    free(k->isim);  // Önce iç belleği serbest bırak
    free(k);        // Sonra struct'ı serbest bırak
}

int main() {
    Kisi *k = kisi_olustur("Ahmet", 25);

    printf("İsim: %s, Yaş: %d\n", k->isim, k->yas);

    kisi_sil(k);
    k = NULL;

    return 0;
}
```

---
## Altın Kurallar

| Kural                                 | Açıklama                            |
| ------------------------------------- | ----------------------------------- |
| Her `malloc` için `free`              | Bellek sızıntısını önler            |
| `free` sonrası `NULL` ata             | Dangling pointer'ı önler            |
| `NULL` kontrolü yap                   | malloc başarısız olabilir           |
| `realloc`'u geçici pointer ile kullan | Başarısız olursa orijinal kaybolmaz |
| Sınır kontrolü yap                    | Buffer overflow'u önler             |
