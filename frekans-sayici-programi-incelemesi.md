# Frekans Sayıcı Programı İncelemesi

 İnternette Arduino için yazılmış ama çoğunlukla AVR kodlarından oluşan benim de kullandığım bir frekans sayıcı programı vardı. Bu programı beğendiğim için ne kadar Arduino uyumlu olsa da AVR üzerinden anlatarak derslere ilave edeceğim. Bu program Arduino UNO’yu yani Atmega328P mikrodenetleyicisinin zamanlayıcı ünitelerini kullanarak D5 ayağından yani PD5 / T1 ayağından giriş alır. Belli bir zaman periyodunu kullanarak o ayağa gelen sinyalleri ölçerek frekansı bildirir. Uzun zaman periyotu daha fazla hassasiyet verecektir.  Şimdi programı verelim ve sonrasında kodları tek tek açıklayalım.

```c
// Frekans Sayıcı Örneği
// Yazar: Nick Gammon
// Tarih: 17th January 2012

// Giriş D5 ayağı

// Bu değerler ana program tarafından denetlenecektir. 
volatile unsigned long timerCounts;
volatile boolean counterReady;

// sayaç rutini değerleri
unsigned long overflowCount;
unsigned int timerTicks;
unsigned int timerPeriod;

void startCounting (unsigned int ms) 
  {
  counterReady = false;         // Zamanı daha gelmedi
  timerPeriod = ms;             // kaç milisaniye sayılacağı
  timerTicks = 0;               // kesme sayacını sıfırla
  overflowCount = 0;            // taşma henüz yok

  // TC1 ile TC2 yi sıfırla
  TCCR1A = 0;             
  TCCR1B = 0;              
  TCCR2A = 0;
  TCCR2B = 0;

  // D5 ayağındaki olayları sayar. 
  TIMSK1 = bit (TOIE1);   // TC1 taşmasında kesmeye götür 

  // Timer 2 -1 ms ölçme aralığını verir. 
  // 16 MHz saat (62.5 ns çevirim başı) - 128 ön derecelendirici
  //  Sayaç her 8 mikrosaniyede bir artar
  // Bundan 125 adet sayıldığında bize tam 1ms verir. 
  TCCR2A = bit (WGM21) ;   // CTC modu
  OCR2A  = 124;            // 125'e kadar sıfırdan itibaren say

  // Timer 2 - her eşleşmede kesme (her 1 milisaniyede bir )
  TIMSK2 = bit (OCIE2A);   // TC2 kesmesini etkinleştir

  TCNT1 = 0;      // Her iki sayacı da sıfırla
  TCNT2 = 0;     

  // Ön derecelendiricileri sıfırla
  GTCCR = bit (PSRASY);        // Ön derecelendiriciyi sıfırla
  // TC2'yi başlat. 
  TCCR2B =  bit (CS20) | bit (CS22) ;  // Ön derecelendiriciyi 128'e ayarla
  // TC1 'i başlar. 
  // T1 ayağından harici saat kaynağı. (D5). Yükselen kenarda sayım.
  TCCR1B =  bit (CS10) | bit (CS11) | bit (CS12);
  }  // startCounting fonksiyonu bitişi 

ISR (TIMER1_OVF_vect)
  {
  ++overflowCount;               // Counter1 taşma sayısını say.
  }  //  TIMER1_OVF_vect bitimi


//******************************************************************
//  Timer 2 kesme servisi donanım tarafından  her 1 ms de bir işletilir = 1000 Hz
//  16Mhz / 128 / 125 = 1000 Hz

ISR (TIMER2_COMPA_vect) 
  {
  // sayaç değerini değişmeden hemen al
  unsigned int timer1CounterValue;
  timer1CounterValue = TCNT1;  // 16 bit yazmaç okunur. 
  unsigned long overflowCopy = overflowCount;

  // zamanlama periyoduna ulaşıp ulaşmadığı kontrol edilir.
  if (++timerTicks < timerPeriod) 
    return;  // henüz değil. 

  // Eğer taşma kaçırılırsa
  if ((TIFR1 & bit (TOV1)) && timer1CounterValue < 256)
    overflowCopy++;

  // geçit zamanı bitişi, ölçmeye hazır. 

  TCCR1A = 0;    // TC1 i durdur
  TCCR1B = 0;    

  TCCR2A = 0;    // TC2 yi durdur
  TCCR2B = 0;    

  TIMSK1 = 0;    // TC1 kesmesini devre dışı bırak
  TIMSK2 = 0;    // TC2 kesmesini devre dışı bırak
    
  // Toplam değeri hesapla
  timerCounts = (overflowCopy << 16) + timer1CounterValue;  // 65536 her taşma.
  counterReady = true;              // her saymadan sonra genel bayrağı etkinleştir
  }  // TIMER2_COMPA_vect bitişi

void setup () 
  {
  Serial.begin(115200);       
  Serial.println("Frequency Counter");
  } // setup bitişi

void loop () 
  {
  // TC0 kesmelerini durdur 
  byte oldTCCR0A = TCCR0A;
  byte oldTCCR0B = TCCR0B;
  TCCR0A = 0;    // TC0'ı durdur
  TCCR0B = 0;    
  
  startCounting (500);  // ölçüm için kaç milisaniye ayarlayacağını belirler
  while (!counterReady) 
     { }  // Sayım bitene kadar devam et

  // sayılan değeri sayım aralığına göre artır ve frekansı ver
  float frq = (timerCounts *  1000.0) / timerPeriod;

  Serial.print ("Frequency: ");
  Serial.print ((unsigned long) frq);
  Serial.println (" Hz.");
  
  // TC0'ı yeniden başlat.
  TCCR0A = oldTCCR0A;
  TCCR0B = oldTCCR0B;
  
  // bitir
  delay(200);
  }   // döngü bitimi
```

