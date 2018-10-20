# ADC Kütüphanesi İncelemesi ve Kütüphane Kullanmaya Giriş

Elimizde ADC ile ilgili birkaç AVR kütüphanesi vardı. Bu kütüphanelerin arasından birini bütün ayrıntılarıyla inceleyerek hem ADC’nin çalışma prensibini tekrar gözden geçireceğiz hem de size yararlı bir kütüphane vermiş olacağız.  Bu kütüphanenin yazarını ve bağlantısını şimdi bulma imkanım olmadığı için bir referans veremeyeceğim. adc.h ve adc.c olarak iki dosyanın tüm kodlarını elimizden geldiği kadarıyla size açıklayacağız ve çalışma mantığını bir daha anlatacağız. Burada bir daha anlatmak istemem önceki kodların çok da yeterli gelmeyişinden dolayıdır. Şimdi çalışmamıza başlayalım.

**adc.h dosyası**

```c
#define MCU_FREQ 16000000UL
uint16_t convert;
uint16_t loop_count;

void     initADC( void );
uint16_t  ADC10bit( uint8_t channel );
uint16_t  ADC08bit( uint8_t channel );
void     DELAY_US( uint16_t microseconds);
```

Bu dosyada kullanılacak fonksiyonların ve değişkenlerin tanımları yapılmıştır. .h uzantılı dosya bizim kütüphaneyi kullanmak için müracaat edeceğimiz ilk dosya olmalıdır. .c dosyası kullanıcıyı pek alakadar etmeyen ama kütüphanenin asıl bölümünü oluşturan dosyadır. Öncelikle .h dosyası üzerinden tanımlanan fonksiyon ve değerleri anlatarak .c dosyasındaki fonksiyonların anlaşılmasına yardımcı olalım.

**\#define MCU\_FREQ 16000000UL ,** Bu kütüphanede MCU\_FREQ adındaki değerin mikrodenetleyicinin saat hızı olduğunu anlıyoruz. Demek ki bu kütüphane 16MHz’de çalışan bir mikrodenetleyici ile kararlı çalışabiliyor. Biz kullanıcı olarak bu değeri değiştirerek kullanmamız gerekir. AVR kütüphanelerinde sık sık göreceğiniz gibi kütüphaneyi olduğu gibi kullanmak pek mümkün değildir. Bunun bir sebebi ise kütüphanelerin C dilinde yazılmış olmalarından kaynaklanır. Arduino kütüphaneleri C++ dilinde yazılıp sınıf-nesne ilişkisinden dolayı gerekli parametreleri sınıf içerisinde nesne oluştururken yazabiliyorduk. C dilinde yazılan bir kütüphanede ise bu parametreler başlık dosyasında yer alıp doğrudan başlık dosyasından değiştirilmeye ihtiyaç duyar.

**uint16\_t convert;**  Burada convert \(dönüştürme\) adında 16 bitlik işaretsiz tam sayı değişken tanımlanmış. 16 bit olmasından dolayı bunun çevirim sonucunu barındıracak değişken olduğunu çok rahat tahmin edebiliriz. ADC’nin nasıl çalıştığını anlamamız başkalarının yazdığı kodu ilk okumada anlamamızı sağladı. Bunun doğru olup olmadığını ise .c uzantılı dosyada göreceğiz. Şimdi devam edelim.

**uint16\_t loop\_count;**   Burada yine 16 bitlik yani “word” cinsinde işaretsiz bir tamsayı değişken tanımlanıp adı loop\_count \(döngü sayısı\) olarak belirlenmiş. Tahminimize göre bu kütüphane kendi içerisinde delay fonksiyonu kullanıyor ve bu delayda kaç kere döngünün çevrildiğine dair bilgiyi bu değişken içerisinde barındırıyor. .c dosyasına henüz bakmadığımız için yine kabaca bir tahminde bulunuyoruz.

**void initADC\( void \);**  Bu fonksiyon önceki yazıda kullandığımız fonksiyona benzer bir işleve sahip olmalıdır. ADC’yi açıp hazırlamak için kullanılacak fonksiyon olduğunu “init” yani “initialize” \(başlatmak\) ifadesinden anlıyoruz.

**uint16\_t ADC10bit\( uint8\_t channel \);  
uint16\_t ADC08bit\( uint8\_t channel \);**  Bu iki fonksiyonun aldığı argüman ve döndürdüğü değerden ADC okuma fonksiyonu olup adlarından birinin 10-bit okuma diğerinin ise 8-bit okuma yapacağını anlıyoruz.

**void DELAY\_US\( uint16\_t microseconds\);**  Bu fonksiyon ise adından ve aldığı argümandan anlaşıldığı üzere bekleme işlemini yapıyor.

