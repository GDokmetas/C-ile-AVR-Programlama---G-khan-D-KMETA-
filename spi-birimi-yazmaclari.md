# SPI Birimi Yazmaçları

Bu dersimizde her zaman olduğu gibi yazmaçları anlatarak konumuza devam edeceğiz. Kopyala yapıştır kodla, ezbere kod yazma ile ne kadar üst seviye dillerde program yazılabilse de biz gömülü sistemlerde çalıştığımız için üst seviye bilgiye ihtiyacımız vardır. Üst seviye bilgi alt seviyede çalışmayı gerektirir. Burada alt seviyeden kastımız ise donanıma en yakın seviye demektir.

Şimdi yazmaçları açıklayalım ve sonraki dersimizde ise örnek kodları inceleyerek konumuza devam edelim.

**SPCR0 – SPI Denetim Yazmacı 0** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/spi2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-48-spi-birimi-yazmaclari/attachment/spi2/)

**Bit 7 – SPIE0 : SPI0 Kesme Etkin**

Bu bit bir \(1\) yapıldığında ve SPIF biti de bir \(1\) yapıldığında SPI kesmesini yürütür.

**Bit 6 – SPE0 : SPI0 Etkin**

Bu bit bir \(1\) yapıldığında SPI etkinleştirilir. Herhangi bir SPI işlemini gerçekleştirmek için bu bit bir \(1\) yapılmalıdır.

**Bit 5 – DORD0 : Veri0 Düzeni**

DORD biti bir  \(1\) yapıldığında alt bit öncelikli olarak veri gönderilir.  
DORD biti sıfır \(0\) yapıldığında ise üst bit öncelikli olarak veri gönderilir.

**Bit 4 – MSTR0 : Ana/Uydu 0 Seçimi**

Bu bit bir \(1\) yapıldığında SPI iletişimini ana aygıt modunda çalıştırır. Eğer sıfır \(0\) yapılırsa mikrodenetleyici uydu modunda çalışır. Eğer SS ayağı giriş olarak tanımlanır ve LOW konuma çekilirse MSTR biti sıfırlanır ve SPIF biti bir \(1\) konumuna gelir. Yani uydu moduna otomatik olarak geçer. Tekrar ana modu açmak için bu bitin bir \(1\) yapılması gerekir.

**Bit 3 – CPOL0 : Saat0 Kutbu**

Bu bit bir \(1\) yapılırsa bekleme konumunda SCK ayağı HIGH konumda kalır. Eğer sıfır \(0\) yapılırsa bekleme konumunda LOW konumda kalır. Bu SPI modunu seçmek için kullanılır.

**Bit 2 – CPHA0 : Clock0 Evresi**

Bu bitin durumuna göre örneklenecek veri düşen kenarda veya yükselen kenarda alınır. Sıfır \(0\) iken yükselen kenarda bir \(1\) iken ise düşen kenarda veri örneklenir. İleride SPI modlarına değineceğimiz için bu iki bitin toplamda 4 ayrı SPI modunu seçtiğini bilmemiz yeterlidir.

**Bit 1:0 – SPR0n : SPI0 Saat Oranı Seçimi n \[ n = 1:0 \]**

Bu bit ana aygıtın SCK bacağının hızını belirlemeye yarar. Uydu aygıtta bu bitler çalışmaz. Osilatör  ve SCK ayağı arasındaki bağlantı aşağıdaki tabloda verilmiştir.

| SPI2X | SPR01 | SPR00 | SCK Frekansı |
| :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | Osilatör / 4 |
| 0 | 0 | 1 | Osilatör / 16 |
| 0 | 1 | 0 | Osilatör / 64 |
| 0 | 1 | 1 | Osilatör / 128 |
| 1 | 0 | 0 | Osilatör / 2 |
| 1 | 0 | 1 | Osilatör / 8 |
| 1 | 1 | 0 | Osilatör / 32 |
| 1 | 1 | 1 | Osilatör / 64 |

**SPSR0 – SPI Durum Yazmacı 0**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/spi3.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-48-spi-birimi-yazmaclari/attachment/spi3/)

**Bit 7 – SPIF0 : SPI Kesme Bayrak Biti**

Seri gönderim tamamlandığında SPIF bayrak biti bir \(1\) konumuna geçer. Eğer SPIE yani SPI Kesme Biti bir \(1\) konumunda ise kesme yürütülür. Eğer SS ayağı giriş olarak tanımlanmış ve LOW konumuna gelmiş ise bu bit yine bir \(1\) konumuna geçer. SPIF biti kesme fonksiyonu yürütüldükten sonra donanım tarafından sıfırlanır.

**Bit 6 – WCOL0 : Yazma Çakışması Bayrak Biti**

Eğer veri gönderimi sırasında SPI Veri Yazmacına bir yazma işlemi yapılırsa bu bit bir \(1\) konumuna geçer. Bu bit bir \(1\) konumuna geçtikten sonra ilk okumanın gerçekleşmesiyle tekrar sıfırlanır.

**Bit 0 – SPI2X0 : SPI Çift Hız Biti**

Bu bir bir yapıldığında SPI hızı \(SCK Frekansı\) ikiye katlanır. Sadece ana aygıt modunda çalışmaktadır.

**SPDR0 – SPI Veri Yazmacı 0**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/spi4.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-48-spi-birimi-yazmaclari/attachment/spi4/)

Bu yazmaç hem gönderilen veriyi hem de alınan veriyi içerisinde barındırır. USART protokolünde gördüğümüz üzere her defasında 8 bitlik bir veri gönderilir. SPI yazmaçları çok da fazla değildir. Toplamda 3 adet yazmaç bulunur ve bütün iletişim bu yazmaçların denetimi ve okuyup yazılması ile sağlanır.

Yazmaçları anlatırken biraz ezberci anlatmak durumunda kalıyoruz. Yapısı gereği daha alt seviye bir ünite olmadığı için “bu bu işe yarar” demekten başka bir seçeneğimiz yok. Çünkü daha ilerisi ancak mikrodenetleyici üretmek için gerekli bilgilerden oluşuyor. Üreticinin de bu bilgileri bizimle paylaşması beklenemez. Yine de bu dersler bittikten sonra mikroişlemci mimarisi derslerimizde kendi işlemcimizi üretme yolunda teorik bilgileri sizinle paylaşacağız.

Bir sonraki derste örnek kodları inceleyeceğiz ve eksik kalan yerleri tamamlayacağız.

_**Kaynaklar**_

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

