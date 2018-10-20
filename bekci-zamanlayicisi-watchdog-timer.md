# Bekçi Zamanlayıcısı \(Watchdog Timer\)

İsmi oldukça ilginç olan bir zamanlayıcı ile karşı karşıyayız. Watchdog timer bir terim olsa da aslında pek çok yerde karşılaşacağımız gibi anlamsız ve uydurulmuş bir tabirdir. Türkçe’ye tam olarak çevrimi “Bekçi köpeği zamanlayıcısı” olup bizim kafamızda soru işaretleri bırakmaktadır. Bu İngilizce’nin zengin kelime üretme kabiliyetinden dolayı değildir. İngilizce yeni kelime üretmeye oldukça kapalı bir dil olduğundan yeni bir kelime ihtiyacı duyulduğunda böyle anlam kaymasına uğrayan uydurma kelimeler üretilir. Aynı erkek arı anlamına gelen “drone” kelimesinin uçan bir cihaz için kullanılmasında olduğu gibi böyle tabirlerin Türkçeleştirilme zorluğu anlamsızlığından dolayı olmaktadır.

Bu zamanlayıcıyı tanımlamak için böyle bir tabir kullanılmasından dolayı kısa bir dilbilim dersi vermek durumunda kalmış olduk. Şimdi zamanlayıcıyı anlatalım.

Bekçi zamanlayıcısı oldukça basit bir işleve sahip olan zamanlayıcı ünitesidir. Zamanlayıcılar eşleşme veya taşma anında bir çıkış veriyordu. Bu çıkışı reset ayağına bağlayıp mikrodenetleyiciyi her çıkış anında yeniden başlatırsak bekçi zamanlayıcısının bir kopyasını yapmış oluruz.  Bekçi zamanlayıcı taşma veya eşleşme anında mikrodenetleyiciyi yeniden başlatan bir zamanlayıcıdır. Yine osilatör, ön derecelendirici, karşılaştırma yazmacı, sayaç yazmacı gibi standart zamanlayıcı birimlerine sahiptir.

Mikrodenetleyiciyi sürekli resetleyen bir sayıcıyı düzgün kullanamadıkça faydadan çok zarar göreceğimiz ortadadır. Bu düzgün kullanım ise yazılımla bu sayıcıyı sürekli sıfırlayıp resetlemesinin önüne geçmekle mümkün olur. Program düzgün çalıştığı sürece bu sayıcıyı sıfırlayarak “Mavi ekran” vermesinin önüne geçer. Eğer program sonsuz döngüye girer veya bir yerde takılıp cevap veremez konuma gelir ise bu sayıcı sıfırlanmaz ve taşarak “Mavi ekran” verip mikrodenetleyiciyi yeniden başlatır.

İşte programımız istenildiği gibi çalışmazsa bizi kurtaracak birim bekçi zamanlayıcısıdır. Program sonsuz döngüye girdiğinde veya bir yerde donduğunda bizi kurtaracak bir özellik yoktur. Ya kullanıcı tarafından el ile yeniden başlatılmalı ya da bu yeniden başlatma otomatik yolla yapılmalıdır.

Bu zamanlayıcı ayrı bir dahili osilatör ile beslenir. Kesme, sistem reset ve kesme ve sistem resetinin beraber yürütüldüğü üç ayrı çalışma modu vardır. 16 mili saniyeden 8 saniyeye kadar zaman aşımı modu vardır. Yani en fazla 8 saniye içinde sıfırlanmadığı takdirde sistemi yeniden başlatır. Donanımsal olarak sigorta bitine sahip olup sürekli açık hale getirilebilir.

Her zaman olduğu gibi öncelikle bekçi zamanlayıcısının blok diyagramını paylaşalım ve bunun üzerinden anlatmaya devam edelim.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/wdt.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-59-bekci-zamanlayicisi-watchdog-timer/attachment/wdt/)

