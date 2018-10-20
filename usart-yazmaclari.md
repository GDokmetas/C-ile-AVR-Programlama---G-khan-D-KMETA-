# USART Yazmaçları

Bu dersimizde Atmega328P mikrodenetleyicisinin USART yazmaçlarını anlatmaya kaldığımız yerden devam ediyoruz. Yazmaçları anlattıktan sonra örnek kodlar üzerinden anlatmaya devam edeceğiz. Dersleri yazmaya başladığımda belli bir sıra gözetmesem de artık belli bir düzene oturduğunu görüyoruz. Diğer konuları da aynı düzende anlatıp hepsini bitirdikten sonra ise uygulamalarla devam edeceğiz. Şimdi kaldığımız yerden devam edelim.

**UCSR0B – USART Denetim ve Durum Yazmacı 0 B**

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/usar1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-21-usart-yazmaclari/attachment/usar1/)

**Bit 7 – RXCIE0 : Alım Tamamlandı Kesmesi Etkinleştirme 0**

Bu bite bir \(1\) yazmak RXC0 bayrak bitindeki kesmeyi faal hale getirir. USART Alım Tamamlandı kesmesi ancak RXCIE0 bitinin bir \(1\)  yapılmasıyla gerçekleşir. Ayrıca kesme için SREG’deki  Genel Kesme Bayrağı \(Global Interrupt Flag\) ve UCSR0A’daki RXC0 biti de bir \(1\) yapılmalıdır.

**Bit 6 – TXCIE0 : Gönderim Tamamlandı Kesmesi Etkinleştirme 0**

Bu bite bir \(1\) yazmak TXC0 bayrak bitindeki kesmeyi etkin hale getirir. USART gönderim tamamlanma kesmesi ancak TXCIE0 bitinin bir \(1\) yapılmasıyla, SREG yazmacındaki genel kesme bayrak bitinin bir \(1\) yapılmasıyla ve UCSR0A yazmacındaki TXC0 bitinin bir \(1\) yapılmasıyla yürür.

**Bit 5 – UDRIE0 : USART Veri Yazmacı Boş Kesmesini Etkinleştirme 0**

Bu biti bir \(1\) yapmak UDRE0 bayrak bitindeki kesmeyi etkin hale getirir. Veri yazmacı boş kesmesi ancak UDRIE0 bitinin bir\(1\) yapılmasıyla, SREG’deki genel kesme bayrak bitinin bir \(1\) yapılmasıyla ve UCSR0A yazmacındaki UDRE0 bitinin bir \(1\) yapılmasıyla yürür.

**Bit 4 – RXEN0 : Alıcı Etkinleştirme 0**

Bu biti bir \(1\) yapmak USART alıcısını etkinleştirir.

**Bit 3 – TXEN0 : Verici Etkinleştirme 0** 

Bu biti bir \(1\) yapmak USART vericisini etkinleştirir.

**Bit 2 – UCSZ02 : Karakter boyutu 0**

UCSZ02 biti UCSZ0 \[1:0\] bitleriyle beraber çalışır. Bu bitler alıcı ve verici görevleri yürütülürken belirlenen veri boyutu çerçevesini belirlemede kullanılır.

**Bit 1 – RXB80 : Alıcı 8. Veri Biti**

Eğer veri çerçevesi boyutu  9 bit ise burada 9. bit saklanır. UDR0’ın düşük bitlerini okumadan önce okunmalıdır.

**Bit 0 – TXB80 : Verici 8. Veri Biti**

Eğer veri çerçevesi boyutu 9 bit olarak belirlenirse gönderilecek 9. bit buraya yerleştirilir. UDR0’ın düşük bitlerini yazmadan önce yazılmalıdır.

**UCSR0C – USART Denetim ve Durum Yazmacı 0 C** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/usart2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-21-usart-yazmaclari/attachment/usart2/)

**Bit 7:6 UMSEL0n : USART Modu Seçimi 0 \[n = 1:0\]**

Bu bitler USART’ın çalışma modunu seçer. Modlar aşağıdaki tablodaki gibidir.

| UMSEL0 \[1:0\] | Mod |
| :--- | :--- |
| 00 | Asenkron USART |
| 01 | Senkron USART |
| 10 | Rezerve |
| 11 | Ana SPI \(MSPIM\) |

MSPIM etkinleştirildiğinde UDORD0, UCPHA0 ve UCPOL0 bitleri aynı yazma işleminde bir \(1\) konumuna alınabilir.

**Bit 5:4 – UPM0n : USART Eşlik Modu 0 \[n = 1:0\]**

