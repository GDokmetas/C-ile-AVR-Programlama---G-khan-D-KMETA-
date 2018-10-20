# Bit Tabanlı Giriş ve Çıkış

Önceki derste görüldüğü üzere portlara değer atamakla giriş ve çıkış işlemini yapıyorduk. Bir port yazmacına değer atayarak giriş ve çıkışı yapmak ancak bir portu bütünüyle kullanırken kullanışlı olabilir. Aksi halde sadece değer atayarak giriş ve çıkış kontrolünü sağlamak oldukça yetersiz kalır. Bunun için bitwise operatörler ve makroları kullanmaktayız. C dilinde bulunan bitwise operatörler bir değer üzerinde bit ve mantık işlemlerini yapmayı sağlar. Böylelikle bitlerden oluşan port yazmacına bit seviyesinde müdahale edebiliriz.

**Değer Atama Yoluyla Port Denetimi** 

Bu yol en basit yol olup “=” operatörü ile port yazmacına istediğimiz sabit bir değeri veya değişkeni atayabiliriz. 8-bitlik portun bütün bitleri atadığımız değere göre güncellenir. O yüzden ayrı ayrı değil bütünüyle portu ele almamız gerekir. Bu değer atama şu şekilde olabilir,

```c
PORTB = 0xFF; // PortB'nin tüm ayakları 1 (HIGH) olarak ayarlandı. 
PORTD = 0x00; // PORTD'nin tüm ayakları 0 (LOW) olarak ayarlandı. 
PORTE = 0b11110000; // E portunun ilk 4 ayağı 0 (LOW) son 4 ayağı 1 (HIGH) olarak ayarlandı. 
```

 Kayan ışık programı yapmak istediğimizde ise portlara her defasında sıfırdan değer atamamız gerekecektir. Yani sekiz bite de baştan değer atamak gerekecektir.

```c
PORTE = 0b00000001;
PORTE = 0b00000010;
PORTE = 0b00000100;
PORTE = 0b00001000;
PORTE = 0b00010000;
PORTE = 0b00100000;
PORTE = 0b01000000;
PORTE = 0b10000000;
```

Eğer istediğimiz bir led yanıyor ve öteki ledi de yakmak istiyorsak hem önceki ledi yakmak ve hem sonraki ledi yakmak durumunda kalacağız.  Eğer bir ledi yakmak isterken diğer atadığımız değerler öncekinden farklı olursa istenmeyen bir değişiklik meydana gelecektir. Bunun için diğer bitleri önemsemeyecek, bitleri kaydıracak, bitleri değiştirecek veya bitlere istenmedikçe dokunmayacak bir özellik gereklidir. Bunlar bitwise operatörleri ile sağlanır.

**Bitwise Operatörleri ile Port Manipülasyonu**

Bitwise Operatörler C ile gömülü sistem programcılığında olmazsa olmazlardandır. C dilinin alt seviye özelliklere sahip bir programlama dili olduğunu söylemeliyiz. Bu alt seviye özelliklerin başında ise işaretçiler \(pointer\) ve bitwise operatörler gelir. Bu alt seviye özellikler sayesinde daha alt seviye bir dile muhtaç olmadan hem de alt seviye dilde olmayan pek çok özelliğe sahip olarak pek çok gelişmiş program yazılabilir.  Günümüzde gömülü sistemlerde C dili bir numaralı dil olarak canlılığını hiç kaybetmeden kullanılmaktadır.  Şimdi bitwise operatörleri sayıp sonrasında örneklerle açıklayalım.

* &  : AND Operatörü
* \|  : OR Operatörü
* ^ : XOR Operatörü
* ~ : Tersleme Operatörü
* &lt;&lt; : Sola kaydırma Operatörü
* &gt;&gt; : Sağa Kaydırma Operatörü

Bu operatörler C’deki mantıksal operatörlerle karıştırılmamalıdır. Herhangi bir mantıksal karşılaştırma yapmayan bu operatörler sadece bitler üzerinde işlem yapar.

Bir porttaki bir ayağı bir \(1\) konumuna getirmek istiyoruz diyelim. Burada porttaki diğer ayakların durumunu bilmiyoruz ve bu ayaklara dokunulmadan sadece bizim belirttiğimiz bitte değişiklik yapılacak. Bunun için portun değeri ile bizim atadığımız değere OR  işlemi yaptırıp sonra tekrar porta atamamız gerekir. D portunun son birine bağlı bir ledi yakmak istiyorsak bunun için şöyle bir işlem yapmalıyız,

**PORTD = PORTD \| 00000001;**

