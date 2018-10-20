# Temel Giriş ve Çıkış İşlemlerine Giriş

C ile AVR programlama derslerine bu derslere başlamadan çok önce başlamış ve sadece giriş ve çıkış işlemlerini anlatıp bırakmıştım. Önceki yazdığımı silip yedeği kaybettiğimden dolayı tekrar yazmak durumundayım. Bu sefer daha ayrıntılı ve açıklayıcı yazmaya gayret edeceğim.

Arduino’da temel giriş ve çıkış için üç adet fonksiyon kullanıyorduk. Bu üç fonksiyonun kullanımı oldukça kolay ve anlaşılırdı. AVR’de ise yeni başlayanların kafasını karıştıracak şekiller ve kısaltmalar karşımıza çıkıyor. Bu kısaltmaları yani yazmaç adlarını önceki yazıda uzunca anlatıp iyi bir hazırlık yaptık. Şimdi ise bu şekilleri yani operatörleri ve bunlar üzerinden nasıl işlem yapıldığını anlatacağız.

Arduino’da tek bir ayaktan çıkış almak için digitalWrite\(\) tek bir ayağı okumak için digitalRead\(\) ve ayağın görevini tanımlamak için pinMode\(\) fonksiyonları kullanılır. Üç görev için üç fonksiyon kullanılmaktadır. Bu durum AVR’de de çok farklı değildir. Yine üç görev için üç yazmaç kullanacağız ve fonksiyonlar aracılığı ile yazmaçlara değer atamak yerine doğrudan yazmaçlara müdahale edeceğiz. Böylelikle programımız çok daha hızlı olacak ve az yer kaplayacak.

