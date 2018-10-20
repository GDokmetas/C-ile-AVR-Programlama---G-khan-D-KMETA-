# SPI Örnek Kod İncelemesi

 SPI Protokolünü kullanmak için öncelikle açıp hazır hale getirmemiz gereklidir. Aynı ADC veya UART birimlerinde olduğu gibi bir “initialize” yani hazır hale gelme süreci gereklidir. Bunu ise program başında şu fonksiyon ile yapıyoruz. İleride Arduino kodlarını inceleyeceğimiz için SPI.begin\(\) fonksiyonunun da buna benzerlik gösterdiğini görebilirsiniz.

```c
void SPI_MasterInit(void)
{
/* MOSI ve SCK Çıkış olarak tanımla, diğerleri giriş olacak*/
DDR_SPI = (1<<DD_MOSI)|(1<<DD_SCK);
/* SPI'ı ana aygıt modunda başlat, saat oranını belirle*/
SPCR = (1<<SPE)|(1<<MSTR)|(1<<SPR0);
}
```

**void SPI\_MasterInit\(void\)**  Bu fonksiyon ne argüman alır ne de bir değer döndürür. O yüzden iki kısım da void olarak tanımlandı.

**DDR\_SPI = \(1&lt;&lt;DD\_MOSI\)\|\(1&lt;&lt;DD\_SCK\);**  Burada hiç görmediğimiz adlar var. Bu adlar C diline entegre edilen ve mikrodenetleyicinin SPI ayaklarının giriş ve çıkış \(DDR\) adreslerini bulunduran sabitlerdir. DDR\_SPI SPI portunun adresi \(örneğin PORTB\) DD\_MISO, DD\_MOSI, DD\_SCK ise bu ayakların adresidir \(PD1, PD2 gibi..\). Bu sadece kullanımı kolaylaştıran bir yapı olup programlayıcı hatalarının önüne geçer. Mikrodenetleyicinin kendisinde böyle bir yapı yoktur. Burada MOSI ve SCK ayakları yapısı itibariyle çıkış olarak tanımlanmıştır. Tanımlanmayan ayaklar ise giriştir.

**SPCR = \(1&lt;&lt;SPE\)\|\(1&lt;&lt;MSTR\)\|\(1&lt;&lt;SPR0\);**  SPCR yani SPI denetim yazmacında SPE biti bir yapılır. Bu bitin bir \( 1\) yapılması ile SPI biriminin etkinleştirildiğini önceki derste anlattık. MSTR bitinin bir \(1\) yapılmasıyla ile de SPI ana aygıt modunda çalışmaya başlar. SPR0 biti bir \(1\) yapılarak SPI saat hızının 16’da biri hızda çalışır.

SPI birimini başlatmak için gereken kodlar bu kadardı. Hangi ayakta hangi şekilde başlatmamız gerekiyorsa bunu yazmaçlardaki bitlerle oynayarak sizin yapmanız gereklidir. Bu kod standart bir modda SPI birimini başlatır. Bazen SPI aygıtlarla farklı modda iletişime geçmemiz gerekebilir. Bu kodu ezbere yazarak bunu yapmamız mümkün değildir. Önceki derste anlattığımız üzere CPOL ve CPHA bitlerinin konumuna göre SPI çalışma modları değişiyordu. İki bit olduğu için toplamda dört farklı çalışma modumuz vardı. Bu modları özetlersek,

* Mod 0 \(standart\), saat normalde LOW konumunda \(CPOL = 0\) yükselen kenarda veri örneklenir. \(CPHA = 0\)
* Mod 1, saat normalde LOW konumunda \(CPOL = 0\) düşen kenarda veri örneklenir.  \(CPHA = 1\)
* Mod 2, saat normalde HIGH konumunda \(CPOL = 1\), düşen kenarda veri örneklenir. \(CPHA = 0\)
* Mod 3, saat normalde HIGH konumunda \(CPOL = 1\), yükselen kenarda veri örneklenir. \(CPHA = 1 \)

SPI üzerinde çalışma yaparken bu modların olduğunu unutmamak gereklidir. Şimdi örnek bir gönderim fonksiyonunu inceleyelim.

```c
void SPI_MasterTransmit(char cData)
{
/* İletişimi başlat */
SPDR = cData;
/* İletişimin bitmesini bekle */
while(!(SPSR & (1<<SPIF)))
;
}
```

**void SPI\_MasterTransmit\(char cData\)**  Bu fonksiyon cData adında char tipinde bir argüman alır. 8 bitlik değer olduğu için 8 bitlik bir değişken olması lazımdır.

**SPDR = cData;**  SPI veri yazmacına veri yazılmasıyla iletişim başlar. Bundan önce eğer SS ayağı donanımsal değilse kendi elimizde LOW konumuna çekilmelidir.

**while\(!\(SPSR & \(1&lt;&lt;SPIF\)\)\) ;**  SPIF biti bir \(1\) olana kadar program sonsuz döngüye sokulur. Bu bitin bir \(1\) olmasıyla iletişimin bittiği anlaşılır.

Şimdi mikrodenetleyicimizi uydu aygıt olarak kullanmak için nasıl bir kod yazmamız gerekir ona bakalım. İki AVR mikrodenetleyiciyi birbirine bağlamanın en sağlıklı yollarından biri SPI protokolünü kullanmaktır.

```c
void SPI_SlaveInit(void)
{
/* MISO çıkış diğerleri giriş. */
DDR_SPI = (1<<DD_MISO);
/* SPI başlat*/
SPCR = (1<<SPE);
}
```

**DDR\_SPI = \(1&lt;&lt;DD\_MISO\);**  Burada sadece MISO ayağını çıkış olarak tanımladık. Bu ayak ana aygıta veri göndermek için kullanılır. Diğer tüm sinyaller ana aygıttan gelir ve burada uydu aygıtın bir denetimi yoktur.

**SPCR = \(1&lt;&lt;SPE\);**  Sadece SPI başlat bitini kullandık. MSTR bitinin bir \(1\) yapılmadığına dikkat ediniz. Bu bit ile ana aygıt olarak başlatırız. Aksi halde mikrodenetleyicimiz uydu aygıt olarak tanımlanır.

```c
char SPI_SlaveReceive(void)
{
/* Gelen verinin bitmesini bekle */
while(!(SPSR & (1<<SPIF)))
;
/* veri yazmacını döndür  */
return SPDR;
}
```

**while\(!\(SPSR & \(1&lt;&lt;SPIF\)\)\)**  Burada SPIF biti bir \(1\) olana kadar program sonsuz döngüye sokulur ve SPDR yani veri yazmacı değer olarak döndürülür.

Temel seviyede baktığımızda oldukça basit olarak görünmektedir. Bu fonksiyonları yürütmek için SPI kesmesini kullanmamız mikrodenetleyiciyi gereksiz yere meşgul etmekten kurtarır. Bu birimlerde ayrı kesmelerin olması çok faydalı bir özelliktir. Kesmelerin nasıl kullanılacağına dair bilgiyi önceki derslerimizde bulabilirsiniz. Burada SPI derslerini bitirsek de Arduino kodlarını incelediğimiz sıra Arduino’nun SPI kütüphanesini sizlere açıklayacağız. Bir sonraki konuda görüşmek üzere.

_**Kaynaklar**_

AVR151: Setup and Use of the SPI, Microchip Technology,  http://ww1.microchip.com/downloads/en/AppNotes/Atmel-2585-Setup-and-Use-of-the-SPI\_ApplicationNote\_AVR151.pdf, Erişim Tarihi : 27.09.2018

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

