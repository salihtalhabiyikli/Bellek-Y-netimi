İkili arama ağacı her düğümün iki çocuğu olduğ ve şu kurala uyan bir veri yapısıdır 
Bunu alta doğr ayrılan iki kol gibi düşünürsek 
Sol alt ağaç ----> Düğümden küçük değerler 
Sağ alt ağaç ----> Düğümden büyük değerler 
[[Bellek Yönetimi]]
### Nerelerde Kullanılır 
### 1. Veritabanı İndeksleme

Veritabanları (MySQL, PostgreSQL) kayıtları hızlı bulmak için BST tabanlı yapılar kullanır. "WHERE id = 1000" gibi bir sorgu tüm tabloyu taramak yerine ağaçta log(n) adımda sonuca ulaşır. Pratikte daha dengeli olan **B-Tree** ve **B+ Tree** varyantları kullanılır.

---

### 2. İşletim Sistemi — Bellek Yönetimi

Linux çekirdeği boş bellek bloklarını **Red-Black Tree** (dengeli BST) ile tutar. Bir program bellek istediğinde uygun boyuttaki blok hızlıca bulunur ve tahsis edilir.

---

### 3. Dosya Sistemleri

NTFS (Windows) ve Ext4 (Linux) dosya ve klasörleri BST türevi yapılarda saklar. Bir klasörde binlerce dosya olsa bile arama çok hızlı yapılır.

---

### 4. Otomatik Tamamlama / Sözlük Uygulamaları

Telefon klavyesi veya IDE'lerdeki kod tamamlama özelliği kelimeleri BST'de tutar. "pro" yazınca "program", "printf", "process" gibi öneriler hızlıca getirilir.

---

### 5. Oyunlarda Çarpışma Tespiti

Oyun motorları (Unity, Unreal) nesnelerin koordinatlarını BST tabanlı **k-d Tree** yapısında saklar. "Bu karaktere en yakın düşman hangisi?" sorusu tüm nesneleri tek tek kontrol etmeden cevaplanır.

---

### 6. Ağ Yönlendirme (Router)

Router'lar IP adreslerini BST benzinde yapılarda tutar. Gelen her pakete "bu IP hangi yöne gitsin?" sorusu milisaniyeler içinde cevaplanır.

---

### 7. İfade Hesaplama (Compiler / Hesap Makinesi)

`(3 + 5) * 2` gibi matematiksel ifadeler **Expression Tree** olarak tutulur. Derleyiciler ve hesap makineleri bu ağacı postorder dolaşarak hesaplar.

```
        *
       / \
      +   2
     / \
    3   5
```

---

### Özet Tablo

|Alan|Ne İçin Kullanılır|
|---|---|
|Veritabanı|Kayıt arama (INDEX)|
|İşletim Sistemi|Bellek / process yönetimi|
|Dosya Sistemi|Dosya arama|
|Metin Editörü|Otomatik tamamlama|
|Oyun Motoru|Çarpışma / mesafe hesabı|
|Ağ / Router|IP yönlendirme|
|Derleyici|Kod/ifade analizi|

### Node 
```c
typehedef struct Node {
int data;
struct Node*left ;
struct Node* right;
}Node;
```

## Temel Operasyonlar 
### Yeni Düğüm Oluşturma 
```c
Node* yeniDugum(int deger){
Node* dugum=(Node*)malloc(sizeof(Node));
dugum->data = deger; dugum->left = NULL;
 dugum->right = NULL;
 return dugum;
}
```
### 2. Ekleme (Insert)

Küçükse sola, büyükse sağa git. Boş yer bulunca ekle.

```c
Node* ekle(Node* kok, int deger) {
    if (kok == NULL)
        return yeniDugum(deger);

    if (deger < kok->data)
        kok->left  = ekle(kok->left,  deger);
    else if (deger > kok->data)
        kok->right = ekle(kok->right, deger);

    return kok;  // eşitse ekleme (duplicate yok)
}
```
### 3. Arama (Search)

```c
Node* ara(Node* kok, int deger) {
    if (kok == NULL || kok->data == deger)
        return kok;

    if (deger < kok->data)
        return ara(kok->left,  deger);
    else
        return ara(kok->right, deger);
}
```
### 4. Minimum & Maksimum

```c
Node* minimum(Node* kok) {
    while (kok->left != NULL)
        kok = kok->left;
    return kok;
}

Node* maksimum(Node* kok) {
    while (kok->right != NULL)
        kok = kok->right;
    return kok;
}
```

