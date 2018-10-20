# Güç Tüketimini En Az Seviyeye İndirmek

Öncelikle önceki derste yarım bıraktığımız uyku modlarını anlatarak derse başlayalım.

**Güç Tasarrufu \(Power-Save\) Modu**

SM bitleri “011” yapıldığında denetleyici Power-Save modunda uykuya geçer. Bu mod power-down moduyla aynı olsa da bir yönden farklılığı vardır. Eğer TC2 zamanlayıcısı etkinleştirildi ise bu zamanlayıcı uyku süresinde çalışmaya devam eder. Taşma ve karşılaştırma eşleşmesi kesmelerinde denetleyici uyandırılır. Böylelikle mikrodenetleyici belli aralıklarla uyanıp kod işler ve sonrasında tekrar kendini derin uykuya sokar. TC2 zamanlayıcısına dışarıdan bir sinyal vermekle bu aralık uzatılabilir. Böylelikle güç tasarrufu daha da artırılmış olur.  Eğer TC2 zamanlayıcısı işletilmiyorsa power-down modunun kullanılması tavsiye edilir.

**Bekleme \(Standby\) Modu**

SM bitleri “110” yapıldığı zaman denetleyici bekleme moduna girer. Bu mod power-down moduyla aynı olsa da osilatör çalışmaya devam eder. Aygıt altı saat sinyalinde uyanır.

**Genişletilmiş Bekleme \(Extended Standby\) Modu**

SM bitleri “111” yapıldığında denetleyici genişletilmiş bekleme moduna girer. Bu mod power-save modu ile aynı olsa da bu modda osilatörler çalışmaya devam eder.

**Güç Tasarrufu Yazmacı**

Bu yazmaç çeşitli çevre birimlerini durdurarak güç tüketimini düşürmeyi sağlar. PRR \(Power Reduction Register\) yazmacındaki biti tekrar sıfır \(0\) yaparak o çevre birimini kullanmaya devam edebiliriz.

**Güç Tüketimini Asgari Seviyeye Çekmek**

Güç tüketimini en dibe çekmek için çeşitli yollar mevcuttur. Uyku modları mümkün olduğu kadar fazla kullanılmalıdır. Çünkü uyku modları en büyük güç tasarrufunu sağlayan yollardan biridir. Ayrıca uyku modunda bizim çalıştıracağımız denetleyici özelliklerinin de mümkün olduğu kadar az olması gereklidir. Uyku modunda mikrodenetleyiciye neredeyse hiçbir şey yaptırmamak lazımdır.

Eğer ADC etkinleştirilirse ADC birimi tüm uyku modlarında çalışmaya devam eder. Güç tasarrufu için uyku moduna girmeden önce ADC’nin kapatılması gereklidir.

Bekleme \(Idle\) moduna giriliyorsa eğer kullanılmayacaksa analog karşılaştırıcı kapatılmalıdır. ADC gürültü önleme moduna girilirken ise analog karşılaştırıcı muhakkak kapatılmalıdır. Diğer modlarda analog karşılaştırıcı otomatik olarak kapatılır. Bu birimlerin nasıl açılıp kapatılacağını yazmaçlarından bahsettiğimiz derslerde açıkladık.

Kararma algılayıcısı uygulamada gereksiz ise kapatılması gereklidir. Eğer sigortadan etkinleştirilmiş ise tüm uyku modlarında çalışacak ve güç tüketecektir. Derin uyku modlarında ciddi bir oranda güç tüketimi olarak karşımıza çıkacaktır. Bunu devre dışı bırakmayı önceki derste açıklamıştık.

WDT zamanlayıcısı gerek duyulmuyorsa kapatılması gereklidir. Eğer WDT zamanlayıcısı açık ise tüm uyku modlarında çalışacaktır. Bu zamanlayıcısı sistem denetim ve reset konusunda ileride açıklayacağız.

Bütün port ayakları uyku moduna girilmeden önce asgari güç tüketimine göre ayarlanmış olmalıdır. Dijital giriş tamponunu devre dışı bırakmak analog işlemlerde güç tasarrufu sağlayacaktır.

Eğer projemiz hız gerektirmiyorsa harici kristal osilatör yerine dahili osilatörü kullanarak da güç tüketimini azaltabiliriz. Hız düştükçe güç tüketimi her alanda düşecektir.

Güç tasarrufuna dair anlatacağımız bilgiler bu kadardı. Geriye ise bu konuda kullanılacak yazmaçlar kalmıştır. Yazmaçları ise sonraki dersimizde anlatacağız.

_**Kaynaklar**_

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

