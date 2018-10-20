# Port Üzerinden Giriş/Çıkış ve Delay Kütüphanesi

AVR programlamada portlar ve bunlar üzerinde yapılan işlemlerden devam ediyoruz. Bu konuyu bu kadar uzatıp birkaç derse yaymamın sebebi ise AVR’ye geçiş için en büyük adım olmasından dolayıdır. Bu konu anlaşıldığı zaman diğer konular çok kolay anlaşılacaktır. Gömülü sistemlerde C dilini kullanmayı, yazmaçları ve mikrodenetleyicinin mantığını anlamak için  gerektiği kadar uygulama yapıp başlangıçta ağır ağır ilerlememiz lazımdır.  Bir örnek kod verip iki satır açıklama yaparak bu konuları anlatmak mümkün değildir.

Önceki yazımızda port yazmaçlarına değer atamış ve bu değere göre bir ledi yakmıştık. Şimdi DDRx, PORTx ve PINx yazmaçlarını bir arada kullanacağımız bir uygulama yapacağız. Böylelikle üç yazmacın tüm özelliklerini uygulamada görmüş olacağız. Sonraki derste ise bitwise operatörleri ve makrolar ile yazmaçlar üzerinde bit tabanlı işlem yapacağız. Makrolar işimizi kolaylaştırsa da işin mantığını anlamamıza engel olmaktadır. O yüzden zordan başlayıp derslerin sonunda da makroları kullanacağız. Şimdilik şöyle bir devre kuralım ve bunun üzerinden bu üç yazmacı anlatalım.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/Untitled-Sketch_%C5%9Fema.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/Untitled-Sketch_%C5%9Fema.png)

Arduino UNO’da Dijital 2. ayak Atmega328P’de PD2’ye 10. ayak ise PB2’ye denk gelmektedir. Kısa sürede hatırlamak için kağıda kabaca bir Arduino resmi çizip ayakların yanına Atmega328P’nin ayak adlarını yazabilirsiniz. Oldukça faydası oluyor. Şimdilik devreyi Arduino üzerinden verelim fakat ileride entegre üzerinden vereceğiz ve Arduino kartında hangi ayağa denk geldiğini bulmak sizin sorumluluğunuzda olacak. Bu devrede pull-up ve pull-down yöntemini kullandık. Bunu açıklamadan önce örnek pull-up ve pull-down devrelerini resimde verelim.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/37_1290066934.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/37_1290066934.png)

Resim : http://images.elektroda.net/37\_1290066934.png

Dijital devrelerde ayağın açıkta olması tri-state adı verilen kararsız bir durum ortaya çıkarır. Ne bir \(1\) ne de sıfır \(0\) olan ve bir şey bağlanmadığı sürece okunduğunda bir anlam ifade etmeyen kararsız bir durumdur. Buna mantık kararsızlığı ya da lojik kararsızlık da denebilir. Bunu önlemek için ayağı boşta bırakmak yerine bir direnç ile ya beslemeye ya da şaseye bağlayıp bir \(1\) veya sıfır \(0\) yapmamız gerekir. Böylelikle o ayağın sabit değerini belirlemiş oluruz ve farklı bir değeri bu sabit değere göre saptayabiliriz. Örneğin pull-up direnci ile beslemeye bağladığımız bir giriş ayağı şaseye de düğme ile bağlanmış olsun. Bu ayaktan düğmeye basılı olmadığı zaman alacağımız değer sabittir ve her zaman bir \(1\) olarak alınır. Düğmeye bastığımızda ayak şase ile kısa devre olur ve sıfır konumuna düşer. Bu durumda ayaktan sıfır \(0\) değerini okuruz ve muhakkak düğmeye basıldığını anlarız. Yoksa elektriksel gürültüye göre bir veya sıfır değerlerini rastgele almamız mümkündür. Onun için dijital giriş devrelerinde pull-up ve pull-down devrelerini ihmal etmemek gerekir. Bu dirençlerin değeri 10K olabilir.

Devreye dönersek pull-down olarak bağlandığı için o ayaktan sürekli sıfır verisi okunması gerekir. Düğme beslemeye bağlandığı için düğmeye basıldığı anda bir \(1\) değeri okunacaktır.

PB2 ayağına ise bir led 220ohm direnç ile bağlanmıştır. Artık düğmeye bastığımız anda ledi yakacak bir programa ihtiyacımız vardır. O program ise aşağıdaki gibidir.

```c
#include <avr/io.h>

int main(void)
{
	DDRD = 0x00; 
	DDRB = 0xFF;
    while(1)
    {
    PORTB = PIND;
    }
}
```

**DDRD = 0x00;**  Bu komutla D portunun tüm ayakları giriş olarak tanımlanır. Burada bit veya ayak dememizin bir önemi yoktur ikisi de aynı anlama gelmektedir.

**DDRB = 0xFF;** Bu komutla B portunun tüm ayakları çıkış olarak tanımlanır. Giriş veya çıkış olarak tanımlamak bizim atadığımız değere göredir. Bir kısmını giriş veya bir kısmını çıkış yapmamız da yine atadığımız değere göre olmaktadır. Örneğin 0b11110000 değerini atasaydık ilk 4 bacak giriş son 4 bacak çıkış olarak tanımlanacaktı. Bunun onaltılık karşılığı ise 0xF0’dır.

