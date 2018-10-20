# Karaşimşek ve Düğmeler ile Yürüyen Işık Uygulaması

AVR ile giriş ve çıkış işlemlerini yaparken pek örnek uygulama vermemiştik. Sadece kod sözdizimleri ve tek kod örnekleri üzerinden çalışma mantığını anlatmıştık. Şimdi biraz örnek uygulama yaparak konuyu anlamanıza biraz daha yardım edelim.

**Karaşimşek Devresi**

Sadece mikrodenetleyicileri değil elektroniği anlatan uygulamalı kitaplarda muhakkak yer edinen bir devre vardır. Bu devreye karaşimşek devresi adı verilir. Sağa ve sola sırayla yürüyen ışık, kimi zaman zamanlayıcı ve entegrelerle kimi zaman ise mikrodenetleyicilerle yapılır. Biz bunu AVR mikrodenetleyiciler için yeniden yazdık. Öncelikle devremizi kuralım ve kodu bütün ayrıntısıyla açıklayalım.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/ddd-1-1024x466.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/ddd-1.png)

Resme tıklayarak tam çözünürlüklü halini görebilirsiniz.

Şimdi programı yükleyelim ve çalışma prensibini anlatalım.

```c
// KARAŞİMŞEK 

#define F_CPU 16000000UL
#include <avr/io.h>
#include <util/delay.h>

int main(void)
{
	
	DDRD = 0xFF;
	int bekleme = 150;
    while (1) 
    {
    
   for(int i = 0; i < 7; i++)
    {
	 PORTD |=_BV(i);
	   _delay_ms(bekleme);
	 PORTD &=~_BV(i);
	}
	
	for(int i = 7; i > 0; i--)
	{
		PORTD |=_BV(i);
		_delay_ms(bekleme);
		PORTD &=~_BV(i);
	}
    }
}
```

**\#define F\_CPU 16000000UL** ile işlemcinin saat hızının 16MHz olduğunu belirliyoruz. Böylece delay.h başlık dosyasını çağırdığımızda işlemci hızına göre doğru bekleme sağlanacaktır.

**DDRD = 0xFF;** ile D portunun bütün ayaklarını dijital çıkış olarak tanımlıyoruz.

**int bekleme = 150;**  bekleme adında bir tamsayı değişkeni tanımlayıp buna “150” değerini atıyoruz. Birazdan bunu delay komutlarında kullanacağız. Böylelikle bekleme süresini bu değişkenin değerini değiştirmekle tüm programda değiştirebiliriz.

**for\(int i = 0; i &lt; 7; i++\)**  Toplam 8 kere çalışacak bir \(0 dahil\) bir döngü ayarlıyoruz. 8 kere çalışmasının sebebi portta 8 ayak olması ve buna 8 adet led bağlamamızdan dolayıdır.

**PORTD \|=\_BV\(i\);**  For döngüsündeki “i” değişkenini burada ayak numarası olarak kullanıyoruz. For döngüsü i değişkenini 0’dan itibaren 1, 2, 3.. diye artırmaya başladığında biz burada sırayla birinci, ikinci, üçüncü… ayakları seçip bunları yakacağız.

**\_delay\_ms\(bekleme\);** Yukarıda bekleme adında bir değişken tanımlayıp bu değişkene “150” değerini atamıştık. İşte bu değişkenin değerini burada kullanacağız. Komuttan önce bir ayağı HIGH yaparak ledi yakmıştık ve şimdi yanılı halde 150 milisaniye kadar bekleteceğiz.

**PORTD &=~\_BV\(i\);** Aynı ayağı LOW yapıp ledi söndürmek için bu komutu kullanıyoruz. For döngü yapısının içersindeki bu kod bloğu toplam 8 defa çalıştırılıp ayak numarası sırasında yakıp söndürür.

