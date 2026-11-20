- Kısaca yönlü graflardır 
- Sistemin davranışlarını kontrol etmekte kullanılır 
- Bir sistemin herhangi bir zamanda sadece **tek bir durumda** bulunabildiği, gelen bir girdiyle bir durumdan diğerine geçtiği matematiksel bir modeldir.
[[Bellek Yönetimi]]
- ### Temel Bileşenler:

- **States (Durumlar):** Sistemin içinde bulunabileceği aşamalar (Örn: Beklemede, Çalışıyor, Hata).
    
- **Transitions (Geçişler):** Bir durumdan diğerine geçme eylemi.
    
- **Events/Inputs (Olaylar):** Geçişi tetikleyen girdi (Örn: Butona basıldı).
    
- **Actions (Eylemler):** Geçiş sırasında veya bir duruma girildiğinde yapılan işler 
```c
#include <stdio.h>

// 1. Durumları tanımla
typedef enum {
    DURUM_IDLE,
    DURUM_WALK,
    DURUM_JUMP
} Durumlar;

int main() {
    Durumlar su ankiDurum = DURUM_IDLE;
    char girdi;

    while(1) { // Oyun veya sistem döngüsü
        switch(su ankiDurum) {
            case DURUM_IDLE:
                printf("Karakter bekliyor...\n");
                // Girdi kontrolü
                scanf(" %c", &girdi);
                if(girdi == 'w') su ankiDurum = DURUM_WALK;
                break;

            case DURUM_WALK:
                printf("Karakter yuruyor...\n");
                scanf(" %c", &girdi);
                if(girdi == 's') su ankiDurum = DURUM_IDLE;
                break;
                
            // Diğer durumlar...
        }
    }
    return 0;
}
```