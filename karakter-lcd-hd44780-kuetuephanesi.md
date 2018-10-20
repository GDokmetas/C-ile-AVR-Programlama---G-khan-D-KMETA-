# Karakter LCD \(HD44780\) Kütüphanesi

Arduino’dan AVR’ye geçenlerin en büyük sıkıntısı artık kütüphane olmayışı ve çoğu işi sıfırdan kendimiz yapmamız gerektiği algısıdır. Temel giriş ve çıkış bir şekilde halledilse de geri kalan işimizi kolaylaştıran kütüphaneler elimizin altında yoktur. Fakat ufak bir araştırma yapıldığı zaman aslında AVR için oldukça fazla kütüphane yazıldığının farkına varabilirsiniz. Arduino AVR kütüphanelerinin yazılıp paylaşılmasına bir bakıma engel olmaktadır. Çünkü AVR programcısı olacak kişiler Arduino’yu tercih etmektedir. O yüzden AVR bilenler AVR kütüphanesi yazmak yerine Arduino kütüphanesi yazmayı daha yararlı bulmaktadır. Kütüphane bulmada ilk engelimiz binlerce Arduino kütüphanesi olup AVR kütüphanelerinin arka planda kalmasıdır. Yine de Github’dan arattığımız zaman yeterli miktarda kütüphane bulabiliyoruz. Mevcut olmayan kütüphaneleri ise Arduino kütüphanelerini değiştirerek yazmak mümkündür. Şimdi karakter LCD kullanımı için yazılmış bir kütüphanenin bağlantısını ve referansını açıklayacağız.

> Önümüzde yazılacak onlarca ders olduğu için uygulama yapmaya vakit bulamıyoruz. Uygulama eksiği olanlar internette yer alan onlarca Arduino uygulamasına bakabilir. Bu dersler Arduino ile yeterli uygulamayı yapmış, nasıl devre kuracağını anlamış kişiler için yazılmıştır. O yüzden bu konuda eksiği olanlar şimdilik Arduino uygulamalarına bakabilir.

Karakter LCD kütüphanesi aşağıdaki bağlantıdan indirilebilir.  
[http://homepage.hispeed.ch/peterfleury/lcdlibrary.zip](http://homepage.hispeed.ch/peterfleury/lcdlibrary.zip)

#### Tanımlar

**\#define LCD\_CONTROLLER\_KS0073 0**   
Bu tanım LCD üzerinde bulunan kontrolcü entegreyi seçmeye yarar. HD44780 kullanılıyorsa sıfır \(0\), KS0073 kullanılıyorsa bir \(1\) yapılmalıdır. Günümüzde genelde HD44780 entegreli LCD ekranlar kullanılmaktadır. Buna dikkat edilmezse ekran çalışmayabilir.

Bu tanımlamalar lcd\_definations.h dosyasında bulunmaktadır. Program yazmadan önce muhakkak kullanacağınız ekranın değerlerini bu tanımlardan değiştirmeniz gereklidir. Eğer değiştirmezseniz programınız çalışmayabilir.

#### LCD EKRAN AYARLARI

**\#define LCD\_LINES 2**  
Bu tanım LCD ekranın kaç satır olacağını belirler. Genellikle 2 veya 4 satır olmaktadır. Buradan kaç satır olacağını belirleyebilirsiniz.

**\#define LCD\_DISP\_LENGTH 16**

Bu tanım LCD ekranının kaç sütün olacağını belirler. Eğer elimizde 16×2 LCD varsa burası 16 olmalıdır. Eğer 20×4 gibi bir ekran varsa burayı 20 yapmalıyız.

**\#define LCD\_LINE\_LENGTH 0x40**

Bu tanım LCD’nin satırındaki veri uzunluğunu belirlemeye yarar.

**\#define LCD\_START\_LINE1 0x00**

LCD’nin birinci satırındaki DRAM adresini belirlemeye yarar.

**\#define LCD\_START\_LINE2 0x40**

LCD’nin ikinci satırındaki DRAM adresini belirlemeye yarar.

**\#define LCD\_START\_LINE3 0x14**

LCD’nin üçüncü satırındaki DRAM adresini belirlemeye yarar.

**\#define LCD\_START\_LINE4 0x54**

LCD’nin dördüncü satırındaki DRAM adresini belirlemeye yarar.

**\#define LCD\_WRAP\_LINES 0**

LCD’de satırları döndürmeye yarar. Görünen satırın sonunu döndürür.

#### LCD AYAK AYARLARI

**\#define LCD\_IO\_MODE 1**  
0: Hafıza haritalantırma modu, 1: IO Port Modu

**\#define LCD\_PORT PORTA**

LCD ayakları için port seçimi

**\#define LCD\_DATA0\_PORT LCD\_PORT**

4-bit verinin 0. biti için port seçimi

**\#define LCD\_DATA1\_PORT LCD\_PORT**

4-bit verinin 1. biti için port seçimi

**\#define LCD\_DATA2\_PORT LCD\_PORT**

4-bit verinin 2. biti için port seçimi

**\#define LCD\_DATA3\_PORT LCD\_PORT**

4-bit verinin 3. biti için port seçimi

**\#define LCD\_DATA0\_PIN 0**

4-bit verinin 0. biti için ayak seçimi

**\#define LCD\_DATA1\_PIN 1**

4-bit verinin 1 . biti için ayak seçimi

**\#define LCD\_DATA2\_PIN 2**

4-bit verinin 2. biti için ayak seçimi

**\#define LCD\_DATA3\_PIN 3**

4-bit verinin 3. biti için ayak seçimi

**\#define LCD\_RS\_PORT LCD\_PORT**

RS ayağı için port seçimi. Buraya başka port adı da girilebilir.

**\#define LCD\_RS\_PIN 4**

RS ayağı için ayak seçimi.

 **\#define LCD\_RW\_PORT LCD\_PORT**

RW ayağı için port seçimi.

**\#define LCD\_RW\_PIN 5**

RW ayağı için pin seçimi

**\#define LCD\_E\_PORT LCD\_PORT**

Enable ayağı için port seçimi

**\#define LCD\_E\_PIN 6**

Enable ayağı için ayak seçimi

**\#define LCD\_DELAY\_BOOTUP 16000**

Açılışta kaç mikrosaniye bekleneceği belirlenir.

**\#define LCD\_DELAY\_INIT 5000**

Yükleme komutu gönderildikten sonra kaç mikrosaniye bekleneceği belirlenir.

**\#define LCD\_DELAY\_INIT\_REP 64**

Yükleme komutu tekrarlandıktan sonra kaç mikrosaniye bekleneceği belirlenir.

**\#define LCD\_DELAY\_INIT\_4BIT 64**

4-bit modunu seçtikten sonraki beklemeyi belirler. \(mikrosaniye\)

**\#define LCD\_DELAY\_BUSY\_FLAG 4** 

Meşgul bayrağı temizlenip adres satırı güncellendikten sonraki mikrosaniye beklemenin ne kadar olacağını kararlaştırır.

**\#define LCD\_DELAY\_ENABLE\_PULSE 1**

Enable ayağının sinyal genişliğini mikrosaniye olarak ayarlar.

#### LCD KOMUT AÇIKLAMALARI

Bu komutlar lcd\_command\(\) fonksiyonu ile kullanılabilir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/lcdkomut.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-24-karakter-lcd-hd44780-kutuphanesi/attachment/lcdkomut/)