Öncelikle loop\(\) fonksiyonundan kodları incelemeye başlayalım.

```c
 byte oldTCCR0A = TCCR0A;
 byte oldTCCR0B = TCCR0B;
```

 Burada TCCR0 yazmaçlarında olan ilk değer oldTCCR0A ve oldTCCR0B değişkenlerine aktarılarak saklanır.

```c
  TCCR0A = 0;    
  TCCR0B = 0;    
```

 Burada TCCR0A ve TCCR0B yazmaçları sıfırlanarak zamanlayıcı durdurulur.

```c
startCounting (500); 
```

 Frekans ölçme fonksiyonu 500 argümanı ile birlikte çalıştırılır. Bunun anlamı frekans ölçme fonksiyonu 500 milisaniye boyunca gelen frekansı ölçecek demektir. Şimdi startCounting\(\) fonksiyonuna geçelim ve ondan sonra tekrar ana program döngüsüne dönelim.

```c
void startCounting (unsigned int ms) 
  {
  counterReady = false;         // Zamanı daha gelmedi
  timerPeriod = ms;             // kaç milisaniye sayılacağı
  timerTicks = 0;               // kesme sayacını sıfırla
  overflowCount = 0;            // taşma henüz yok
```

 Burada startCounting fonksiyonunun aldığı argüman “ms” adıyla adlandırılır. “ms” değişkeninin kullanıldığı yer bizim milisaniye değerimiz olan 500 olacak. Önceden dört adet genel değişken program başında tanımlanmıştı. Şimdi bunlara fonksiyonun başlangıcında ilk değerlerin verildiğini görüyoruz. CounterReady yani sayacın hazır olduğu false durumuna getirilmiştir. Böylelikle sayacın meşgul olduğu belirtilir. timerPeriod değişkenine ise bizim argüman olarak aldığımız ms değişkeninin değeri aktarılmıştır. 500 değeri şimdi de timerPeriod değişkenine geçmiştir. timerTicks değişkeni ise zamanlayıcının kaç defa attığını yani kesmeye geçtiğini sayma verisini tutan değişkendir. overflowCount ise kaç defa taşma gerçekleştiğinin sayım verisini tutan değişken olarak belirtilmiştir. Bu değişkenlerin eski değerleri yeni sayımda önemli olmadığı için hepsi sıfırlanmıştır.

