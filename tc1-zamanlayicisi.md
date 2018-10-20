# TC1 Zamanlayıcısı

TC0 zamanlayıcısını nihayet bitirsek de geriye TC1 ve TC2 zamanlayıcıları kaldı. Bundan sonra ise bu zamanlayıcıların kullanımını ve örnek kodları inceleyeceğiz. TC1 zamanlayıcısı TC0 zamanlayıcısına benzese de diğerinden en önemli farkı bu zamanlayıcının 16-bit olmasıdır. Böylelikle daha geniş değer aralığına sahip olan sayma değeri daha esnek bir zamanlama imkanı sunar. Bu esneklik ön derecelendirici değerini artırarak bir noktaya kadar artırılsa da 8 bitlik bir değer ile oldukça az oluyordu. 16 bitlik zamanlayıcıda bu geniş değer aralığı ve ön derecelendiriciler ile oldukça uzun süre aralıklarında çalışan bir zamanlayıcı elde edebiliriz.

TC1 zamanlayıcısının yazmaçlarını incelediğimde TC0 zamanlayıcısının yazmaçları ile hemen hemen aynı olduğunu görürüz. Aradaki en büyük fark sayaç ve karşılaştırma değerlerini barındıran yazmaçların her birinin toplamda 16 bitlik değer bulundurması olarak görünüyor. Yine bu değerler 8 bitlik yüksek ve düşük bayt değeri olarak ayrılsa da ikisinin toplamı 16 bit olduğu için toplamda 16 bitlik değer saklayabiliyor. Yine TC0 yazmaçlarında olduğu gibi ayar bitleri, kesme bitleri, bayrak bitleri bu yazmaçlarda da mevcuttur.

