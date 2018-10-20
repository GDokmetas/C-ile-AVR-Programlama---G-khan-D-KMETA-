# SPI \(Serial Peripheral Interface\) İletişim Protokolü

Türkçe olarak Seri Çevrelik Arayüz adını verebileceğimiz bu iletişim protokolü senkron olarak işleyen bir seri iletişim protokolüdür. Aynı hat üzerinde sayısız aygıt ile yüksek hızda seri iletişime geçebiliriz. Aygıt seçmek için ne bir adres ne bir programa ihtiyaç vardır. Aygıtlarda bulunan seçme ayağının dijital konumunda yapacağımız değişiklik ile bu seçme işlemini yaparız. Bu yönden diğer iletişim protokollerine \(özellikle I2C\) göre üstünlüğü bulunur. Her ne kadar sayısız cihaz bağlayabiliriz desek de dijital giriş ve çıkış ayağımız kadar bir sınırlamamız vardır. Bu sınırlamayı kaldırmak için çoklayıcı entegreler gibi dış birimleri kullanmamız gerekebilir. Bu iletişim protokolü Motorola firması tarafından gömülü sistemler üzerinde kullanmak üzere tasarlanmıştır. SPI protokolü herhangi bir amaca veya donanıma yönelik bir sistem olmadığı için oldukça geniş bir alanda kullanılmaktadır. Pek çok mikrodenetleyicide SPI protokolü bulunmakla beraber SPI üzerinden çalışan entegre ve birimler belli bir markaya ait değildir. Aynı zamanda da belli bir alanda da gruplaşmamıştır. Ada dilinin havacılıkta kullanılması, CAN Bus iletişiminin otomotiv sektöründe kullanılması gibi bazı dil, platform ve protokoller belli bir sektörde kullanılmak üzere tasarlanmıştır. Bunlar gibi olmayan SPI protokolünü ise gömülü sistemlerin hemen her alanında çalışacak programcıların öğrenmesi gereklidir.

SPI iletişiminde veri sinyali gönderilirken aynı zamanda da bir saat sinyali gönderilir. UART protokolünde gördüğümüz üzere baud oranı ile sınırlandırılmış bir çalışma sahamız yoktur.  SPI iletişiminde ana aygıt \(master\) ve uydu aygıt \(slave\) olmak üzere aygıtlar ikiye ayrılır. Tüm uydu aygıtlar tek bir ana aygıta bağlanmak zorundadır. Bu özellik protokolün temellerinden olup bizim ana aygıtımız ise mikrodenetleyici olacaktır. Mikrodenetleyiciye bağlayacağımız tüm aygıtlar uydu görevinde olup okuma ve yazma işini ana aygıt yapacaktır. Bu iletişimde her zaman ana aygıtın sözü geçmektedir.  SPI protokolünü kullanırken mikrodenetleyici neredeyse tamamen ana aygıt olarak kullanılmaktadır. Eğer birden fazla mikrodenetleyiciyi SPI ile birbirine bağlamak istiyorsak uydu aygıt olarak da kullanma imkanımız vardır.

SPI protokolünde dört adet ayak kullanılır. Bu ayaklardan üçü ortak olup biri ise her aygıt için ayrı olmalıdır. Bu ayakların açıklaması aşağıdaki gibidir.

* SCLK – Saat Sinyal Ayağı Olup Tüm Aygıtlarda Ortaktır. Bu ayaktan çıkan saat sinyali senkronizasyonu sağlar.
* MOSI – “Master Out Slave In” kelimelerinin kısaltımı olup ana aygıttan uydu aygıtlara giden veri bu ayak üzerinden iletilir.
* MISO – “Master In Slave Out” kelimelerinin kısaltımı olup uydu aygıtlardan ana aygıta giden veri bu ayak üzerinden iletilir.
* SS – “Slave Select” kelimelerinin kısaltımı olup her uydu ayak için ayrı bir seçilmiş ayaktır. Bu ayaklar hangi uydu aygıt üzerinde okuma ve yazma işlemi yapacağımızı belirler.

Şimdi SPI biriminin teknik veri kitapçığında belirtilen başlıca özelliklerine göz atalım.

* Tam dubleks, üç ayaktan senkronize veri transferi
* Ana ya da Uydu çalışma \(İstenilirse ana bir aygıta bağlanabilir.\)
* LSB \(Son bit önde\) ya da MSB \(Baş bit önde\) Veri Aktarımı
* Yedi Programlanabilir Bit Oranı
* İletişim Bittiğinde Kesme Bayrağı
* Yazma Çakışması Koruma Bayrağı
* Bekleme modundan uyanış
* Çift Hızlı \(CK/2\) Ana SPI Modu

Örnek bir SPI devresinin şeması aşağıdaki gibi olmalıdır.  
[![](http://www.lojikprob.com/wp-content/uploads/2018/09/350px-SPI_three_slaves.svg.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-46-spi-serial-peripheral-interface-iletisim-protokolu/attachment/350px-spi_three_slaves-svg/)

Resim: https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/SPI\_three\_slaves.svg/350px-SPI\_three\_slaves.svg.png

Resimde görüldüğü üzere SCLK, MOSI ve MISO ayakları ortak SS ayakları ise ana aygıtın ayrı ayaklarıdır. Biz de SPI bağlantımızı buna göre yapmak zorundayız.

SPI ile kullanabileceğimiz aygıt sayısı burada sayılamayacak kadar fazladır. Başlıca aygıtlar TFT ekranlar, algılayıcılar, hafıza birimleri, sd kart, modüller olarak sıralansa da onlarca firma tarafından onlarca farklı alanda kullanılmak üzere sayılamayacak kadar çok aygıt üretilmektedir. Bu aygıtları AVR mikrodenetleyiciler ile kullanabilmek için ise SPI öğrenmek şarttır. Bir sonraki yazıda SPI protokolünü ayrıntılı olarak anlatmaya devam edeceğiz.

_**Kaynaklar**_

DÖKMETAŞ, Gökhan, “_Arduino Eğitim Kitabı”_, İstanbul, 2016

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

