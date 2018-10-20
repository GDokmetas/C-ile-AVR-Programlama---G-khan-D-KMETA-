# Zamanlayıcı Çalışma Modları

Önceki yazılarımızda zamanlayıcıların yazmaçlarını ve iç yapısını anlatmıştık. Bu yazıda çalışma modlarından bahsedeceğiz ve sonrasında uygulamalara geçeceğiz. TC1 zamanlayıcısının yazmaçlarını da anlatmayı düşünsek de TC0 zamanlayıcısının yazmaçları ile hemen hemen aynı olduğu ve aradaki farklara ise TC1 zamanlayıcısının iç yapısını anlatırken değindiğimiz için bu konuyu atlayacağız. Eğer isteyen olursa Atmega 328p teknik veri kitapçığına \(datasheet\) başvurabilir. Yazmaç adları ve açıklamaları TC0 yazmacı ile benzer olduğu için TC0 yazmaçları hakkında anlattığımız çoğu şey TC1 yazmaçları için de geçerlidir. Aynı şekilde TC0 ve TC1 hakkında anlattığımızın çoğu TC2 için de geçerlidir.  Şimdi çalışma modlarından bahsedelim ve konumuzu bitirelim.

Çalışma modları zamanlayıcı ve sayıcı ile çıkış karşılaştırma ayaklarının nasıl çalışacağını belirler. Bu modları seçmek için Dalga formu üretici modu bitleri \(WGM1\[3:0\]\) ile karşılaştırma çıkış modu bitleri \(TCCR1A.COM1x\[1:0\]\) kullanılır. Bu bitlerin durumuna göre zamanlayıcımız farklı modlarda çalışır.

**Normal Mod**

Bu mod en basit çalışma modu olup WGM1\[3:0\] bitlerinin tamamı sıfır olunca etkin hale gelir. Bu modda sayma hep yukarıya doğrudur ve sayıcı sıfırlama yürütülmez. Sayıcı azami 16-bit değere ulaşınca otomatik sıfırlanır ve 0’dan itibaren tekrar saymaya başlar. TCNT1 yani sayaç verisi yazmacı sıfırlandığında zamanlayıcı taşma bayrak biti bir konumuna geçer. Zamanlayıcı taşma kesmesinde bu bayrak biti sıfırlanır. Zamanlayıcı çözünürlüğü yazılımsal olarak artırılabilir. Yeni sayaç değeri istenilen bir zamanda yazılabilir.

**Karşılaştırma Eşleşmesinde Sıfırlama Modu \(CTC\)**

Bu moda WGM1\[3:0\] bitlerinin 0x4 yapılmasıyla geçilir. OCR1A ve ICR1 yazmaçları sayaç çözünürlüğünün manipulasyonunda kullanılır. Sayaç değeri OCR1A ya da ICR1 yazmacına ulaştığında sıfırlanır. OCR1A ve ICR1 yazmaçları sayacın üst değerini belirler bundan dolayı da çözünürlüğünü etkiler. Bu mod karşılaştırma eşleşmesi çıkışı frekansını denetimini sağlar. Ayrıca harici olayları sayma işini de kolaylaştırır. CTC modunun zaman diyagramı aşağıda verilmiştir. TCNT1 değeri \(sayaç değeri\) OCR1A ya da ICR1 ile eşleşene kadar artmaya devam eder. Sonrasında TCNT1 değeri sıfırlanır.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc112.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-32-zamanlayici-calisma-modlari/attachment/tc112/)

Eğer sayıcı değeri TOP değere ulaşırsa kesme yürütülebilir. Bunlar OCF1A’nın ve ICF1’in bayrak bitlerini kullanılarak yapılır. Eğer kesmeler etkin ise kesme fonksiyonu TOP değeri güncellemek için kullanılabilir.

CTC modunda dalga formu çıkışı isteniyorsa OC1A çıkışının mantıksal değeri her eşleşmede değiştirilmelidir. OC1A değeri o ayak çıkış yapılmadığı sürece okunamaz. O yüzden DDR\_OC1A = 1 olmalıdır. Üretilen dalga formunun azami frekansı foc1a = fclk\_io/2 olabilir. Dalga formu frekansı aşağıdaki formülde verilmiştir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc113.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-32-zamanlayici-calisma-modlari/attachment/tc113/)

Burada f clk\_i/o işlemci frekansı, N ön derecelendirici faktörü, n ise aygıt numarasıdır. Normal modda olduğu gibi zamanlayıcı sayıcı TOV bayrak biti sıfırlanma sırasında bir \(1\) konumuna geçer.

**Hızlı PWM Modu**

Hızlı PWM modu yüksek frekansta PWM dalgası oluşturmayı sağlar. Sayaç BOTTOM konumundan TOP konumuna kadar saymaya devam eder ve sonrasında tekrar BOTTOM konumundan saymaya başlar. Terslemesiz karşılaştırma çıkış modunda çıkış karşılaştırma yazmacı \(OC1x\) TCNT1 ve OCR1x yazmaçları arasında eşleşme olduğunda BOTTOM değerine sıfırlanır. Terslemeli karşılaştırma modunda ise çıkış karşılaştırma eşleşmesine eşlenir ve BOTTOM değerine sıfırlanır. Yüksek frekanslı PWM modu güç regülasyonu, doğrultma ve DAC uygulamaları için uygundur. Yüksek frekans ayrıca daha küçük boyutlu dış parça kullanımına olanak tanıdığı için maliyetleri düşürür.

Hızlı PWM’nin çözünürlüğü 8, 9 ya da 10-bit ‘e ayarlanabilir. Ya da isteniyorsa kesin çözünürlük ICR1 ve OCR1A yazmaçlarıyla ayarlanabilir. Asgari çözünürlük ise 2 bittir. Azami çözünürlük ise 16 bittir. Bu durumda ICR1 ve OCR1A yazmaçları azami değere alınmalıdır. PWM çözünürlüğü aşağıdaki formülden hesaplanabilir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc114.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-32-zamanlayici-calisma-modlari/attachment/tc114/)

**Faz Düzeltmeli PWM Modu**

Faz düzeltmeli PWM modu yüksek çözünürlüklü faz düzeltmeli dalga formu üretimi olanağı sağlar. Bu sefer sayıcı BOTTOM \(0\) değerinden TOP değerine saymaya başlar ve ardından TOP değerinden BOTTOM değerine saymaya devam eder.

**Faz ve Frekans Düzeltmeli PWM modu** 

Faz ve frekans düzeltmeli PWM modu yüksek çözünürlüklü faz ve frekans düzeltmeli PWM dalga formu olanacağı sağlar.  Bu modları PWM konusunda daha ayrıntılı anlatacağımız için kısa kesiyoruz.

Buraya kadar zamanlayıcıların yazmaçlarını, iç yapısını ve çalışma modlarını anlattık. Sonraki derslerde bu konuyu toparlayacağız ve buraya kadar bahsettiğimiz bilgiler üzerinden konuyu anlatmaya devam edeceğiz.

Kaynaklar

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

