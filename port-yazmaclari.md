# Port Yazmaçları

Bu derste geride kalan 8 dersin anlattığının tamamından daha önemli ve zor bir konuya giriş yapacağız. Artık kolay konuları geride bıraktık diyebilirim. Bu konu en önemli konulardan biri olduğu için tamamen anlaşılmadıkça diğer konulara geçilmemesi gerekir. AVR’de en sık yapacağımız giriş ve çıkış işlemleri aynı zamanda ilk yapacağımız işlem olacaktır. Önceki derste led yakma uygulaması örneğinde olduğu gibi temel giriş ve çıkışların kullanılmadığı uygulama yok gibidir. En basit uygulamadan en karmaşık uygulamaya kadar kullanmak zorunda kalacağımız temel giriş ve çıkış birimlerini anlamakla aynı zamanda yazmaçlar üzerinde işlem yapmayı da anlayıp diğer pek çok konuyu rahatça anlayabilirsiniz.

**Yazmaç \(Register\) Nedir?**

Sürekli bahsettiğimiz ama şimdiye kadar ayrıntılı olarak açıklamadığımız yazmaçlar küçük veri hücreleridir. Bu hücreler mikroişlemcinin parçalarından olup gerekli ayar, okuma, yazma ve çalışma verilerini içinde barındırır. Küçük veri hücrelerine yazılan bilgi mikrodenetleyicinin nasıl çalışacağını belirler. Aynı zamanda bu hücrelerden okunan bilgiler ile mikrodenetleyicinin işlemcisi işlem yapar. Mikrodenetleyicinin ayarlarını bu hücrelerdeki verilerin konumlarını değiştirerek yaparız. 8-bit mikrodenetleyicide 8-bitlik yazmaçlar bulunur. Bir yazmacın örnek bir görüntüsü aşağıdaki tablodaki gibidir,

| 0 | 0 | 1 | 0 | 0 | 1 | 0 | 1 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |


> Burada bir \(1\) ve sıfır \(0\) olarak temsil edilen rakamlar aslında gerçekte elektriğin var olup olmamasıdır. Elektrik tabanlı olmayan ROM, CD gibi aygıtlarda ise iki durumdan birinin karşılığıdır. Anlaşılması için biz 1 ve 0 olarak sayılarla ifade ediyoruz. bir \(1\) HIGH ve TRUE, sıfır \(0\) ise LOW ve FALSE olarak adlandırılır.

AVR mikrodenetleyicilerde bulunan bazı yazmaçların bazı bitleri belli ayarları yapmaya, bazı bitleri belli bilgileri okumaya, bazı yazmaçların kendisi veri tutmaya ayarlanmıştır. Bu yazmaçların ve bitlerinin görevleri teknik veri kitapçığında açıklanmıştır. Yazmaçların bitleri sağdan itibaren 0-7 arası rakamlandırılır. Programcı bir yazmacın değerini okur, yazmaca değer yazar, yazmacın bir bitindeki değeri değiştirir, yazmacın bir bitindeki değeri okur ve buna göre programlama yapar.  Bu yazmaçlar mikrodenetleyicinin çevre birimleri ile irtibatını, ayarlarını, denetimini sağlar. Yazmaçların bitlerine müdahaleyi C’nin bitwise operatörleri ile sağlarız. Genelde pek üzerinde durulmayan bitwise operatörler gömülü sistemlerde en önemli operatörlerden biridir.

Mikrodenetleyicinin teknik veri kitapçığına baktığımızda giriş ve çıkış ile alakalı üç ana yazmaç ve bununla alakalı bir ayar bitini bulunduran bir yazmaç karşımıza çıkıyor. Öncelikle ayar bitini bulunduran yazmacı açıklayalım. Bu yazmacı açıklamakla yazmaç mantığını biraz daha hızlı öğrenmenizi umuyoruz.

**MCU Control Register \( Mikrodenetleyici Denetim Yazmacı \)**

| Yazmacın Biti | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Bit Adı | —BOŞ— | BODS | BODSE | PUD | —BOŞ— | —BOŞ— | IVSEL | IVCE |
|  Erişim |  |  Y/O |  Y/O |  Y/O |  |  | Y/O | Y/O |

