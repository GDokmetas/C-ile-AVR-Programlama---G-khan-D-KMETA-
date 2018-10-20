# Analog Karşılaştırıcı \(Analog Comparator\)

45. derse başlarken konulara şöyle bir baktığımızda anlatılacak konuların oldukça azaldığını görüyoruz. Şu ana kadar önemli konuların çoğunu bitirmemizin yanında toplam konu sayısı olarak konuların çoğunu anlatmış olduk. Geriye kalan konular ise zor konular olmayıp sizi yormayacaktır. Dersleri yazarken mümkün olduğu kadar birinci kaynakları \(üretici tarafından yayınlanan dokümanlar\) kullandığımız için bu kaynaklarda bahsedilmeyen bilgileri mümkün olduğu kadar AVR programlama derslerimize almayacağız. Bu ders dizisi bittikten sonra farklı bir başlık altında diğer konulardan bahsedeceğiz.

Şimdi yeni konumuz olan Analog Karşılaştırıcı birimine giriş yapalım.

**Analog Karşılaştırıcı Birimi**

Analog karşılaştırıcı iki değeri analog olarak karşılaştırır ve bu iki değerin durumuna göre dijital bir çıkış verir. Aslında bu işlemi biz iki ADC kanalından okuma yapıp bunları if-else yapısı ile karşılaştırarak yapabiliriz. Fakat bu biraz usulsüz olacağı için bu görev için mevcut olan birimi kullanmamız daha doğru olacaktır. Bir de Analog Karşılaştırıcı biriminde ayrı kesmeler ve özellikler vardır. O yüzden öğrenmeden geçmemek gereklidir.

Analog karşılaştırıcı pozitif ve negatif ayak olmak üzere iki adet ayağa sahiptir. Pozitif ayak AIN0 olarak, nagatif ayak ise AIN1 olarak adlandırılır. AIN0 ayağına giden gerilim AIN1 ayağına giden gerilimden yüksek ise analog karşılaştırıcının çıkışı \(ACO\) bir \(1\) konumuna gelir. Karşılaştırıcının çıkışı TC1 zamanlayıcısında giriş yakalama işlemi için de kullanılabilir. Bu özelliğin olması bu çıkışın sayma işleminde kullanılmasına olanak sağlar. Ayrıca karşılaştırıcı ayrı bir kesme yürütür. Analog karşılaştırıcı için özel bir kesmenin olmasının bize faydası dokunacaktır.

Analog karşılaştırıcıda ADC Multiplexer kullanmak için PRR.PRADC bitinin sıfır \(0\) yapılması gereklidir. Şimdi fikir edinmeniz açısından analog karşılaştırıcının blok diyagramını aşağıda verelim.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/ac1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-45-analog-karsilastirici-analog-comparator/attachment/ac1/)

Analog karşılaştırıcı mikroişlemciden ayrı bir birim olup mikroişlemciden bağımsız olarak çalışır. Bütün analog ölçme ve karşılaştırma işlemini kendi içinde yürütür. Yürüttüğü kesmelerle mikroişlemciye müdahale eder. Mikrodenetleyicinin erişimi ise çıkış biti olan ACO bitini okumakla olur.

**Analog Karşılaştırıcı Çoklu Giriş Ayağı**

ADC\[7:0\] yani analog giriş ayakları analog karşılaştırıcının negatif ayağı yerine kullanılabilir. Bu özelliğin kullanılması için ADC kapalı konumda olmalıdır. ADC Denetim ve Durum Yazmacı B’deki Analog Karşılaştırıcı Çoklayıcısı Etkin hale getirilirse  \(ADCSRB.ACME\) ve ADC kapatılırsa \(ADCSRA.ADEN=0\) ADMUX yazmacının son üç bitindeki çoklayıcı bitleri analog karşılaştırıcının negatif girişini belirler. Aşağıdaki tablodan daha anlaşılır halde görebilirsiniz.

| ACME | ADEN | MUX\[2:0\] | Analog Karşılaştırıcı Negatif Girişi |
| :--- | :--- | :--- | :--- |
| 0 | x | xxx | AIN1 |
| 1 | 1 | xxx | AIN1 |
| 1 | 0 | 000 | ADC0 |
| 1 | 0 | 001 | ADC1 |
| 1 | 0 | 010 | ADC2 |
| 1 | 0 | 011 | ADC3 |
| 1 | 0 | 100 | ADC4 |
| 1 | 0 | 101 | ADC5 |
| 1 | 0 | 110 | ADC6 |
| 1 | 0 | 111 | ADC7 |