Bu bitler eşlik üretimi tipini ayarlar ve bunu denetler. Eğer etkinleştirilirse verici otomatik olarak eşlik bitini oluşturur ve veri çerçevesi içerisinde bunu gönderir. Alıcı eşlik değerini oluşturur ve gelen veri ile UPM0 ayar kısmında karşılaştırır. Eğer eşliksizlik tespit edilirse UCSR0A yazmacındaki UPE0  bayrağı bir \(1\) konumuna getirilir.

| UPM0\[1:0\] | Eşlik Modu |
| :--- | :--- |
| 00 | Kapalı |
| 01 | Rezerve |
| 10 | Etkin, Çift Sayı |
| 11 | Etkin, Tek Sayı |

**Bit 3 – USBS0 : USART Stop biti seçimi 0**  
Bu bit verici tarafından yerleştirilen stop bitlerinin sayısını belirler. Alıcı bu ayarı görmezden gelir.

| USBS0 | Stop Biti |
| :--- | :--- |
| 0 | 1-bit |
| 1 | 2-bit |

**Bit 2 – UCSZ01 / UDORD0 : USART karakter boyutu / veri sırası**

**UCSZ0 \[1:0\] USART modunda:** UCSZ0\[1:0\] bitleri UCSR0B yazmacındaki UCSZ02 biti ile beraber kullanılır. Bu bitler veri çerçevesindkei veri bitlerinin sayısını belirler. Aşağıdaki tabloda işlevi açıklanmıştır.

| UCSZ0\[2:0\] | Karakter Boyutu |
| :--- | :--- |
| 000 | 5-bit |
| 001 | 6-bit |
| 010 | 7-bit |
| 011 | 8-bit |
| 100 | Rezerve |
| 101 | Rezerve |
| 110 | Rezerve |
| 111 | 9-bit |

**UDPRD0 : Ana SPI modu :** Eğer bir \(1\) yapılırsa son bitten itibaren veri gönderilir. Eğer sıfır \(0\) yapılırsa baş bitten itibaren veri gönderilir. SPI modundaki USART’a sonra geleceğimiz için bu bilgi şimdilik gerekli değildir.

**Bit 1 – UCSZ00 / UPCHA0 : USART karakter boyutu / Saat evresi**

UCSZ00 : Usart modunda UCSZ01’i referans eder.

UCPHA0: Ana SPI modunda eğer veri XCK0’ın ilk veya son kenarında örnekleniyorsa bunun ayarını yapar.

**Bit 0 – UCPOL0 – Saat Frekansı Kutbu 0** 

USART0 Modunda: Bu bit sadece senkron modda kullanılır. Asenkron mod kullanılacaksa bu bir sıfır yapılmalıdır. UCPOL0 biti veri çıkışındaki değişiklik ve veri girişi örneklemesindeki ilişki ve senkron saatini ayarlamaya yarar \(XCK0\) USART saat frekans kutbu ayarı aşağıdaki gibidir.

| UCPOL0 | Gönderilen Veri Değişti \(TxD0 ayağındaki çıkış\) | Alınan veri örneklendi \(RxD0 ayağındaki giriş\) |
| :--- | :--- | :--- |
| 0 | Yükselen XCK0 Kenarı | Alçalan XCK0 Kenarı |
| 1 | Alçalan XCK0 Kenarı | Yükselen XCK0 Kenarı |

Ana SPI Modu: UCPOL0 biti XCK0 saatinin kutbunu ayarlar. UCPOL0 ve UCPHA0 bitlerinin beraber kullanımı veri transferindeki zamanlamayı ayarlar.

**UBRR0L – USART Baud Oranı 0 Yazmacı – Düşük**

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/usart3.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-21-usart-yazmaclari/attachment/usart3/)

**Bit 7:0 – UBRR0 \[7:0\] USART Baud Oranı \(Veri iletim hızı\)**

Bu 12 bitlik yazmaç USART veri iletim hızı bilgisini içinde bulundurur. UBRR0H baş dört biti bulundururken UBRR0L son sekiz biti bulundurur. Baud hızı iletişim sırasında değişirse Alıcı ve verici düzgün işleyemez. UBRR0L yazmacına veri yazmak baud oranında ani bir değişme yapar.

UBRR0H – USART Baut Oranı 0 Yazmacı – Yüksek

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/uart4.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-21-usart-yazmaclari/attachment/uart4/)

Bu yazmaçta UBRR0L yazmacının geri kalan baş bitleri \(most significant bits\) saklanır.

USART birimi için yazmaçlar bu kadardır. Bunları anlatmak oldukça yorucu olsa da eksiksiz anlatmaya çalıştık. Bir sonraki yazımızda örnek kodları inceleyerek çalışma prensibini ayrıntılı olarak anlatacağız. Biz yazmaçların tamamının işlevini ezberlemenizi istemiyoruz. Yazmaçlar ve özellikleri hakkında bilginiz olsun yeterli. Bir sonraki yazıda görüşmek üzere.

Kaynaklar:

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018

