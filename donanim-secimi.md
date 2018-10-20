# Donanım Seçimi

Bir mikrodenetleyici platformuna giriş için gerekli donanım ve yazılım elemanlarını temin etmek gerekir. Günümüzde yazılım İnternet sayesinde erişilebilir olsa da donanım yönünde son yıllara kadar yurt içi pazara bağlı konumdaydık. Üreticiden ya da yabancı tedarikçilerden satın almak da ödeme yöntemlerinin kısıtlanması ve döviz kurlarından dolayı oldukça maliyetli ve zor hale gelmiştir. Buna çözüm getiren tek yer Aliexpress gibi çin menşeili sitelerdir. Bir platforma ait ürünleri Aliexpress’de bulabilmemiz o platformun tedarik sıkıntısını neredeyse ortadan kaldırır durumdadır.  Bu başlıkta da birinci şart olarak donanımın Aliexpress’de ya da Türkiye pazarında bulunabilir olmasına göre konuya dahil edeceğiz.

**Deneme Kartları ve Geliştirme Kartları**

Mikrodenetleyici eğitim kartlarını iki ana sınıfa ayırmak istersek bunları deneme kartları ve geliştirme kartları olarak ikiye ayırırız. Deneme kartları eski usul olup üzerinde tuş takımı, LCD ekran, LED, buzzer gibi birçok elektronik elemanı üzerindeki mikrodenetleyiciye bağlı halde tek kart olarak sunan ve kullanıcıyı devre kurmak yerine sadece program yazmaya sevk eden kartlardır. Bu kartların olumsuz özelliği yenilenebilir olmaması, prototipleme yapılamaması, sadece tek bir yönde kullanılabilmesidir. Üstelik üzerinde bulundurduğu devre elemanları ve işçilikten dolayı geliştirme kartına göre oldukça pahalıdır. Üzerinde programlama devresini bulundurması ayrı bir programcı alma ihtiyacını ortadan kaldırsa da mikroişlemcilerin Ön yükleyici \(bootloader\) özelliği sayesinde bu avantaj ortadan kalkmaktadır.

> **Ön yükleyici \(bootloader\)** özelliği mikrodenetleyiciyi devreden sökmeden ve seri iletişim yoluyla programın güncellenebilmesine olanak sağlayan bir donanım özelliğidir. Mikrodenetleyicide ana program önceden olağan programlama ile program hafızasına yüklenir. Bu ana program ön yükleyici görevde olup hafızada küçük bir yer tutar. Seri iletişimde aldığı verileri program hafızasının geri kalan kısmına yükleyerek çalışacak programın \(uygulamanın\) seri iletişim üzerinden yüklenebilmesine olanak sağlar.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/pic-development-board-08-300x300.jpg)  
_Deneme Kartı_  
Resim: https://www.pantechsolutions.net/media/catalog/product/cache/1/image/600×600/9df78eab33525d08d6e5fb8d27136e95/p/i/pic-development-board-08.jpg

Geliştirme kartları ise temel olarak bir mikrodenetleyiciyi çalıştırmak için asgari gereklilikte parçayı üzerinde bulundurup mikrodenetleyicinin ayaklarına kolay erişim sağlatan kartlardır. Bazı geliştirme kartları en sık kullanılan ve ihtiyaç duyulan elemanları üzerinde barındırabilir. Programlama devresi, USB yuvası, LED bunlardan başlıcalarıdır. Bazı geliştirme kartlarının kendilerine ait derleyicisi hatta programlama dili bile bulunur. Bunlara geliştirme platformu denir. Arduino, BASIC Stamp gibi geliştirme platformları hobiciler için hem yazılım hem donanım olarak oldukça kolaylaştırılmış tam kapsamlı platformlardır. AVR programlamayı istediğimizden dolayı geliştirme platformlara mümkün olduğunca yazılım yönünden uzak kalmayı tercih edeceğiz. Yalnız AVR ile Arduino artık oldukça birbirine yaklaştığı için kısmen Arduino kütüphanelerinden ve kodlarından yardım alabiliriz. Çünkü Arduino’nun temeli AVR donanımı ve yazılımıdır.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/a000066_featured_4-300x190.jpg)

