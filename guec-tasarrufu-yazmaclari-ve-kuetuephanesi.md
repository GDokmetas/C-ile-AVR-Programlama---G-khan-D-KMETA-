# Güç Tasarrufu Yazmaçları ve Kütüphanesi

AVR mikrodenetleyicileri sadece yazmaç denetimi ile uykuya sokamayız. Üreticiler uyku moduna geçmek için özel bir mikroişlemci komutu belirlemiştir. Bu mikroişlemci komutunu C dilinde kullanmak için avr/sleep.h başlık dosyasını kullanırız. Bütün bu ayar ve etkinleştirmeler sonunda SLEEP komutu ile mikrodenetleyici uykuya geçer. Dersin sonunda bundan bahsedeceğimiz için şimdi yazmaçları anlatarak konumuza devam edelim.

**SMCR – Uyku Modu Denetim Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/sm1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-57-guc-tasarrufu-yazmaclari-ve-kutuphanesi/attachment/sm1/)

**Bit 3, 2, 1 – Uyku Modu Seçme Biti**

Bu bitler aşağıdaki tabloya göre uyku modunu seçmemizi sağlar.

| **SM2, SM1, SM0** | **Uyku Modu** |
| :--- | :--- |
| 000 | Idle |
| 001 | ADC Gürültü Önleme |
| 010 | Power-Down |
| 011 | Power-Save |
| 100 | Rezerve |
| 101 | Rezerve |
| 110 | Standby |
| 111 | Extended Standby |

**Bit 0 – SE : Uyku Etkin**

Bu bit mikrodenetleyicinin uyku moduna girmesi için gerekli izni verir. Uyku moduna bu biti bir \(1\) yaparak girmeyiz. Uyku modu için özel mikroişlemci komutu vardır. Fakat bu komutun işletilmesi için bu bitin bir \(1\) olması lazımdır. SLEEP komutu işletilmeden hemen önce bu biti bir \(1\) yapmak daha iyi olacaktır.

 **MCUCR – Mikrodenetleyici Denetim Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/sm2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-57-guc-tasarrufu-yazmaclari-ve-kutuphanesi/attachment/sm2/)

**Bit 6 – BODS – BOD \(Brown-out Detection\) Uyku**

Bu bit bir \(1\) yapıldığında kararma algılayıcısı uyku sırasında devre dışı bırakılır. BODSE biti ise bir etkinleştirme bitidir ve güvenlik amacıyla öncelikle iki bit aynı anda bir \(1\) yapılmalı ve dört saat çevrimi içerisinde BODSE biti sıfır \(0\) yapılmalıdır. BODS biti üç saat çevrimi boyunca etkin halde olur. Bu durumda ise uyku komutu işletilmelidir.  BODS biti üç saat çevrimi sonunda otomatik olarak sıfırlanır.

**Bit 5 – BODSE : BOD Uyku Etkin**

Bu bitin görevini yukarıda açıkladık.

**PRR – Güç Tasarrufu Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/sm3.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-57-guc-tasarrufu-yazmaclari-ve-kutuphanesi/attachment/sm3/)

**Bit 7 – PRTWI0 : TWI Güç Tasarrufu**

Bu biti bir \(1\) yaparsak TWI modülü çalışmayı durdurur. Giden saat sinyalini kesmekle bu gerçekleşir. Tekrar etkinleştirmek için modülü yeniden başlatmak gerekir.

**Bit 6 – PRTIM2 : TC2 Güç Tasarrufu**

Bu biti bir \(1\) yaparsak TC2 zamanlayıcısı senkron modda çalışmayı durdurur. Zamanlayıcı etkinleştirildiğinde tekrar çalışmaya devam eder.

**Bit 5 – PRTIM0 : TC0 Güç Tasarrufu**

Bu biti bir \(1\) yaparsak TC0 zamanlayıcısı senkron modda çalışmayı durdurur. Zamanlayıcı etkinleştirildiğinde tekrar çalışmaya devam eder.

**Bit 3 – PRTIM1 : TC1 Güç Tasarrufu**

Bu biti bir \(1\) yaparsak TC1 zamanlayıcısı senkron modda çalışmayı durdurur. Zamanlayıcı etkinleştirildiğinde tekrar çalışmaya devam eder.

**Bit 2 – PRSPI0 : SPI Güç Tasarrufu**

Bu biti bir \(1\) yaparsak SPI modülü çalışmayı durdurur. Tekrar etkinleştirmek için modülü yeniden başlatmak gereklidir.

**Bit 1 – PRUSART0 : USART Güç Tasarrufu**

Bu biti bir \(1\) yaparsak USART çalışmayı durdurur. Tekrar etkinleştirmek için modülü yeniden başlatmak gereklidir.

**Bit 0 – PRADC : ADC Güç Tasarrufu**

Bu biti bir \(1\) yaparsak ADC modülü kapatılır. Kapatılmadan önce ADC devre dışı bırakılmalıdır. \(ADEN biti\)

Yazmaçlar bitmiş oldu. Şimdi SLEEP komutunu nasıl işleteceğimiz hakkında kısa bir bilgi verelim.

Uyku işlemlerini yapmak istiyorsak öncelikle programımıza şu başlık dosyasını dahil etmemiz gereklidir. sleep.h başlık dosyası derleyicinin içinde gelmektedir.

```c
#include <avr/sleep.h>
```

 Şimdi fonksiyonları kısaca açıklayalım.

```c
sleep_bod_disable();
```

 Kararma algılayıcısını devre dışı bırakır. \(Her aygıtta geçerli değildir.\)

```c
sleep_cpu();
```

 İşlemciyi uyku moduna sokar. Assembly dilindeki sleep komutu ile aynıdır. SE biti önceden bir yapılmalıdır. Uykunun ardından tekrar sıfır yapılması önerilir.

```c
sleep_disable();
```

 SE bitini sıfırlar. Böylelikle denetleyici bir daha uykuya bu bit bir \(1\) yapılana kadar girmez.

```c
sleep_enable();
```

 SE bitini bir \(1\) yapar.

```c
sleep_mode();
```

SE bitini önceden bir \(1\) yapar ve ardından aygıtı uykuya sokar.

Yazmaçları öğrendiğimiz için sadece SLEEP mikroişlemci komutunu kullanmamız yeterlidir. Bunun için ise yazmaç ayarlarını yaptıktan sonra sleep\_cpu\(\) fonksiyonunu kullanacağız.

SLEEP komutu ise aşağıdaki bitlerden ibarettir. Bu bitler program hafızasına yazılır ve mikroişlemcinin anlayacağı makine dilindedir.

| 1001 | 0101 | 1000 | 1000 |
| :--- | :--- | :--- | :--- |


Güç tasarrufu ve uyku konusunu bitirmiş olduk. Şimdi sırada tek bir konumuz kaldı sayılır. Sistem denetimi ve yeniden başlatma konusunun ardından derslerimizi bitirmiş olacağız. Geriye kalan konuları ise derslerden bağımsız olarak daha sonra ele alacağız.

_**Kaynaklar**_



ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