İşte kullanıcının okuyup anlaması gereken ve buna göre kullanması gereken .h başlık dosyasından .c dosyasını okumadan anladığımız bu kadardı. Bazı kütüphanelerde .h başlık dosyasında fonksiyonların işlevi ve kütüphane kullanımı hakkında bilgi yer almaktadır. Bu bilgiler kütüphaneyi anlamamıza yardımcı olsa da her zaman bu tarz kütüphanelerle karşılaşmak pek mümkün değildir. Kütüphane referansı ya da kullanma talimatı yoksa iş başa düşmektedir.  Şimdi .c dosyasını inceleyelim.

**adc.c dosyası**

```c
#include <stdint.h>
#include <avr/io.h>
#include <avr/interrupt.h>
#include "adc.h"
void initADC( void )
{
  ADCSRA = ( 1 << ADEN  ) | ( 1 << ADSC  )
        | ( 1 << ADPS2 ) | ( 1 << ADPS1 );
  while ( ADCSRA & ( 1 << ADSC ) );
}

uint16_t ADC08bit( uint8_t channel )
{
    /* set for 8-bit results for the desired channel number then start the
       conversion; pause for the hardware to catch up */
    
    ADMUX  = ( 1 << ADLAR ) | ( 1 << REFS0 ) | channel;
    ADCSRA = ( 1 << ADEN  ) | ( 1 << ADSC  );
    DELAY_US( 64 );

    /* wait for complete conversion and return the result */

    while ( ADCSRA & ( 1 << ADSC ) );
    
    return ADCH;
}

uint16_t ADC10bit( uint8_t channel )
{
    ADMUX  = ( 1 << ADLAR ) | ( 1 << REFS0 ) | channel;
    ADCSRA = ( 1 << ADEN  ) | ( 1 << ADSC  );
    DELAY_US( 64 );
    while ( ADCSRA & ( 1 << ADSC ) );
	convert = ADCH;
	convert <<= 2;
	convert += (ADCL >> 6);
	return convert;
}

void DELAY_US( uint16_t microseconds )
{

#if MCU_FREQ == 8000000UL

    /* 8mhz clock, 4 instructions per loop_count  */
    loop_count = microseconds * 2;

#elif MCU_FREQ == 1000000UL

    /* 1mhz clock, 4 instructions per loop_count */
    loop_count = microseconds / 4;

#elif MCU_FREQ == 16000000UL

    /* 1mhz clock, 4 instructions per loop_count */
    loop_count = microseconds / 4;

#else
#error MCU_FREQ undefined or set to an unknown value!
    loop_count = 0; /* don't really know what to do */
#endif

    __asm__ volatile (
        "1: sbiw %0,1" "\n\t"
        "brne 1b"
        : "=w" ( loop_count )
        : "0"  ( loop_count )
    );
}
```

 Bu dosya biraz uzun olduğu için parça parça inceleyeceğiz. Yukarıda dosyanın tamamına bir göz gezdirmeniz yeterli. Şimdi ilk fonksiyonla başlayalım.

```c
void initADC( void )
{
  ADCSRA = ( 1 << ADEN  ) | ( 1 << ADSC  )
        | ( 1 << ADPS2 ) | ( 1 << ADPS1 );
  while ( ADCSRA & ( 1 << ADSC ) );
}
```

 Bu fonksiyon ADCSRA yazmacının bazı bitlerine yazma işlemi yaparak işe başlar. Eğer teknik veri kitapçığından yazmaçları öğrenmeseydik burada nasıl bir işlem yapıldığı hakkında bir fikrimiz olmayacaktı. Fonksiyonun ilk komutunda ADCSRA’nın ADEN, ADSC, ADPS2 ve ADPS1 bitleri bir \(1\) yapılır. Bu komutun sonucu özetle şöyle olacaktır.  ADC Faal hale getirilecektir sonrasında ADC’nin hazır olması için ilk dönüşüm başlatılacaktır ve ADC’nin ön derecelendiricisi 64 olarak seçilecektir. 16MHz’de çalışan bir işlemci için bu 250kHz demektir. İkinci komutta ise ADCSRA yazmacındaki ADSC yani ADC çevriminin bittiğini belirten bayrak biti 1 olana kadar program döngüde kalacaktır. Burada yapılan en önemli işlem ADEN bitinin bir \(1\) yapılması ve ADC’nin çalışır hale gelmesi ve ADC ön derecelendiricisinin ayarları yapılıp ADC’nin düzgün çalışmasının sağlanmasıdır.