_Bir geliştirme kartı olarak Arduino UNO_

Resim:  
https://store-cdn.arduino.cc/usa/catalog/product/cache/1/image/520×330/604a3538c15e081937dbfbd20aa60aad/a/0/a000066\_featured\_4.jpg

AVR için üreticinin sağladığı resmi geliştirme platformları mevcuttur. Fakat bunların maliyeti alternatiflerine göre oldukça yüksek olup fiyatına göre özellikleri yeterli değildir. O yüzden basit bir prototipleme kartı ya da geliştirme kartı ile başlamak en akıllıca çözümdür. AVR için üretilen üçüncü parti geliştirme ve deneme kartlarını Çin sitelerinde bulmak mümkündür. Bu kartların Arduino’ya göre ciddi eksiklikleri vardır. Pek çoğuna USB üzerinden program atılamamakta ve harici bir programlayıcı istemektedir. Harici programlayıcının yanı sıra ekstradan bir güç adaptörüne ihtiyaç duymaktadır. Arduino hem gücünü USB üzerinden almakta hem de aynı yoldan program atılabilmektedir.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/font-b-atmega8-b-font-atmega48-development-board-font-b-avr-b-font-development-board_1_-300x300.jpg)  
_Çok uygun fiyatlara bulabileceğimiz AVR prototipleme kartı. Üzerinde programlayıcı bulundurmadığı için ayrı bir programlayıcıya ihtiyaç duyuracaktır. Aynı zamanda pek çok modelinde güç soketinde adaptör yuvası vardır._   
Resim : https://megaeshop.pk/media/catalog/product/-/f/-font-b-atmega8-b-font-atmega48-development-board-font-b-avr-b-font-development-board\_1\_.jpg

#### Arduino Uno Geliştirme Kartı

Geliştirme ve deneme kartları arasında tercihe değer olarak Arduino Uno kartını bulduk. Arduino kart kullanılması kesinlikle Arduino’nun yazılımını kullanacağımızı akla getirmesin. Arduino burada sadece fiziksel kart ve ön yükleyici olarak kalacak. Geri kalan tüm kodlar saf dile uygun olarak yazılacak. Donanım olarak Arduino’nun tercih edilmesinin pek çok avantajı vardır. Bunları madde madde sıralayabiliriz,

* Arduino UNO kartı harici bir beslemeye ve besleme devresine ihtiyaç duymaz. Üzerinde 5V ve 3.3V regülatörler bulunur ve programlayıcı, mikrodenetleyici ve dış elemanlar buradan beslenir.
* Arduino UNO kartının beslemesi USB üzerinden yapılabilir. Böylelikle DC adaptör kullanmaya ihtiyaç kalmaz.
* Arduino UNO kartı harici bir programcıya ihtiyaç duymaz. Kendi programcısı üzerinde gelir.
* Arduino UNO kartı bilgisayar ile üzerinde bulundurduğu çevirici entegre ile USB üzerinden doğrudan iletişime geçebilir.
* Arduino UNO kartında kısa devre ve akım koruması vardır.
* Arduino UNO kartı için üretilmiş üstlükleri \(shield\) kullanma imkanımız vardır.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/Arduino-Uno-Pin-Diagram.png)

_Arduino Uno Ayak Haritası_   
Resim: https://components101.com/sites/default/files/component\_pin/Arduino-Uno-Pin-Diagram.png

Derslerimizde yettiği kadarıyla Arduino UNO kartını kullanacağız. O yüzden bu derslerle AVR programlamayı öğrenmek isteyenlerin Arduino UNO kartını tercih etmesi gerekir.  İleri derslerde yazılım, programcı seçiminden bahsedip Arduino Uno kartının donanımını anlatarak devam edeceğiz.