**PORTB = PIND;**  İşte bütün işi yapan ve en önemli kodumuz bu koddur. Burada önceki programdan farklı olarak PORT yazmacına bir sabit değer atamak ve buna göre ayaklardan çıkış almak yerine PIN yazmacının değerini doğrudan PORT yazmacına attık. Bu kodun Türkçesi şudur, PORTD’ye bağlı ayaklarda okunan değeri PORTB yazmacına aktar. PIN yazmacı giriş olarak tanımlanan port ayaklarının ayak durumunun verisini içerisinde barındırır. Bu veri bir değişkene atanıp kullanılabilir. Örneğin 8-bitlik bir paralel iletişim hattı kurduk ve bir portun 8 ayağı da karşı cihaza bağlı. Bu karşı cihazdan aldığımız 8-bitlik veriyi degisken = PINx; komutu ile bir değişkene atayabiliriz.  8-bit mikrodenetleyici olduğu için DDR, PORT, PIN yazmaçlarının hepsi 0 ile 255 değer aralığında olmaktadır.

Özetlemek gerekirse, bu devre düğmeye basınca ledi yakmaktadır düğme basılı değilken ise led sönmektedir. Ledin bu durumu düğmeye bağlı ayağın durumuna eşittir.

### Bekleme işlemi ve Delay kütüphanesi

Mikrodenetleyicilerde delay yani bekleme komutu sıkça kullanılır. Mikrodenetleyici bilgisayarlara göre çok daha düşük hızda çalışsalar da yazılı komutları peş peşe çalıştırdıkları için günlük hayata göre oldukça hızlı çalışmaktadırlar. Çoğu zaman komutlar arasına biraz bekleme süresi koymamız gerekebilir. Komutların ne kadar hızlı çalıştığı değil zamanında çalışması daha önemlidir. Bekleme özelliği aslında donanımsal bir özellik olmayıp yazılım ile mikrodenetleyiciyi boş yere meşgul etmekten ibarettir. Biz delay kütüphanesini kullansak da internette örnek delay komutları bulmanız mümkündür. Ana mantık mikrodenetleyicinin hızlı işlem gücünü boş bir yerde harcatıp mikrodenetleyiciyi bir süre meşgul etmektir. Zamana bağlı uygulamalarda delay komutu asla ilk sırada olmamalı ve zamanlayıcılar büyük ölçüde kullanılmalıdır. Aşırı derecede kullanılan delay komutu mikrodenetleyiciyi olduğundan fazla yavaşlatır ve gerekli komutların zamanında çalışmasına engel olur. Şimdilik delay kullanmamızda bir sakınca olmadığı için yanıp sönen led uygulamasıyla delay komutlarını anlatalım.

```c
#include <avr/io.h>
#define F_CPU 16000000
#include <util/delay.h>

int main(void)
{
	DDRB = 0xFF;
    while(1)
    {
    PORTB = 0xFF;
	_delay_ms(1000);
	PORTB = 0x00;
	_delay_ms(1000);
    }
}
```

Bu program B portuna bağlı tüm ayakları yakıp söndürecektir. B portuna bağlı dahili bir ledimiz bulunduğu için herhangi bir devre kurmamıza gerek yoktur. Arduino kartının üzerindeki ledin birer saniye aralıklarla yanıp söndüğünü göreceksiniz. Şimdi kodu bütün ayrıntısıyla inceleyelim ve sonrasında delay kütüphanesinin tamamını anlatalım.

**\#define F\_CPU 16000000** F\_CPU değerinin 16000000 olduğunu belirledik. Bu delay kütüphanesinin kullanacağı bir değer olup mikrodenetleyicinin saat hızını öğrenmek için kullanılır. Mikrodenetleyici kaç MHz’de çalışıyorsa onun Hertz değeri yazılmalıdır. Bizdeki 16MHz olduğu için delay.h kütüphanesini dahil etmeden önce o değeri belirledik.

**\#include &lt;util/delay.h&gt;**  Delay kütüphanesini çağırıyoruz. Burada delay kütüphanesine ait olan fonkisiyon ve değerler programda kullanılabilir oluyor.

**DDRB = 0xFF;**  B Portunun bütün ayaklarını çıkış olarak tanımlıyoruz.

**PORTB = 0xFF;** B portunun bütün ayaklarını bir \(1\) konumuna getiriyoruz böylelikle lambayı yakıyoruz.

**\_delay\_ms\(1000\);** 1000 milisaniye bekliyoruz. \_delay\_ms\(\) fonksiyonunun içindeki paranteze yazdığımız sabit değer veya değişken kadar bekler.

**PORTB = 0x00;**  B portunun bütün ayaklarını sıfır \(0\) konumuna getiriyoruz böylelikle lambamız sönüyor.

#### \_delay\_us\(\)

Bu fonksiyon ise mikrosaniye kadar bekletmeyi sağlar. Parantezin içerisine double değer aralığında değer yazılabilir. Diğer fonksiyon için de bu geçerlidir. F\_CPU ise unsigned long olabilir.

**Tekrar özetlersek,**   
**DDRx, Bir portun giriş mi çıkış mı olacağını belirler**  
**PORTx, Bir portun ayaklarının bir \(1\) mi sıfır \(0\) mı olacağını belirler.**  
**PINx, Bir porttaki girişlerin değerini okur.**

Buraya kadar portlar üzerinde kabaca işlem yaptık ve bitlerine doğrudan müdahale etmedik. Bundan sonraki yazıda bitwise operatörler ile bunlara müdahale edeceğiz. Sonrasında ise makroları kullanmayı anlatacağız. Böylelikle dijital giriş ve çıkış konusunda anlatılmadık bir şey kalmayacak.

Kaynaklar:  
&lt;util/delay.h&gt;: Convenience functions for busy-wait delay loops, https://www.nongnu.org/avr-libc/user-manual/group\_\_util\_\_delay.html, Erişim Tarihi : 17.08.2018

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 17.08.2018

Kapak Resmi:  
https://d3s11pzv7w3h1q.cloudfront.net/wp-content/uploads/IC-ATTINY13A-PU-2.jpg

