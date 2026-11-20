# Pointer ile Dinamik Bellek Yönetimi — C Dili
[[Bellek Yönetimi]]
## 1. Pointer Temelleri (Hızlı Hatırlatma)


```c
#include <stdio.h>

int main() {
    int x = 42;
    int *ptr = &x;   // ptr, x'in adresini tutar

    printf("x'in değeri     : %d\n", x);
    printf("x'in adresi     : %p\n", (void*)&x);
    printf("ptr'nin değeri  : %p\n", (void*)ptr);   // adres
    printf("ptr'nin gösterdiği: %d\n", *ptr);        // 42 (dereference)

    *ptr = 100;  // ptr üzerinden x'i değiştir
    printf("x artık         : %d\n", x);  // 100

    return 0;
}

x'in değeri     : 42
x'in adresi     : 0x7ffde8fea164
ptr'nin değeri  : 0x7ffde8fea164
ptr'nin gösterdiği: 42
x artık         : 100
```

```
Bellek:
        ┌────────┐
x  →    │   42   │  adres: 0x1000
        └────────┘
           ↑
ptr →   0x1000
```

---

## 2. Pointer Aritmetiği

c

```c
#include <stdio.h>

int main() {
    int dizi[] = {1 , 20, 30, 40, 50};
    int *ptr = dizi;  // dizi[0]'ın adresini gösterir

    printf("ptr    → %d\n", *ptr);       // 10
    printf("ptr+1  → %d\n", *(ptr+1));   // 20
    printf("ptr+2  → %d\n", *(ptr+2));   // 30

    // ptr++ ile ilerle
    ptr++;
    printf("ptr++  → %d\n", *ptr);       // 20

    ptr += 2;
    printf("ptr+=2 → %d\n", *ptr);       // 40

    // İki pointer arasındaki fark
    int *bas = dizi;
    int *son = &dizi[4];
    printf("Fark   → %ld\n", son - bas); // 4

    return 0;
}
```

```
Bellek (her int 4 byte):
┌────┬────┬────┬────┬────┐
│ 10 │ 20 │ 30 │ 40 │ 50 │
└────┴────┴────┴────┴────┘
1000  1004  1008  1012  1016   ← adresler
 ↑
ptr başlangıçta burada
```

---

## 3. Pointer ile Dinamik Dizi


```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Kaç eleman: ");
    scanf("%d", &n);

    // Heap'te n elemanlık dizi oluştur
    int *dizi = (int *)malloc(n * sizeof(int));
    if (dizi == NULL) {
        fprintf(stderr, "Bellek hatası!\n");
        return 1;
    }

    // Pointer aritmetiği ile doldur
    for (int i = 0; i < n; i++) {
        *(dizi + i) = i * i;   // dizi[i] ile aynı
    }

    // Yazdır
    int *ptr = dizi;
    while (ptr < dizi + n) {
        printf("%d ", *ptr);
        ptr++;
    }
    printf("\n");

    free(dizi);
    dizi = NULL;
    return 0;
}
```

---

## 4. Pointer to Pointer (Çift Pointer)

c

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int x = 42;
    int *ptr  = &x;    // x'i gösterir
    int **pptr = &ptr; // ptr'yi gösterir

    printf("x        = %d\n", x);
    printf("*ptr     = %d\n", *ptr);
    printf("**pptr   = %d\n", **pptr);

    // **pptr üzerinden x'i değiştir
    **pptr = 99;
    printf("x artık  = %d\n", x); // 99

    return 0;
}
```

```
        ┌──────┐      ┌──────┐      ┌──────┐
pptr →  │ 0x200│ →    │ 0x100│ →    │  42  │
        └──────┘      └──────┘      └──────┘
          pptr           ptr             x
```

### Çift Pointer ile 2D Dinamik Dizi

c

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int satir = 3, sutun = 4;

    // int** → satır pointer'larını tut
    int **matris = (int **)malloc(satir * sizeof(int *));
    if (matris == NULL) return 1;

    // Her satır için ayrı bellek ayır
    for (int i = 0; i < satir; i++) {
        matris[i] = (int *)malloc(sutun * sizeof(int));
        if (matris[i] == NULL) {
            // Hata: şimdiye kadar ayrılanları temizle
            for (int j = 0; j < i; j++) free(matris[j]);
            free(matris);
            return 1;
        }
    }

    // Doldur
    for (int i = 0; i < satir; i++)
        for (int j = 0; j < sutun; j++)
            matris[i][j] = i * sutun + j;

    // Yazdır
    for (int i = 0; i < satir; i++) {
        for (int j = 0; j < sutun; j++)
            printf("%4d", matris[i][j]);
        printf("\n");
    }

    // Belleği temizle (ters sıra!)
    for (int i = 0; i < satir; i++)
        free(matris[i]);   // Önce satırları serbest bırak
    free(matris);          // Sonra pointer dizisini
    matris = NULL;

    return 0;
}
```