```c
uint16_t ADC08bit( uint8_t channel )
{
    /* set for 8-bit results for the desired channel number then start the
       conversion; pause for the hardware to catch up */
    
    ADMUX  = ( 1 << ADLAR ) | ( 1 << REFS0 ) | channel;
    ADCSRA = ( 1 << ADEN  ) | ( 1 << ADSC  );
    DELAY_US( 64 );

    /* wait for complete conversion and return the result */

    while ( ADCSRA & ( 1 << ADSC ) );
    
    return ADCH;
}
```

Bu fonksiyon argüman olarak işaretsiz 8 bit tamsayı türünde \(uint8\_t\) channel adında bir değer alır. Bu değer tabi ki analog okumanın yapılacağı kanalın numarası olacaktır. 8-bit değer döndürse de fonksiyon uint16\_t yani işaretsiz 16-bit tamsayı değer döndürür. Bunun sebebi değeri atayacağımız değişkenin genellikle 16-bit olmasından dolayıdır. Şimdi ilk komutu inceleyelim.

**ADMUX = \( 1 &lt;&lt; ADLAR \) \| \( 1 &lt;&lt; REFS0 \) \| channel;**  Soldan itibaren açıklamaya başlarsak öncelikle ADMUX yazmacı üzerine bir değer atama söz konusu olduğunu söyleyebiliriz. ADLAR biti bir \(1\) yapılarak değerin sola hizalı olması sağlanır. Bunun sebebi değerin ilk 8 bitlik parçası okunup geriye kalan 2 bitlik teferruat kısmı okunmayacaktır. Böylelikle değeri kabaca okumuş oluyoruz.  ADMUX yazmacının REFS0 biti 1 yapılır böylelikle analog gerilim referansı besleme gerilimi \(5V\) olarak seçilmiş olur. Burada hemen şu yorumu yapmamız mümkündür, bu kütüphanenin eksik yanlarından biri .h dosyasında referans gerilimi seçmek için bir seçenek verilmemiştir ve kullanıcı referans gerilimini değiştirmek için kodlarla oynamak zorundadır. En son olarak da channel değişkenine atadığımız değer ADMUX yazmacının son 3 bitine etki edecektir. Eğer bu fonksiyonun argüman kısmına aşırı bir değer yazarsak tüm bu ayarlar bozulacaktır. Çünkü argümandan gelen değer bir engelle karşılaşmadan doğrudan yazmaca gitmektedir. Bunu engelleyici bir kod yazılmamıştır.

**ADCSRA = \( 1 &lt;&lt; ADEN \) \| \( 1 &lt;&lt; ADSC \);**  Bu fonksiyon ADCSRA yazmacının ADEN bitini bir \(1\)  yaparak analog çeviriciyi eğer kapalıysa tekrar açar ve ADSC yazmacını bir \(1\) yaparak analog çevirimi başlatır.

**DELAY\_US\( 64 \);**  Analog çevrim için bir bekleme süresi verilmiştir.

**while \( ADCSRA & \( 1 &lt;&lt; ADSC \) \);** Bu komut analog çevirimin bittiğini kontrol etmek amacıyla ADCSRA yazmacındaki ADSC bitinin bir \(1\) olup olmadığını kontrol ederken mikrodenetleyiciyi döngüya sokar. Analog-Dijital Çevirici mikrodenetleyicinin işlemcisinden bağımsız çalıştığı için işlemci döngüdeyken analog-dijital çevirici işlemciden bağımsız çalışmaya devam eder. İşlemci bu bayrak bitini kontrol ederek işlemin bittiğinden haberdar olur.

**return ADCH;**   8 bitlik ADCH yazmacındaki değer fonksiyonda döndürülür. Böylelikle 8 bitlik analog ölçüm işlemi bitmiş olur. Şimdi 10-bit ADC çevriminin nasıl yapıldığına bakalım.

```c
uint16_t ADC10bit( uint8_t channel )
{
    ADMUX  = ( 1 << ADLAR ) | ( 1 << REFS0 ) | channel;
    ADCSRA = ( 1 << ADEN  ) | ( 1 << ADSC  );
    DELAY_US( 64 );
    while ( ADCSRA & ( 1 << ADSC ) );
	convert = ADCH;
	convert <<= 2;
	convert += (ADCL >> 6);
	return convert;
}
```

**ADMUX = \( 1 &lt;&lt; ADLAR \) \| \( 1 &lt;&lt; REFS0 \) \| channel;**  Bu komut yukarıdaki fonksiyonun aynısı. Burada da yine değeri sola hizalıyor.

**ADCSRA = \( 1 &lt;&lt; ADEN \) \| \( 1 &lt;&lt; ADSC \);** Öteki fonksiyonun aynısı olan bu komut ile ADC başlatılıyor. Alttaki iki fonksiyon da işlemin aynısı olduğu için bir daha anlatma gereği duymuyoruz.