#### LCD Fonksiyonları

#### **void lcd\_init\(uint8\_t dispAttr\)**

Ekranı tanımlar ve imleci belirler.

Parametreler  
dispAttr:  
– **LCD\_DISP\_OFF**  : Ekran Kapalı  
– **LCD\_DISP\_ON**    : Ekran Açık, İmleç Kapalı  
– **LCD\_DISP\_ON\_CURSOR** : Ekran açık, imleç açık  
– **LCD\_DISP\_ON\_CURSOR\_BLINK** : Ekran açık, Yanan imleç

Örneğin ekranımız açık olsun ve imlecimiz sabit olsun bunun için şöyle bir kod yazacağız,  
**lcd\_init\(LCD\_DISP\_ON\_CURSOR\);**

#### **void lcd\_clrscr \(void\)**

Ekranı temizler ve imleci başa getirir.  Örnek kullanım:

**lcd\_clrscr\(\);**

#### void lcd\_home \( void \)

İmleci başa alır. Örnek kullanım:

**lcd\_home\(\);**

#### void lcd\_gotoxy \( uint8\_t x, uint8\_t y \)

İmleci belirlenen bir konuma getirir.

**Parametreler** 

**x**: Yatay konum \(0 en sol\)  
**y**: Dikey konum \(0 en üst\)

Örnek bir kullanım şöyle olabilir,  
**lcd\_gotoxy\(5,1\);**

#### void lcd\_putc \( char c \)

Mevcut imleç konumunda bir karakter gösterir

**Parametreler**  
**c** : gösterilecek karakter

Örnek bir kullanım şöyle olabilir,  
**lcd\_putc \(‘a’\);**

#### void lcd\_puts \( const char\* s\)

Otomatik satır beslemesi ile harf dizisini gösterir.  
**Parametreler**  
**s** : Gösterilecek harf dizisi verisi  
Örnek bir kullanım şöyle olabilir,  
**lcd\_puts\(“Merhaba”\);**

#### void lcd\_puts\_p \( const char\* progmem\_s\)

Program hafızasındaki harf dizisi verisini gösterir.  
**Parametreler**

**progmem\_s** Program hafızasındaki gösterilecek harf dizisi.

#### void lcd\_command \( uint8\_t cmd\)

LCD’ye işlemci komutu gönderilir.

**Parametreler**  
**cmd** : LCD gönderilecek işlemci komutu. HD44780 teknik veri kitapçığında bulunabilir.

#### void lcd\_data \( uint8\_t data \)

LCD’ye bayt verisi gönderir.  
Parametreler  
**data** : LCD sürücüye gönderilecek bayt verisi. Hd44780 teknik veri kitapçığına bakın.

**LCD’ye Integer Yazdırmak** 

Yukarıdan anlaşılacağı üzere LCD’ye sadece karakter ve string yazdırıldığını fark etmişsinizdir. Şimdi sprintf\(\) fonksiyonu ile integer değerini harf dizisine dönüştürüyoruz ve LCD’ye yazdırıyoruz. Aynı zamanda LCD ile örnek bir programı da görebilirsiniz.

```c
#define F_CPU 16000000UL
#include <avr/io.h>
#include <stdio.h>
#include "lcd.h"

int main (void)
{
	lcd_init(LCD_DISP_ON);
	lcd_clrscr();
	lcd_home();
	char str [16];
	int pi = 30;
	sprintf(str, "Pi = %i", pi);
	lcd_puts(str);
	while(1)
	{
```

 LCD’nin çalışma prensibini Gömülü Sistemler kategorisinde, LCD ile örnek uygulamaları ise AVR uygulamalarına başladığımız zaman yazacağız. Şimdilik ihtiyacı olanlar için kütüphaneyi açıklayarak derslerimize devam edelim. Şimdiye kadar anlattığımız bilgilerle bu kütüphaneyi kullanmak mümkün olduğu için bu kadarıyla yetinip bir sonraki konuya geçelim.

