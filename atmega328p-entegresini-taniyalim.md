# Atmega328P Entegresini Tanıyalım

Arduino UNO kartı üzerinde bulunan Atmega328P dersler boyunca kullanacağımız ve onun üzerinden dersleri anlatacağımız mikrodenetleyici olacaktır. O yüzden programlamaya girmeden önce mikrodenetleyiciyi teknik veri kitapçığından \(datasheet\) faydalanarak anlatalım. Her entegre devrenin üretici tarafından yayınlanan ve bize lazım olacak her bilgiyi içeren teknik veri kitapçığı vardır. Bir mikrodenetleyiciyi veya işlemciyi programlamak için bu teknik veri kitapçığına ihtiyaç duyarız.  Assembly dilinde programlama yapmayacağımız için komut seti kitapçığına ve diğer alt seviye bilgiye ihtiyacımız olmasa da yazmaçlar ve bunların özelliklerini muhakkak öğrenmemiz lazımdır. C dilinde programlama yapmamız bu kitapçıktaki bilgilerin tamamını öğrenmek zorunda bırakmıyor fakat büyük bir kısmını yine de öğrenmemiz gerekli. Teknik veri kitapçıklarının neredeyse tamamı İngilizce yazılmaktadır ve bunu geliştiricinin okuyup anlaması gereklidir. Biz burada işi biraz kolaylaştırarak teknik veri kitapçığında gerekli yerleri Türkçe olarak anlatmaya çalışacağız. Böylelikle dil engelini elimizden geldiği kadarıyla ortadan kaldıracağız.

**Atmega328 Özellikleri**

* 131 işlemci komutu
* Çoğu komut tek çevirimde çalışır. \(Single cycle\)
* 32×8 Genel kullanım yazmacı
* 20MHz’e kadar saat hızı
* Entegre 2 çevirim çarpıcı \(multiplier\)
* 32KB Programlanabilir FLASH program hafızası
* 1KB EEPROM
* 2KB Dahili SRAM Bellek
* 10.000 Flash / 100.000 EEPROM okuma-yazma kapasitesi
* 85 derecede 20 yıl, 25 derecede 100 yıl veri koruma.
* Kilit Bitleri ve Kod koruma
* İki adet 8-bit zamanlayıcı ve sayıcı ölçeklendirme ve karşılaştırma modlarıyla.
* Bir adet 16-bit zamanlayıcı ve sayıcı ölçeklendirme, yakalama ve karşılaştırma modlarıyla.
* Gerçek zaman sayıcı ayrı osilatör ile
* Altı PWM kanalı
* 6 kanal 10-bit ADC, Sıcaklık ölçümü
* İki adet ana/uydu SPI Seri Arayüz
* Bir adet programlanabilir Seri USART
* Bir I2C arayüzü
* Programlanabilir WDT
* Bir Analog Karşılaştırıcı
* Ayağa bağlı uyandırma ve kesme
* Açılışta reset ve kararma \(brown-out\) saptaması
* Dahili ayarlı osilatör
* Harici ve Dahili Kesme Kaynakları
* Altı uyku modu
* 23 programlanabilir giriş ve çıkış ayağı
* 1.8 – 5.5 V arası çalışma gerilimi
* -40 ve +105C arası çalışma sıcaklığı
* Aktif modda 0.2mA Power-down modunda 0.1uA
* Power-save  modunda 0.75uA
* Güç tüketimi. \(Buna diğer unsurlar dahil değildir. \)

Atmega328p mikrodenetleyicisinin özellikleri aşağı yukarı yukarıdaki kadardır. Bazı özellikler çok kullanılan özellikler olmayıp bazıları da muhakkak bilinmesi gereken özelliklerdir. Dersler boyu konuların önemine göre her konuya farklı ağırlık vereceğiz. Şimdi yukarıdaki özelliklerin bir kısmını açıklayalım.

**İşlemci Komutu**: İngilizcesi “Instruction Set” olan bu tabir işlemcinin kaç komutla çalıştığını ifade eder. Daha fazla komut sayısı öğrenmeyi zorlaştırsa da komut fazlalığından dolayı işleri daha az komutla yapmayı sağlar. Az komut ise öğrenmesi daha kolay fakat karmaşık işlerde komut fazlalığı ve yazılması daha zor bir program gibidir. 1000 kelimelik bir dil ile 10000 kelimelik bir dil gibi düşünebiliriz bu durumu. Biz C dilinde programlama yapacağımız için işlemci komutlarıyla pek işimiz olmayacak. Eğer Assembly dili üzerinde çalışsaydık bunları öğrenmemiz gerekecekti.

**Genel Kullanım Yazmacı**

İşlemci ile RAM bellek arasında bilgisayarlarda Cache bellek olduğu gibi mikrodenetleyicilerde de yazmaçlar vardır. Bu yazmaçların bazıları genel kullanıma ayrılmıştır. Assembly dilinde programlama yapanlar bu yazmaçlar üzerinden verileri alıp işleyip kaydederek programlamayı yaparlar.

**Saat Hızı**