```c
  // TC1 ile TC2 yi sıfırla
  TCCR1A = 0;             
  TCCR1B = 0;              
  TCCR2A = 0;
  TCCR2B = 0;
```

 TC1 ve TC2 zamanlayıcılarının kontrol yazmaçları sıfırlanmıştır. Böylelikle zamanlayıcıların tüm özellikleri sıfırlanıp devre dışı bırakılmıştır.

```c
// D5 ayağındaki olayları sayar. 
  TIMSK1 = bit (TOIE1);   // TC1 taşmasında kesmeye götür
```

 TIMSK1 yazmacının TOIE1 biti bir \(1\) yapılmıştır. Buradaki bit\(\) fonksiyonu AVR’deki BV\_\(\) makrosu ile aynı işlevi görmektedir. TOIE0 bitine bir \(1\) yazıldığında Zamanlayıcı/Sayıcı Taşma Kesmesi etkin hale gelir.

```c
// Timer 2 -1 ms ölçme aralığını verir. 
  // 16 MHz saat (62.5 ns çevirim başı) - 128 ön derecelendirici
  //  Sayaç her 8 mikrosaniyede bir artar
  // Bundan 125 adet sayıldığında bize tam 1ms verir. 
  TCCR2A = bit (WGM21) ;   // CTC modu
  OCR2A  = 124;            // 125'e kadar sıfırdan itibaren say
```

 TCCR2A yani TC2 zamanlayıcısının A isimli kontrol yazmacındaki WGM21 biti bir \(1\) konumuna getirilir. Böylelikle zamanlayıcı CTC modunda çalışmış olur. Bunun için karşılaştırma değeri olan OCR2A yazmacına ise 124 değeri atanmıştır.

```c
// Timer 2 - her eşleşmede kesme (her 1 milisaniyede bir )
  TIMSK2 = bit (OCIE2A);   // TC2 kesmesini etkinleştir
```

 TC2 sayıcısının TIMSK2 yazmacındaki OCIE2A biti bir \(1\) yapılmıştır. OCIE0A bitine bir \(1\) yazıldığına Zamanlayıcı/Sayıcı karşılaştırma eşleşmesi kesmesi etkin hale gelir. Böylelikle yukarıda OCR2A yazmacına atanan 124 değeri ile eşleşme sağlanırsa kesme yürütülür.

```c
TCNT1 = 0;      // Her iki sayacı da sıfırla
TCNT2 = 0;
```

 Her iki zamanlayıcının sayaç değerlerini sayaç değeri yazmacına sıfır \(0\) atayarak sıfırlıyoruz.

```c
 // Ön derecelendiricileri sıfırla
  GTCCR = bit (PSRASY);        // Ön derecelendiriciyi sıfırla
```

 GTCCR yazmacının PSRASY biti bir yapıldığında ön derecelendirici sıfırlanır.

