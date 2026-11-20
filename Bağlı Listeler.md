- Bağlı listeler verilerin bellekte ardışık olarak tutulmasını gerektirmeden her bir veri parçasının başka bir veri parçasının bellek adresini tuttuğu veri yapısıdır.
- Yani misal benim "0x45FG44" adresinde bir verim var ben sonraki veriyi "0x45FG45" adresine koymak yerine ilk adrese senden sonraki sonraki veri şu adreste diyoruz. 
#### Tek Yönlü Bağlı Liste 
- Bunlar tren vagonlarına benzer her vagon sonraki vagonun bellek adreisni bilir 
- A-B-C sıralamasını düşünelim A dan Bye gidebiliriz fakat tekrar a ya dönemeyiz B dende Cye gideriz fakat geri ne a ya ne bye dönebiliriz.
-  next keywordünü kullanırız

[[Bellek Yönetimi]]
#### Çift Yönlü Bağlı Liste
- Her vagon hem kendinden sonraki hemde kendinden önceki vagonun yerini bilir. 
- Tahminde edeceğiniz üzere A-B-C döngüsünde istediğiniz şekilde hareket edebilirsiniz.
- prev keywordünü kullanırız.
![[Pasted image 20260419121548.jpg]]

- Bu ikisini kullandığımız yapılarda klasik bir OOp hiyerarşisi bulunuyor.
```c
struct Film {
    char ad[50];
    int sure;
    struct Film* next; // Sonraki filmin adresi
    struct Film* prev; // Önceki filmin adresi (Çift yönlü olmasını bu sağlar)
};
```
Kodun en başında bilgisayara "Hangi kutuda ne saklayacağız?" bilgisini veriyorsun