Bu yazmaç  mikrodenetleyicinin denetimi ile ilgili 5 adet ayarı içinde barındırır. Bu bitleri açık ya da kapalı yaparak ayarlamayı yapabiliriz. Her konumdaki bit özel bir ad ile adlandırılmıştır. Bu anlaşılabilirliği artırmak içindir. Programlarken yazmacın adı ise MCUCR olarak yazılmalıdır. Yazmacın adı ve bitlerin adları üretici tarafından belirlenmiştir ve programlarken teknik veri kitapçığına uyulmak zorunludur. Bu bitlere erişim ise yine üretici tarafından belirlenir. Bazıları sadece okunabilirken bazıları hem okunup hem yazılabilir. Y/O olarak belirtmemiz hem okunabilir hem yazılabilir olduğunu ifade etmek içindi. Şimdi bizi ilgilendiren PUD bitini açıklayalım.

**PUD \(Pull-Up Disable\)**  
Bu bit “1” yapıldığında tüm giriş ve çıkış portlarındaki dahili pull-up dirençleri devre dışı kalır.  DDxn ve PORTxn yazmaçlarında pull-up tanımlansa dahi bu işlem gerçekleşir. Pull-Up dirençlerini devre dışı bırakmayı kaldırmak için bu bit tekrar sıfır yapılır.

MCUCLR yazmacının PUD bitini \(4. bit\) nasıl bir veya sıfır yapacağımızı ileride açıklayacağım. Şimdilik sadece yukarıdakileri okuyup anlamak yeterlidir. Şimdi giriş ve çıkış için kullanacağımız üç yazmacı anlatalım.

**DDRx**  
Bu yazmaç giriş ve çıkış portunun giriş mi çıkış mı olacağını belirler. Bir programı yazarken program başında hangi portun hangi ayağının hangi görevde olacağını bu yazmaca atadığımız değer ile belirleriz. Mikrodenetleyiciye göre kaç adet yazmaç olacağı değişiklik gösterir. Giriş ve çıkış yazmaçları alfabedeki harflerin sırasına göre adlandırılır. DDRA, DDRB, DDRC.. gibi her portun ayrı tanımlama yazmacı bulunur. DDRx \(x burada portun harf adı\) yazmacının genel yapısı şu şekildedir.

| Yazmacın Biti | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Karşılık Geldiği Ayak | Px7 | Px6 | Px5 | Px4 | Px3 | Px2 | Px1 | Px0 |

Yazmacın portun ayağına karşılık gelen biri bir \(1\) yapıldığında o ayak çıkış sıfır \(0\) yapıldığında o ayak giriş olur. Örneğin D portunun 3 numaralı ayağını giriş olarak tanımlamak istiyorsak DDRD’nin 3 numaralı \(4. biti\) bitini sıfır \(0\) yapmamız gerekir. İşlemcinin 8-bit olması burada 8-bitlik port ve yazmaçlar olmasına sebep olmuştur. Yine bu yazmaçlara değer atarken bayt olarak değer atayıp değer okurken ise bayt olarak değer okumamız gerekir.

**PORTx**

AVR mikrodenetleyicilerde giriş ve çıkış yapılan bitler 8-bitlik port olarak bir araya toplanmıştır. Her bir ayağı tamamen bağımsız olarak kontrol etmemiz yazılımsal olarak mümkünse de ayakların kendisi tamamen porttan yani ayak grubundan bağımsız değildir. Bu ayakların 8’li gruplara ayrılmasının en önemli nedenlerinden biri mikrodenetleyicinin 8-bit olmasıdır fakat daha önemli bir neden tek başına bağımsız bir ayaktan alınan giriş ve çıkışın fazla birşey ifade etmeyişidir. Tek bir düğmenin durumunu okuma, tek bir led yakma gibi basit işlemler için tek bir ayak yeterli olsa da 7-segman gösterge sürücüsüne bir değer göndermek istediğimizde 4 bitlik paralel bir hatta ihtiyacımız vardır. LCD kullanırken de 8 ya da 4 bitlik paralel bir hat gereklidir. İşte bu 8-bitlik portlar aynı zamanda 8-bitlik paralel iletişim portlarıdır. Bu portlar 0 ile 255 arasında değer alabilir ve bu değerin ikilik sistemdeki karşılığını ayaklara yansıtabilir. Aynı zamanda bu 0 ve 255 arasında yani bir bayt büyüklüğünde değeri de porttan okuyabilir. Pek çok dekoder ya da sürücü entegresi paralel bağlantıya ihtiyaç duyduğundan giriş ve çıkış portları gelişmiş uygulamalarda oldukça gereklidir.

