**Mutlaka öğren (her C programcısı bilmeli)**

- **Bellek yönetimi** (`malloc/free`, stack vs heap) — C'nin özü bu. Bilmeden ciddi program yazamazsın.
- **Fonksiyon pointer'ları** — Standart kütüphane bile bunlara dayanır (`qsort`, `bsearch`). Callback mantığını anlamak şart.
[[Bellek Yönetimi]]
**Çok önemli (orta-ileri seviye)**

- **Hash tablosu** — Gerçek dünya projelerinde en sık kullanılan yapılardan biri. Veritabanı, derleyici, oyun motoru hepsi kullanır.
- **İkili arama ağacı (BST)** — Algoritmik düşünceyi geliştiren temel yapı. Mülakatlarda da sık çıkar.
- **Dairesel buffer** — Gömülü sistemler, ağ programlama, ses işleme yapıyorsan kaçınılmaz.

**Duruma göre önemli**

- **Graf** — Ağ topolojisi, harita/navigasyon, bağımlılık çözümü gibi spesifik alanlarda kritik. Genel C programlamada daha az çıkar.
- **AVL/Dengeli ağaçlar** — Kütüphane yazıyorsan önemli, yoksa teorik bilgi yeterli.
- **Bit alanları** — Gömülü sistem veya donanım programlama yapıyorsan çok önemli, değilse nadiren lazım olur.