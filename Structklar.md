## 1. Struct Nedir? (Farklılıkların Birliği)
[[Bellek Yönetimi]]
Dizilerde (Arrays) herkesin aynı tipte olma zorunluluğu vardı. Ama gerçek hayattaki bir nesneyi (örneğin bir `Kitap`) tek bir tiple ifade edemezsin Kısaca farklı veri tipleri kullanarak bir değişken sınıfı tanımlıyoruz.
> Structlar nesne tabanlı değildir bir gnc çalıştırmaz veya kalıtım kullanmaz.

- Adı (Karakter dizisi)
    
- Sayfa Sayısı (Tam sayı)
    
- Fiyatı (Ondalıklı sayı)
    

Struct, bu farklı tipleri tek bir paket altında toplar.
```c
struct Kitap {
    char ad[50];
    int sayfaSayisi;
    float fiyat;
}; // Noktalı virgülü asla unutma, C'nin en sevdiği hata budur!
```

## 2. Bellek Yerleşimi (Derin Mevzu: Alignment & Padding)

"Hiçbir yeri atlamayalım" demiştik ya, işte çoğu kişinin geçtiği ama mülakatlarda karşına çıkacak o kısım burası.

Bir struct tanımladığında, içindeki değişkenler belleğe **tanımlanma sırasına göre** dizilir. Ama bir bit yeniği var: **Memory Padding.**

İşlemciler belleği genellikle 4 veya 8 byte’lık bloklar halinde okur. Eğer sen şöyle bir struct yaparsan:

```c
struct Veri {
    char a;    // 1 Byte
    int b;     // 4 Byte
};
```

Sence bu struct kaç byte yer kaplar? `1 + 4 = 5` mi? **Hayır, genellikle 8 byte kaplar.**

**Neden?** Çünkü işlemci `int b` değerini okurken zorlanmasın diye, `char a`'dan sonra **3 byte boşluk (padding)** bırakır ki `int b` tam 4'ün katı olan bir adresten başlasın.

> **Profesyonel İpucu:** Eğer değişkenleri büyükten küçüğe (double -> int -> char) dizersen, bellekteki bu boşluk israfını (padding) minimuma indirirsin.

## 3. `typedef` Kullanımı (Kısayol Oluşturma)

Her seferinde `struct Kitap benimKitabim;` yazmak yorucudur. `typedef` kullanarak bu yapıya yeni ve kısa bir isim verebilirsin:

```c
typedef struct {
    char model[20];
    int yil;
} Araba;

// Artık sadece "Araba" yazabilirsin:
Araba benimArabam = {"Tesla", 2024};
```

---
## 4. Struct ve Pointerlar 

Structlar ile çalışırken en çok karşılaşacağın şey pointerlardır. Bir struct'a pointer üzerinden ulaşıyorsan nokta (`.`) yerine **ok (`->`)** işareti kullanırsın.

```c
Araba *ptr = &benimArabam;

printf("Yıl: %d", ptr->yil); 
// Bu aslında şununla aynıdır: (*ptr).yil
```

Bu `->` işareti, "Bu adrese git ve içindeki `yil` kutusunu bul" demektir.

---

## 5. İç İçe Geçmiş (Nested) Structlar

Bir struct'ın içine başka bir struct'ı üye olarak koyabilirsin. Gerçek dünya projelerinde (ve Unreal Engine'de) her şey böyle inşa edilir.

``` c
struct Personel {
    char isim[30];
    struct Tarih iseGirisTarihi; // Struct içinde struct
};
```

---

## 6. Struct Dizileri (Veritabanı Mantığı)

Bir şirketteki 100 personeli tutmak istersen, struct'lardan oluşan bir dizi yaparsın:

```c
struct Personel liste[100];
liste[0].iseGirisTarihi.yil = 2026;
```

