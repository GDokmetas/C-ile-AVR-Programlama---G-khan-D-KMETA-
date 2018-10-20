---
description: Bu sayfada uygun AVR mikrodenetleyiciler hakkında bilgiler yer almaktadır.
---

# Uygun AVR Seçimi

Öğrenmek için seçeceğimiz mikrodenetleyici aynı zamanda gelecekteki projelerimizde kullanacağımız mikrodenetleyicidir. İçini dışını iyi bildiğimiz bir parçayı öncelikli olarak seçmek yeni parçayı öğrenme zahmetinden bizi kurtaracaktır. O yüzden öğrenmeye başlarken uygun mikrodenetleyiciyi seçmek çok önemlidir. Atmel AVR mikrodenetleyicilerde yanlış mikrodenetleyiciyi seçmek pek mümkün olmasa da PIC mikrodenetleyicilerden hepimizin bir yerden duyduğu 16F84 entegresinde ADC bile yoktur. Buna rağmen 16F84 üzerine pek çok ders yazılmış hatta kitap bile basılmıştır. Doğru olan ortanın biraz üstü seviyede bir entegre kullanmaktır.

AVR mikrodenetleyicilerde PIC mikrodenetleyicilerde olduğu gibi yüzlerce kod addan oluşan farklı entegreler yoktur. Çeşit az olup özellikler dağınık halde değil toplu haldedir. O yüzden uygun entegreyi seçmemiz oldukça kolay olmaktadır. Bizim derslerde kullanmak ve ileride proje yapmak için tavsiye ettiğimiz entegreler %99’a yakın işimizi görecek seviyede olacaktır.

Uygun AVR seçimi üst seviye denecek bir entegre seçiminin yanında yaygın olarak kullanılan entegrelerden birini seçmekle olur. AVR kodları birbirine benzese de özellikle ayak sayısı ve bazı özelliklerin kullanılması bakımından her proje için yazılmış kod her entegrede çalışmayabilir. Bunun için çoğu projeyi kodda ufak düzeltmelerle çalıştıracak kapasitede bir entegrenin seçilmesi gerekir.

Entegrelerin kılıfları aşırı derecede önem arz etmektedir. DIP \(Dual Inline Package\) kılıfta olmayan SMD entegreler yeni başlayanların pek sıcak bakmadığı, pahalı ekipmanlara ihtiyaç duyan, prototiplemeyi zorlaştıran yapıdadır. O yüzden yeni başlayanlar DIP kılıftaki entegreleri kullanmak zorundadır.

Seçilecek AVR 8-bit mikrodenetleyici ailesinin özelliklerinin çoğunu barındırmak zorundadır. Böylelikle mikrodenetleyici ile yapabileceklerimizin alanı genişlerken diğer entegrelere ait özellikleri de öğrenmiş oluruz. Bu özellikler özetle ADC, PWM, Zamanlayıcı, Kesmeler, USART, SPI, I2C gibi iletişim ve çevre birimleridir.

Şimdi derslerde ve projelerde kullanmak üzere seçtiğimiz üç entegreden bahsedelim.

**Atmega328P**

## 

Bu entegreyi seçmemizin en büyük nedeni Arduino Uno kartında kullanılmasıdır. Pek çok Arduino kodu ve projesi Atmega328P üzerine yazıldığından internette oldukça geniş kaynak bulmamız mümkündür. Ayrıca ATmega mikrodenetleyiciler arasında özellik ve kullanışlılık bakımından en üst seviyede olan entegrelerden biridir. Mikrodenetleyicinin özellikleri şu şekildedir,

**Ayak Adedi: 32**

**Program Hafızası : 32KB**

**SRAM: 2KB  
EEPROM: 1KB**

**ADC \(bit\) : 10**

**ADC \(ch\) : 6**

**PWM : 6  
8-bit zamanlayıcı: 2**

**16-bit zamanlayıcı: 1**

**Atmega32A**

40-pin DIP kılıfta olan Atmega32A fazladan ayak sayısı gereken durumlarda kullanılmaya uygundur. Ayrıca uzun zamandan beri hobicilerin tercih ettiği bir entegre olup hakkında pek çok kaynak kod ve proje vardır.

**Ayak Adedi: 40**

**Program Hafızası : 32KB**

**SRAM: 2KB  
EEPROM: 1KB**

**ADC \(bit\) : 10**

**ADC \(ch\) : 8**

**PWM: 4  
8-bit zamanlayıcı: 2**

**16-bit zamanlayıcı: 1**

**Atmega32u4**

Arduino Leonardo’da da kullanılan bu entegre pek çok ATmega ailesinin ferdinde bulunmayan USB özelliği olması sebebiyle bazı projelerde kullanmak zorunda olacağımız bir entegre olacaktır. Bu entegrenin en büyük eksi yanı DIP kılıfının olmayışıdır. O yüzden SMD lehimleme ekipmanlarına ihtiyacımız olacaktır.

**Ayak Adedi: 44**

**Program Hafızası : 32KB**

**SRAM: 2.5KB  
EEPROM: 1KB**

**ADC \(bit\) : 10**

**ADC \(ch\) : 12**

**PWM:4**

**8-bit zamanlayıcı: 2**

**16-bit zamanlayıcı: 1**

###  **Kataloglar**

Mikrodenetleyici seçiminde Microchip tarafından yayınlanan resmi kataloglardan yararlanabilirsiniz.

**8-bit AVR® Microcontrollers Peripheral Integration Quick Reference Guide**  
[http://ww1.microchip.com/downloads/en/DeviceDoc/30010135D.pdf](http://ww1.microchip.com/downloads/en/DeviceDoc/30010135D.pdf)

**8-bit PIC® and AVR® Microcontrollers**  
[http://ww1.microchip.com/downloads/en/DeviceDoc/30009630M.pdf](http://ww1.microchip.com/downloads/en/DeviceDoc/30009630M.pdf)

**Atmel Product Selection Guide**  
[http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-45154-Product-Selection-Guide\_Brochure.pdf](http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-45154-Product-Selection-Guide_Brochure.pdf)

**Kaynaklar**  


ATmega16U4/32U4 Datasheet – Microchip Technology, http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7766-8-bit-AVR-ATmega16U4-32U4\_Datasheet.pdf, Erişim Tarihi : 14.08.2018

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 14.08.2018

Atmel ATmega32A – Microchip Technology, http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-8155-8-bit-Microcontroller-AVR-ATmega32A\_Datasheet.pdf, Erişim Tarihi: 14.08.2018

Kapak Resmi :https://protostack.com.au/wp-content/uploads/IC-ATMEGA328-PU.jpg