Şimdi yazmaçlara geçelim ve Analog Karşılaştırıcı hakkında anlatılmadık bir şey kalmasın.

**ADCSRB – ADC Denetim ve Durum Yazmacı B** 

Bu yazmacın kullanımını daha önce açıkladığımız için şu bağlantıdan tekrar okuyun.  
[http://www.lojikprob.com/avr/c-ile-avr-programlama-16-analog-dijital-cevirici-adc-yazmaclari/](http://www.lojikprob.com/avr/c-ile-avr-programlama-16-analog-dijital-cevirici-adc-yazmaclari/)

**ACSR – Analog Karşılaştırıcı Denetim ve Durum Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/ac2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-45-analog-karsilastirici-analog-comparator/attachment/ac2/)

Bu yazmaç analog karşılaştırıcı hakkında bütün işlemleri yapacağımız yazmaçtır.

**Bit 7 – ACD : Analog Karşılaştırıcı Devre Dışı**

Bu bit bir \(1\) yapıldığında analog karşılaştırıcıya giden besleme devre dışı bırakılır. Bu bit istenilen her zaman devre dışı bırakmak için kullanılabilir. Güç tasarrufu için aklımızda bulundurmakta fayda vardır. ACD biti değiştirilirken ACIE biti ile kesmenin de devre dışı bırakılması gereklidir.

**Bit 6 – ACBG : Analog Karşılaştırıcı Bant Aralığı Seçimi**

Bu bit bir \(1\) yapıldığında bant aralığı referansı pozitif girişin yerine geçer. Bu bit sıfır \(0\) olduğunda ise AIN0 pozitif giriş olarak kullanılır.

**Bit 5 – ACO : Analog Karşılaştırıcı Çıkışı**

Analog karşılaştırıcının çıkışını bu bitten okuruz. Arada senkronizasyon işlemi olduğu için 1-2 saat çevirimi gecikmeli okunur.

**Bit 4 – ACI: Analog Karşılaştırıcı Kesme Bayrak Biti**

Bu bit donanım tarafından analog karşılaştırıcı kesmesi yürütüldüğünde bir \(1\) konumuna getirilir. ACI biti kesme fonksiyonu yürütüldükten ya da üzerine bir \(1\) yazıldıktan sonra sıfırlanır.

**Bit 3 – ACIE : Analog Karşılaştırıcı Kesmesi Etkin**

Analog karşılaştırıcının kesme yürütmesini istiyorsak bu biti bir \(1\) yapmamız gereklidir.

**Bit 2 – ACIC : Analog Karşılaştırıcı Giriş Yakalama Etkin**

Bu bit bir \(1\) yapıldığında TC1 zamanlayıcısının giriş yakalama fonksiyonu analog karşılaştırıcı tarafından tetiklenir. Bu bit sıfır \(0\) yapıldığında analog karşılaştırıcı ile zamanlayıcı arasında bir bağlantı kalmaz.

**Bit 1:0 – Analog Karşılaştırıcı Kesme Modu Seçimi \[ n = 1:0 \]**

Bu bitlerin durumuna göre analog karşılaştırıcı kesme modu seçimi yapılır. Kesme modlarını aşağıdaki tabloda görebilirsiniz.

| ACIS1 | ACIS0 | Kesme Modu |
| :--- | :--- | :--- |
| 0 | 0 | Çıkış Değişmesinde Kesme |
| 0 | 1 | Rezerve \(Kullanım Dışı\) |
| 1 | 0 | Düşen çıkış kenarında kesme |
| 1 | 1 | Yükselen çıkış kenarında kesme |

**DIDR1 – Dijital Girişi Devre Dışı Bırakma Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/ac3.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-45-analog-karsilastirici-analog-comparator/attachment/ac3/)

Bu yazmaçtaki AIN1D ve AIN0D bitleri bir yapılırsa bu ayaklardaki dijital giriş devre dışı bırakılır. Böylelikle analog sinyal uygulanan ayaklarda dijital giriş tamponu devre dışı bırakılarak güç tasarrufu sağlanır.

Böylelikle analog karşılaştırıcı hakkında anlatılabilecek her konuyu anlatmış olduk. Konu oldukça basit olduğu için örnek kod vermemize gerek yoktur. Bir sonraki derste yeni bir konuya geçeceğiz.

_**Kaynaklar**_

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