Görüldüğü gibi zamanlayıcının kendine ait bir osilatörü bulunmaktadır. Bu osilatör 128kHz’de çalışmaktadır. Bu osilatörün saat sinyali bekçi zamanlayıcısı ön derecelendiricisine gitmektedir. Ön derecelendirici 2 ile 1024 arasında sinyali bölme işlemine tabi tutup sayaç değerini artırmaktadır. Sayacı sıfırlamak için Watchdog Reset sinyali kullanılır. Sayaç çıkışı ise kesme ve mikrodenetleyici yeniden başlatmaya bağlanmıştır.

Bekçi zamanlayıcısı hem kesme yürütebilir hem de sistemi resetleyebilir. Bunun için sayaç değerinin taşma değerine ulaşması lazımdır. Normal bir sistem çalışmasında taşma değerine ulaşmadan önce bekçi zamanlayıcısı sıfırlanmak zorundadır. Eğer sistem sayacı sıfırlamazsa kesme ya da sistem reseti yürütülür.

Kesme modunda sayaç taştığı \(zaman aşımı olduğu\) zaman kesme yürütülür. Bu kesme aygıtı uyku modundan çıkarmak için kullanılabilir. Sistem reset modunda ise aygıt taşma gerçekleştiğinde yeniden başlatılır. Üçüncü mod olan kesme ve sistem reset modunda önce kesme yürütülür ve sonrasında ise sistem yeniden başlatılır. Bu modla kritik bilgiler sistem yeniden başlatılmadan önce kaydedilme şansı bulur.

Eğer WDTON sigortası programlanırsa bekçi zamanlayıcısı sürekli sistem reset modunda çalışmaya zorlanır. Güvenlik açısından bekçi  zamanlayıcısı şu prodesürlerle kullanılmalıdır.

Aynı fonksiyonda  bekçi zamanlayıcısı değişim etkinleştirme biti \(WDCE\) ve bekçi  zamanlayıcısı sistem reset etkinleştirme biti \(WDE\) bir \(1\) yapılmalıdır. WDE biti öncelikle bir \(1\) yapılması lazımdır.

Sonraki dört saat çevrimi içerisinde WDE ve ve ön derecelendici bitleri istenildiği gibi düzenlenebilir. Fakat WDCE bitinin de sıfır \(0\) yapılması lazımdır. Bu aynı komut içinde yapılmalıdır.

Zamanlayıcıyı etkinleştirmek için tek bir biti açmak \(WDCE\) yeterli olsa da taşmadan önce kapatmak için biraz fazlaca kod yazmamış gereklidir. Zamanlayıcıyı kapatmak için şöyle bir fonksiyon kullanılmalıdır.

```c
void WDT_off(void)
{
__disable_interrupt();
__watchdog_reset();
/* WDRF bitini sıfırla */
MCUSR &= ~(1<<WDRF);
/* WDCE ve WDE bitine aynı anda bir yaz. */
/* Eski ön derecelendirici ayarını muhafaza etmek daha sağlıklı olur.*/
WDTCSR |= (1<<WDCE) | (1<<WDE);
/* WDT kapat */
WDTCSR = 0x00;
__enable_interrupt();
}
```

 Zamanlayıcıyı sıfırlamak için ise öncelikle avr/wdt.h başlık dosyası eklenmelidir. Aşağıdaki komutu programın başına eklememi lazımdır.

```c
#include <avr/wdt.h>
```

 Sonrasında ise şu fonksiyonu kullanarak zamanlayıcıyı kolaylıkla sıfırlayabiliriz.

```c
wdt_reset();
```

 Ön derecelendiriciyi değiştirmek için ise şöyle bir fonksiyon kullanılmalıdır.

```c
void WDT_Prescaler_Change(void)
{
__disable_interrupt();
__watchdog_reset();
/* Zamanlamayı başlat (4 saat çevrimi) */
WDTCSR |= (1<<WDCE) | (1<<WDE);
/* Yeni ön derecelendirici değerini belirle 64K çevirim (~0.5 s) */
WDTCSR = (1<<WDE) | (1<<WDP2) | (1<<WDP0);
__enable_interrupt();
}
```

 Bekçi zamanlayıcısını etkinleştirmek için ise şöyle bir komut kullanılmalıdır.

