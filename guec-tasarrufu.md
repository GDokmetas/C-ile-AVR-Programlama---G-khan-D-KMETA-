# Güç Tasarrufu

Güç tasarrufu batarya ile beslenebilen ve taşınabilir uygulamalar için oldukça önem arz etmektedir. Sabit bir besleme kaynağına bağlı bir uygulamada çok lazım olmasa da 200-300 mili amper bir batarya veya pil ile besleme sağladığımız ve uzun süre çalışması gereken bir cihazda tasarruf edilecek birkaç mili amper bile uzun vadede oldukça önemli olacaktır. ATmega mikrodenetleyiciler kendi başlarına oldukça az bir elektrik tüketimine sahip olup en çok elektrik tüketimini ise giriş ve çıkış portları  vasıtasıyla gerçekleştirir. Mikrodenetleyicinin çıkış portundan onlarca led lambayı yakıyorsak bu konuda güç tasarrufunu mikrodenetleyicide beklememiz pek doğru olmaz.

Burada anlatacağımız güç tasarrufu zaten oldukça düşük güç tüketimine sahip olan mikrodenetleyicinin ne kadar daha az elektrik harcayacağıdır. Bazı dijital sistemler o kadar az elektrik tüketir ki ufak bir pille yıllar boyu çalışabilir. Örneğin, bilgisayarlarımızdaki BIOS pili buna bir örnektir. BIOS pili aslında bir gerçek zaman saatini besleyerek bilgisayarın saat ve tarih verisini güncel tutmaya yarar. BIOS pillerinin ortalama çalışma süresi 5 yıldır.

ATmega mikrodenetleyicilerin bazı birimlerini kapatarak gereksiz güç sarfiyatını önlemiş oluruz. Bir mikrodenetleyici olarak tek başına bir işlemci değil bu işlemciye bağlı ondan fazla çevre biriminden oluştuğu için her birim bağımsız olarak çalıştığı sürece beslemeden güç almaktadır. Sadece işlemcinin tükettiği elektrik söz konusu değildir.

Güç tasarrufu sadece güç tasarrufu amacıyla kullanılmaz. Mikrodenetleyici içerisinde çalışan bu birimler elektrik gürültüsüne sebep olmaktadır. Analog devrelerde ve analog işlemlerde bu gürültü hissedilmektedir. Analog dijital çevirimin doğru olması için de “ADC Noise Reduction” adında bir güç tasarrufu modu vardır.

Güç tasarrufu hakkında ipuçlarını önceki derslerde çok az da olsa dile getirmiş olsak da bu derste en büyük güç tasarrufu yöntemi olan uyku modlarını anlatacağız. Çevre birimlerini anlatırken her birimin kontrol yazmacında bir etkinleştirme biti olduğundan bahsetmiştik. Bu birimlerin sürekli etkin olmaları yerine ayrı bir bit ile etkinleştirmeleri başta sadece ayak seçimi ile ilgili görünse de aslında bir sebebi de güç tasarrufundan dolayıdır.

AVR mikrodenetleyicilerdeki uyku modları aşağıdaki tabloda özetlenmiştir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/uyk1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-54-guc-tasarrufu/attachment/uyk1/)

Burada iki ayrı bölüm vardır. Bunlardan biri saat sinyali ve osilatörler diğeri ise uyandırma kaynaklarıdır. Uyku moduna göre bazı osilatörler ve saat sinyalleri devre dışı bırakılır. Uykuda çalışmayan bir mikrodenetleyicinin ise kendini uyandırması için bir dış uyarana ihtiyaç vardır. Bazen bu bir ayak okuma ile bazen de dışarıdan bağlı bir düşük frekanslı osilatörle olabilir. Bu uyku modlarını yine yazmaçlar vasıtasıyla denetlememiz gereklidir.

Bu altı uyku modundan birine girmek için uyku modu denetim yazmacındaki uyku modu etkinleştirme bitini bir \(1\) yapmamız gereklidir. Uyku modunu seçmek için ise yine uyku modu seçme bitleri üzerinde işlem yapmamız gereklidir.

Uyku modunun özelliklerinden biri ise kesmelerin mikrodenetleyiciyi uyandırabilmesidir. Böyellikle uyandırma kaynaklarından biri  de kesmeler olmuş olur. Mikrodenetleyici kesme yürütüldükten sonra kendini otomatik olarak uyutur.

Bir sonraki derste güç tasarrufu ve uyku modlarını ayrıntılı olarak anlatmaya devam edeceğiz.

_**Kaynaklar**_

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

