# Analog-Dijital Çevirici \(ADC\)

Dijital olmayan herhangi bir sistem analog sistemdir. Bu bir pil ile lamba devresi de olabilir bir amfide olabilir bir güç kaynağı da olabilir Bunlar mikrodenetleyicilerin anlayacağı dilden konuşmazlar. 1 ve 0 üzerinden işlem yapan bir mikrodenetleyici değişken gerilim değerlerini anlayamaz. Sadece elektriğin belli bir seviyede var olup olmadığını anlar. Dijital veri okumakla değişken gerilim değerlerini ölçmek mümkün değildir. Atmel AVR mikrodenetleyicilerde 10-bit analog-dijital çevirici bulunur ve bu çevirici çoğu projede yeterli hassasiyeti sağlar. Çeşitli seri iletişim yolları üzerinden hassas analog dijital çevrimi yapan modüller ve entegreler olsa da konumuz AVR mikrodenetleyicilerdeki ADC birimini öğrenmek ve bunu kullanmak olduğu için sadece AVR üzerinden anlatmakla yetineceğiz. ADC ile bir termistörün veya ısı sensörünün çıkışını, bir potansiyometreyi, bir gaz sensörünü okuyup bu değeri dijital veriye çevirip işleyebiliriz.

* Atmega328’deki ADC biriminin genel özellikleri-10-bit Çözünürlük – 13-260 mikrosaniye çeviri zamanı -Saniyede  76.9K ölçüm \(saniyede  15k ölçüm \(10bit\)\) – Sıcaklık Sensörü giriş kanalı – 0-VCC Giriş gerilimi – Seçilebilir 1.1v referans gerilimi – Serbest çalışma veya tek ölçüm modu – Ölçüm bitince kesmeye giriş özelliği – Uyku modunda gürültü engelleme

Şimdi teknik veri sayfasındaki ADC blok diyagramına bir göz atalım.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/adcblok.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/adcblok.png)

Resim: ATmega328P – Microchip Technology , sf. 306,  http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018

Burada dikkatimizi çeken 3 adet yazmaç bulunuyor. Bu üç yazmaç ile bütün ADC işlemlerini yapıyoruz. ADC için gerilim referans bölümü ise dahili veya harici gerilim referansını belirlemekle görevlidir. ADC’den alınan çıkış ADC Veri Yazmacında bulundurulur. ADC Control ve Status Register \(ADC Denetim ve Durum Yazmacı\) ise ADC’yi denetleme görevini üstlenir. Geriye kalan ADC multiplexer select \(ADC çoğaltıcı seçimi\) yazmacı ise hangi ayağın ADC olarak kullanılacağını belirler. Bu yazmaçların her bitinin görevlerini aşağıda açıklayacağız. Bu yazmaçlar anlaşılırsa ADC üzerinde işlem yapmayı anlamış olacağız.

ADC Okuma yapmak için şu adımları yerine getirmek gereklidir.

1. ADC Ayarlanır
2. ADC Faal Edilir
3. ADC Dönüşüm Başlatılır
4. ADC Data Yazmacından ölçülen değer okunur.

Not: Yazmaçlarda bir bite değer atamak için aşağıdaki maskeleme şablonu kullanılır

YAZMAÇADI \|= \(BITDEGERI&lt;&lt;BITADI\)

Yazmaçadı : Datasheet’den bulacağımız 8 bitlik yazmacın adıdır.

Bitdeğeri: 1 veya 0 değeri.

BitAdı: Yazmacın bitinin özel adı veya bitinin numarası. Bunu da datasheet’den bakacağız.

**OKUMA NASIL BAŞLAR?**

