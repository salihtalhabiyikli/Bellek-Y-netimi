[[Bellek Yönetimi]]
### Bellek Adresleri 
- Bilgisayarda tutulan her şeyin bir adresi v bir adres numarası varıdr. Bu adresleri uç uca dizdiğimizde biz bellek yani RAM dediğimiz geçici hafızaları yapıyoruz 
- Değişken: Kutunun içinde ne olduğudur 
- Bellek Adresi : Kutunun o bellekte nerde olduğudur 

- Bilgisayar bir değişkeni bulmak istediğinde direkt olarak bellek adrsine bakar bu adresin formatı genellike 0x7ffe.. tarzında olur. Siz değişkenin adını değilde direkt bu adı yazdırsanızda değişkenin içinde ne varsa o gelir. 

### Pointer Nedir?
- Bir değişken tipi diyebiliriz aslında. Ama diğer değişkenlerden bir farkı vardır. Diğer değişkenler içerisinde bir sayı veya bir harf tutarken pointer yukarıda bahsettiğimiz 0x3tte yapısını yani belleğin adresini tutar. 

Burda iki tane kritik sembol işimize yarayacak ampersan(&) ve yıldız (*)  

Kısaca ampersan değişkene sen nerde oyutuyorsun sorusudur bir değişkenin önüne gelirse onun adresini verir 

Yıldız ise bu adreste ne var sorusudur adres değerinin içeriğini verir
```c
#include <stdio.h>

int main() {
    int sayi = 20;       // Normal bir tam sayı
    int *ptr;            // Bir pointer tanımladık (int tipinde adres tutacak)

    ptr = &sayi;         // ptr'ye, sayi'nin adresini ("kapı numarasını") verdik.

    printf("Sayinin degeri: %d\n", sayi);
    printf("Sayinin adresi (&sayi): %p\n", (void*)&sayi); // %p adres yazdırır
    printf("Pointer'in tuttugu adres: %p\n", (void*)ptr);
    printf("Pointer'in gosterdigi yerdeki deger (*ptr): %d\n", *ptr);

    return 0;
}
```

Burda niye yerini öğrenelim değişkenin adını zaten biz veriyoruz diyebilirsiniz. Önemli olan burada hafızayı yönlendirmek oluyor. Siz değişkendeki tüm verileri okuyarakta girebilirsiniz ama siz direk adresi verdiğinzide bilgisayar kendiside okuyabilir. Bilgisayarın amacı aslında budur. 

- **Değişken adıyla çağırırsan:** Bilgisayar o 1 milyon karakterin tamamını kopyalar ve yeni bir yere yapıştırır. Bu hem RAM'i doldurur hem de bilgisayarı yavaşlatır.
    
- **Adresle (Pointer) çağırırsan:** Bilgisayar sadece "Bak, veri şu adreste başlıyor" der. Sadece küçük bir adres bilgisi (genellikle 8 byte) gönderirsin. Verinin kendisi yerinden kıpırdamaz.

Bu kısmı daha iyi anlamamız için bir benzetme yapalım bir mavi ve bir kırmızı bardak düşünün kırmızı bardak solda mavi bardak sağda dursun. Bu şekilde bir fotoğraf çekelim. ardından bardakların yerlerini değiştirelim. sizce bilgisayar gerçeğimi görür resmimi görür. Bu soruyu iki ihtimalle açıklayalım .

1. Bellek adresini kullanarak değiştirirseniz bilgisayar gerçeği görür. 
2. Değişken adını kullanarak değiştirirseniz bilgisayar resmi görür. 

Bellekler bilgisayarla eş zamanlı çalışır bellek hep bilgisayardadır fakat değişken adları çağırılmadıkça aranmaz bile. Unutmayın ki bir çocuğa x kişisini bul demekle çocuğun elinden tutup götürmek farklı şeylerdir. 
```c
#include <stdio.h>

// Bu fonksiyon sadece "RESİM" üzerinden çalışır (Değişmez)
void yanlisSwap(int a, int b) {
    int gecici = a;
    a = b;
    b = gecici;
}

// Bu fonksiyon "GERÇEK" adresler üzerinden çalışır (DEĞİŞİR!)
void dogruSwap(int *ptr1, int *ptr2) {
    int gecici = *ptr1; // Adresteki değeri al
    *ptr1 = *ptr2;      // Birinci adrese ikinci adresteki değeri yaz
    *ptr2 = gecici;     // İkinci adrese eski değeri yaz
}

int main() {
    int x = 10, y = 20;

    printf("Başlangıç: x=%d, y=%d\n", x, y);

    yanlisSwap(x, y);
    printf("Yanlış Swap Sonrası (Resim): x=%d, y=%d\n", x, y);

    dogruSwap(&x, &y); // Adreslerini (&) gönderiyoruz!
    printf("Doğru Swap Sonrası (Gerçek): x=%d, y=%d\n", x, y);

    return 0;
}
```


### Pointerlarda Boyut Mantığı 
- Değişkenlerin farklı boyutları olabilir fakat pointerlar tamamen tek boyutludur. Yani içindeki değişken ne olursa olsun tek pointerda tek bir boyutta tutulur. (8byte)
- Bu yüzden pointerlar daha hızlı daha stabil ve daha optimizedir.
- Profesyonel sistem yazılımlarında daima pointerın içine en başta 0 tanımlamak programı çökmekten kurtarır.  Pointerın içinin boş kalması ve 0 barındırması farklı şeylerdir 

 


