# ADC Kullanımı ve Örnek Kodlar

Önceki yazılarda ADC birimini anlatmış ve ADC’nin görevlerinden bahsetmiştik. Teknik veri kitapçığında yer alan yazmaçları anlatmış ve bu yazmaçların ve yazmaçlardaki bitlerin görevlerini ayrıntılı açıklamıştık. Şimdi ise C dilinde bu yazmaçları nasıl kontrol ederek ADC birimini çalıştıracağımızı anlatalım.

ADC işlemini adım adım anlatmak gerekirse şu şekilde olmalıdır,

* Seçili ADC kanal ayağını giriş olarak tanımlamalıdır.
* AVR’nin ADC modülü faal hale getirilmelidir. Çünkü açılış resetinde güç tüketimini önlemek için devre dışıdır.
* Çevirim hızı seçilir. ADPS2:0 bitleri bu işlemi yapar.
* ADC giriş kanallarındaki gerilim referansı seçilir. ADMUX yazmacındaki REFS0 ve REFS1 bitleri referans seçmek için görevlidir. ADMUX yazmacındaki MUX4:0 bitleri ise ADC giriş kanalını seçmeye yarar.
* ADCSRA yazmacındaki ADSC biti HIGH yapılarak çevirim başlatılır.
* ADCSRA yazmacındaki ADIF yani ADC kesme bayrağı biti kontrol edilerek çevirimin bitmesi beklenir.
* ADIF biti HIGH konuma geçtikten sonra ADCL ve ADCH yazmaçlarındaki dijital veri çıkışı okunur. Öncelikle ADCL yazmacını sonrası ADCH yazmacını okumak gereklidir.

İşte AVR mikrodenetleyicilerde ADC okuma işlemleri bu şekilde olmaktadır. Elimizin altında bir makina var ve bu makinayı talimatına uygun kullanıp analog değerden dijital değeri elde etmemiz gereklidir. Bu talimatları üretici bize bildirmektedir ve bunun mantığını ve iç yapısını bu noktadan sonra anlatmamız pek mümkün değildir. Çünkü üretici mikrodenetleyici ve çevre birimlerinin bizim için gerekli kısmını teknik veri sayfasında paylaşmaktadır. Daha alt seviyeye inerek konu dışına çıkmış oluruz. İnşallah ileride mikrodenetleyici mimarisini ve dijital elektroniği anlattığımız derslerde buna değineceğiz. Şimdilik yazmaç boyutuna insek dahi biraz ezbere işlem yapmak zorundayız.

Şimdi elimdeki örnek kodlardan dersi anlatmaya devam edelim. C dilinde oluşturduğumuz iki fonksiyonumuz var. Bunlardan biri ADC’yi başlatan ve diğeri ise ADC çevirimi yapıp sonucu geri döndüren asıl fonksiyon olarak görev yapıyor. Öncelikle adc\_init\(\) fonksiyonunu yazmaçlar üzerinden anlatarak nasıl çalıştığını anlatalım.

```c
void adc_init(void){
 ADCSRA |= ((1<<ADPS2)|(1<<ADPS1)|(1<<ADPS0));   // 125Khz ADC referans saati
 ADMUX |= (1<<REFS0); // Referans AVCC yani 5V
 ADCSRA |= (1<<ADEN); // ADC'yi Aç
 ADCSRA |= (1<<ADSC); // İlk deneme ölçümünü yap ve diğer ölçüme hazır hale getir. 
}
```

**ADCSRA \|= \(\(1&lt;&lt;ADPS2\)\|\(1&lt;&lt;ADPS1\)\|\(1&lt;&lt;ADPS0\)\);** Bu komutta ADCSRA yazmacının ilk üç biti olan ADPS0, ADPS1 ve ADPS2 bir \(1\) yapılır. ADC saati için ön derecelendirici ayarı ile 16Mhz/128 yani 125KHz ADC referans saati ayarlanmış olur. İşlemci 16MHz’de çalışırken ADC 50kHz ile 200kHz arası çalışmaktadır. O yüzden işlemcinin saat hızı ön derecelendirici \(prescaler\) ile bölünmelidir. Yüksek frekans hızlı ölçüm fakat düşük doğruluk verir. Düşük frekans ise yavaş ölçüm ve yüksek doğruluk. İhtiyaca göre bölme oranı belirlenmelidir.