Bu işlemi tabloda anlatmamız gerekirse. Tablodaki ilk satır D portunun ilk hali, ortadaki satır bizim atadığımız değer en son satır ise D portunun son halidir.

| 0 | 1 | 0 | 1 | 1 | 1 | 0 | 0 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| 0 | 1 | 0 | 1 | 1 | 1 | 0 | 1 |

Görüldüğü gibi D portunun diğer bitlerine herhangi bir müdahalede bulunulmayıp ilk bitine 1 biti ile OR işlemi uygulanmıştır. Bu işlemin doğruluk tablosuna göre sonuç 1 olması gerektiği için o bit 1 ile değiştirilmiştir. Eğer bit 1 olsaydı yine bir olacaktı. Sıfır yazdığımız yerlerde de sıfır veya bir olmasına bakılmadan aynı değer OR işlemi ile elde edilmiştir. Bu işlemde 1 yapmak istediğimiz yere 1 dokunmak istemediğimiz yere 0 yazıyoruz. Böylelikle atama operatöründeki gibi değerler tamamen değişmiyor. Arduino’da olduğu gibi tek bir bacağı HIGH ya da LOW yapmak için bir komut C dilinde yoktur. Mikrodenetleyicinin donanımında ise giriş ve çıkış tek bitlerden değil portlardan  oluşmaktadır.O yüzden böyle bir çözüm üretiyoruz.

Şimdi bu kodu bu kadar uzun yazmak yerine kısaltarak yazalım. C dilinde aritmetik operatörlerde nasıl kısaltma söz konusu oluyorsa bu bitwise operatörlerde de kısaltma söz konusudur. Yukarıda koyu renkte yazdığımız kodun daha kısa bir ifadesi şu şekildedir.

**PORTD \|= 00000001;**

Şimdi bu işaretin neye karşılık geldiğini önce uzun yolu öğrenerek rahatça anlayabilirsiniz. Bu işareti başta anlatsaydık büyük ihtimalle ezberleyecek ve asıl işlevini anlamadan kullanacaktınız. Bu işleme terim olarak “maskeleme” de denmektedir. Şimdi ise sağ taraftaki değeri kısaltarak işe devam edelim. AVR mikrodenetleyicilerde her portun bir ayağının adı vardır. Bu ayağın adını zikrederek bu ayak üzerinde işlem yapabiliriz. Örneğin D portunun 5. ayağını PORTD5 olarak zikredebiliriz. Şimdi D portunun 0. ayağı üzerinden aynı kodu tekrar yazalım.

**PORTD \|= \(1&lt;&lt;PORTD0\);**

Bu komut yukarıdaki komuttan daha pratik ve daha anlaşılır oldu. Ama sadece bununla sınırlı kalmıyoruz ve kısaltmaya devam ediyoruz. Bu noktadan sonra hangi şekilde kullanılacağı programcının tercihine kalmış bir durumdur.

**PORTD \|= \(1&lt;&lt;PD0\);**

Yukarıdaki kadar olmasa da yine de P ve D harflerinden anlaşılır bir komut oldu. PORTD0 ile PD0 aynı işlevdedir. Şimdi ise benim tercih ettiğim yöntem olan en kısa yöntemi anlatıp bu “&lt;&lt;” operatörünün aslında ne işe yaradığını açıklayacağız.

**PORTD \|= \(1&lt;&lt;0\);**

Burada işaretler yığını görünen bir komutta aslında çok basit bir işlem yapılmaktadır. En sağda ayak numarası \(aslında bitin kaç adım sola kaydırılacağı\) &lt;&lt; işaretinin solunda bitin kendisi ortadaki \|= ise maskeleme için kullanılan OR işleminin kısaltılmışı olarak yer alıyor. Burada sola kaydırma \(&lt;&lt;\) operatörünü aslında yukarıda 0 ve 1’ler ile yazdığımız değeri elde etmek için kullanılıyoruz. Örneğin \(1&lt;&lt;1\) yazdığımızda bu “00000010” sonucunu veriyor. Yani “1” değerindeki biti sola bir kere kaydır demiş oluyoruz. Aynı şekilde \(1&lt;&lt;7\) dediğimizde “10000000” sonucunu elde ediyoruz. Yani “1” değerindeki biti sola 7 kere kaydırmış oluyoruz.

Yukarıda verdiğim en basit örnek ile en karmaşık örnek arasında değer bakımından değil kısaltma ve pratikleştirme açısından fark vardır. Her durumda aynı işlemi yapmış oluyoruz. İnternette incelediğim 10’a yakın İngilizce ders yazılarının hiçbirinde bunlar bu kadar ayrıntılı ve toplu halde açıklanmamıştır. Bunları bir yönden anlatıp geçmek de mümkündür ama biz bu kısa kesmek istemiyoruz.