**convert = ADCH;**  Burada “convert” değişkeninin nerede tanımlandığını önceden okumuş olduğumuz .h dosyasından hatırlayabiliriz. .h dosyasının .c dosyasından önce okunması gerektiğini buradan çıkarabiliriz. uint16\_t tipinde değişken olan “convert” burada analogdan dijitale çevirilen değeri saklayacaktır. Öncelikle ADCH yazmacındaki değerler convert değişkenine aktarılıyor.

**convert &lt;&lt;= 2;**  Burada convertteki tüm bitler iki bit sola kaydırılıyor. ADCH yazmacındaki bit örneğin “11111111” şeklinde olup “convert” değişkenine aktarıldığında ise “000000001111111” şeklinde olacaktır. Burada sağda iki yer açmak için bunu iki adım sola kaydırırsak sonuç şu şekilde olacaktır, “0000001111111100”.

**convert += \(ADCL &gt;&gt; 6\);**  Burada ADCL yazmacındaki tüm bitler 6 adım sağa kaydırılıyor. Bunun sebebi yazmacın son iki bitinde kalan veri bulundurulup bunun da normalde son bit verileri olmasından dolayıdır. Örneğin ADCL biti “11000000” ise bu işlemden sonra “00000011” olacaktır. Bu değer de convert değişkeninin içindeki boş bitlere aktarılacaktır. Böylelikle convert değişkeni 10 bitlik sonuçla doldurulmuş olacaktır. Böyle bit kaydırma işleminin yapılması değerlerin sola hizalanmasından dolayıdır. Bu da bu kütüphaneyi yazan kişinin tercihidir.

**return convert;**  Burada 16 bitlik convert değeri fonksiyondan döndürülür.

```c
void DELAY_US( uint16_t microseconds )
{

#if MCU_FREQ == 8000000UL

    /* 8mhz clock, 4 instructions per loop_count  */
    loop_count = microseconds * 2;

#elif MCU_FREQ == 1000000UL

    /* 1mhz clock, 4 instructions per loop_count */
    loop_count = microseconds / 4;

#elif MCU_FREQ == 16000000UL

    /* 1mhz clock, 4 instructions per loop_count */
    loop_count = microseconds / 4;

#else
#error MCU_FREQ undefined or set to an unknown value!
    loop_count = 0; /* don't really know what to do */
#endif

    __asm__ volatile (
        "1: sbiw %0,1" "\n\t"
        "brne 1b"
        : "=w" ( loop_count )
        : "0"  ( loop_count )
    );
}
```

Burada yazarın kullandığı bekleme fonksiyonunun kendisi görülmektedir. loop\_count bekleme ölçüsünü belirlemede MCU\_FREQ değerini esas almaktadır. Kütüphane yazarı 3 adet MCU\_FREQ değeri belirlemiştir. Yani bu kütüphane bu hali ile .h dosyasındaki MCU\_FREQ değiştirilse dahi 8MHz, 10MHz ve 16MHz hızlarında kullanılabilir. Burada yazar bunun farkında olup \#error komutu ile bu üç frekanstan başka bir frekans değeri giren kullanıcıya hata mesajını iletmiştir. Fonksiyonun aşağısında ise Assembly dilinde bekleme komutu yer almaktadır. Kütüphaneyi incelediğimizde bazı zayıflıklar söz konusu olsa da bunu inceleyerek kütüphanelerin nasıl bir yapıda olduğunu satır satır size gösterdik. Şimdi ise Atmel Studio’da projemize nasıl kütüphane dosyalarını ekleyeceğimizi gösterelim.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/kutuphaneekleme.png)](http://www.lojikprob.com/diger/c-ile-avr-programlama-18-adc-kutuphanesi-incelemesi-ve-kutuphane-kullanmaya-giris/attachment/kutuphaneekleme/)

Yukarıdaki resimdeki gibi **Solution Explorer** penceresinde projeye sağ tıklayıp **Add / Existing Item** diyoruz ve dosya seçimi ekranından .h ve .c uzantılı dosyaların ikisini de seçiyoruz.

Projemizde ise baş kısma **\#include “kutuphaneadi.h”** şeklinde yazıyoruz. Bundan sonra bu kütüphaneye ait fonksiyonları kullanabiliriz.

Kütüphaneyi kullanmak isteyenler aşağıdaki bağlantıdan indirebilir.  
[https://github.com/GDokmetas/AVR-ADC-Library](https://github.com/GDokmetas/AVR-ADC-Library)

Kapak Resmi : http://10rem.net/media/82343/Windows-Live-Writer\_b3541542a059\_B73E\_image\_21.png

