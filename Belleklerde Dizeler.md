-  Dizi dediğimiz aslında önceden bahsettiğimiz bellek adreslerinin yan yana sıralanmasıdır. 
- Dizilerin adresleri değiştirilemez 
- Kısaca diziler bellekte blok halinde durur ve dizi ismi aslında gizli bir başlangıç adresidir. 
- C dilinde şu mantık vardır . Bir  dizinin adı o dizinin ilk elemanının bellek adresidir 
- Yani siz ```int sayilar [5] ``` dediğinizde sayilar ismi diziye değil o dizinin ilk elemanına verilir 
- Bunu döngüde anlayalım 
sayilar [0] : Başlangıç adresine git 
sayilar [1] : Başlangıç adresinden bir tane int boyutu kadar ( 4 veya 2 byte) ileri git 
sayilar [2] : Başlangıç adresinden 2 int boyutu kadar ileri git

Asıl mantık şöyledir siz [ ] ifadesini kullandığınız an arka planda c derleyicisi * *(sayilar+2)*  işlemini yapar artırır ve durduğu noktayı gösterir.

- Şimdi bir dizinin toplam boyutunu nasıl bulacağımıza bakalım 
```c
#include <stdio.h>

int main() {
    int dizi[] = {10, 20, 30, 40, 50}; // 5 tane int
    
    // Dizinin toplam kapladığı alan (5 * 4 byte = 20 byte)
    size_t toplamBoyut = sizeof(dizi);
    
    // Tek bir elemanın boyutu (4 byte)
    size_t birElemanBoyutu = sizeof(dizi[0]);
    
    // Eleman sayısını hesaplama (En meşhur C hilesidir!)
    size_t elemanSayisi = toplamBoyut / birElemanBoyutu;

    printf("Dizinin toplam boyutu: %zu byte\n", toplamBoyut);
    printf("Dizideki eleman sayısı: %zu\n", elemanSayisi);

    return 0;
}
```

### 2 Boyutlu Dizeler 
- Bellekleri kullanarak tablo kuruyoruz burdada 5 değişkenli bir dize yerine 10 değişkenli 2 dize kullanacağız 

```c
int matris [3][4] //3 satır ve 4 sütun
```
**Peki bellekte ne oluyor?** İşlemci "2 boyutlu" falan anlamaz. O sadece düz bir sıra bilir. Bilgisayar bu 2 boyutlu yapıyı belleğe aslında **tek bir sıra halinde** açar.

- Önce 1. satırı koyar.
    
- Hemen peşine 2. satırı ekler.
    
- Onun peşine 3. satırı ekler.

Bu kısmada bir hesaplama yapacak olursak şu şekilde gerçekleşir 
```c
int matris[3][4];

size_t toplam = sizeof(matris);           // 3 * 4 * 4 byte = 48 byte
size_t satirBoyutu = sizeof(matris[0]);   // Tek bir satır (4 tane int) = 16 byte
size_t elemanBoyutu = sizeof(matris[0][0]); // Tek bir hücre = 4 byte

size_t toplamEleman = toplam / elemanBoyutu; // 48 / 4 = 12 eleman
size_t satirSayisi = toplam / satirBoyutu;   // 48 / 16 = 3 satır
```
### 1. Karakter Dizileri (Strings): C'de "Kelime" Diye Bir Şey Yoktur!

C dilinde aslında `string` diye bir veri tipi yoktur. Bizim "merhaba" dediğimiz şey, aslında yan yana dizilmiş `char` (karakter) kutularıdır.

**Kritik Detay:** Bilgisayar bir karakter dizisinin nerede bittiğini anlamak için sonuna görünmez bir **`\0` (Null Terminator)** karakteri koyar.

- Eğer "Talha" yazarsan, bellekte 5 değil **6 byte** yer kaplar. Çünkü sonuncu kutu "Burası bitti!" diyen o gizli dur işaretidir.
    

---

### 2. Dizi-Pointer Çürümesi (Array Decay)

İşte en büyük tuzak burası! Dizini bir fonksiyona gönderdiğin an, dizi "dizi" olmayı bırakır ve sadece **basit bir pointer'a** dönüşür.

- Hatırlarsan `sizeof(dizi)` ile toplam boyutu buluyorduk ya?
    
- Eğer diziyi fonksiyona gönderip orada `sizeof` yaparsan, sana dizinin boyutunu değil, sadece **adresin boyutunu (8 byte)** verir.
    

Bu yüzden C'de bir fonksiyona dizi gönderirken yanında mutlaka **"Kaç elemanlı bu?"** bilgisini (size) de göndermen gerekir. Yoksa fonksiyon dizinin sonunu bulamaz ve komşunun verilerini okumaya başlar.

---

### 3. Buffer Overflow

C dili sana çok güvenir, ama bu güven bazen başına iş açar.

- `int dizi[5];` dedin ama yanlışlıkla `dizi[10] = 500;` yazmaya çalıştın.
    
- C seni durdurmaz! "Tamam abi, 5. kutudan sonra 5 kutu daha ileri gidip oraya 500'ü yazıyorum" der.
    
- Oysa orası başka bir değişkenin yeri olabilir, işletim sisteminin bir parçası olabilir. İşte hacker'ların en sevdiği açık (Buffer Overflow) tam olarak buradan çıkar.
    

---

### 4. Çok Boyutlu Diziler (3D, 4D ve Ötesi)

Sadece 2 boyutlu (matris) değil, 3 boyutlu diziler de yapabilirsin.

- `int küp[3][3][3];` Bunu bir **Rubik Küpü** gibi düşünebilirsin. Bellekte yine dümdüz bir sıra halinde dururlar ama sen onlara 3 farklı koordinatla ulaşırsın. Unreal Engine'de 3D dünyayı veya renkleri (RGB) bu şekilde temsil edebiliriz.
### 1. Dizileri İlklendirme (Initialization) Hileleri

Dizi tanımlarken sadece `int dizi[5];` demek yetmez. İçine veri koymanın farklı yolları vardır ve bunlar bellekte farklı sonuçlar doğurur:

- **Eksik İlklendirme:** `int dizi[5] = {1, 2};` dersen, ilk iki kutu 1 ve 2 olur. **Peki ya diğerleri?** C, geri kalan bütün kutuları otomatik olarak **0** ile doldurur.
    
- **Tamamını Sıfırlama:** `int dizi[100] = {0};` Bu, 100 kutunun tamamını tek hamlede tertemiz (sıfır) yapmanın en kısa yoludur.
    
- **Boyut Belirtmeme:** `int dizi[] = {1, 2, 3};` dersen, derleyici kutuları sayar ve "Tamam, bu dizi 3 elemanlı olsun," der.
    

---

### 2. Pointer Dizileri (Array of Pointers)

Burayı çok dikkatli dinle, çünkü bu "pointer" ve "dizi" kavramlarının birleştiği en üst noktadır. Sadece sayıları değil, **adresleri de yan yana dizebilirsin.**

`int *ptrDizisi[3];` Bu ne demek?

- Bu bir dizidir.
    
- İçindeki her bir eleman, başka bir `int` değişkeninin **adresini** tutan bir pointer'dır.
    

Bu yapı özellikle farklı uzunluktaki kelimeleri (isim listesi gibi) bellekte minimum israfla tutmak için kullanılır.
[[Bellek Yönetimi]]