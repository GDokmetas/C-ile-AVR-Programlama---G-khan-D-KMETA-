# Zamanlayıcı ve Sayıcılara Giriş \(Timer/Counter\)

Artık büyük konuları anlatmanın zamanı geldiği için şimdiye kadar anlattıklarımızı tekrar bir gözden geçirelim. Şimdiye kadar anlattıklarımız konuların yarısını bile olmasa dahi size bu konulardan daha büyük bir şey kazandırmış olmalıdır. Çünkü hiçbir zaman üst seviyeden sığ bir şekilde konuları geçmedik ve en gerekli yerleri üşenmeden anlattık. Yeri geldi kütüphanelerin fonksiyonlarını tek tek açıkladık yeri geldi bir kütüphanenin nasıl yazıldığını anlatmak için kodlarını tek tek açıkladık. Çoğu zaman teknik veri kitapçığındaki bilgileri buraya getirdik ve hepsini anlatmaya çalıştık.  Bu dersler yazılırken kısıtlı zamana sahip olduğum için imla, yazım hatası ve dikkatten kaçan noktalar bulunabilir. Bunu dile getirmenizde bir sakınca yoktur. Çünkü dersi yazar yazmaz yayınlayıp bir an önce sonraki derse geçiyorum. Bunun sebebi bütün kısıtlı zamanımı ders yazmaya ayırmak istememden dolayıdır. İlk dersi yazmaya başlamamızdan itibaren 10 gün geçmiş ve bu noktaya kadar toplamda 24 ders yazdık.

Şimdi işlediğimiz konuları madde madde gözden geçirelim,

* AVR mikrodenetleyicisini tanıttık ve gerekli derleyici, yazılım ve programcılardan bahsettik.
* Temel giriş ve çıkış özelliklerinin tamamını açıkladık.
* ADC birimini açıkladık
* USART birimini açıkladık
* LCD kütüphanesini açıkladık

Diğer konularla kıyaslayınca yolun daha başında olduğumuzu görüyoruz. Bizi yolun başında hissettiren konular ise Zamanlayıcılar ve Kesmelerden başkası değil. Bu konuları atladıktan sonra çeşitli iletişim, güç ve çevre birimi konuları kalıyor. Bunlar zamanlayıcılar ve kesmeler kadar mikrodenetleyicinin temel özellikleri içerisinde değildir.

Derslerin nasıl bir formatta olacağına başta hiç karar vermeyip kafama estiği gibi yazmaya başlamıştım. Böylece dersin formatı kendiliğinden ortaya çıktı. Derslerin formatına göre her konuyu şu sırada inceleyeceğiz.

* Anlatılacak konuya temel bir giriş yapılır.
* Anlatılacak konu teknik veri kitapçığından anlatılır.
* Anlatılacak konu hakkında örnek kod varsa açıklanır.
* Anlatılacak konu hakkında kütüphane varsa anlatılır.

Bu dersler sadece AVR dersi olduğu için C dili, elektronik, devre kurma vs. hakkında fazla bir şey anlatmayacağız. AVR dışında modülleri, entegreleri derse dahil etmeyeceğiz ve AVR’yi de mümkün olduğu kadar C dili için alt seviyede anlatacağız.

Şimdi zamanlayıcı ve sayıcılara giriş yapalım.

Zamanlayıcılar sadece mikrodenetleyicilerin dünyasında değil günlük hayatımızda da sıkça kullanılır. Hatta saat ve kronometrenin olması da şart değildir. İşlerimizi sabah, öğle, ikindi, kuşluk, akşam, gece, seher gibi pek çok vakte göre tayin ederiz. Mikrodenetleyiciler de zamana bağlı uygulamaları yerine getirmek için işlemci saatini kullanabilseler de bu oldukça verimsiz olacaktır. Çünkü işlemciyi diğer bütün işlemler için kullanırken aynı zamanda da zamanı saymakla meşgul etmek zorunda kalırız. Bunun için işlemciden ayrı olarak zamanlayıcı birimleri yer almakta ve işlemci bu birimleri gerektiğinde okuyarak işlerini bunlara göre yapmaktadır. Hatta istenildiğinde kesmeler de kullanılarak mikrodenetleyici hiç bununla meşgul olmadan sadece işle meşgul olabilmektedir. Birkaç mikrosaniyeden birkaç saate kadar uzayan bu zaman dilimi mikrodenetleyicinin ihtiyacı olan zamanlama işlemini fazlasıyla karşılamaktadır. Aynı zamanda bu birimlerin yüksek doğrulukta olması işleri güvenle bu birimlere teslim edebilmemizi sağlar.