```c
wdt_enable();
```

 Yine devre dışı bırakmak için kütüphane fonksiyonu kullanmak istiyorsak şöyle bir komut kullanabiliriz.

```c
wdt_disable();
```

**WDTCSR – Bekçi Zamanlayıcısı Denetim Yazmacı** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/wdt2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-59-bekci-zamanlayicisi-watchdog-timer/attachment/wdt2/)

**Bit 7 – WDIF : Bekçi Zamanlayıcısı Kesme Bayrak Biti** 

Bu bit zamanlayıcı kesme modunda çalışırken taşma olduğu sırada bir \(1\) konumuna geçer. Bu bit kesme fonksiyonu yürütüldüğü zaman otomatik sıfırlanır. Alternatif olarak el ile bir \(1\) yazarak bu biti sıfırlayabiliriz .

**Bit 6 – WDIE – Bekçi Zamanlayıcısı Kesme Etkin**

Bu biri bit \(1\) yapmak kesmeyi etkinleştirir. Aşağıda zamanlayıcının ayarlarına dair bir tablo vardır.

| WDTON Sigortası | WDE Biti | WDIE Biti | Mod | Taşma Sırasında Yürütülen İşlem |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 0 | 0 |  Durdurulur | Yok |
| 1 | 0 | 1 | Kesme Modu | Kesme |
| 1 | 1 | 0 | Sistem Reset Modu | Reset |
| 1 | 1 | 1 | Kesme ve Reset Modu | Kesme ve sonrasında Reset |
| 0 | x | x | Sistem Reset Modu | Reset |

**Bit 5 – WDP \[3\] – Bekçi Zamanlayıcısı Ön derecelendiricisi**

**Bit 4 – WDCE : Bekçi Zamanlayıcısı Değiştirme Etkin**

Bu bit zamanlanmış ayar süreci için kullanılır. WDT ayarı ve ön derecelendirici ayarını yapmak için verilen süreci bu bit ile yaparız. Bu bir çeşit güvenlik bitidir. WDE ve ön derecelendirici bitlerini değiştirmek istiyorsak bu biti bir \(1\) yapmak zorundayız. Bir yapıldıktan sonra donanım bu biti dört saat sinyali içerisinde otomatik sıfırlar. O yüzden hemen gereken kodu yazmamız gereklidir.

**Bit 3 – WDE : Bekçi Zamanlayıcısı Sistem Reset Etkin**

Eğer MCUSR içindeki WDRF biti bir \(1\) ise bu bit bir \(1\) olmaya zorlanır. WDE’yi sıfırlamak için önce WDRF’yi sıfırlamak lazımdır. Bu özellik çoklu reset yürütülmesini önlemeye yarar.

**Bit 2:0 – Bekçi zamanlayıcısı ön derecelendirici bitleri** 

Ön derecelendirici aşağıdaki tabloya göre çalışmaktadır.

| WDP\[3\] | WDP\[2\] | WDP\[1\] | WDP\[0\] | WDT Osilatör Çevirim Sayısı | Süre |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 0 | 2K | 16ms |
| 0 | 0 | 0 | 1 | 4K | 32ms |
| 0 | 0 | 1 | 0 | 8K | 64ms |
| 0 | 0 | 1 | 1 | 16K | 0.125ms |
| 0 | 1 | 0 | 0 | 32K | 0.25s |
| 0 | 1 | 0 | 1 | 64K | 0.5s |
| 0 | 1 | 1 | 0 | 128K | 1s |
| 0 | 1 | 1 | 1 | 256K | 2s |
| 1 | 0 | 0 | 0 | 512K | 4s |
| 1 | 0 | 0 | 1 | 1024K | 8s |

Böylelikle AVR konularımızı bitirmiş olduk. Artık tüm dersler bittiğine göre ileri seviye AVR programlamayı öğrenmek veya AVR programlamaya üst seviyeden başlamak isteyen programcıların faydalanabileceği eksiksiz bir ders dizisi ortaya çıkmış oldu. İlerleyen zamanlarda bu derslere yardımcı kaynak olacak şekilde kaynak makale yazmaya devam edeceğiz

Kaynaklar,

 ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

