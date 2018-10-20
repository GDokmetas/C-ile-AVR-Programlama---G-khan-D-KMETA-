# SPI Protokolünü AVR’de Kullanmak

AVR ile ilgili konulara geçmeden önce örnek bir SPI iletişiminin lojik analizör grafiğini verip bunun analizini yapalım. Böylelikle SPI protokolünün nasıl çalıştığını kolaylıkla anlayacaksınız.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/SPI_logical_analyzer_1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-47-spi-protokolunu-avrde-kullanmak/attachment/spi_logical_analyzer_1/)

Resim : http://www.gammon.com.au/images/SPI\_logical\_analyzer\_1.png  
Burada grafiği anlamamız için bazı noktalar harfler ile işaretlendirilmiştir. Bu harfleri madde madde açıklayalım.

* A : SPI iletişim olmadığı zaman ayakların bu vaziyette olması gereklidir. SS ayağı HIGH konumdadır ve diğer ayaklar durgundur.
*  B : Bu noktada öncelikle uydu aygıt üzerinde okuma ve yazma işlemi yapmak için uydu aygıtın iletişimde etkin hale getirilmesi gereklidir. Bu da SS ayağının LOW konuma çekilmesiyle olur. Uydu aygıt bu ayağın LOW konuma çekilmesiyle iletişimin başlayacağını anlamış olur.
* C : İlk bayt verisi uydu aygıta gönderilir. Her 8 bit için 8 ayrı saat çevrimi yapılmaktadır. MOSI ayağı gönderilecek veriye göre LOW ya da HIGH konuma geçer. MOSI ayağı UART protokolünde olduğu gibi çalışsa da burada SCK ile saat sinyali de gönderilmektedir. Saat sinyali ile veri sinyalinin nasıl karşılıklı olduğuna dikkat ediniz.
* D – Diğer bayt gönderilir.
* E – Diğer bayt gönderilir.
* F – Veri gönderimi yok fakat SS etkin. Uydu aygıt veri gönderimine açık.
* G – SS HIGH konuma getirilerek devre dışı bırakılır ve veri gönderme süreci bitirilir. Böylelikle seçili uydu aygıt SPI sinyallerini görmezden gelir.

Eğer uydu aygıt bir veri yollayacaksa bu süreçte MISO ayağından veri yollayabilirdi. Şimdi tek bir karakterin gönderimi sırasında olan mantıksal grafiğe bakalım.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/SPI_logical_analyzer_2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-47-spi-protokolunu-avrde-kullanmak/attachment/spi_logical_analyzer_2/)

Resim: http://www.gammon.com.au/images/SPI\_logical\_analyzer\_2.png

Burada F harfi \(0b01000110\) gönderilirken her saat sinyalinin yükselen kenarında veri yollanmaktadır. Bitler tek tek sırayla yollanırken baş bit önde son bit ise sonra yollanmaktadır. Yollanma moduna göre bu tersten de olabilir. Ayrıca yine moda göre düşen kenarda vd. konumlarda kullanabiliriz. Bu konuları şimdi açıklayacağız.

**Hız**

Atmega328P entegresinde yukarıda görüldüğü üzere 3 mikrosaniyede bir bayt gönderilmektedir. Bu da 325.5 kilobayt / saniye hıza denk gelmektedir.  Ayrıca teorik olarak 888 KB/s hıza kadar ulaşmak mümkündür bu da yakın bir zamana kadar pek çoğumuzun kullandığı internetten daha hızlı bir hız anlamına gelmektedir.

SPI Hakkında temel bilgiler ve Arduino ile kullanımı için şu bağlantıyı ziyaret edebilirsiniz,  
[http://www.gammon.com.au/spi](http://www.gammon.com.au/spi)

AVR mikrodenetleyicilerde SPI biriminin blok diyagramı aşağıdaki gibidir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/spi1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-47-spi-protokolunu-avrde-kullanmak/attachment/spi1/)

Bu blok diyagramı UART protokolünü okuduysanız biraz tanıdık gelecektir. Her birimde olduğu gibi durum, denetim ve veri yazmaçları yer alsa da burada bir de belli bir saat sinyali ve ön derecelendirici vardır. Artık bu noktadan sonra mikrodenetleyici birimlerinin mantığını anladığımız için yazmaçların görevlerini az çok öngörmemiz lazımdır.

SPI ana aygıtı iletişimi uydu seçimi \(SS\) ayaklarını sıfır \(0\) yaparak başlatır. Ana ve uydu aygıt kendi kayan yazmaçlarından \(shift register\) gönderilecek veriyi hazırlar. Ana aygıt, SCK ayağından gereken saat sinyalini üretir. Veri her zaman ana aygıttan uydu aygıtlara MOSI ayağından gönderilir. Uydu aygıtlardan ana aygıtlara ise MISO ayağından gönderilir. Her veri paketinden sonra ana aygıt SS ayağını bir \(1\) yaparak uydu aygıtla senkronize olur.

SS ayağı eğer otomatik bir denetime sahip değilse her zaman yazılım tarafından denetlenmesi gerekir. SS ayağı denetlendikten sonra SPI Veri yazmacına uygun veri yazılmalıdır. Veri yazıldıktan sonra SPI saat üreteci çalışmaya başlar ve donanım sekiz bitlik veriyi uydu aygıta doğru kaydırır yani seri olarak gönderir. Eğer SPI kesmesi etkin ise kesme yürütülür. Ana aygıt tekrar bir veri göndermek istiyorsa veri yazmacına o veriyi yazar. Eğer veri alışverişi bitti ise SS ayağı bir \(1\) konumuna getirilir.

Eğer mikrodenetleyici uydu aygıt olarak tanımlanırsa SS ayağı sıfır \(0\) olana kadar SPI birimi uykuya geçer. Bu durumda yazılım SPI Veri Yazmacını güncelleyebilir fakat bu verinin gönderilmesi için muhakkak SS ayağının ana aygıt tarafından sıfır \(0\) konumuna getirilmesi gerekir. Bir baytın gönderimi tamamlanınca gönderim bittiğine dair bayrak biti bir \(1\) konumuna geçer. Eğer SPI kesmesi etkin hale getirilmişse kesme yürütülür. Uydu aygıt ana aygıta göndereceği veriyi tekrar veri yazmacına yazabilir.

Sonraki dersimizde yazmaçları ve geri kalan teorik konuları anlatarak SPI konusuna devam edeceğiz.

_**Kaynaklar**_

Nick Gammon, SPI – Serial Peripheral Interface – for Arduino, http://www.gammon.com.au/spi, Erişim Tarihi : 27.09.2018

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

