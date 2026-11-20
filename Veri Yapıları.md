### Diziler
Bellekte arka arkaya yerleşen, aynı türden elemanların koleksiyonu. İndeks 0'dan başlar.
[[Bellek Yönetimi]]
```c
int sayilar[5] = {10, 20, 30, 40, 50}; 
char isim[11] = "Merhaba"; 
double notlar[3]; /* Başlangıç değeri yok */ 
sayilar[0] = 99; /* İlk eleman */ 
sayilar[4] = 77; /* Son eleman */ 
for (int i = 0; i < 5; i++) 
{ 
printf("%d ", sayilar[i]); 
}
```
![[Pasted image 20260413001255.png]]

```c
int matris[3][3] = { {1, 2, 3}, {4, 5, 6}, {7, 8, 9} }; 
printf("%d\n", matris[1][2]); /* 6 */
```