Arduino programlayanların hiç alışık olmadığı hatta çoğunlukla adını bile duymadığı portlar AVR’nin temelini oluşturan birimlerden biridir. Arduino’da tek ayak üzerinden yapılan işlemler bile arka planda AVR portları üzerinden yapılmaktadır. Portların 8’li ayak grubu olması tek bir ayak üzerinden işlem yapamayacağımız anlamına gelmez. Fakat doğrudan değil dolaylı olarak bu işlemi gerçekleştiririz. Bunun hiçbir dezavantajı yoktur fakat yeni başlayanların biraz anlaması uzun sürmektedir.  Örnek bir PORT yazmacının görünümü aşağıdaki gibidir.

| Yazmacın Biti | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Bulundurduğu Değer | 0 | 0 | 1 | 0 | 1 | 1 | 1 | 1 |

Örnek yazmacımızın adı PORTD yazmacı olsun. PORTD’nin 0. bitinin değeri 1 olduğu için mikrodenetleyicinin PD0 ayağına taktığımız led yanacaktır. Bunu söndürmek için PORTD yazmacının 0. bitinin değerini 0 yapmamız gerekir. Bunu da C dilinin bitwise operatörleri ile yapıyoruz. Bunu daha sonra anlatacağız.

**PINx**

Bu yazmaç dijital giriş için kullanılır. Yani ayaklardaki elektriksel durumu okumayı sağlar. Bu bir düğmenin basılı olup olmadığını okumaktan 8-bitlik bir paralel iletişim bağlantısını okumaya kadar uzanabilir. DDRx yazmacı ile giriş olarak tanımlanan port ya da ayaklardan doğrudan port veya pin okuma yöntemi ile yazmaçtan elde edilen değer sonrasında mikrodenetleyicinin hafıza birimlerine kaydedilir ve bu değer üzerinde işlem yapılarak çıkış birimlerine iletilir. Burada verinin okunduktan sonra nasıl kaydedileceği, işleneceği ve çıkış olarak verileceği programcının yazdığı programa bağlıdır. Genel olarak mikrodenetleyici programlamak çevre birimlerinden değerleri okuyup bu değerleri kaydetmek, kaydedilen değerleri işledikten sonra kullanıcıya çıkış olarak vermek diye özetlenebilir. Bir algılayıcıdan sıcaklığı okuyup hesapladıktan sonra lcd ekrana yazdırmak ya da düğmeye basmakla ses seviyesini artırıp bu değeri de güncelleyerek ekrana yazdırmak gibi durumlar buna örnektir. En temel değer okuma olan dijital giriş ise PINx yazmacı ile okunur. Yazmacı şekillendirerek anlatmak istersek şöyle bir tablo çizebiliriz,

| Yazmacın Biti | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Okunan Değer | 0 | 0 | 1 | 0 | 1 | 1 | 0 | 0 |

Yazmacımız PIND yazmacı olsun. DDRD yazmacının 0. bitini 0 yaparak o ayağı giriş olarak tanımlıyoruz. Sonrasında pull-up direnci ile bir adet düğme bağladıktan sonra o düğmeye bastığımızda o ayağın değeri şaseye bağlandığından dolayı 0 oluyor. PIND ile okuma yaptığımızda 0. ayaktan tablodaki gibi 0 değeri elde ediyoruz. Düğmenin basılı olmadığı durumlarda ise 1 değeri okunacaktı.

Yazmaçlar anlaşıldıysa sonraki derste yazmaçlar üzerinde işlem yaparak giriş ve çıkış işlemlerinin nasıl olduğunu kodlarla örnekleyip anlatacağız.

**Kaynaklar**  
ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 17.08.2018

Kapak Resmi:  
https://d3s11pzv7w3h1q.cloudfront.net/wp-content/uploads/IC-ATTINY13A-PU-2.jpg

