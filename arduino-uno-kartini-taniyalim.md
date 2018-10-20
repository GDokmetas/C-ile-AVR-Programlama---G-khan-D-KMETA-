# Arduino UNO Kartını Tanıyalım

![](http://www.lojikprob.com/wp-content/uploads/2018/08/a000066_featured_4-1.jpg)

Resim : https://store-cdn.arduino.cc/usa/catalog/product/cache/1/image/520×330/604a3538c15e081937dbfbd20aa60aad/a/0/a000066\_featured\_4.jpg

Arduino UNO kartı Arduino’yu Arduino yapan karttır diyebiliriz.  Arduino’nun en sık kullanılan kartı olup Arduino dediğimizde akla ilk gelen karttır. Önce RS-232 portlu ve DIP soketli versiyonları çıkmış zamanla USB özellikli ve SMD yapıya dönüşmüştür. Tasarımda zaman içerisinde gelen bu değişim sadece belli kısımlarda olmuş bazı çizgiler hep aynı kalmıştır. O yüzden yıllar önce yazılmış bir kod bile devrede ya da kodda bir değişiklik olmadan kullanılabilir. Arduino için üretilmiş olan üstlüklerin hemen hepsi Arduino Uno kartına göre üretilmektedir. Arduino’nun en yaygın ve pratik kartı olan UNO’yu biz sadece donanımı için tercih edeceğiz. Aşağıdaki tabloda UNO kartının teknik özellikleri verilmiştir.

| Mikrodenetleyici | Atmel Atmega328P |
| :--- | :--- |
| Çalışma Gerilimi | 5V |
| Giriş Gerilimi | 7-12V \(6-20V  MAX\) |
| Dijital Giriş ve Çıkış Ayakları | 14 \( 6 adet PWM \) |
| Analog Giriş Ayakları | 6 |
| Giriş Çıkış Ayağı Akımı | 20mA |
| Program Hafızası | 32KB \(0.5 KB Ön yükleyici\) |
| SRAM | 2KB |
| EEPROM | 1KB |
| Saat Hızı | 16MHz |

AVR geliştiricisi olarak Arduino UNO kartındaki numaralandırılmış dijital giriş ve çıkış ve analog girişlerden değil mikroişlemcinin ayak numaraları bizi ilgilendirdiği için aşağıdaki diyagrama bol bol başvurmamız gerekecek.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/UNOV3PDF.png)

Resim: http://forum.arduino.cc/index.php?action=dlattach;topic=146315.0;attach=90365

Şimdi diyagramdan anladıklarımızı aşağıda özetleyelim.

**Güç Kısmı**

Power diye adlandırılan kısımda 5V, IOREF \(5V ile aynı\), 3.3V regülatöre bağlı 3.3V, Şase ve VIN yani gerilim girişi bulunur. VIN kısmı 5V regülatöre bağlıdır. Eğer USB’den besleme sağlanıyorsa buradan 5V elde edilebilir. Reset kısmı mikrodenetleyicinin reset ayağına aynı zamanda da reset düğmesine bağlıdır. Harici bir resetleme için buradan bağlantı yapılabilir.

**Analog Kısmı**

Analog kısmı C portuna bağlıdır. Burada A4 ve A5 TWI \(I2C\) iletişim için kullanılır.

 **Dijital Kısmı**

Burada aşağıdan başlayarak UART, Kesme, Zamanlayıcı, SPI, Analog Referans ve LED ayakları bulunur.

Analog ve Dijital kısımdaki ayakların tamamı dijital giriş ve çıkış olarak kullanılabilir. Biz programlarken bunu B portu, C portu, D portu olarak programlayacağız. Dijital ayak numaralarıyla bir işimiz olmayacak.  Karta şöyle bir baktığımızda mikrodenetleyicinin ayaklarını kullanıma sunan header soketleriyle mikrodenetleyici ayakları arasında bağlantı kuran ve gerekli güç ve program devresini hazır olarak üzerinde bulunduran bir kart olarak karşımıza çıkıyor.

Arduino UNO’nun şemasını şu bağlantıdan inceleyebilirsiniz,  
[https://www.arduino.cc/en/uploads/Main/Arduino\_Uno\_Rev3-schematic.pdf](https://www.arduino.cc/en/uploads/Main/Arduino_Uno_Rev3-schematic.pdf)

Kaynaklar:

DÖKMETAŞ,Gökhan,”Arduino Eğitim Kitabı”, İstanbul,2016  
Arduino – ArduinoUno, arduino.cc, https://www.arduino.cc/en/Guide/ArduinoUno, Erişim Tarihi : 15.08.2018  
Kapak Resmi : https://store-cdn.arduino.cc/usa/catalog/product/cache/1/image/520×330/604a3538c15e081937dbfbd20aa60aad/a/0/a000