İşlemcinin çalışma hızı demektir. Fakat bu işlem hızı anlamına gelmez. PIC mikrodenetleyiciler bir komutu işlemek için 4 çevirime ihtiyaç duyar. İşlem hızı da saat hızının dörtte birine denk gelir. AVR’de bu genellikle 1/1 oranındadır. Mikrodenetleyiciye bağlayacağımız osilatöre göre saat hızını belirleriz.

**Flash Program Hafızası**

Bilgisayarda yazdığımız ve derlenip .hex dosyasına dönüşen kodun yüklendiği hafızanın adıdır. Sadece program yüklenebilmektedir ve çalışma esnasında bir erişim söz konusu değildir. Bu aynı boş bir CD’ye veri yazıp kullanmak gibidir. Programlama esnasında program hafızasına program kodundan başka değerler de yazabilsek de çalışma esnasında bu değerler sadece okunabilir hale gelir. Atmega328P’de ön yükleyici özelliği sayesinde program hafızasının bir kısmını silinmeden diğer bir kısmına silme ve yükleme işlemi yapılabilir.

**SRAM**

Bilgisayarlardaki RAM bellek ile aynı görevi görür. Bizim algılayıcılardan, kullanıcıdan aldığımız değer bu belleğe atılır. Ayrıca değişkenlerin değeri de RAM bellekte tutulur. RAM bellekteki değerler elektrik kesildiğinde silinmektedir. Çalışma esnasında gerekli olan değerler burada tutulur. Ram bellekler ROM bellekler gibi belli bir okuma ve yazma ömrüne sahip olmadığı için sürekli okuma ve yazma yapılan çalışma esnasında RAM bellek kullanılır.

**EEPROM**

Program hafızasından ayrı olarak kalıcı verilerin saklandığı bellektir. Bu belleğin özelliği önceden programlamaya ihtiyaç duyulmadan çalışma esnasında kalıcı verilerin saklanmasına imkan sağlamasıdır. Program hafızasına çalışma esnasında kayıt olmadığı için mikrodenetleyici ek bir kalıcı belleğe ihtiyaç duyar. EEPROM bu ihtiyaç sonucu mikrodenetleyicilere eklenmiştir. Örneğin ayar değerleri, şifre, ip adresi gibi veriler kullanıcı tarafından girilir ya da değiştirilir ve bunlar EEPROM’da saklanır.

**Zamanlayıcı ve Sayıcı** 

Bunu ayrı zikretmemiz bunları ayrı birer birim olduğu izlenimi vermesin. Mikrodenetleyicinin iç saatine göre sayma işlemi yapıldığında zamanlayıcı, dış kaynaktan sayma yapıldığında ise sayıcı olarak adlandırılır. Mikrodenetleyici içerisinde bir saatin olması zamana bağlı işlerde merkezi işlem birimini \(CPU\) meşgul etmeden harici olarak sayma işlemini yapmaya imkan verir. Mikroişlemciye bekleme komutu verip belli bir zaman kullanılamaz hale getirmek yerine zamanlayıcıdan belli aralıklarla okuma yapıp o değere göre işlemi yapmak zamana bağlı işlemlerde mikrodenetleyiciyi gereksiz meşguliyetten kurtaracaktır. Sayıcı görevinde ise dışarıdan gelen frekansa göre değer artar ve belli bir süre sonra taşma yapar. Bit değeri kapasite bakımından önemlidir. 8-bit bir sayıcıda sadece 255’e kadar sayma işlemi yapılmaktadır o yüzden çok sık aralıklarla kontrol edilip sıfırlanması gerekebilir. 16-bit zamanlayıcı bizi bu konuda biraz daha rahat bırakmaktadır.

**PWM**  
PWM \(Pulse-Width Modulation\) dijital devrelerin sadece HIGH ya da LOW değeri vermesinden dolayı getirilmiş bir çözümdür. Dijital devrelerin analog gibi çıkış vermesini sağlar. Bu analog gibi çıkış belli bir frekanstaki kare dalganın yüksek kenarının genişliğinin miktarı ile belirlenir. Aşağıdaki resimde PWM’nin çalışma prensibine bir örnek verilmiştir.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/main-qimg-284e6cd1504b56902da50778cd233c9a.png)

Resim: https://qph.fs.quoracdn.net/main-qimg-284e6cd1504b56902da50778cd233c9a

**ADC**

Analog-Digital  Converter ifadesinin kısaltılmış halidir. Yani Analog değeri okuyup dijital veriye dönüştüren birimdir. Analog değer mikrodenetleyicinin çalışma gerilimi ve 0V arası ya da referans değeri ile 0V arası olabilir. Bu değer doğru orantıya çok yakın olarak dijital değere çevrilir.  0-5V arası 10-bitlik bir dönüşümde 0-1023 arası değerlere dönüştürülür.

İletişim protokollerine geldiğimizde iletişim protokollerinden ayrıntılı olarak bahsedeceğimiz için şimdilik burada bırakalım. Bir sonraki yazıda görüşmek üzere.

**Kaynaklar**  
ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 16.08.2018  
Kapak Resmi:  
https://d3s11pzv7w3h1q.cloudfront.net/wp-content/uploads/IC-ATTINY13A-PU-2.jpg

