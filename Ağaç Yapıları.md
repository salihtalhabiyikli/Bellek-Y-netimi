-   Hiyerarşik bir veri düzenidir. 
 - Veriler arasında ebeveyn çocuk ilişkisi kurar 

*Root*: Ağacın en tepesindeki elemandır 
*Node:*  Veriyi ve diğer düşümlere bağlantıları içeren her bir birim 
*Parenty:* Hem üstünde hem altında eleman olan veri türü 
*Child*: Parentynin altındaki elemandır 
Leaf: Ağacın son elemanıdır 

-  Kendi ağacımızı yazalım:
```c
struct Dugum {
int veri; // veri
struct Dugum* sol; // sol yolun  bellek adresi
struct Dugum* sag;// sağ yolun bellek adresi
}
```
bu yapı bellekte her veri parçası için bir kutucuk açar ve bu kutunun içine iki tane pointer koyar bu adresler hem veriyi hemde sonraki düğümün nerde olduğunu söyler.

https://visualgo.net/en/segmenttree
[[Bellek Yönetimi]]
### Ağaç Yönetimi 
Şimdi bu ağacın içerisinde dolanalım 
```c
struct Dugum* yeniDugum(int veri){
struct Dugum* dugum=(struct Dugum*)malloc(sizeof(struct Dugum));
dugum->sol = NULL;
dugum->sag = NULL;
return dugum;
}
In-order
```

Bu kısım temel tanımlamalardır. Bu tarz ağaçlarda gezinebilmemiz için Özyinelemeli Fonksiyonlar kullanılır şimdi bu fonksiyonlara bakalım 


- Bu fonksiyonlar birer metod olarak yazılır ve parametre olarak çağırılır 

### In Order Traversal (Sol Kök Sağ)

- Binary arama yapar küçükten büyüğe sıralamada kullanılır.
```c
void inOrder(struct Dugum* kok){
if(kok!= NULL){
	inOrder(kok->sol);// solu bitir
	printf("%d",kok->veri);//Kendini yazdır
	inOrder(kok->sag); //sağı bitir
	}
}
```

### Pre-Order Traversal(Kök Sol Sağ)
- Ağacın kopysını oluşturmak veya matetiksel ifadeleri prefix notasyonuna çevirmede kullanılır 
```c
void preOrder (struct Dugum* kok){
if(kok!= NULL)
printf("%d",kok->veri);
preOrder(kok->sol);
preOrder(kok->sag);
}
}
```

### Post Order Traversal (Sol-sağ-kök)
-  Çok acil durumlarda bellek boşaltmada kullanılır. Bir düğümü silmeden önce çocuklarını silersiniz. Verilerin dağılmaması için gereklidir 
```c
void postOrder(struct Dugum* kok){
if (kok!= NULL){
postOrder(kok-> sol);// solu temizle
postOrder(kok->sag);// sağı temizle
printf("%d",kok->veri);// kökü temizle
}
}
```

## Özet Karşılaştırma Tablosu

| **Yöntem**     | **Sıralama**                            | **En İyi Kullanım Alanı**    |
| -------------- | --------------------------------------- | ---------------------------- |
| **In-order**   | Sol $\rightarrow$ Kök $\rightarrow$ Sağ | BST'de sıralı liste almak.   |
| **Pre-order**  | Kök $\rightarrow$ Sol $\rightarrow$ Sağ | Ağacın kopyasını oluşturmak. |
| **Post-order** | Sol $\rightarrow$ Sağ $\rightarrow$ Kök | Ağacı bellekten silmek.      |