Okuma PRR \(Power Reduction\) yazmacının PRADC bitini 1 yaptıktan sonra ADCSRA yazmacının ADSC bitini 1 yaparak başlar. Okuma süreci boyunca bu bit 1 konumunda kalır. Eğer okuma sırasında farklı bir veri kanalı seçilirse adc mevcut kanalı okumaya devam eder ve bitirir. ADC çevrimi otomatik olarak diğer kaynaklar tarafından tetiklenebilir. Otomatik tetikleme ADCSRA yazmacının ADATE bitini HIGH yapmakla  faal hale gelir. Tetikleme kaynağı ADCSRB yazmacının ADTS bitine değer atamakla seçilir. Tetik sinyalinde pozitif kenar meydana geldiğinde ADC ön derecelendirici \(prescaler\) resetlenir ve ölçüm başlar. Bu metod adc dönüşümünü fix interval ile başlatır. Eğer trigger sinyali ölçüm bittiğinde de faal haldeyse yeni ölçüm yapılmaz. Eğer ölçüm sırasında başka bir pozitif kenar tetikleyici sinyalde oluşursa bu sinyaller göz ardı edilir. Bu kesme bayrağı SGREG.I biti kapalı iken veya spesifik kesmeler devre dışı iken bile hazır hale gelir. Bir çevirim kesmeye sebep olmadan da yapılabilir. Bunun için kesme bayrağı her yeni çevrimde sıfırlanmalıdır. ADC kesme bayrağını tetikleyici kaynağı yapmak çevrim biter bitmez yeni çevrim yapmaya sebep olur.  Serbest çalışma modu denilen bu yöntem ile sürekli adc okuması yapılabilir. İlk ölçüm ADCSRA. ADSC bitini 1 yaparak başlamak zorundadır.

**PRESCALER**

ADC frekansının sağlıklı ölçüm yapabilmesi için 50-200kHz aralığında olması gerekir. Bu aralıkta düşükfrekanslar daha yüksek doğruluk verir.  ADC prescaler’i CPU frekansına göre prescaler bitleriyle ayarlanmalıdır. ADCSTA ADPS bitleriyle bu ayarlanır . Prescaler ADCSRA.ADEN \(Adc Enable\) biti ile çalışır ve ADCEN biti 0 olana kadar çalışmaya devam eder ve ardından resetlenir.

#### ADC Yazmaçları

Yukarıda eski notlarımdan kopyaladığım biraz farklı dilde olan bir parafgraf vardı. Eğer yeterince karışık bulunduysa şimdi giriş ve çıkış işlemlerinde olduğu gibi önce yazmaçları anlatmakla işe başlayalım. ADC için üç yazmaç kullanıldığını önceden söylemiştik. Bu yazmaçlar üzerinde yapılan doğru işlem ve okuma ile biz bir bacağa giden analog sinyalin dijital veriye dönüştürülmüş halini elde edebiliyoruz. Onun için öncelikle yazmaçları tanımamız kodları anlamamız için muhakkak gereklidir. Şimdi yazmaçları sırasıyla anlatalım.

#### **ADMUX Yazmacı** 

Yazmacın görüntüsü aşağıdaki gibidir. Tıklayarak büyütebilirsiniz.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/ADMUX-1024x145.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/ADMUX.png)

Bit 7:6 – REFSn Referans Seçimi

Yazmacın REFS0 ve REFS1 adındaki bitleri analog ölçümdeki gerilim referansını seçmeye yarar. Bu bitlerin konumları ve bu konumların getirdiği özellik aşağıdaki tablodaki gibidir.

| REFS \[1:0\] |  Gerilim Referansı Seçimi |
| :--- | :--- |
| 00 |  AREF, İç Gerilim Referansı Kapalı |
| 01 |  AVcc AREF ayağındaki harici kondansatör ile |
| 10 | Rezerve \(Kullanım dışı\) |
| 11 | İç 1.1V gerilim AREF ayağındaki harici kondansatör ile |

Görüldüğü gibi biz gerilim referansını besleme \(5V\), Dahili 1.1V ve AREF ayağında uygulanacak gerilim olarak üç ayrı şekilde seçebiliyoruz. Arduino’da bu analogReference\(\) fonksiyonu ile oluyordu. Biz burada doğrudan mikrodenetleyicinin donanımına müdahale ediyoruz. Örneğin gerilim referansını AVcc yani besleme olarak seçmek istersek şöyle bir komut yazmalıyız.

**ADMUX \|= \(1&lt;&lt;6\);**  
**ADMUX &= ~\(1&lt;&lt;7\);**

Bu komut ADMUX yazmacının 6. bitini bir \(1\) 7. bitini ise sıfır \(0\) yapar. Tabloda 01 ile belirtilen AVcc referansı sağlanmış olur.

**Bit 5 ADLAR \(ADC Sola Hizalama\)**

ADLAR biti ADC Veri Yazmacındaki dönüşüm sonucunu sola hizalamaya yarar. Bu bit HIGH konumda sonuç sola hizalı iken normalde sonuç sağa hizalıdır. ADLAR bitinin değeri değişince doğrudan hizalama işlemi yapılır.

**Bit 3:0 – MUXn \(n = 3:0\) – Analog Kanal Seçimi** 

