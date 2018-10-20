# USART Kullanımı ve Örnek Kod İncelemesi

AVR’nin USART yazmaçlarını önceki yazımızda bitirmiştik. Artık bu yazmaçlar üzerinden nasıl program yazılacağı ve USART biriminin nasıl kullanılacağını anlatalım. Yazmaçların ADC birimine göre biraz daha ayrıntılı olması ve farklı iletişim tiplerinin ortak kullandığı bitler olması biraz kafamızı karıştırabilir. USART prensipte çok basit olduğu için sadece bayt alma ve bayt gönderme olarak kısa fonksiyonlar yazarak bunu gerçekleştirmemiz mümkündür. Aynı ADC biriminde olduğu gibi öncelikle USART birimini hazır hale getirmek için bir fonksiyon yazmak zorundayız. Bu fonksiyonu yazarak işe başlayalım.

USART birimi yüklenmesi için şu adımlar gerçekleştirilmelidir,

1. Baud Oranı Ayarlanmalıdır
2. Veri Boyutu Ayarlanmalıdır
3. TXEN ve RXEN bitleri ile Alıcı ve Verici Etkinleştirilmelidir.
4. Eşlik biti ve Stop bitlerinin sayısı ayarlanmalıdır.

Şimdi bunu program kodu üzerinden görelim.

| 1234567891011 | \#define BAUD 9600                                   // Baud Oranını Tanımlıyoruz\#define BAUDRATE \(\(F\_CPU\)/\(BAUD\*16UL\)-1\)            // UBRR için baud verisini ayarlıyoruz   void uart\_init \(void\){    UBRRH = \(BAUDRATE&gt;&gt;8\);                      // Baudrate'in üst bitlerini kaydırıyoruz    UBRRL = BAUDRATE;                           // kalan bitleri yazdırıyoruz    UCSRB\|= \(1&lt;&lt;TXEN\)\|\(1&lt;&lt;RXEN\);                // Alıcı ve vericiyi etkinleştiriyoruz    UCSRC\|= \(1&lt;&lt;URSEL\)\|\(1&lt;&lt;UCSZ0\)\|\(1&lt;&lt;UCSZ1\);   // Veri formatını ayarlıyoruz} |
| :--- | :--- |


Bu örnek kodda her komutun ne iş gördüğünü açıklamada yazsak da komutlara bakarak anlamak için bunu yazmaçlar üzerinden anlatmak lazımdır. Öncelikle yukarıda tanımladığımız makroların işlevinden bahsedelim. Yukarıda baud 9600 olarak belirlenmiştir. Bunu anlamak kolay olsa da BAUDRATE olarak tanımlanan makrodaki matematik işlemi bize yeni görünecektir. Mikrodenetleyicinin teknik veri sayfasında UBRR yazmacına yazılacak baud oranının hesabına dair bir formül mevcuttur. Bu kod bu formülün kod haline dönüşmüş şeklidir. Merak edenler için aşağıda formülü verelim.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/formul.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-21-usart-kullanimi-ve-ornek-kod-incelemesi/attachment/formul/)

Kısaca açıklarsak F\_CPU değerinde bulunan işlemcinin saat hızı ile BAUD değerinin 16’ya çarpımı bölünür ve bundan 1 çıkarılır. Delay fonksiyonlarında olduğu gibi F\_CPU tanımını yapmamışsak yapmamız gerekir. Örneğin \#define F\_CPU 16000000UL komutu işlemci saat hızının 16MHz olduğunu belirtir.

**UBRRH = \(BAUDRATE&gt;&gt;8\);**

Bu kod elde edilen BAUDRATE değerinin bitlerini 8 bit sağa kaydırarak ilk bitleri 8 bitlik ölçüde açığa çıkarır. Örneğin elimizde 12 bitlik bir “0000111111111111” değeri olsun. Bunu sekiz bitlik bir yazmaca yüklediğimizde sağdaki sekiz bit yüklenecek fakat soldaki sekiz bit yüklenemeyecektir. Bunu Yüksek ve Düşük yazmaçlarına sığdırmak için öncelikle sağdaki sekiz birlik düşük yazmaç değerini atmamız gerekir. Bu işlemden sonra değerimiz “00000000000001111” olacaktır. Böylelikle bu dört bit yüksek yazmacın ilk dört bitine yazılacaktır. Geri kalan değer ise bir sonraki komutta düşük yazmaca yazılacaktır. Elimizdeki mikrodenetleyici 8-bit olduğu için böyle büyük değerleri sığdırmak zorunda kalıyoruz.

**UBRRL = BAUDRATE;**  Burada düşük yazmaca BAUDRATE’in tamamı yazdırılmış gibi görünse de aslında sağdaki sekiz biti yazdırılmıştır. Geri kalan bitler de yazdırıldığına göre baud oranı değerini yazdırma ve bu oranı ayarlama işlemi bitmiş oluyor.

**UCSR0B\|= \(1&lt;&lt;TXEN\)\|\(1&lt;&lt;RXEN\);**  UCSRB yazmacındaki TXEN ve RXEN bitleri bir \(1\) yapılır. Bu aynı ADC’de ADEN bitinin bir \(1\) yapılması gibi alıcı ve vericiyi etkinleştirir.

**UCSR0C\|= \(1&lt;&lt;UCSZ0\)\|\(1&lt;&lt;UCSZ1\);**  Veri formatının nasıl seçildiğini anlamamız için tekrar yazmaçlara bakmamız lazım. Burada UCSZ0 ve UCSZ1 bitleri bir \(1\) yapılmıştır. Tablodan anlaşıldığı üzere iletişim 8 bitlik bir veri genişliğinde yapılacaktır. Bundan başka iletişim UMSEL bitlerinin 00 olmasıyla asenkron olduğu için değiştirme gereği duymadık.

Şimdi veri gönderme ve veri alma fonksiyonlarını inceleyelim.

```c
// veri gönderme fonksiyonu
void uart_transmit (unsigned char data)
{
    while (!( UCSRA & (1<<UDRE)));                // yazmacın boş olmasını bekle
    UDR = data;                                   // yazmaca veri yükle
}
```

 Burada UDRE veri yazmacı bayrağı bir \(1\) olmadığı sürece program döngüde tutulur ve sonrasında UDR yazmacına fonksiyonun argüman olarak aldığı data değeri yazılır. İşlem bu kadardır.

```c
// function to receive data
unsigned char uart_recieve (void)
{
    while(!(UCSRA) & (1<<RXC));                   // tüm verinin gelmesi için bekle
    return UDR;                                   // 8 bit veriyi döndür
}
```

Öncelikle UCSRA yazmacındaki RXC bayrak bitinin bir \(1\) olup olmadığı kontrol edilir. Veri alma tamamlanınca bu bit bir \(1\) olacağı için döngüden çıkılır. Sonrasında UDR yazmacının içindeki değer fonksiyondan döndürülür.

Temel olarak USART işlemi bu kadardır. Sonraki yazıda diğer kodları inceleyeceğiz ve konumuza devam edeceğiz.

Kaynaklar:

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018

_Yash Tambi, Mayank Prasad,_ The USART of the AVR, http://maxembedded.com/2013/09/the-usart-of-the-avr/, Erişim Tarihi 21.08.2018

