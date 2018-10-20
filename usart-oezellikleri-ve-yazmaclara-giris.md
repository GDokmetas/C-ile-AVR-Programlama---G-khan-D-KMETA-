# USART Özellikleri ve Yazmaçlara Giriş

ADC birimini yazmaçlar üzerinden anlattığımız gibi USART birimini de yazmaçlar üzerinden anlatacağız. Yazmaçlar yapısı itibariyle “şu şu ile yarar, bu bu işe yarar” şeklinde anlatılıp ezberci bir anlatımla bitirilse de yazmaçlar öğrenildikten sonra bütün kodları ve kütüphanelerin çalışma prensibi çok rahat anlaşılabilir. Yazmaçlar öğrenilmediği durumda kodları ve kütüphane fonksiyonlarını anlamadan ezberlemek zorunda kalırız bu durum da ezbere ve anlamadan kod yazmaya sebep olur. Günümüzde ezbere kod yazan programcıların aşırı derecede fazla olmasının sebebi kaynakların ezbere ve yüzeysel bir anlatımla konuyu geçiştirmesinden dolayıdır. Bu anlatımın yüzeysel olmasının en büyük sebebi yabancı olan kaynakların Türkçe’ye aktarımında kırpma ve özensiz Türkçeleştirme yapılmaktadır. Örneğin yabancı dilde 2-3 sayfada anlatılan bir Arduino projesi Türkçe’ye aktarılırken iki paragrafta sığ bir  dille anlatılıp kod ve şemadan ibaret ve bilgi verme yönünden oldukça yetersiz bir hale geliyor. Bu projeyi ezbere yapmak öğrenmeye bir katkı sağlamaz. Orjinal içerik yazılması olmazsa olmaz şartlardan değil. Türkçeleştirilmiş bir yabancı içeriğin düzgün ve eksiksiz Türkçeleştirilmesi bile Türkçe kaynak açısından büyük bir ilerleme olacaktır. Buradan içerik yazarlarına tavsiyemizi verdikten sonra Atmega328P entegresinin USART biriminin özelliklerini anlatmaya başlayalım.

**USART Özellikleri** 

* Tam Dubleks Çalışma \( Bağımsız Seri Alıcı ve Verici Yazmaçları\)
* Asenkron veya Senkron \(Eş zamanlı\) çalışma
* Ana ya da Uydu Saat sinyali ile senkron çalışma
* Yüksek çözünürlüklü veri iletim hızı jeneratörü
* 5, 6, 7, 8 ya da 9 Data biti desteği ve 1 ya da 2 stop biti desteği
* Tek ya da Çift Eşitlik oluştırma ve donanım destekli eşitlik biti denetleme.
* Data OverRun Algılaması
* Framing hatası Algılaması
* Yanlış başlangış biti algılaması ve dijital düşük geçiş filtresi ile gürültü filtreleme.
* TX bitişi, TX Veri yazmacı boş ve RX bitişi olarak üç ayrı kesme özelliği
* Çoklu işlemci iletişim modu
* Çift hızlı asenkron iletişim modu

**Blok Diyagramı** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/usart.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-20-usart-ozellikleri-ve-yazmaclara-giris/attachment/usart/)

Resim: ATmega328P – Microchip Technology , sf. 226 , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018  _\(Resme tıklayarak tam çözünürlükte görebilirsiniz.\)_

Diyagramı incelediğimizde üç ana bölüme ayrıldığını görüyoruz. Clock Generator \(Saat sinyali üretici\), Trasmitter \(Verici\), Receiver \(Alıcı\) olarak üçe ayrılan bu bölümlerden hariç bazı yazmaçların da veri yoluna bağlandığını görüyoruz. Ayrıca sağ kenarda XCKn, TxDn, RxDn olarak adlandırılan üç adet bölüm mikrodenetleyicide yer alan seri iletişim ayaklarını temsil ediyor. Buradan USART’ın dış dünya ile bağlantısının bu üç ayaktan sağlanacağını çıkarabiliriz. Bu bacakların mikrodenetleyici kılıfındaki kaç numaralı ayağa denk geldiğini önceki yazılarda verdiğim mikrodenetleyicinin ayak haritasından görebilirsiniz.

Saat sinyali üretici hem XCKn ayağından saat frekansı çıkışında hem de alıcı ve verici ünitelerinin ölçüm yapması için veri okuma hızının belirlenmesinde görevlidir. Saat sinyali üretecinin UBRRn yazmacı ile ayarlandığını görüyoruz.

Verici ünitesinde ise UDRn yazmacı dikkatimizi çekiyor. Verici ünitesindeki bu tek yazmaç veri yoluna bağlıdır. Alıcı ünitesinde ise yine tek yazmaç bulunup veri yoluna bağlıdır. Buradan bunların veri alışverişinde kullanılacak yazmaçlar olduğunu anlıyoruz.

