# Uyku Modları ile Güç Tasarrufu

AVR denetleyicilerde uyku modlarından önceki yazımızda bahsetmiştik. Bu modların geniş açıklamasını ise bu yazıda yapacağız. Dersin devamında ise yazmaçları açıklayıp konumuzu bitireceğiz. Bu konu kolay konulardan biri olduğu için anlamanız için pek fazla çaba sarf etmeyeceğim. Derslerin başından beri yaptığımız gibi yazmaçların belli bitlerini değiştirerek istediğimiz işi yaptırabiliyoruz. Sizin bu modların nasıl olduğu ve nasıl çalıştığı konusunda bilgi sahibi olmanız için biraz daha ayrıntılı olarak modları anlatmaya devam edelim.

**BOD Devre Dışı Bırakma**

Mikrodenetleyicide “Brown-out Detector” adı verilen kararma algılayıcısı bulunmaktaydı. Bu algılayıcı mikrodenetleyiciye giden beslemede düşüş olması durumunda \(örneğin 2.7V aşağısı\) mikrodenetleyiciyi otomatik olarak resetliyordu. Bu mikrodenetleyicinin yetersiz beslemeye bağlı kararsız çalışmasını önlemek içindi. Kararma algılayıcısı sigorta bitleri tarafından denetlendiği için bunu programlarken programlayıcı arayüzünden seçip yapabiliriz. Programlama yazılımı olarak AVRDUDESS programını tavsiye ederim. Arayüz olarak kullanışlı olan bu programda gerekli sigorta ayarlarını yapabilirsiniz.

Sigorta hesaplamaları ise şu bağlantıdan yapılabilir. Sigorta değerleri hesaplandıktan sonra program vasıtasıyla yazdığımız programdan ayrı olarak mikrodenetleyiciye yüklenir. Sigorta bitlerini daha sonra anlatacağımız için burada bırakalım.

[http://www.engbedded.com/fusecalc](http://www.engbedded.com/fusecalc)

Sigorta ile etkin hale getirilen kararma algılayıcısı sürekli besleme gerilimini ölçerek belli bir güç tükettiğinden bunu sonraca yazılım ile kapatmamız mümkündür. MCUCR yazmacının 6. biti olan BODS biti bir \(1\) yapılarak kararma algılayıcısı devre dışı bırakılabilir. Bu devre dışı bırakma ile uyku modunda fazladan güç tasarrufu sağlanır.

**Bekleme Modu \(Idle\)**

SM bitleri “000” yapıldıında uyku modu olarak bekleme modu seçilir. Bekleme modunda mikroişlemci durdurulur fakat SPI, USART, Analog Karşılaştırıcı, WDT ve kesmeler çalışmaya devam eder. Bu uyku modu işlemci ve flash hafıza saatini durdurur fakat diğer saatlerin işlemesine izin verir. Bekleme modunda denetleyici uyandırıcılardan biri olan kesmeler tarafından rahatlıkla uyandırılabilir.

**ADC Gürültü Önleme Modu**

Arduino’da ADC gürültüsünü önlemek için meşhur bir program vardı. Bu program hali hazırda gürültülü olan ADC değerlerini toplayıp bu gürültülü değerlerin ortalamasını alarak gürültüyü önlediğini iddia ediyordu. Aslında bu bir yönden işe yarar olsa da mevcut gürültü hiçbir zaman ortadan kalkmış olmuyordu. Üreticiler bu gürültünün farkında olup programcıları böyle yollara itmemek için gürültü önlemeye yönelik özel bir mod getirmiştir. SM bitleri “001” yapıldığında denetleyici ADC gürültü önleme moduna girer. İşlemci durdurulsa da ADC, harici kesmeler, TWI, TC2 ve WDT işlemeye devam eder. Böylece diğer birimlerin ve işlem biriminin yaptığı gürültü ölçüme etki etmemiş olur. Hassas analog işlemlerde bu gürültü bazen hiç doğru çalıştırmayacak derede etkiye sahip olduğu için bu özellik oldukça faydalı olacaktır.

ADC gürültü önleme modundan ADC çevirim tamamlandı kesmesi ile çıkabilir ve okunan değeri kaydedebiliriz. Bundan başka harici kesme, WDT sistem reset, WDT kesmesi, kararma reseti, TC2 kesmesi, TWI adres eşleşmesi, EEPROM hazır kesmesi gibi durumlarda da uyku modundan uyanılabilir.

**Power-Down \(Güç Kesme\) Modu**

SM bitleri “010” yapıldığında uyku modu olarak Power-Down modu seçilmiş olur. Bu modda harici osilatör durur ve dış kesmeler, TWI adres izlemesi , WDT \(Watch-Dog Timer\) çalışmaya devam eder. Ancak aşağıdaki durumlardan biri mikrodenetleyiciyi uyandırabilir.

* Dış reset
* WDT Sistem Reseti
* WDT kesmesi
* Kararma reseti
* TWI adres eşleşmesi
* INT ayağından dış kesme
* Ayak değişme kesmesi

Bu uyku modu temel olarak bütün üretilen saat sinyallerini durdurur. Sadece asenkron çalışan modüllerin çalışmasına izin verir.

Bir sonraki yazıda bu modları anlatmaya devam edeceğiz.

_**Kaynaklar**_

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