Bu bitler hangi ayaktan analog okumayı yapacağımızı seçmeye yarar. Sekiz kanal analog olduğundan bahsetmiştik. İşte bu bitlerin konumlarını değiştirerek bu kanalları seçiyoruz. Mesela ADC0 ayağından okuma yapmak istiyorsak MUX bitlerini 0000 yapmamız gerekir. Aşağıdaki tabloda MUX bitlerinin konumu ve bunların sonucu verilmiştir.

| MUX \[3:0\] |  Giriş |
| :--- | :--- |
| 0000 | ADC0 |
| 0001 | ADC1 |
| 0010 | ADC2 |
| 0011 | ADC3 |
| 0100 | ADC4 |
| 0101 | ADC5 |
| 0110 | ADC6 |
| 0111 | ADC7 |
| 1000 | Sıcaklık Algılayıcısı |
| 1001 | Rezerve |
| 1010 | Rezerve |
| 1011 | Rezerve |
| 1100 | Rezerve |
| 1101 | Rezerve |
| 1110 | 1.1V \(Vbg\) |
| 1111 | 0V \(GND |

#### ADCSRA Yazmacı \( ADC Kontrol ve Durum Yazmacı \)

Yazmacın görüntüsü aşağıdaki gibidir. Tıklayarak büyütebilirsiniz.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/adcsra-1024x140.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/adcsra.png)

**Bit 7 – ADEN : ADC Faal**  
Bu biti HIGH yapmak ADC’yi faal hale getirir. Sıfır yapınca ADC kapatılır. ADC’yi dönüşüm aşamasında kapatmak dönüştürmeyi sonlandırır.

**Bit 6 – ADSC: ADC Dönüşüm Başlatımı**

Tek dönüştürme modunda her bir çevirimi başlatırken bu biti HIGH yaparız. Serbest çalışma modunda ise ilk çevirimi yapmak için bu biti HIGH yapmamız gerekir. Çevirim yapmadan önce ADEN bitini HIGH konumuna getirmek sonra ADSC bitini HIGH konuma getirmek gerekir. Bunun ardından Analog-Dijital Çevirimi yaparız. ADSC biti bu çevirim boyunca HIGH konumunda kalır ve çevirim bittikten sonra tekrar LOW konumuna geçer. Bu bite sıfır değerini yazmanın bir işlevi yoktur.

**Bit 5 – ADATE : ADC Otomatik Tetikleyici Faal**

Bu bit HIGH yapılırsa, ADC’nin otomatik tetikleyici faal hale gelir. ADC çevirime seçili tetikleyici sinyalinin yükselen kenarında başlar. Tetikleme kaynağı ADC Tetikleme Seçim Bitleriyle seçilir. Bu bitler ADTS olup ADCSRB yazmacındadır.

**Bit 4 – ADIF : ADC Kesme Bayrağı**

Bu bit ADC çevirimi bittikten ve veri yazmaçları güncellendikten sonra HIGh konumunda olur. ADC çevirim bitme kesmesi, ADIE biti ve SREG yazmacındaki I biti HIGH konumda ise yürütülür. ADIF donanım tarafından eğer uyan bir kesme kullanma vektörü kullanılırsa LOW konumuna geçer. Bunun yerine ADIF bitine LOW yazmakla da bu bayrak sıfırlanır.

**Bit 3 – ADIE: ADC Kesme Faal**

Bu bit HIGH yapılırsa ve SREG kesmesindeki I biti de HIGh konumdaysa ADC Çevirim Bitme Kesmesi faal hale gelir.

**Bit 2:0 – ADPSn : ADC Ön derecelendirici seçimi**

Bu bitler sistem saat frekansı ile ADC’nin giriş saati arasındaki bölme faktörünü belirlemeye yarar.

| ADPS\[2:0\] | Bölme Faktörü |
| :--- | :--- |
| 000 | 2 |
| 001 | 2 |
| 010 | 4 |
| 011 | 8 |
| 100 | 16 |
| 101 | 32 |
| 110 | 64 |
| 111 | 128 |

Buraya kadar ADC hakkında bilgi verip yazmaçların bir kısmını tanıttık. Sıradaki yazıda yazmaçların geri kalanını anlatıp örnekler üzerinden devam edeceğiz.

**Kaynaklar**

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018  
Dökmetaş, Gökhan, “Arduino Eğitim Kitabı”, İstanbul, 2016

Kapak Resmi, https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/ATmega8\_01\_Pengo.jpg/1200px-ATmega8\_01\_Pengo.jpg

