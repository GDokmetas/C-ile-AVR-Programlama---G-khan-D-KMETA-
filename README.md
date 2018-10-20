---
description: Bu sayfada AVR Mikrodenetleyicilere Giriş Yapacağız
---

# Atmel AVR Nedir ?

#### AVR Nedir?

AVR, Atmel firması tarafından tasarlanıp 1996’dan beri piyasaya arz edilen mikrodenetleyici ailesinin adıdır. Bu mikrodenetleyiciler modifiye edilmiş Harvard mimarisi üzerine RISC komut kümesiyle tasarlanmıştır. Mikrodenetleyiciler 8-bit olup istisna olarak bir dönem 32-bit modelleri üretilmiştir. Gömülü sistemlerde ve özellikle hobi devrelerinde en çok kullanılan mikrodenetleyicilerden olup Arduino geliştirme platformunun da temelini oluşturmuştur.

Ülkemizde AVR mikrodenetleyiciler, PIC mikrodenetleyiciler ile beraber en yaygın kullanılan iki mikrodenetleyici ailesinden biridir. Dünyada ise özellikle batıda PIC mikrodenetleyicilerden daha yaygın kullanılmaktadır. Bunun sebebi ucuzluğu ve ücretsiz geliştirme ortamı sağlamasından dolayıdır. Birim maliyeti ucuz olduğundan seri üretimi yapılan cihazların içerisinde AVR mikrodenetleyicileri görmek mümkündür.

### ![](http://www.lojikprob.com/wp-content/uploads/2018/08/IC-ATMEGA328-PU-300x300.jpg)

Resim: https://protostack.com.au/wp-content/uploads/IC-ATMEGA328-PU.jpg

### 

### AVR Mikrodenetleyici Aileleri

Avr mikrodenetleyici aileleri aşağıda sayacağımızdan daha fazla olsa da işimize yarayacak ve günümüzde üretimi hala devam eden olanlardan bahsetmekle yetindik. Bu mikrodenetleyici aileleri arasında temelde bir fark olmayıp aynı geliştirme ortamı üzerinden programlama yapılsa da özellik ve kullanım alanı bakımından fark bulunur.

**tinyAVR**

Entegrelerin üzerinde ATtiny olarak kodlanan bu ailedeki mikrodenetleyicilerin özellikleri şöyledir,  
– 0.5 – 32 KB Program Hafızası

-6-32 pin arası entegre kılıfı

-Sınırlı arayüz tabanı

**megaAVR**

Entegrelerin üzerinde ATmega olarak kodlanan bu ailedeki mikrodenetleyicilerin özellikleri şöyledir,  
– 4-256KB Program Hafızası

-28-100 pin arası entegre kılıfı

-Genişletilmiş Komut kümesi

– Geniş arayüz tabanı

**XMEGA**

Entegrelerin üzerinde ATxmega olarak kodlanan bu ailedeki mikrodenetleyicilerin özellikleri şöyledir,

-16-384KB Program Hafızası

-44-64-100 pin entegre kılıfı

-Artırılmış performans özellikleri

-Artırılmış arayüz tabanı

Bundan başka 32-bit AVR ve FPSLIC yani FPGA ile AVR’yi birleştiren bir yapı olsa da bunlar artık bulunmamaktadır.

### PIC mi AVR mi ?

Bu sorunun kesin cevabı olmamakla beraber benim şahsi görüşüme göre AVR mikrodenetleyiciler çok büyük sebeplerden dolayı olmasa da PIC mikrodenetleyicilerden üstündür. Sebeplerini madde madde yazarsak,

1.  PIC mikrodenetleyiciler bir komutu işlemek için 4 çevirime ihtiyaç duyar. Yani 16 MHz’de çalışan bir mikrodenetleyici aslında 16/4 yani 4MHz’de çalışmaktadır. AVR Mikrodenetleyiciler ise bir komutu işlemek için çoğunlukla bir çevrime ihtiyaç duyar. O yüzden AVR mikrodenetleyiciler daha hızlıdır.
2. PIC mikrodenetleyiciler aynı özellikteki AVR mikrodenetleyicilere göre daha pahalıdır. AVR mikrodenetleyiciler ucuzdur.
3. AVR mikrodenetleyicilere başlamak için oldukça ucuza Arduino kartları bulabiliriz. Arduino kendi başına bir platform olsa da donanımsal olarak bir  geliştirme kartı olarak kullanılabilir.
4. AVR’nin Avrfreaks gibi uzun yıllardan beri canlılığını koruyan toplululuğu ve internette kütüphane, kaynak kod gibi alanlarda büyük bir birikimi vardır.
5. AVR’nin Visual Studio tabanlı tam C dili desteği veren AVR Studio geliştirme ortamı vardır. PIC’de ise uzun yıllar resmi olarak sadece assembly derleyicisi kullanıldı ve üçüncü parti birbirinden farklı C derleyicileri günümüzde hala kullanılmaktadır.

### Çevre Birimleri

Mikrodenetleyiciyi mikroişlemciden ayıran en büyük fark içerisinde gömülü halde bulunan çevre birimleridir \( ing. peripheral\).  Program hafızası, RAM, EEPROM, Zamanlayıcılar, Kesmeler, ADC gibi temel çevre birimlerinin çoğu hemen her AVR mikrodenetleyicide eksiksiz olarak bulunmaktadır. AVR mikrodenetleyicilerin her birinde ayrı bir özellik olan bir modeli yerine az model olup çoğu özelliğin toplandığı mikrodenetleyiciler olması bir mikrodenetleyiciyle hemen hemen tüm işleri yapmamıza imkan sağlar. O yüzden birkaç AVR mikrodenetleyiciyi öğrenmek bize yeterli gelecektir.

Kaynaklar :

AVR microcontrollers, Wikipedia, bağlantı: [https://en.wikipedia.org/wiki/AVR\_microcontrollers](https://en.wikipedia.org/wiki/AVR_microcontrollers), Erişim Tarihi : 14.08.2018  
PIC vs. AVR smackdown, ladyada.net, bağlantı:  http://www.ladyada.net/resources/picvsavr.html, Erişim Tarihi: 14.08.2018  
Kapak Resmi :https://protostack.com.au/wp-content/uploads/IC-ATMEGA328-PU.jpg

