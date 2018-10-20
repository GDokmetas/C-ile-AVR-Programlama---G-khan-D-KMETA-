# TC1 Zamanlayıcı Örnek Kodu

 Bu dersimizde teknik veri kitapçığı üzerinden anlattığımız zamanlayıcıları bu sefer örnek kodlar üzerinden anlatacağız. Kod üzerinden anlatılması anlaşılması için çok daha faydalı olacaktır. Yine önceden olduğu gibi kodu satır satır açıklayacağız. Şimdi ilk programımıza geçelim.

```c
// Bu program 16Mhz saat hızında 1 saniyelik zamanlama işlemi yapar

#include <avr/io.h>
#include <avr/interrupt.h>


int main(void)
{
    OCR1A = 0x3D08;

    TCCR1B |= (1 << WGM12);
    // CTC Mod 4

    TIMSK1 |= (1 << OCIE1A);
    //Karşılaştırma eşleşmesi kesmesini etkinleştir

    TCCR1B |= (1 << CS12) | (1 << CS10);
    // Ön derecelendiriciyi 1024 olarak belirle ve zamanlayıcıyı aç


    sei();
    // kesmeler etkin


    while (1)
    {
       
    }
}

ISR (TIMER1_COMPA_vect)
{
    // Her saniyede bir yapılacak işlemler
}
```

Bu program 16MHz çalışma frekansında her saniyede bir zamanlayıcı kesmesini çalıştırır. Öncelikle kesmeleri kullanabilmek için **\#include &lt;avr/interrupt.h&gt;** komutu ile başlık dosyası eklenmiştir.

**OCR1A = 0x3D08;**  OCR1A yazmacını önceki derslerde karşılaştırıcı yazmacı olduğunu anlatmıştık. Bu yüzden bu yazmaca bir karşılaştırma değeri atamak lazımdır. Bu ise 16MHz hızda ve 1024 ön derecelendirme ile toplamda 1 saniyelik saat çevirim değeri olmalıdır.  TC1 zamanlayıcısı 16-bit olduğu için 16 bitlik bir değer atanmıştır. OCR değeri aşağıdaki formüle göre hesaplanır

**OCRn =  \[ \(saat hızı / ön derecelendirici değeri\) \* Saniye olarak istenilen zaman \] – 1**

**TCCR1B \|= \(1 &lt;&lt; WGM12\);**  TCCR1B yazmacı zamanlayıcı ile ilgili ayar verilerini bulunduran yazmaçtı. Bu yazmacın WGM12 biti bir \(1\) yapıldığında diğer ayar bitleri sıfır olacağından CTC modunda çalışacaktır. Yani zamanlayıcı karşılaştırma değerine göre sayma işlemini yapacaktır.

**TIMSK1 \|= \(1 &lt;&lt; OCIE1A\);**  Zamanlayıcı eşleşme kesmesi etkinleştirilir.

**TCCR1B \|= \(1 &lt;&lt; CS12\) \| \(1 &lt;&lt; CS10\);**  Bu CS bitlerinin tamamı sıfır olduğunda zamanlayıcı çalışmıyordu. Bu bitleri değiştirmek hem ön derecelendirici modunu seçmemizi sağlayacaktır hem de zamanlayıcıyı çalıştıracaktır. Bu bitler ile beraber zamanlayıcı 1024lük ön derecelendirici modunda çalışmaya başlar.

**sei\(\);** Bu komut ile kesmeler etkinleştirilir. Kesmelerin sürekli etkin halde olması hassas işlemlerde doğru olmayabilir. Bölünmesini istemediğimiz kısımlarda kesmeleri kapatıp gerektiği yerde açmak uygun olacaktır.

**ISR \(TIMER1\_COMPA\_vect\)**  Kesme gerçekleştiğinde bu fonksiyon içerisindeki komutlar çalıştırılır.

Bu programda zamanlayıcımız sıfırdan 0x3D08 değerine kadar her 1024 saat çeviriminde \(Çünkü ön derecelendirici ile 1024’e böldük.\) birer birer saymaya başlar. O değere ulaştığı zaman CTC modunda olduğu için otomatik sıfırlanır ve tekrar sıfırdan bu değere saymaya başlar. Bu sayma işlemini kesinlikle mikroişlemci yapmamaktadır. Mikroişlemci o sırada başka komutları işlerken zamanlayıcı ünitesi saymaya devam etmektedir. Zamanlayıcı ünitesi ne zaman belirtilen değere kadar saymayı bitirirse o zaman kesme yürütülerek mikroişlemcinin haberdar olması sağlanır.

Sonraki derslerde örnek kodları açıklayarak devam edeceğiz.

Kaynaklar:

Timers on the ATmega168/328, https://sites.google.com/site/qeewiki/books/avr-guide/timers-on-the-atmega328, Erişim Tarihi: 30.08.2018

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

