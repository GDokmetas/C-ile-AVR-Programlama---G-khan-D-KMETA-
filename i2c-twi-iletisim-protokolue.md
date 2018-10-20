# I2C \(TWI\) İletişim Protokolü

50. derse başlarken önümüzdeki konuları şöyle bir gözden geçirelim,  
Zor konulardan geriye sadece I2C iletişim protokolü kalmış oluyor. Bu da basit bir prensipte olduğu için aslında çok da zor sayılmaz. Fakat teknik veri kitapçığıda çok tafsilatlı anlatıldığı için uzun bir konu olacaktır. Teknik veri kitapçığını incelediğimizde ise geriye şu konuların kaldığını görüyoruz,

* Güç Denetimi ve Uyku Modları
* Sistem Denetimi ve Reset
* USARTSPI \(Bu konu gerek görülmediği için atlanacak.\)
* Debug Bağlantısı \(Yine bu konu atlanacak.\)
* Bootloader
* MEMPROG- Hafıza Programlama \(Bu konu yazılımla halledildiği için geçilecek.\)

Görüldüğü gibi AVR’ye ait konulardan geriye pek bir şey kalmamış. Geri kalan fiziksel konular ise \(karakteristikler\) bizi programlamada ilgilendirmediği için lazım olduğu zaman ileride bahsedilecektir.

Şimdi I2C Protokolünü anlatmaya başlayalım.

I2C iletişim protokolü lisans engelinden dolayı Atmel mikrodenetleyicilerde TWI olarak adlandırılır. Biz her ne kadar doğrusunu telaffuz etmeye çalışsak da TWI dediğimiz zaman sizin I2C olarak anlamanız gereklidir. Bu protokolü Philips firması kendi ürettiği entegre sistemler arasındaki düşük hızda iletişimi gerçekleştirmek için geliştirmiştir. Pratik bir protokol olduğu için çoğu mikrodenetleyici ve dijital denetim birimi kendi içerisinde I2C iletişim birimini bulundurur. Bu protokolde tüm veri alışverişi sadece 2 hat üzerinden sağlanır. Üstelik asenkron bir iletişim değil senkron bir iletişim söz konusudur. İki hat üzerinde çalışan protokole bağlı cihazların her birinin farklı bir adresi vardır. Bir I2C sistemine en fazla 119 aygıt bağlanabilir. I2C aygıtların 7 bitlik bir adres değeri olduğu gibi bu aygıtların belli bir adres sınırı vardır. Her aygıta kafamıza göre adres veremeyiz. Bu adres programlama işi aygıtların adres ayakları vasıtasıyla yapılır.

I2C Protokolünde iki adet hattın olduğunu söylemiştik. Bu hatların biri SDA yani Veri hattı öteki ise SCL yani Saat hattıdır. Bu iki hatta bütün aygıtlar bağlanır. Yine SPI Protokolünde olduğu gibi Ana ve Uydu aygıt olmak üzere iki ayrı aygıt kategorisi vardır. Ana aygıt uydu aygıtlar üzerinde denetimi sağlar.  Örnek bir I2C devresi şekildeki gibidir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/36684.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-50-i2c-twi-iletisim-protokolu/attachment/36684/)

Resim: http://www.analog.com/-/media/analog/en/landing-pages/technical-articles/i2c-primer-what-is-i2c-part-1-/36684.png?la=en&w=900

Burada SDA ve SCL ayaklarına Pull-up dirençlerinin bağlandığına dikkat edin. Sağlıklı bir iletişim için bu dirençleri bağlamak gereklidir. Besleme haricinde bütün bağlantılar için sadece iki hattın olduğuna dikkat ediniz.

Şimdi I2C protokolünde örnek bir veri gönderiminin lojik analizör grafiğine bakalım.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/I2C_logic_analyzer_1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-50-i2c-twi-iletisim-protokolu/attachment/i2c_logic_analyzer_1/)

Resim: http://www.gammon.com.au/images/I2C\_logic\_analyzer\_1.png

Bu resimde önemli noktalar yazı ile belirtilmiştir. Şimdi bu noktaları madde madde açıklayalım.

* Start noktasında SCL \(Seri Saat\) ayağı HIGH konumdayken SDA \(Seri Veri\) ayağı LOW konumuna çekilir. Böylelikle iletişim başlatılmış olur.
* Uydu aygıta veri göndermek için öncelikle aygıtın 7 bitlik adresini yazmak gereklidir. Burada baş bit öncelikli olarak gönderilir. Bu grafikte adres değeri 42’dir. Eğer herhangi bir uydu aygıt bağlı değilse veya adres gerektirmiyorsa bu duruma adres görmezden gelinir ve SDA ayağı pull-up direnci sayesinde HIGH konmda kalır. Bu NAK \(Negatif Onaylama\) olarak sayıdır. Bu yazılım tarafından denetlenebilir.
* Gönderilecek bayt verisi sonrasında aynı hattan gönderilir. Yine üst bit en önce gönderilmektedir. Bu grafikte bu değer 53’dür.
* Bundan sonra diğer baytlar da gönderilebilir fakat burada gösterilmemiştir.
* Gönderim Stop durumuna gelme ile biter. SCL HIGH durumda iken SDA ayağı HIGH konuma çekilir ve iletişim bitmiş olur.

> Düzgün bir kare dalga elde etmek istiyorsak 4.7K direnç ile pull-up olarak hattın beslemeye bağlanması gereklidir. 2.2K veya 1K direnç de kullanılabilir. Direncin değeri düştükçe çekilen akım artsa da dalganın düzgünlüğü de artmaktadır.

Şimdi iki adet osiloskop görüntüsü ile bu ikisi arasındaki farkı açıklayalım. İlk görüntüde sadece dahili pull-up dirençleri etkin hale getirilmiş ve herhangi bir dış pull-up direnci takılmamıştır.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/Two_pullups.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-50-i2c-twi-iletisim-protokolu/attachment/two_pullups/)

Resim: http://www.gammon.com.au/images/2.2K\_pullup.png

İkinci görüntüde ise 2.2K harici pull-up direnci hatta bağlanmıştır. Öncekine göre ne kadar düzgün bir kare dalga elde edildiğini göreceksiniz.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/2.2K_pullup.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-50-i2c-twi-iletisim-protokolu/attachment/2-2k_pullup/)

Resim: http://www.gammon.com.au/images/2.2K\_pullup.png

Bir sonraki dersimizde teknik veri sayfası üzerinden I2C iletişim protokolünü anlatmaya devam edeceğiz.

_**Kaynaklar**_

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

DÖKMETAŞ, Gökhan, “_Arduino Eğitim Kitabı_“, İstanbul, 2016

I2C – Two-Write Peripheral Interface – for Arduino, Nick Gammon, http://www.gammon.com.au/i2c, Erişim Tarihi : 27.09.2018