Zamanlayıcıların temeli yazmaçlardır. Bu yazmaçlardaki veri sırayla artar ve bu yazmaçlar okunarak zamana bağlı işlemler yapılır. Mikrodenetleyicinin özelliklerini anlatırken iki ayrı tipte zamanlayıcı olduğundan bahsetmiştik. 8-bit ve 16-bit olmak üzere ikiye ayrılan bu zamanlayıcılardan 8-bit olanı 0-255 arası, 16-bit olanı ise 0-65535 arası değeri içinde saklayabilir. Ayrıca zamanlayıcılar sayıcı olarak kullanılabilse de bu sayıcı özelliği içeride farklı bir özellik olmayıp saat frekansını dışarıdan alıp ona göre değeri artırmakla olur. Zamanlayıcıların yazmaçları azami değere geldiği zaman taşar ve tekrar sıfırlanır. Atmega328P mikrodenetleyicisinde TC0, TC1 ve TC2 olmak üzere üç adet ayrı zamanlayıcı bulunur. Bunlardan ikisi \(TC0 ve TC2\) 8-bit olup biri \(TC1\) 16 bittir. Zamanlayıcının en önemli özelliği yukarıda bahsettiğimiz gibi işlemciden bağımsız olmasıdır. Eğer işlemciye bağımlı bir zamanlayıcı olsaydı doğrulukta büyük sıkıntı çekilirdi. Çünkü işlemci başka kodları işlemekle meşgul olmaktadır ve aynı anda iki kodu işleyememektedir. Atmega328P’de bulunan 3 zamanlayıcının özelliklerini verelim ve diğer konuları sonraki derse bırakalım.

**TC0 – 8-bit Zamanlayıcı/Sayıcı \(PWM destekli\)**

* İki bağımsız çıkış karşılaştırma ünitesi
* Çift tamponlu çıkış karşılaştırma yazmacı
* Karşılaştırma eşleşmesinde zamanlayıcıyı sıfırlama
* PWM \(Pulse Width Modulator\)
* Ayarlanabilir PWM Periyodu
* Frekans üreteci
* Üç adet bağımsız kesme kaynağı \(TOV0, OCF0A ve OCF0B\)

**TC1 – 16-bit Zamanlayıcı/Sayıcı \(PWM destekli\)**

* Gerçek 16-bit tasarım \(16-bit PWM’yi destekler.\)
* İki adet bağımsız çıkış karşılaştırma ünitesi
* Çift tamponlu çıkış karşılaştırma yazmacı
* Bir adet giriş yakalama ünitesi
* Giriş yakalaması gürültü önleyici
* Karşılaştırma eşleşmesinde zamanlayıcıyı sıfırlama
* PWM desteği
* Ayarlanabilir PWM periyodu
* Frekans üreteci
* Harici olay sayıcı
* Bağımsız kesme kaynakları \(TOV, OCFA, OCFB ve ICF\)

**TC2 – 8-bit Zamanlayıcı/Sayıcı PWM ve Asenkron Çalışma Destekli**

* Kanal Sayıcı
* Karşılaştırma eşleşmesinde zamanlayıcıyı sıfırlama
* PWM
* Frekans üreteci
* 10-bit saat ön derecelendiricisi
* Taşma ve eşleşme kesme kaynakları \(TOV2, OCF2A ve OCF2B\)
* Harici saat kristali bağlanabilir \(32kHz\)

Kaynaklar:

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