Bundan başka UCSRnA, UCSRnB ve UCSRnC adında üç ayrı yazmaç da veri yoluyla USART bloğuna bağlı görünüyor. Bu yazmaçların görevlerini de aşağıda açıklayacağız.

**UDR0 – USART Giriş ve Çıkış Veri Yazmacı**

Bu yazmaç USART veri iletiminde tampon bellek \(buffer\) görevini görür. Alıcı ve verici veri yazmaçları aynı adresteki giriş ve çıkış yazmacını paylaşır ve bu yazmaç da UDR0 olarak adlandırılır. Verici Veri Tampon Yazmacı \(TXB\) UDR0 yazmacına yazılacak veriyi hedef alacaktır. UDR0 yazmacını okumak da alıcı veri tampon yazmacının içeriğini bize verecektir. Verici tampon verisi ancak UCSR0A yazmacındaki UDRE0 bayrak bitinin bir \(1\) olması ile yazılabilir. Verici tamponuna veri yazıldığında verici faal hale gelmiş olur. Verici değiştirme yazmacına \(Transmit Shift Register\) boş olduğu zaman verici veriyi yükler. Sonrasında veri seri olarak TxD0 ayağından gönderilir. Yazmacın görüntüsü şu şekildedir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-20-usart-ozellikleri-ve-yazmaclara-giris/attachment/1/)

Burada gönderim ve alım için ortak kullanılan ve okuma/yazma yapılabilen 8 bitlik bir yazmaç görüyoruz. Yapacağımız okuma ve yazmalar bu yazmaç üzerinden olacaktır. Diğer yazmaçlar ise iletişimin nasıl olacağına dair ayarları içeren yazmaçlardır. Yazmaçların fazlalığından biraz karmaşık görünse de prensipte oldukça basittir. Şimdi diğer yazmaçları anlatmakla devam edelim.

**UCSR0A – USART Denetleme ve Durum Yazmacı 0 A**

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-20-usart-ozellikleri-ve-yazmaclara-giris/attachment/2/)

**Bit 7 – RXC0 : USART Veri alımı tamamlandı**

Bu bayrak biti tampon bellekte okunmamış bir gelen veri olduğunda bir \(1\) konumuna geçer. Tampon bellekte veri kalmadığı zaman ise tekrar sıfır \(0\) konumuna geçer. Ayrıca Seri veri alım kesmesini gerçekleştirmek için de bu bit kullanılır. Kısaca bu bayrak bite bize verinin gelip okunmayı beklediğini haber verir.

**Bit 6 – TXC0 : USART Veri Gönderimi Tamamlandı**

UDR0 yazmacındaki tüm bitler gönderildiği zaman bu bit bir \(1\) konumuna geçer. TXC0 bayrak biti eğer gönderim tamamlanma kesmesi yürürse otomatik olarak sıfır \(0\) konumuna geçer. Ayrıca elle de bu biti sıfırlama imkanımız vardır. Bu bayrak biti USART gönderim tamamlanma kesmesini yürütmek için kullanılabilir.

**Bit 5 – UDRE0 : USART Veri Yazmacı Boş** 

Bu bit gönderici tamponunun yani UDR0’ın yeni veri almaya müsait olduğunu belirtir. Eğer UDRE0 biti bir \(1\) konumunda ise tampon bellek boştur. Ayrıca bu bayrak biti Veri yazmacı boş kesmesini yürütmek için kullanılabilir.

**Bit 4 – FE0 : Çerçeve Hatası**

Bu bit alıcı tamponunun sonraki karakteri çerçeve hatasına sahipse bir \(1\) konumuna geçer.

**Bit 3 – DOR0: Veri Aşma**

Bu bit veri aşma tespit edilirse bir \(1\) konumuna geçer. Bir veri aşma hatası alış tamponu dolu olduğunda gerçekleşir.

**Bit 2 – UPE0: USART Eşlik Hatası**

Bu bir eşlik hatası gerçekleştiğinde bir \(1\) konumuna geçer.

**Bit 1 – U2X0: USART Çift Gönderim Hızı**

Bu bit ancak asenkron çalışmada etkili olur. Senkron iletişimde bu bit bir \(1\) olmalıdır. Bu biti bir \(1\) yapmak veri aktarım hızı bölücüsünü 16’dan 8’e düşürür ve böylelikle transfer hızını ikiye katlar.

**Bit 0 – MPCM0: Çoklu işlemci iletişim modu**

Bu bit çoklu işlemci iletişim modunu faal hale getirir.

Sonraki yazımızda geri kalan yazmaçları açıklayacağız ve ardından örnek kodlar üzerinden iletişimin nasıl gerçekleştiğini anlatacağız. Teknik veri kitapçığından anlatmak oldukça yorucu olsa da bunları anlamadıkça kodları anlamak mümkün olmuyor. Aksi halde kod örneklerini verip bunları kopyalayın kullanın demekle yetinecektik. O yüzden gerçekten öğrenmek istiyorsanız  sabırla dersleri okuyup anlamanız gereklidir.

Kaynaklar:

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018

