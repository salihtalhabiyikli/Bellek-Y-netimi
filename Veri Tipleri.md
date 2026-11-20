- Bidliğimiz üzere 4 temel veri tipimiz var ``` char int floar double```
[[Bellek Yönetimi]]

| **Veri Tipi** | **Bellek Boyutu (Tipik)** | **Kullanım Amacı**                                    |
| ------------- | ------------------------- | ----------------------------------------------------- |
| `char`        | 1 Byte (8 bit)            | Karakterler ve küçük tam sayılar (-128 ile 127 arası) |
| `int`         | 4 Byte (32 bit)           | Standart tam sayılar                                  |
| `float`       | 4 Byte (32 bit)           | Tek duyarlıklı ondalıklı sayılar                      |
| `double`      | 8 Byte (64 bit)           | Çift duyarlıklı (daha hassas) ondalıklı sayılar       |


```c
#include <stdio.h>

int main() {
    printf("Char boyutu: %zu byte\n", sizeof(char));
    printf("Int boyutu: %zu byte\n", sizeof(int));
    printf("Short int boyutu: %zu byte\n", sizeof(short));
    printf("Long int boyutu: %zu byte\n", sizeof(long));
    printf("Long long boyutu: %zu byte\n", sizeof(long long));
    printf("Double boyutu: %zu byte\n", sizeof(double));
    
    return 0;
}
```

Bu temel yapıları oturttuğumuzda, veri yapılarının (structs, arrays, pointers) belleği nasıl organize ettiğini çok daha rahat kavrayacaksın. Hazırsan bu tiplerin "signed" (işaretli) ve "unsigned" (işaretsiz) olarak bellekte nasıl temsil edildiğine veya doğrudan pointer mantığına geçebiliriz.

> Burdaki %zu mantığına gelecek olursak size_t ile dönecek değerler vardır. Bu demektirki bu  bir sayıdır fakat özel bir tiplidir korunması gerekir.  bu doğrudan kullanıcıya iletilecek olan değer. Bu tipide %zu olarak gösteriyoruz 
	Fakat bu kodda size t satırını göremeyebilirsiniz size of dediğinizde sistem bir boyutla birlikte o veriyide çektiği için size t ofun içerisinde çalışacaktır 
 Burda  bunu kullanmamızın sebebide büyük veirlerle çalışırken sistemin hata yapmasını önlemektir.

### Signed and Unsigned Variable Types

- Böyle yazdığıma bakmayın işaretli ve işaretsiz verileri kastettim 
- İşaret aslında bilgisayar dünyasında bir sayının pozitifmi negatifmi olduğudur. signed değişkenler hem pozitif hem negatif değişkenler tutabilir 
- Fakat unsignet değişkenler sadece pozitif sayıları alabiliyor 
- Bu bize bellekteki verinin nasıl kullanılacağına karar veren mekanizmayı oluşturuyor. 
> 1 byte değerinde 8 bitlik bir veri alanı düşünelim biliyoruzki bnun içini 0 ve 1ler dolduracak 
- Baştaki sayı 0 sa veri pozitif 1 se negatif sayılıyor 
- Output değerinden çok emin olduğumuz dosyalarda sadece pozitif değer kullanabilriz mesela 
- Bu kısım hem optimizasyonu hemde süremi biraz uzatır 
- **Signed yaparsak:** En baştaki 1. bit, "sayının işareti" için ayrılır. Eğer o bit `1` ise sayı negatif, `0` ise pozitiftir. Bu yüzden sayı yazmak için bize sadece 7 bit kalır.
    
- **Unsigned yaparsak:** "İşaretle işim olmaz, hepsi pozitif!" deriz. 8 bitin tamamını sayı yazmak için kullanırız.
### Neden Unsigned Kullanırız? (Menzil Meselesi)

Eğer bir değişkenin (örneğin yaşın veya bir dosya boyutunun) asla negatif olmayacağından eminsen `unsigned` kullanmak sana **iki kat daha fazla yer** kazandırır

|**Veri Tipi**|**Alt Limit**|**Üst Limit**|
|---|---|---|
|`signed char`|-128|127|
|`unsigned char`|0|**255**|
|`signed int`|-2,147,483,648|2,147,483,647|
|`unsigned int`|0|**4,294,967,295**|
- Yazdığımız değişkenler ise varsayılan olarak signed kabul edilir 
```c
#include <stdio.h>

int main() {
    // İşaretli (Negatif olabilir)
    signed int derece = -20; 
    
    // İşaretsiz (Sadece pozitif, %u ile yazdırılır)
    unsigned int yas = 20; 
    
    printf("Hava sıcaklığı: %d derece\n", derece);
    printf("Talha'nın yaşı: %u\n", yas);
    
    // EĞER unsigned bir değişkene negatif sayı verirsen:
    unsigned int hata = -1;
    printf("Hatalı çıktı: %u\n", hata); // Burası çok büyük, saçma bir sayı basacaktır!

    return 0;
}
```

### SHORT VE LONG 
- Bir tam sayı verisi her zaman aynı boyutta olmak zorunda değildir. Bunun boyutunu ayarlayabilirsiniz. 
- Short ve long keyleri veri tipinin bellekte ne kadar yer kaplayacağını yani hangi aralıkta değer alabileceğini belirler. 
-  **`short int` (veya sadece `short`):** "Bana çok büyük sayılar lazım değil, bellekte az yer kaplasın" dediğin durumlar içindir. Genellikle **2 Byte** yer kaplar.
    
- **`long int` (veya sadece `long`):** Daha büyük sayılar için kullanılır. Genellikle **4 veya 8 Byte** yer kaplar (sisteme göre değişir).
    
- **`long long int`:** "Bana devasa bir sayı lazım" dediğin yerdir. Kesinlikle en az **8 Byte** yer kaplar.
### 3. Hepsini Bir Arada Görelim

İşte C dünyasındaki tam sayı hiyerarşisi (64-bit modern sistemler için tipik değerler):

| **Veri Tipi**   | **Boyut (Byte)** | **Format Belirleyici** | **Aralık (Yaklaşık)**     |
| --------------- | ---------------- | ---------------------- | ------------------------- |
| **`short`**     | 2                | `%hd`                  | -32.768 ile 32.767        |
| **`int`**       | 4                | `%d`                   | -2 Milyar ile +2 Milyar   |
| **`long`**      | 4 veya 8         | `%ld`                  | Çok geniş                 |
| **`long long`** | 8                | `%lld`                 | Akıl almaz derecede büyük |

Burda önemli bir konuya değinelim farkettiyseniz veriler 1 2 4 veya 8 bytelık yer kaplıyor  yani 3 byte veya 7 byte kaplayan bir veri yok. Bunun sebebi işlemcilerdir.İşlemciler paketli yapılara göre tasarlanmışlardır. Yani isiz bir işlemciyi parçalara bölmek isterseniz bunu yine 2nin katlarını kullanarak yapacaksınız. Bu da demek oluyorki 3 bytelık bir veri tanımlasaydınız bilgisayar sürekli bunun nerde başlayıp nerde bittiğini hesaplamak zorunda kalacaktır 
bunu bir dizi içerisinde düşünürsek 40 bytelık bir dizi olsun elimizde 4 4 gruplandırıldığını biliyoruz bilgisayar direkt olarak 4x10 mantığından 40. byte ulaşak aynı zamanda değişken sayısını döndürecek aynı zamanda boyutu döndürecektir. Bu hız farklı bir paketlemede daha uzun süreler alacaktır 