```
Bellek Yapısı:
matris → [ *satir0 | *satir1 | *satir2 ]
               ↓         ↓         ↓
           [0,1,2,3] [4,5,6,7] [8,9,10,11]
```

---

## 5. Fonksiyona Pointer ile Bellek Geçirme

### Yanlış Yol — Değer ile geçirme

c

```c
#include <stdio.h>
#include <stdlib.h>

// YANLIŞ: pointer'ın kopyası gelir, orijinal değişmez
void bellek_ayir_yanlis(int *ptr, int n) {
    ptr = (int *)malloc(n * sizeof(int)); // sadece lokal kopya değişir
}

int main() {
    int *dizi = NULL;
    bellek_ayir_yanlis(dizi, 5);
    // dizi hâlâ NULL! malloc'un etkisi dışarı yansımadı
    printf("%p\n", (void*)dizi); // 0x0 (NULL)
    return 0;
}
```

### Doğru Yol — Çift Pointer ile geçirme

c

```c
#include <stdio.h>
#include <stdlib.h>

// DOĞRU: pointer'ın adresi gelir, orijinal değiştirilebilir
int bellek_ayir(int **ptr, int n) {
    *ptr = (int *)malloc(n * sizeof(int));
    if (*ptr == NULL) return -1;

    for (int i = 0; i < n; i++)
        (*ptr)[i] = i + 1;

    return 0;
}

void bellek_serbest(int **ptr) {
    if (ptr == NULL || *ptr == NULL) return;
    free(*ptr);
    *ptr = NULL; // orijinal pointer'ı NULL'a çek
}

int main() {
    int *dizi = NULL;
    int n = 5;

    if (bellek_ayir(&dizi, n) != 0) {
        fprintf(stderr, "Hata!\n");
        return 1;
    }

    for (int i = 0; i < n; i++)
        printf("%d ", dizi[i]); // 1 2 3 4 5
    printf("\n");

    bellek_serbest(&dizi);
    printf("dizi = %p\n", (void*)dizi); // NULL

    return 0;
}
```

---

## 6. Dinamik Bağlı Liste

Pointer + dinamik belleğin en güçlü kullanımı:

c

```c
#include <stdio.h>
#include <stdlib.h>

// Düğüm yapısı
typedef struct Dugum {
    int veri;
    struct Dugum *sonraki;
} Dugum;

// Yeni düğüm oluştur
Dugum *dugum_olustur(int veri) {
    Dugum *yeni = (Dugum *)malloc(sizeof(Dugum));
    if (yeni == NULL) return NULL;
    yeni->veri    = veri;
    yeni->sonraki = NULL;
    return yeni;
}

// Başa ekle
void basa_ekle(Dugum **bas, int veri) {
    Dugum *yeni = dugum_olustur(veri);
    if (yeni == NULL) return;
    yeni->sonraki = *bas;
    *bas = yeni;
}

// Sona ekle
void sona_ekle(Dugum **bas, int veri) {
    Dugum *yeni = dugum_olustur(veri);
    if (yeni == NULL) return;

    if (*bas == NULL) {
        *bas = yeni;
        return;
    }

    Dugum *ptr = *bas;
    while (ptr->sonraki != NULL)
        ptr = ptr->sonraki;

    ptr->sonraki = yeni;
}

// Yazdır
void listele(Dugum *bas) {
    Dugum *ptr = bas;
    while (ptr != NULL) {
        printf("%d → ", ptr->veri);
        ptr = ptr->sonraki;
    }
    printf("NULL\n");
}

// Tüm belleği serbest bırak
void listeyi_sil(Dugum **bas) {
    Dugum *ptr = *bas;
    while (ptr != NULL) {
        Dugum *silinecek = ptr;
        ptr = ptr->sonraki;
        free(silinecek);
    }
    *bas = NULL;
}

int main() {
    Dugum *liste = NULL;

    sona_ekle(&liste, 10);
    sona_ekle(&liste, 20);
    sona_ekle(&liste, 30);
    basa_ekle(&liste, 5);

    listele(liste);
    // 5 → 10 → 20 → 30 → NULL

    listeyi_sil(&liste);
    printf("Liste silindi: %p\n", (void*)liste); // NULL

    return 0;
}
```

```
Liste yapısı bellekte:

bas → [5|*] → [10|*] → [20|*] → [30|NULL]
       heap     heap      heap      heap
```

---

## 7. Özet Tablo

|Kavram|Sözdizimi|Açıklama|
|---|---|---|
|Pointer tanımla|`int *ptr`|int'e pointer|
|Adres al|`ptr = &x`|x'in adresini al|
|Dereference|`*ptr`|pointer'ın gösterdiği değer|
|Çift pointer|`int **pptr`|pointer'ın pointer'ı|
|Dinamik ayır|`malloc(n * sizeof(int))`|heap'te yer aç|
|Serbest bırak|`free(ptr); ptr = NULL`|belleği geri ver|
|Pointer ilerlet|`ptr++` veya `ptr + i`|sonraki elemana git|