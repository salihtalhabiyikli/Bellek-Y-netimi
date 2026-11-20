- Hash tabloları exel benzeri veri ve veri karşılıklarını saklamak için kullanılan bir veri yapısıdır. 
- Hash mantığında  bir fonksiyondur. Fonksiyona belirli parametreler göndererek bir tablo yapısına uygun şekilde async formatında deploy etmektir 
### Hash Fonksiyonu Nasıl Çalışır?
[[Bellek Yönetimi]]
Diyelim ki 10 elemanlık bir dizimiz var. "Elma" kelimesini saklamak istiyoruz.

- `hash("Elma")` fonksiyonu bize `4567` gibi büyük bir sayı üretir.
    
- Bu sayıyı dizi boyutuna böleriz: `4567 % 10 = 7`.
    
- "Elma" verisi dizinin **7. indeksine** yerleştirilir.
### Temel Kavramlar 
***Hash Fonksiyonu*** : Anahtarı alıp bir dizi indeksine çeviren fonksiyondur. 
```c
index=hash(key)% tablo_boyutu
```
> Eğer iki farklı fonksiyonda aynı keyi kulanırsnaız çakışma meydana gelir bu çakışmayı önlemenin veya çözmein iki yolu vardır  

## Zincirleme 
- Her dizi bildiğiniz üzere heap alanında bir bağlı liste tutar Çalışma olduğu anda bu şerit yapıya yeni bir düğüm eklenir. İki listeyi tendak fonksiyondur
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define TABLO_BOYUTU 10

// Bağlı liste düğümü
typedef struct Dugum {
    char *anahtar;
    int deger;
    struct Dugum *sonraki;
} Dugum;

// Hash tablosu
Dugum *tablo[TABLO_BOYUTU];

// Hash fonksiyonu
int hash(const char *anahtar) {
    int toplam = 0;
    while (*anahtar)
     toplam += *anahtar++;
    return toplam % TABLO_BOYUTU;
}

// Ekleme
void ekle(const char *anahtar, int deger) {
    int indeks = hash(anahtar);
    Dugum *yeni = malloc(sizeof(Dugum));
    yeni->anahtar = strdup(anahtar);
    yeni->deger   = deger;
    yeni->sonraki = tablo[indeks]; // Başa ekle
    tablo[indeks] = yeni;
}

// Arama
int ara(const char *anahtar) {
    int indeks = hash(anahtar);
    Dugum *curr = tablo[indeks];
    while (curr) {
        if (strcmp(curr->anahtar, anahtar) == 0)
            return curr->deger;
        curr = curr->sonraki;
    }
    return -1; // Bulunamadı
}

int main() {
    memset(tablo, 0, sizeof(tablo));

    ekle("ali",  25);
    ekle("veli", 30);
    ekle("ayse", 22);

    printf("ali  -> %d\n", ara("ali"));   // 25
    printf("veli -> %d\n", ara("veli"));  // 30
    printf("ayse -> %d\n", ara("ayse"));  // 22

    return 0;
}
```

### Açık Adresleme 
- Çakışma olduğu anda tabloda boş hücreler taranır 
```c
#include <stdio.h>
#include <string.h>

#define BOYUT 10

typedef struct {
    char anahtar[50];
    int  deger;
    int  dolu;   // 1 = dolu, 0 = boş
} Hucre;

Hucre tablo[BOYUT];

int hash(const char *anahtar) {
    int toplam = 0;
    while (*anahtar)
        toplam += *anahtar++;
    return toplam % BOYUT;
}

void ekle(const char *anahtar, int deger) {
    int indeks = hash(anahtar);
    // Boş yer bulana kadar ilerle
    while (tablo[indeks].dolu) {
        indeks = (indeks + 1) % BOYUT;
    }
    strcpy(tablo[indeks].anahtar, anahtar);
    tablo[indeks].deger = deger;
    tablo[indeks].dolu  = 1;
}

int ara(const char *anahtar) {
    int indeks = hash(anahtar);
    while (tablo[indeks].dolu) {
        if (strcmp(tablo[indeks].anahtar, anahtar) == 0)
            return tablo[indeks].deger;
        indeks = (indeks + 1) % BOYUT;
    }
    return -1;
}
```
## Karşılaştırma

|Özellik|Zincir (Chaining)|Açık Adresleme|
|---|---|---|
|Bellek|Ekstra pointer kullanır|Daha verimli|
|Çakışma yönetimi|Kolay|Silme zor|
|Cache performansı|Düşük|Yüksek|
|Doluluk oranı|Esnektir|~%70'i geçmemeli|