**for\(int i = 7; i &gt; 0; i–\)** Burada yukarıda yaptığımız işlemin tersini yapıyoruz. Yukarıda 0 numaralı ayaktan başlayıp 7 numaralı ayağa kadar yakıp söndürmeye devam etmiştik. Şimdi 7 numaralı ayaktan başlayıp 0 numaralı ayağa kadar yakıp söndürmeye devam edeceğiz ve program tekrar başa alarak çalışmaya devam edecek.

**Düğmeler İle Yürüyen Işık**

Yaptığımız karaşimşek devresine iki düğme ekleyelim ve farklı bir uygulama yapalım. Bu sefer bir düğmeye basınca sağa diğer düğmeye basınca sola yürüyen ışık uygulaması yapacağız. İki düğme de şaseye bağlanacak ve dahili pull-up dirençleri ile beraber kullanılacak. Öncelikle devre şemasını verelim ardından kodun ayrıntılı açıklamasını yapalım.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/ddd-2-1024x381.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/ddd-2.png)

_Resme tıklayarak tam çözünürlükte görebilirsiniz._ 

```c
// Düğmeler ile yürüyen ışık 

#define F_CPU 16000000UL
#include <avr/io.h>
#include <util/delay.h>

int main(void)
{
	
	DDRD = 0xFF;
	DDRB &=~ ((1<<0) | (1<<1));
	PORTB |= ((1<<0) | (1<<1));
	PORTD |= (1<<PD0);
	
	while (1)
	{
	if(bit_is_clear(PINB,0))
	{
	PORTD<<=1;
	_delay_ms(250);
	}
	
	
	if(bit_is_clear(PINB,1))
	{
		PORTD>>=1;
		_delay_ms(250);
	}
    }
}
```

Programda önceden anlatmadığımız bir ifade yer almakta. Gözümüzden kaçan ifadeyi buradaki kodları anlatırken anlatalım.

**DDRB \|= \(\(0&lt;&lt;0\) \| \(0&lt;&lt;1\)\);**  Bu kodda iki adet bit ifadesinin yan yana yazıldığı ve aralarında \| operatörünün olduğunu görüyoruz. Birden fazla bit ifadesi yazmak istediğimiz zaman parantez içerisinde olmak kaydıyla araya “\|” operatörünü koyarak yazabiliriz. Böylelikle daha pratik olur. Burada B portunun 0. ve 1. bitlerini 0 yani giriş olarak ayarlıyoruz.

**PORTB \|= \(\(1&lt;&lt;0\) \| \(1&lt;&lt;1\)\);** İfadesinde ise bu iki bitin pull-up dirençlerini bu giriş bitlerini HIGH yaparak aktif hale getiriyoruz. Bu komutla high çıkış alınacağı akla gelmesin çünkü ayakları çıkış olarak değil giriş olarak önceden tanımladık.

**PORTD \|= \(1&lt;&lt;PD0\);**  D portunun 0 numaralı bitini HIGH konumuna getirerek ona bağlı ledi yakıyoruz. Şimdi düğmeler ile bu biti sola ve sağa kaydırmamız gerekli. Sakın en sağdan ve en soldan taşırmayın ![&#x1F642;](https://s.w.org/images/core/emoji/11/svg/1f642.svg)

**if\(bit\_is\_clear\(PINB,0\)\)** B portunun 0 numaralı ayağına basılırsa bu karar yapısının içindeki kod bloğu çalışır.

**\_delay\_ms\(250\);**  Düğmeye basılıp işlem yapıldıktan sonra peş peşe aynı işlemi tekrar yapmaması için bir gecikme ekliyoruz. Döngüler ve diğer yapılarla da bu sağlanabilse de şu an için en basit yolunu kullandık.

Şu an için bu iki uygulama yeterli olur diye düşünüyorum. Ledler ile animasyon, trafik ışığı gibi pek çok uygulama bu komutlar ile yapılabilse de hemen hepsi aynı prensipte olacağı için her uygulamayı anlatmak gereksiz tekrara sebep olacaktır. 