```c
 // TC2'yi başlat. 
  TCCR2B =  bit (CS20) | bit (CS22) ;  // Ön derecelendiriciyi 128'e ayarla
  // TC1 'i başlar. 
  // T1 ayağından harici saat kaynağı. (D5). Yükselen kenarda sayım.
  TCCR1B =  bit (CS10) | bit (CS11) | bit (CS12);
```

 Buraya kadar zamanlayıcıları ayarladık, sıfırladık ve bazı değişkenlere değer atadık. Zamanlayıcıları ise şimdi bu kodlarla başlatıyoruz ve ardından kesme fonksiyonları ile diğer işlemleri yapıyoruz. Öncelikle TCCR2B yazmacının CS20 ve CS22 bitleri bir \(1\) yapılır. Böylelikle TC2 zamanlayıcısının ön derecelendiricisi 128’e ayarlanmış olur. Şimdi ise 16 bitlik TC1 zamanlayıcısının giriş ayağından frekans ölçmemiz gerekli. Bunun için TCCR1B yazmacının CS10, CS11 ve CS12 bitleri bir \(1\) yapılır. Böylelikle zamanlayıcının sayması için gereken saat frekansı T1 ayağından \(PD5\) verilmiş olur. Verilen saat frekansına göre yükselen kenarda sayım yapılır. Asıl frekans ölçüm verisinin depolandığı yer T1 zamanlayıcısının sayaç yazmacıdır. Bizim fonksiyonumuz bundan ibaret olsa da TC1 zamanlayıcısının dolup boşalan sayacı bizim için bir anlam ifade etmez. O veriyi işleyip yorumlamak için diğer komutlara ihtiyacımız vardır. Bunu da kesmeler ile yapıyoruz. Şimdi kesme fonksiyonlarına göz atalım.

```c
ISR (TIMER1_OVF_vect)
  {
  ++overflowCount;               // Counter1 taşma sayısını say.
  }  //  TIMER1_OVF_vect bitimi
```

 TIMSK1 = bit \(TOIE1\); komutuyla TC1 zamanlayıcısını taşma sağlandığında kesmeye götürmesini söylemiştik. Şimdi ise TIMER1\_OVF\_vect bu kesmede ne yapılacağını belirlememiz için burada kullanılmıştır. Her taşma gerçekleştiğinde taşma sayıcı değişkenin değeri bir artırılacaktır. Bu taşma verisi kullanılmak üzere şimdilik saklanacaktır.

```c
ISR (TIMER2_COMPA_vect) 
  {
  // sayaç değerini değişmeden hemen al
  unsigned int timer1CounterValue;
  timer1CounterValue = TCNT1;  // 16 bit yazmaç okunur. 
  unsigned long overflowCopy = overflowCount;
```

Frekans değerini dışarıdan alıp depolayan TC1 yazmacının yanında süre hesabını yapan TC2 yazmacı vardı. Bu yazmaca OCR2A = 124; komutu ile önceden karşılaştırma değerini atayıp bu karşılaştırma gerçekleştiğinde ise kesmeye götürmesini söylemiştik. Şimdi bu kesme fonksiyonunda neler yapılacağı ise burada belirtilmiştir. Öncelikle teknik veri sayfasında belirtildiği üzere kesmeden sonra ilk öncelikle sayaç değerini okumamız gerekiyordu. Bunun için unsigned int cinsinde timer1CounterValue değişkeni tanımlanmıştır. Ardından sayaç verisini içinde barındıran TCNT1 yazmacı bu değişkene aktarılmıştır. TCNT1 yazmacı önceden de bahsettiğimiz üzere TC1 zamanlayıcısının sayaç yazmacıydı. Bunun ardından overflowcopy adında bir değişken tanımlanmış ve taşma sayımını yapan değişken olan overflowCount’un değeri buna aktarılmıştır.  


```c
// zamanlama periyoduna ulaşıp ulaşmadığı kontrol edilir.
if (++timerTicks < timerPeriod) 
return; // henüz değil.
```

 TC2 karşılaştırma eşleşme kesmesi her bir \(1\) mili saniyede bir yürütüldüğü için bizim timerPeriod olarak tanımladığımız ve kaç mili saniyelik bir ölçüm gerçekleştirileceğini belirlediğimiz değere ulaşılmadıkça bu ölçüm yapılmayacaktır. Öncelikle timerTicks yazmacı her bir mili saniyede yürütülen kesmeden dolayı bir artırılır ve timerPeriod değişkeni ile karşılaştırılır. ++ operatörünün başta olması işlem önceliği için önemlidir. return; komutu ile kesme fonksiyonundan çıkılır ve diğer kodlar işletilmez.