Doğrudan yazmaçlardan bahsederek önceki konuyu tekrarlamamak için şimdi TC0’ı anlatırken değinmediğimiz konulardan bahsederek dersimizi devam ettireceğiz. Yine yazmaç kısmında belli noktalara değineceğiz ve iki zamanlayıcının yazmaçlarını karşılaştırarak farklı noktaları dersimize dahil edeceğiz. Öncelikle anlatacağımız konunun anlaşılması için TC1’in blok diyagramını verelim.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc11.png)](http://www.lojikprob.com/diger/c-ile-avr-programlama-29-tc1-zamanlayicisi/attachment/tc11/)

Resim : Resim: ATmega328P – Microchip Technology ,sf. 150,http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

Burada fark edildiği üzere TC1 zamanlayıcısının yapısı TC0 zamanlayıcısı ile hemen hemen aynıdır. O yüzden zamanlayıcının yapısını açıklamamız diğer zamanlayıcıları tanıma konusunda size yardımcı olacaktır. Bundan önce 16 bitlik yazmaçları kullanma konusunda biraz bilgi vermemiz gereklidir.

#### **16 Bit Yazmaçlara Erişim** 

TCNT1, OCR1A/B ve ICR1 yazmaçları 16 bit olup AVR işlemcisinin 8 bitlik veri yolu ile erişilebilir. Ancak bu erişim bayt tabanlı olup iki okuma ve yazma işlemi ile sağlanır. Her 16-bit zamanlayıcı tek bir 8-bit geçici yazmaca sahip olup 16 bitlik verinin yüksek baytını içinde bulundurur. Aynı geçici yazmaç bütün 16 bitlik yazmaçlar ile paylaştırılır. Düşük bayta erişim 16 bitlik okuma ya da yazma işlemini tetikler. Eğer 16 bitlik yazmacın düşük baytı işlemci tarafından yazılırsa yüksek baytı TEMP yazmacında saklanır ve düşük bayt ile beraber 16 bitlik yazmaca aynı saat çeviriminde kopyalanır. 16 bitlik yazmacın düşük baytı okunduğunda yüksek bayt TEMP yazmacına işlemci tarafından kopyalanır.

16 bitlik yazmaca erişim Assembly ve C dillerinde farklı şekilde yürür. Biz şimdilik C dilinde verilmiş örnek kodu buraya koyacağız.

```c
unsigned int i;
...
/* TCNT1 yazmacına bir değer ver */
TCNT1 = 0x1FF;
/* TCNT1 yazmacını oku*/
i = TCNT1;
...
```

Burada i değişkeninin unsigned int cinsinde olduğunu unutmayalım. Bu değişken 0 ile 65535 arasındaki 16 bitlik değeri saklayabilir.

Eğer TCNT1 \( Önceki konuda bahsettiğimiz üzere sayıcı değerini bulunduran yazmaç.\) yazmacını okumak veya yazmak bir kesmeye sebep oluyorsa öncelikle kesmeleri kapatıp sonra okuma ve yazma işlemini yapıp bunun ardından tekrar eski mikrodenetleyici ayarını yapmak gereklidir. Okuma yapmak için örnek fonksiyon aşağıdaki gibi olmalıdır.

```c
unsigned int TIM16_ReadTCNT1( void )
{
unsigned char sreg;
unsigned int i;
/* Genel kesme bayrağını kaydet.  */
sreg = SREG;
/*Kesmeleri kapat */
_CLI();
/* TCNT1 değerini oku */
i = TCNT1;
/* Eski değeri yükle */
SREG = sreg;
return i;
}
```

Yazma işlemindeki fonksiyonda ise sadece TCNT1 yazmacı ve i değişkeninin yerleri değişmelidir.

#### **Sayıcı Ünitesi**

16 bitlik zamanlayıcı ve sayıcı ünitesinin ana parçası programlanabilir 16-bit iki yönlü sayıcı ünitesidir. Aşağıda blok diyagramını görebilirsiniz.  
[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc12.png)](http://www.lojikprob.com/diger/c-ile-avr-programlama-29-tc1-zamanlayicisi/attachment/tc12/)

Burada bazı sinyalleri açıklamak gereklidir. Bu sinyallerin açıklaması aşağıdaki tabloda verilmiştir.

| **Sinyal Adı** | **Açıklama** |
| :--- | :--- |
| Count | TCNT1 yazmacının birer azaltımı veya artırımı |
| Direction | Azaltma ve Artırım seçimi |
| Clear | TCNT1 yazmacını temizleme \(Bitlerin sıfırlanması\) |
| clkt1 | Zamanlayıcı saat sinyali |
| TOP | TCNT1 azami değerine ulaştığında verilecek sinyal |
| BOTTOM | TCNT1 dip değerine ulaştığında verilecek sinyal \(0\) |

16-bit sayıcı iki adet 8-bit giriş ve çıkış hafıza konumuna haritalandırılmıştır. Yüksek Sayaç \(TCNT1H\) sayacın üst sekiz bitini barındırırken, Alçak Sayaç \(TCNT1L\) sayacın alt sekiz bitini barındırır. İşlemci TCNT1H yazmacının giriş ve çıkış konumuna eriştiğinde işlemci geçici yüksek bit yazmacına \(TEMP\) erişmiş olur. TCNT1L okunduğunda geçici yazmaç güncellenir. Ayrıca TCNT1L yazmacına veri yazıldığı zaman TCNT1H yazmacı güncellenir. Böylelikle bu özellik işlemcinin tek bir saat çeviriminde 8 bitlik veri yolu üzerinden 16 bit veri okumasını sağlar.

Çalışma moduna bağlı olarak sayıcı her saat çevriminde \(clkt1\) sıfırlanır, artırılır ve azaltılır. clkt1 saat sinyali dahili ya da harici saat kaynağından üretilebilir. Bunlar saat seçme bitlerinden denetlenir ve bunlar TCCR1B yazmacında bulunur. Eğer hiçbir saat kaynağı seçilmezse zamanlayıcı durur ve saymayı bırakır. Her durumda da TCNT1 değeri işlemci tarafından okunup yazılabilir. İşlemci yazma işlemi bütün zamanlayıcı sayma ve sıfırlama işlemlerini geçersiz kılar. Yani öncelik sırası işlemcidedir.

Sayma sırası dalga formu üreteci modu bitlerinin değiştirilmesiyle belirlenir. Bunlar TCCR1B ve TCCR1A yazmaçlarında bulunur. Zamanlayıcının sayımı ile dalga formlarının nasıl üretildiği arasında sıkı bir bağlantı vardır. Zamanlayıcı taşma bayrağı TIFR1 yazmacı içerisinde bulunur. Bu bit işlemci kesmesi oluşturmak için kullanılır.

Sıradaki derste diğer zamanlayıcı ünitelerini anlatmakla dersimize devam edeceğiz.

Kaynaklar:

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

