## 1. Yığıt (Stack) Nedir ve Nasıl Çalışır?

Yığıt, verilerin üst üste eklendiği ve işlemlerin her zaman en üstteki eleman üzerinden (tepeden) yapıldığı bir veri yapısıdır.
[[Bellek Yönetimi]]
- **LIFO Mantığı:** "Son giren ilk çıkar" (Last In First Out) veya "İlk giren son çıkar" (First-in-Last-out) prensibiyle çalışır.
    
- **Yazılım ve Sistemlerdeki Yeri:**
    
    - Geliştirdiğin **Antigravity** gibi bir kod editöründe veya Unreal Engine'de yaptığın bir işlemi geri almak (Undo) istediğinde arka planda yığın (stack) yapısı kullanılır.
        
    - Web tarayıcılarında "Geri" (Back) butonuna bastığında ziyaret ettiğin önceki sayfalara dönmek için yine LIFO yapısına başvurulur.
        
    - Bir şarjördeki mermiler gibi düşünebilirsin; şarjöre en son koyduğun mermi, ateşlediğinde namludan ilk çıkan mermi olur.
        

## 2. Temel Yığın İşlemleri

Yığın üzerinde veri manipülasyonu yapmak için belirli fonksiyonlar kullanılır:

- **`push(nesne)`:** Yığına yeni bir eleman ekler. Bir eleman ekleneceği zaman yığıtın en üstüne konulur.
    
- **`pop()`:** Yığının en üstündeki (yani en son eklenen) elemanı çıkarır ve sana döndürür.
    
- **`top()`:** En üstteki elemanı yığından _çıkarmadan_ sadece okumanı sağlar (yardımcı bir fonksiyondur).
    
- **`size()`:** Yığıttaki toplam eleman sayısını (depolanan nesne sayısını) verir.
    
- **`isEmpty()`:** Yığının boş olup olmadığını (boolean olarak) kontrol eder.
    
[[Bellek Yönetimi]]
## 3. Bellek Üzerinde Stack Gerçekleştirimi

Yığın veri yapısını koda dökerken temelde iki farklı yaklaşım kullanılır:

### A. Dizi Tabanlı (Array-Based) Yığın

Yığını gerçekleştirmek için en kolay yol, sabit boyutlu bir dizi kullanmaktır.

- **Avantajı:** Eleman ekleme (`push`) ve çıkarma (`pop`) işlemleri son derece hızlıdır; her bir işlem $O(1)$ sabit zaman alır. Kullanılan alan ise $O(n)$'dir.
    
- **Dezavantajı:** Yığının en üst sayısı önceden tanımlanmalıdır ve sonradan değiştirilemez. Eğer dizi tamamen doluysa ve sen hala `push` yapmaya çalışırsan **`FullStackException` (Dolu Yığın İstisnası)** hatası fırlatır.
    
- **Büyüyebilir Dizi Çözümü:** Dizi dolduğunda istisna fırlatmak yerine, daha büyük bir dizi oluşturulup eski veriler kopyalanabilir. Bu büyütme işlemi sabit bir `c` değeri kadar (artımlı strateji) veya kapasiteyi iki katına çıkararak (ikiye katlama stratejisi) yapılabilir.
    

### B. Bağlantılı Liste (Linked List) Tabanlı Yığın

Dizi sınırlamalarının (dolu yığın hatası gibi) önüne geçmek için düğümlerden oluşan bağlantılı liste yapısı kullanılır.

- Ekleme işlemleri her zaman listenin başına yapılır ("Baş" düğümü). Böylece sabit bir boyut derdi olmaz, eleman eklenip çıkarıldıkça yığın dinamik olarak şekillenir.
    

---

## 4. Matematiksel İfade Gösterimleri (Infix, Prefix, Postfix)

Matematiksel işlemleri bilgisayara ve algoritmik mantığa anlatırken yığın yapısı çok kritik bir rol oynar. Normalde biz işlemleri $a+b$ şeklinde yazarız; operatörün içeride olduğu bu gösterime **Infix** denir.

Ancak $a+b^{*}c$ gibi bir ifadede işlem önceliği veya parantez karmaşası başlar. Infix gösterim, işlem sıralarının düzenlenmesi için paranteze ihtiyaç duyar. Bu karmaşayı çözmek için yığın dostu gösterimler kullanılır:

- **Prefix (Operatör Önde / Polish Gösterimi):** Operatörler, işleme giren değişkenlerin önüne yazılır. Biçim: `işlem işlenen işlenen` şeklindedir (+ 2 7 gibi).
    
- **Postfix (Operatör Sonda / Reverse Polish Notation - RPN):** Operatörler değişkenlerin sonuna yazılır. Biçim: `işlenen işlenen işlem` şeklindedir (2 7 + gibi).
    

**Neden Postfix/Prefix Kullanılır?** Çünkü bu gösterimlerde parantez kullanılmasına gerek yoktur. Bilgisayar parantezlerle ve öncelik kurallarıyla uğraşmadan ifadeyi doğrudan bir yığın  yardımıyla çözebilir.

### Bilgisayarlar Postfix İfadeleri Nasıl Çözer?

Dosyadaki $a~b~c^{*}+d-$ örneğine bakarsak, sistem bunu yığın kullanarak çok temiz bir algoritmaya döker:

1. İfadeyi soldan sağa okur. Sayı (işlenen) görürse bunu doğrudan yığına `push` yapar.
    
2. Eğer bir operatör (örn: `*` veya `+`) görürse, yığından art arda iki tane eleman `pop` eder (çıkarır).
    
3. Çıkardığı bu iki sayıyı okuduğu operatör ile işleme sokar ve elde ettiği sonucu tekrar yığına `push` yapar.