Şimdi **HIGH** yapmak için kullanılan komutları sözdizimsel şekilde tekrar toplayalım.

**PORTx \|= \(1&lt;&lt;PORTxn\);  
PORTx \|= \(1&lt;&lt;Pxn\);  
PORTx \|= \(1&lt;&lt;n\);**

x: port adı  n: ayak numarası

Buraya kadar bir biti bir \(1\) yani HIGH yapmayı anlattık. Şimdi ise bir biti \(0\) yani LOW yapmaya sıra geldi. Fakat burada OR operatörü ile bir biti sıfır yapmamız mümkün değil. O halde farklı bir işleme ihtiyacımız var. Yine aynı mantıkta fakat bu sefer aradığımız işlemi yapıp uygun sonucu verecek bir komuta ihtiyacımız var. Bunun için AND ve tersleme \(~\) operatörlerini kullanacağız. Örneğin D portunun 0. bitini HIGH yaparak ona bağlı olan ledi yaktık. Yanıp sönen led uygulaması yaptığımızdan dolayı belli bir süre sonra söndürmek istedik. Bu durumda şöyle bir kod yazacağız.

**PORTD = PORTD &  ~00000001;**

Öncelikle tersleme operatörü belirlediğimiz sabit değeri tersleyerek işleme başlayacaktır. Değer terslendikten sonra “11111110” olacaktır. Bu değer ile PORTD’nin değeri AND işlemine tabi tutulup elde edilen sonuç PORTD’ye aktarılacaktır. Bunu yine tablo ile verelim ilk değerimiz PORTD’nin ilk değeri olsun, ikinci değerimiz bizim belirlediğimiz sabit değer ve son değer ise PORTD’nin güncellenen değeri olsun.

| 0 | 1 | 0 | 1 | 1 | 1 | 0 | 1 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 1 | 1 | 0 | 0 |

Görüldüğü gibi 1 olan yerlere 1 ile AND işlemi yapıldığında 1 oluyor fakat 0 olan yerlere 0 ile AND işlemi yapıldığında 0 oluyor. Yani diğer hiçbir bitin işleme tabi tutulsa bile değerinde değişme olmuyor. Fakat bizim 0 ile AND işlemine tabi tuttuğumuz bit 1 durumunda ise 0 oluyor. Yani bu durumda HIGH’dan LOW seviyesine çekmiş oluyoruz. Eğer 0 olsa idi yine bir değişme olmayacaktı. Her durumda istenilen bitte LOW sonucunu elde ediyoruz.

Buraya kadar anlaşıldıysa artık bunu da kısaltarak yazıp kullanabiliriz. Öncelikle operatör tarafından bir kısaltma yaparak AND operatörünü kısaltalım.

**PORTD &= ~00000001;**

Yine yukarıdaki gibi bir kısaltma yaparak kodumuzu daha anlaşılır bir seviyeye çekebiliriz.

**PORTD &= ~\(1&lt;&lt;PORTD0\);**

Şimdi yine PORT kısmından kısaltalım ve PD0 olarak yazalım

**PORTD &= ~\(1&lt;&lt;PD0\);**

En sonunda ise en kısa sürümüne geçelim. En anlaşılmaz gibi gözüken bu sürümün aslında en saf ve en anlaşılır olanı olduğunu söylemek yanlış olmaz.

**PORTD &= ~\(1&lt;&lt;0\);**

Şimdi  **LOW**  yapmak için kullanılan komutları sözdizimsel şekilde tekrar toplayalım.

**PORTx &= ~\(1&lt;&lt;PORTxn\);  
PORTx &= ~ \(1&lt;&lt;Pxn\);  
PORTx &= ~ \(1&lt;&lt;n\);**

x: port adı  n: ayak numarası

Buraya kadar dijital çıkışta öğrenmeniz gereken bilgiyi fazlasıyla anlattık. Bir ledi yakıp söndürmek en uzun böyle anlatılabilirdi diyebilirsiniz fakat anlamamıza gerek olmasa da isteyenler için makrolarla kullanımı da sonraki yazıda anlatacağız. Makrolarla dijital giriş birbiriyle alakalı olduğundan ikisini beraber anlatmak üzere dijital girişi sonraki yazıya bırakıyorum.

Kaynaklar:

Bitwise Operators in C Programming, https://www.programiz.com/c-programming/bitwise-operators, Erişim Tarihi: 18.08.2018

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018

Kapak Resmi:  
https://d3s11pzv7w3h1q.cloudfront.net/wp-content/uploads/IC-ATTINY13A-PU-2.jpg