**ADMUX \|= \(1&lt;&lt;REFS0\);** ADC’nin hangi gerilim değerini esas alarak ölçüm yapacağını belirler. ADC yazmaçlarını anlatırken 1.1V, Harici ya da Besleme olarak üç ayrı referans gerilimi alabileceğinden bahsetmiştik. Burada REFS0 bitini bir \(1\) yaparak referans gerilimini besleme gerilimi yapıyoruz. Yani 0-5V arası 0-1023 farklı değer elde edeceğiz.

**ADCSRA \|= \(1&lt;&lt;ADEN\);** ADC’yi açıyoruz. ADC açılıştan itibaren kapalı olup boş yere çalışmaz. O yüzden ADC’yi kullanmak üzere şimdi açıyoruz. Bu bit bir makinanın açma kapama düğmesi gibi düşünülebilir. ADEN bitini bir \(1\) yaparak ADC’yi çalışır hale getirdik.

**ADCSRA \|= \(1&lt;&lt;ADSC\);**  ADSC biti bir \(1\) yapılarak bir deneme çevirimi başlatılır. Buradaki amaç birimi bir sonraki çevirime hazırlamaktır. İlk çevirim yavaş olduğu için ilk çevirimi burada yaptık.

```c
uint16_t read_adc(uint8_t channel){
 ADMUX &= 0xF0; // Eski kanal bilgisini temizle
 ADMUX |= channel; // Yeni kanal bilgisini yükle
 ADCSRA |= (1<<ADSC); // Yeni Çevirim Başlat
 while(ADCSRA & (1<<ADSC)); // Çevirim bitene kadar bekle (bu kısım çok önemli)
 return ADCW; // ADC çevirim değerini geri döndür.
}
```

**ADMUX &= 0xF0;**  ADMUX yazmacının gerekli kısımlarını bir sonraki kanal bilgisi yazılmak üzere temizliyoruz. Her çevirimden önce hangi ayaktan ADC bilgisinin okunacağı belirlenmelidir. Bunu da fonksiyon byte değerinden \(uint8\_t\) argüman alarak bir sonraki kodda işletecektir.

**ADMUX \|= channel;**  ADMUX yazmacına kanal numarası\(yani ayak numarası 0, 1, 2.. vd.\) yazılır. Böylelikle hangi ayaktan analog okuma yapacağımızı belirlemiş oluruz.

**ADCSRA \|= \(1&lt;&lt;ADSC\);**  ADCSRA yazmacındaki ADSC biti bir \(1\) yapılarak yeni çevirim başlatılır. Artık çevirimin bitmesini ve değer okumayı bekleyeceğiz.

**while\(ADCSRA & \(1&lt;&lt;ADSC\)\);**  ADCSRA yazmacındaki ADSC bayrak biti çevirim bitince bir \(1\) konumuna geçer. O yüzden bunun bir olmasını bekleyene kadar mikrodenetleyiciyi döngüye sokuyoruz.

**return ADCW;**  Çevirim bittiğini bayrak bitinden öğrendikten sonra ADCW yani ADCH ve ADCL yazmaçlarının toplamı olan veriyi değer olarak döndürüyoruz. ADCW’in açılımı ADC Word demektir yani 16-bit veriden oluşur. Bu mikrodenetleyicinin teknik veri sayfasında verilmese de kodda yer almaktadır. Bu olmasaydı önce ADCL yazmacındaki veriyi okuyup sonra da ADCH yazmacındaki veriyi 8-bit sola kaydırıp ADCL yazmacındaki veriyi attığımız word tipindeki değişkene atabilirdik.  ADC verisinin atılacağı değişken muhakkak 8 bitten büyük olmalıdır. Bu int \(word\) olabilir.

ADC hakkında anlatacaklarımızın hepsi bu kadarla sınırlı olmasa da bu kadarı şimdilik yeterlidir. Geri kalanı ilerleyen derslerde başka konular içinde veya ayrı bir konu olarak anlatabiliriz. Şimdilik ADC hakkında yeterli bilgiyi verdiğimize inanıyoruz. Bir sonraki derste görüşmek üzere.

**Kaynaklar**

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 19.08.2018

The ADC of the AVR Analog to Digital Conversion, http://maxembedded.com/2011/06/the-adc-of-the-avr/, Erişim Tarihi: 19.08.2018

ADC \(Analog To Digital Converter\) of AVR Microcontroller, http://extremeelectronics.co.in/avr-tutorials/using-adc-of-avr-microcontroller/, Erişim Tarihi : 19.08.2018

