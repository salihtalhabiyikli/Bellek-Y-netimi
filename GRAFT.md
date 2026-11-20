-  Graft oluşturduğumuz nodeları birnirine bağlarken kullanılır 
```
A ——— B
|     |
C ——— D
```
- Bu kısımda a b c noktaları vertex bağlantılarda edge olarak adlandırılır
## Gerçek Hayatta Ne Gösterir
[[Bellek Yönetimi]]

| Graf          | Düğüm  | Kenar      |
| ------------- | ------ | ---------- |
| Google Maps   | Kavşak | Yol        |
| Sosyal Ağ     | Kişi   | Arkadaşlık |
| Internet      | Sunucu | Bağlantı   |
| Oyun Haritası | Oda    | Geçiş      |

### Graft Türleri
- Kenarların yönü yoktur teke tek bağlanır 
```
A ——— B    A'dan B'ye gidilir
           B'den A'ya da gidilir
```

***Yönsüz Graf*** : Çift yönlü bağlantı vardır 
***Yönlü Graf*** : Tek yönlü bağlantı vardır 
***Weighted Graf*** : kenarların bir değeri vardır seçim yapar

### A. Adjacency Matrix (Komşuluk Matrisi)

İki boyutlu bir dizi (`int graf[V][V]`) kullanarak düğümler arasındaki bağlantıları `1` veya `0` ile işaretleriz.

- **Avantajı:** "A ile B bağlı mı?" sorusuna $O(1)$ sürede cevap verir.
    
- **Dezavantajı:** Çok fazla bellek harcar ($O(V^2)$). Eğer 1000 düğümün varsa ama sadece 10 bağlantın varsa, belleğin çoğu boşa gider.
    

### B. Adjacency List (Komşuluk Listesi)

Her düğüm için bir **Bağlı Liste (Linked List)** tutulur. Her düğüm, sadece bağlı olduğu komşularının listesini saklar.

- **Avantajı:** Bellek dostudur ($O(V + E)$).
    
- **Dezavantajı:** "A ile B bağlı mı?" diye bakmak için listenin sonuna kadar gitmen gerekebilir.
## 3. Graf Gezinme Algoritmaları

Grafta yolumuzu bulmak için iki temel yöntem kullanırız:

### 1. BFS (Breadth-First Search - Genişlik Öncelikli)

Ağaçlardaki Level-order gezinmeye benzer. Önce en yakın komşuları, sonra onların komşularını ziyaret eder.

- **Kullanılan Yapı:** Kuyruk (Queue).
    
- **Kullanım Alanı:** En kısa yolu bulmak (unweighted graflarda).
    

### 2. DFS (Depth-First Search - Derinlik Öncelikli)

Bir yola girer ve gidebildiği kadar derine gider, yol bitince geri döner (**backtracking**).

- **Kullanılan Yapı:** Yığın (Stack) veya Özyineleme (Recursion).
    
- **Kullanım Alanı:** Labirent çözme, oyunlardaki karar ağaçları.