```c
// Eğer taşma kaçırılırsa
if ((TIFR1 & bit (TOV1)) && timer1CounterValue < 256)
overflowCopy++;
```

 Kesme fonksiyonuna girildiği zaman mevcut taşma durumu kaçırılırsa TIFR1 yazmacındaki taşma bayrak biti olan TOV1 denetlenir overflowCopy değişkeni bir artırılır. Normalde TC1 yazmacının özel taşma kesmesi fonksiyonu ile bu taşma verisi artırılıyordu.

```c
// geçit zamanı bitişi, ölçmeye hazır. 

  TCCR1A = 0;    // TC1 i durdur
  TCCR1B = 0;    

  TCCR2A = 0;    // TC2 yi durdur
  TCCR2B = 0;    

  TIMSK1 = 0;    // TC1 kesmesini devre dışı bırak
  TIMSK2 = 0;    // TC2 kesmesini devre dışı bırak
```

 Şimdi bütün sayaçlar ve kesmeleri ölçümün hesaplanması üzere devre dışı bırakılır.

```c
 // Toplam değeri hesapla
  timerCounts = (overflowCopy << 16) + timer1CounterValue;  // 65536 her taşma.
  counterReady = true;              // her saymadan sonra genel bayrağı etkinleştir
```

Şimdi toplam sayılan frekansı hesaplamak için kaç kere taşma yapıldıysa bu değerin bitleri 16 kere sola kaydırılır. Bu işlem mevcut taşma değerini 65536 değeri ile çarpmaya eşdeğerdir. Ardından timerCounts değişkenine bu değer aktarılır.  Sonrasında ise sayacın işlemi bitirdiği ve hazır olduğu anlamında counterReady değeri “true” yapılır.

Frekans ölçme ile ilgili tüm fonksiyonlar ve kesme fonksiyonlarını anlatmayı bitirdik. Şimdi ana programa dönelim ve frekansın nasıl hesaplandığına bakalım.

```c
float frq = (timerCounts * 1000.0) / timerPeriod;
```

 Frekans değeri timerCounts değişkeninin 1000 ile çarpımının timerPeriod değişkeninin değerine bölünmesiyle elde edilir. 1000 ile çarpılmasının sebebi her 1 mili saniyede bir TC2 eşleşme kesmesinin yürütülmesidir.

```c
  // TC0'ı yeniden başlat.
  TCCR0A = oldTCCR0A;
  TCCR0B = oldTCCR0B;
```

Bu kodlarla ise TCCR0A ve TCCR0B değerlerinin ilk halleri tekrar bu yazmaçlara yüklenir ve sayma işlemi yeniden başlar. Görmezden geldiğimiz Arduino kodları bizim işimize yaramasa da buraya kadar incelediğimiz kodlar Arduino kodu olmayıp AVR programlamada geçerli kodlardır. O yüzden AVR programcıları için faydalı olacağına inandığım bu programı satır satır inceledim.

Kaynaklar:

Timers and counters, Nick Gammon, http://www.gammon.com.au/timers, Erişim Tarihi: 30.08.2018

Kapak Resmi :https://d3s11pzv7w3h1q.cloudfront.net/wp-content/uploads/IC-ATMEGA8A-PU-2.jpg

C ile AVR Programlama -27- TC0 Zamanlayıcı Yazmaçları, Gökhan Dökmetaş, http://www.lojikprob.com/avr/c-ile-avr-programlama-27-tc0-zamanlayici-yazmaclari/, Erişim Tarihi : 30.08.2018

C ile AVR Programlama -26- TC0 Zamanlayıcısı, Gökhan Dökmetaş, http://www.lojikprob.com/avr/c-ile-avr-programlama-26-tc0-zamanlayicisi/, Erişim Tarihi: 30.08.2018

