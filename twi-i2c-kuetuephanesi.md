# TWI \(I2C\) Kütüphanesi

AVR derleyicisinin içerisinde bir I2C kütüphanesi bulunmasa da bir programcının yazmış olduğu ve tüm AVR aygıtlara uyumlu bir kütüphane mevcuttur. AVR I2C kütüphanesini aşağıdaki bağlantıdan indirebilirsiniz.

[http://homepage.hispeed.ch/peterfleury/i2cmaster.zip](http://homepage.hispeed.ch/peterfleury/i2cmaster.zip)

Bu kütüphaneyi programımıza dahil etmek için şu komutu kullanmamız gereklidir.

```c
#include <i2cmaster.h>
```

Bu kütüphane ana aygıt \(Master\) kütüphanesidir ve aygıta bağlı diğer uydu aygıtlar ile iletişim kurmamızı sağlar. Çoğu uygulamamızda mikrodenetleyiciyi ana aygıt olarak kullanacağımız için bu kütüphane neredeyse çoğu işimizi görecektir.

_**I2C Kullanırken SDA ve SCL ayaklarını 4.7K direnç ile beslemeye bağlamayı unutmayın.**_

**Makrolar**

Bu kütüphanede iki makro bulunup  i2c\_start\(\) ve i2c\_rep\_start\(\) fonksiyonları ile beraber kullanılır. Hangi özellikte kullanacaksak bu makroları argümanlara “+” operatörü ile eklememiz lazımdır. Bu veri yönünü belirlemek için kolaylaştırılmış bir özellik olarak karşımıza çıkar. Makrolar ise şu şekildedir.

```c
#define I2C_READ 1
#define I2C_WRITE 0
```

Şimdi kütüphane fonksiyonlarını teker teker açıklayalım.

**void i2c\_init \(void\)**

Bu fonksiyon I2C ana aygıt arayüzünü başlatmaya yarar. Sadece bir kere başta çalıştırılması yeterlidir. Fonksiyonun örnek söz dizimi şu şekildedir.

```c
i2c_init();
```

**unsigned char i2c\_start\(unsigned char addr \)**

Bu fonksiyon belirtilen adrese bağlanmak için START durumunu başlatır. İletişimi başlatmak için öncelikle uydu aygıtın adresini bilmeli ve sonrasında ise bu adres üzerinden START durumunu başlatmamız gereklidir. Böylelikle sonrasında veri göndermemiz mümkün olur. Fonksiyon 0 ve 1 değerlerini döndürür. Eğer aygıta erişim sağlandı ise 0, aygıta erişim sağlanamadıysa 1 değerini döndürür. Böylelikle bağlantı hataları tespit edilmiş olur. Fonksiyonun örnek söz dizimi şu şekildedir.

```c
i2c_start(0x42+I2C_READ);  // OKUMA İÇİN
i2c_start(0x42+I2C_WRITE); // YAZMA İÇİN
```

**unsigned char i2c\_rep\_start\( unsigned char addr \)**

Bu fonksiyon yukarıdaki fonksiyon ile aynı görevi görse de farkı tekrarlayan START durumu oluşturmasıdır. Böylelikle tek bir veri değil çoklu bayt verisi gönderme imkanımız olur. Yine bu fonksiyon bir adres değerini argüman olarak alır ve başarılı olunduğunda 0, başarısız olunduğunda ise 1 değerini döndürür. Fonksiyonun örnek söz dizimi şu şekildedir.

```c
i2c_rep_start(0x24+I2C_READ); // OKUMA İÇİN
i2c_rep_start(0x24+I2C_WRITE); // YAZMA İÇİN
```

**void i2c\_start\_wait \( unsigned char addr \)**

Bu fonksiyon START durumunu başlatır ve adres ve transfer yönünü gönderir. Eğer aygıt meşgul ise aygıt hazır olana kadar ACK durumunu sürekli kontrol eder.

```c
i2c_start_wait(0x42);
```

**unsigned char i2c\_write \( unsigned char data \)**

Bu fonksiyon I2c aygıtına bir bayt gönderir ve gönderim başarılı olduğunda 0 değerini döndürür. Eğer başarısız ise 1 değerini döndürür.  Örnek söz dizimi şu şekildedir.

```c
i2c_write(0xF0);
```

**unsigned char i2c\_readAck \( void \)**

I2C aygıtından bir bayt okur ve devamı için aygıta istek yollar. Örnek söz dizimi şu şekildedir.

```c
okunan_veri = i2c_readAck();
```

**unsigned char i2c\_readNak \( void \)**

Bu fonksiyon bağlı aygıttan bir bayt okur ve ardından STOP durumunu başlatır. Yani bir bayt okuyarak iletişimi bitirir. Örnek söz dizimi şu şekildedir.

```c
okunan_veri = i2c_readNak();
```

**unsigned char i2c\_read \( unsigned char ack \)**

Bu fonksiyon yukarıda yürütülen fonksiyonların ikisini yürütebilir. ack argümanına yazdığımız 1,  i2c\_readAck fonksiyonunu çağırırken 0 ise  i2c\_readNak fonksiyonunu çağırır. 1 yazıldığında aygıttan verinin devamını talep eder, 0 yazıldığında ise tek bir okuma yapıp veri aktarımını durdurur. Fonksiyonun örnek söz dizimi şu şekildedir.

```c
okunan_veri = i2c_read(1);
okunan_veri = i2c_read(0);
```

I2C kütüphanesinin bütün fonksiyonları bu kadardır. Bu kütüphane çoğu işimizi görür niteliktedir. Daha ileri işler için ise yazmaçlar üzerinde çalışıp kendi fonksiyonlarımızı yazmamız gereklidir. İleri seviye çalışmalar için AVR LibC’nin twi.h başlık dosyasında bit maskesi tanımları mevcuttur. I2C üzerinde çalışanların ihtiyacı olan bu başlık dosyasını şuradan inceleyebilirsiniz.  
[https://www.nongnu.org/avr-libc/user-manual/group\_\_util\_\_twi.html](https://www.nongnu.org/avr-libc/user-manual/group__util__twi.html)

I2C konusunu burada bitirmiş olduk ve yeni konumuza geçeceğiz. Şimdiye kadar yazılan derslerden faydalanarak pek çok proje yapabilirsiniz fakat biz eksik bir konu bırakmamak istiyoruz. Seri olarak yazdığımız derslerden sonra zamanla daha ayrıntı konuları işleyeceğimiz makaleleri de yazacağız. Bir sonraki derste görüşmek üzere.

_**Kaynaklar**_

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

