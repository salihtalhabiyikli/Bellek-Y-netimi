### 1.  Ön İşleme (Preprocessing)
[[Bellek Yönetimi]]
- `#include`, `#define` gibi direktifler işlenir
- Yorumlar kaldırılır
- Makrolar genişletilir
- Çıktı: Saf kaynak kodu (`.i` dosyası)

---

### 2. Sözcüksel Analiz (Lexical Analysis)

- Kaynak kod **token**'lara ayrılır (anahtar kelimeler, operatörler, değişkenler)
- Örnek: `int x = 5;` → `[int] [x] [=] [5] [;]`

---

### 3.  Sözdizimsel Analiz (Syntax Analysis / Parsing)

- Token'lar dil kurallarına göre kontrol edilir
- **Sözdizim Ağacı (Parse Tree / AST)** oluşturulur
- Hatalı yazım burada yakalanır

---

### 4.  Anlamsal Analiz (Semantic Analysis)

- Tür uyumluluğu kontrol edilir (type checking)
- Değişken tanımları doğrulanır
- Mantıksal hatalar tespit edilir

---

### 5.  Ara Kod Üretimi (Intermediate Code Generation)

- Platforma bağımsız **ara kod** (IR) üretilir
- Örnek: LLVM IR, three-address code

---

### 6.  Optimizasyon (Optimization)

- Gereksiz kod temizlenir
- Döngüler optimize edilir
- Bellek ve hız iyileştirmeleri yapılır

---

### 7.  Hedef Kod Üretimi (Code Generation)

- Ara kod, **assembly** veya **makine koduna** çevrilir
- Platforma özgü talimatlar üretilir

---

### 8.  Bağlama (Linking)
- Farklı nesne dosyaları (`.o`, `.obj`) birleştirilir
- Kütüphaneler eklenir
- Çalıştırılabilir dosya (`.exe`, `.out`) oluşturulur