```c
Node* sil(Node* kok, int deger) {
    if (kok == NULL) return NULL;

    if (deger < kok->data) {
        kok->left  = sil(kok->left,  deger);
    } else if (deger > kok->data) {
        kok->right = sil(kok->right, deger);
    } else {
        // Durum 1 & 2: 0 veya 1 çocuk
        if (kok->left == NULL) {
            Node* temp = kok->right;
            free(kok);
            return temp;
        } else if (kok->right == NULL) {
            Node* temp = kok->left;
            free(kok);
            return temp;
        }
        // Durum 3: İki çocuk
        Node* temp   = minimum(kok->right);
        kok->data    = temp->data;
        kok->right   = sil(kok->right, temp->data);
    }
    return kok;
}
```
### 6. Dolaşma (Traversal)
```c
// Inorder → Sıralı çıktı verir (sol - kök - sağ)
void inorder(Node* kok) {
    if (kok == NULL) return;
    inorder(kok->left);
    printf("%d ", kok->data);
    inorder(kok->right);
    
}

// Preorder (kök - sol - sağ)
void preorder(Node* kok) {
    if (kok == NULL) return;
    printf("%d ", kok->data);
    preorder(kok->left);
    preorder(kok->right);
}

// Postorder (sol - sağ - kök)
void postorder(Node* kok) {
    if (kok == NULL) return;
    postorder(kok->left);
    postorder(kok->right);
    printf("%d ", kok->data);
}
```

### 7. Tam Örnek — main()
```c
#include <stdio.h>
#include <stdlib.h>
void inorder(Node* kok) {
    if (kok == NULL) return;
    inorder(kok->left);
    printf("%d ", kok->data);
    inorder(kok->right);
    
}

// Preorder (kök - sol - sağ)
void preorder(Node* kok) {
    if (kok == NULL) return;
    printf("%d ", kok->data);
    preorder(kok->left);
    preorder(kok->right);
}

// Postorder (sol - sağ - kök)
void postorder(Node* kok) {
    if (kok == NULL) return;
    postorder(kok->left);
    postorder(kok->right);
    printf("%d ", kok->data);
}



int main() {
    Node* kok = NULL;

    // Ekleme
    int degerler[] = {50, 30, 70, 20, 40, 60, 80};
    for (int i = 0; i < 7; i++)
        kok = ekle(kok, degerler[i]);

    // Inorder (sıralı çıktı)
    printf("Inorder: ");
    inorder(kok);     // → 20 30 40 50 60 70 80
    printf("\n");

    // Arama
    Node* bulunan = ara(kok, 40);
    printf("Aranan: %s\n", bulunan ? "Bulundu" : "Bulunamadı");

    // Silme
    kok = sil(kok, 30);
    printf("30 silindi → Inorder: ");
    inorder(kok);     // → 20 40 50 60 70 80
    printf("\n");

    return 0;
}
```
## BST Konusunun Tam Haritası

### ✅ Anlattıklarımız (Temel BST)

- Yapı, ekleme, silme, arama
- Traversal (inorder, preorder, postorder)
- Kullanım alanları

---

### 🔴 Henüz Anlatmadıklarımız

**1. BST Problemleri**

- Dengesiz ağaç problemi (Skewed Tree)
- En kötü durum O(n) neden olur, nasıl çözülür

**2. Dengeli BST'ler (Balanced BST)**

- **AVL Ağacı** → Otomatik dengeleme, rotasyon işlemleri (LL, RR, LR, RL)
- **Red-Black Tree** → Linux, Java, C++ STL'nin içinde kullanılan yapı
- **B-Tree / B+ Tree** → Veritabanlarının kullandığı yapı

**3. Gelişmiş Operasyonlar**

- Yükseklik (height) hesaplama
- Ağacın dengeli olup olmadığını kontrol
- İki BST'yi birleştirme
- BST'yi sıralı diziye / diziden BST'ye çevirme
- LCA (En Yakın Ortak Ata) bulma

**4. Özel BST Türleri**

- **Splay Tree** → Son kullanılan eleman köke çıkar
- **Treap** → BST + Heap birleşimi
- **Segment Tree** → Aralık sorguları için
- **k-d Tree** → Çok boyutlu uzayda arama (oyunlar, haritalar)

**5. Gerçek Hayat Uygulaması**

- C ile tam AVL ağacı kodlamak
- Veritabanı indeksi simülasyonu