AVR mikrodenetleyicilerde portların ayakları Input, Input Pull-up, Sink ve Source konumlarında olabilir. Input konumunda harici olarak pull-up veya pull-down yani direnç ile beslemeye veya şaseye bağlanmış halde ya da dahili pull-up direncine bağlı halde olur. Output konumunda ise Sink ve Source olarak iki durumda olur. Sink; 0, LOW, FALSE konumu olup 0V olup 20mA civarında akım çeker. O yüzden sink yani akmak olarak adlandırılır. Source ise 5v 20mA civarında akım verir ve mantıksal olarak HIGH, TRUE ya da 1 olarak adlandırılır. Source’un kelime anlamı ise kaynaktır.  Bundan başka Tri-state diye adlandırılan bir durum vardır. Bu üçüncü durum olup ne mantıksal HIGH ne de mantıksal LOW demektir. Hükmü olmayan bir durumu tri-state olarak adlandırırız.  
![](http://www.lojikprob.com/wp-content/uploads/2018/08/avrportpin.png)

Resim: ATmega328P – Microchip Technology, sf.99 , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 17.08.2018

Yukarı teknik veri kitapçığından alınan resimde AVR mikrodenetleyicilerin pin konfigürasyonu verilmiştir. Burada MCUCR yazmacındaki PUD bitinin giriş olarak tanımlanan port ve bitlerindeki görevini görmemiz mümkün. Tabloyu madde madde şöyle özetleyebiliriz,

* DDRx yazmacındaki porta ait ayak sıfır yapılır ve port yazmacındaki aynı bit de sıfır yapılırsa giriş ve çıkış fonksiyonu giriş olur ve tri-state olarak kalır. \(Ayak boşta iken bu durum geçerlidir.\)
* DDRx yazmacındaki porta ait ayak sıfır yapılır ve port yazmacındaki aynı bit de bir yapılırsa ve PUD biti sıfır konumunda yani pull-up dirençleri devre dışı bırakılmamışsa o ayak dahili pull-up özelliği gösterir.
* DDRx yazmacındaki porta ait ayak sıfır yapılır ve port yazmacındaki aynı ayağa karşılık gelen bit de bir yapılır ama MCUCR yazmacındaki PUD biti 1 yapılırsa o ayak tri-state konumunda olur. Yani pull-up olmadan giriş konumunda olur.
* DDRx yazmacındaki porta ait ayak bir yapılır ve port yazmacındaki aynı ayağa karşılık gelen bit de sıfır yapılırsa o ayaktan dijital 0 \(sıfır\) çıkışı sağlanır.
* DDRx yazmacındaki porta ait ayak bir yapılır ve port yazmacındaki aynı ayağa karşılık gelen bit de bir yapılırsa o ayaktan dijital 1 \(bir\) çıkışı sağlanır.

Bu tablo aslında bizim önceki yazıda anlattığımız yazmaçlardaki bitlerin konumuna göre ayaklardan nasıl bir giriş ve çıkışı alacağımızı anlatıyor. Eğer önceki başlığı okuyup geldiyseniz burayı anlamak hiç zor olmayacaktır. Şimdilik MCUCR yazmacındaki PUD bitini bir kenara bırakalım ve sadece anlattığımız üç yazmaçtaki bitlerin konumuna göre ayakları nasıl kontrol edeceğimizi anlatalım fakat bundan öncesinde Atmega328p mikrodenetleyicisinin ayaklarını öğrenmekle başlayalım.  
[![](http://www.lojikprob.com/wp-content/uploads/2018/08/ATmega328P-PU-PIN-Diagram-connection-configration-1-1024x648.jpg)](http://www.lojikprob.com/wp-content/uploads/2018/08/ATmega328P-PU-PIN-Diagram-connection-configration-1.jpg)

Resim : http://www.ifuturetech.org/ifuture/uploads/2013/07/ATmega328P-PU-PIN-Diagram-connection-configration.jpg  
\(_Resme tıklayarak tam çözünürlükte açabilirsiniz._ \)

Yukarıdaki şemadan anlaşılacağı gibi mikrodenetleyicinin ayakları portlar ve diğer birimler ile ortak kullanılmaktadır. Bir ayağı hem port, hem iletişim, hem analog hem de kesme olarak kullanmak mümkündür. Fakat biz burada diğer birimleri şimdilik anlatmayacağız ve portları anlatmakla işe başlayacağız. Atmega328P mikrodenetleyicisinin güç ayakları hariç geri kalan tüm ayaklarını portlar oluşturur. Bu portlar PORTB, PORTC ve PORTD olarak üç adettir. PORTD tam 8-bite karşılık gelen 8 ayaktan oluşsa da diğer portlar 8 ayaktan oluşmamaktadır. Mikrodenetleyicide ayaklar kısıtlı olduğu için bu 8-bit çıkış alabileceğimiz tek bir port bulunur. Üstelik UART, I2c, Analog ve SPI kısımları bu portlarla paylaşıldığı için gelişmiş uygulamalarda portlar için kullanılacak ayaklar yetersiz gelebilir. Şimdilik her özelliği kullanmayacağımız için elimizde oldukça fazla ayak var demektir.  Fakat Arduino UNO kartında UART kullanıldığı için RX ve TX ayaklarına denk gelen PD0 ve PD1 ayaklarını kullanmamamız iyi olur. Burada ayak adlandırmaları Pxn şeklinde olup örneğin PD1 , PORTD’in 1 numaralı ayağı anlamına gelmektedir. Yukarıdaki resmi kullanışlı bulmayanlar için aşağıda pratik bir referans resmini verelim.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/ATMega328P-Pinout.png)

Resim: https://components101.com/sites/default/files/component\_pin/ATMega328P-Pinout.png

**Led Yakma**

Şimdi basit bir led yakma programı yazalım ve bu program üzerinden yazmaçların nasıl programlandığını anlatalım. Arduino UNO kartı kullandığımız için şimdilik devre kurmamıza gerek yok. Programı karta atalım ve çalıştıralım. Programı yüklediğimizde L isimli PB5’e bağlı led yanacaktır.

```c
#include <avr/io.h>
 
int main(void)
{
	DDRB = 0xFF; // 0b11111111
	PORTB = 0xFF; //0b11111111
    while(1)
    {
 
    }
}
```

**\#include &lt;avr/io.h&gt;** direktifi ile temel giriş çıkış başlık dosyasını programımıza ekliyoruz. C dilinde stdio.h başlık dosyası eklendiği gibi giriş çıkış ve port işlemleri için io.h başlık dosyası programın başında eklenmelidir.

**int main\(void\)**  fonksiyonu ana program fonksiyonudur. Ana programı main içerisine yazmak zorundayız. Bu fonksiyon içine yazdığımız komutlar yukarıdan aşağıya doğru bir defalığına işletilir.

**DDRB = 0xFF;** ifadesi DDRB yazmacına 0xFF yani ikilik tabanda “11111111” değerini aktarır. Mümkün olduğu kadar değerleri onaltılık tabanda yazmamız bizim için daha pratik olacaktır. Anlaşılır olması için şimdilik onaltılık tabandaki sayıların ikilik tabandaki karşılıklarını veriyoruz. DDRB yazmacına “11111111” değeri atılınca D portunun bütün ayakları çıkış olarak ayarlanmış olur. Böylelikle bu ayaklardan HIGH \(1\) ya da LOW \(0\) çıkışı alınabilir.

**PORTB = 0xFF;** Bu ifade ile PORTB yazmacına 0xFF yani ikilik tabanda “11111111” değerini aktarıyoruz. Böylelikle tüm ayaklardan HIGH \(1\) çıkış alınmış oluyor. Böyle çıkış alındığı zaman bu ayaklara bağlı olan led ya da buzzer gibi elemanlara elektrik gitmiş oluyor ve bunları çalıştırıyor. Eğer sıfır değerini atasaydık bağlı olduğu led sönecekti. C dilinde onaltılık değerler 0x öneki ile ikilik değerler ise 0b öneki ile ifade edilir. Onluk tabandaki sayıları düz yazmak yeterlidir fakat bu tabanlardaki değerleri ifade etmek için değerin önüne bu önekleri \(prefix\) koymak gereklidir.

**while\(1\)** kısmı programın sonsuz döngüsüdür. Arduino’dan farklı olarak main fonksiyonu sonsuz döngü şeklinde değil bir defaya mahsus çalışır. O yüzden sonsuz program döngüsüne ihtiyacımız olduğu durumda bu ifadeyi kullanırız. Bu ifadenin arasına yazılan kodlar çalışma boyunca sürekli işletilir.

Görüldüğü gibi önce DDRx yazmacına bir değer atıyoruz ve sonra atadığımız değere göre portların ayakları çıkış olarak ayarlanıyor. Çıkış olarak ayarlandıktan sonra PORTx yazmacına atadığımız değerlere göre ayaklar bir \(1\) veya sıfır \(0\) oluyor.

Sonraki konularda port yazmaçları üzerinde yapılan işlemleri en ince ayrıntısına kadar anlatacağız. Anlaşılması için konunun uzun tutulması gerektiğinden bu dersi burada bitirelim.

**Kaynaklar**  
ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 17.08.2018

Kapak Resmi:  
https://d3s11pzv7w3h1q.cloudfront.net/wp-content/uploads/IC-ATTINY13A-PU-2.jpg

