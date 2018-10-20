# USART İletişim Protokolü ve Birimine Giriş

Artık ADC ile ilgili öğreneceklerimizin çoğunu öğrendik ve yeni bir konuya giriş yapmamızın vakti geldi. ADC hakkındaki bilgilerin tamamı olmasa da büyük çoğunluğunu anlatmış olduk. Geriye kalan ADC kesmesi, dahili sıcaklık algılayıcısı ve diğer konuları ileride ihtiyaç duyduğumuz zaman anlatacağız. Şimdi Evrensel Senkron Asenkron Alıcı Verici yani USART birimini ve bunun nasıl kullanılacağını anlatarak derslerimize devam edelim.

Bu derse başlamadan önce şunu söylememizde fayda var. Bu dersler elektroniğe ve gömülü sistemlere yeni başlayanlar için anlaşılmaz gelebilir. En azından C dili, temel elektronik ve mikrodenetleyicilerde bir temelinizin olması gerekiyor. Bu dersler tam olarak Arduino’da uzmanlaşmış, çeşitli proje yapmış ve bir ileri adıma geçmek isteyenlere hitap etmektedir. O yüzden bu konularda yeni olanlar Arduino’ya başlayıp Arduino konuları ile beraber temel elektroniği ve C programlamayı da öğrenmelidir. Arduino üzerinde iyi bir seviyeye gelen biri için bu dersler zor gelmeyecektir. Yazdığım “Arduino Eğitim Kitabı” tam da bu amaç için yazılmıştır. Bu kitap elektronik ve yazılıma giriş yapanlarda sağlam bir temel oluşturma görevini üstlenecektir.

**UART ve USART**

Bu protokollerin ikisi birbirine karıştırılabilmektedir. Aslında prensip olarak birbirine çok benzemekte fakat birinde veri asenkron diğerinde ise senkron olarak iletilmektedir. Bu senkronizasyon SPI protokolünde olduğu gibi CLOCK \(Saat\) frekansı ile sağlanmaktadır. UART protokolü bu yüzden USART’a göre daha basit kalmaktadır. Eğer bir UART protokolünde veri göndermek istersek sadece tek bir ayağı kullanmamız ve verinin bitlerini sırasıyla belli bir zamana göre göndermemiz yeterli olacaktır. Bu belli bir zamanı karşı tarafın anlaması için bir saat frekansı olmadığı için sıkıntı çıkabilmektedir. Bunun için baud rate adı verilen veri iletişim hızını iki aygıtta da belirtmemiz gerekir.

Baud rate saniyede gönderilen bit sayısı olarak belirtilir. Örneğin bir aygıtın veri iletişim hızı 9600 ise bu aygıt saniyede 9600 bit gönderebilir. Buna karşılık alıcı aygıtın da 9600 bit veriyi aynı hızda okuması gerekir. Eğer aynı hız sağlanamazsa veriler düzgün okunamaz. Bu yüzden senkronizasyon ayarını bizim iki aygıtta da önceden yapmamız gerekir.

UART protokolü gönderim için bir ayak ve alım için bir ayak kullanır. Gönderilen veri karşı taraftaki aygıtın alım bacağına \(RX &gt; TX\) alınan veri ise karşı taraftaki aygıtın gönderim bacağına \( TX &gt; RX \) bağlanmak zorundadır. İki taraflı iletişim için iki hatta ihtiyacımız olsa da tek taraflı iletişim için iki hattın bağlanma zorunluluğu yoktur.

UART protoklünde veriler seri bir halde tek bir bacaktan gönderilir. Bunun öncesinde Başlama biti ve en sonunda ise eşitlik \(sağlama\) biti vardır. UART veri okumaya başlama bitinin okunmasıyla başlanır. Aşağıdaki resimde örnek bir UART iletişiminin mantıksal şeması vardır.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/UART1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-19-usart-iletisim-protokolu-ve-birimine-giris/attachment/uart1/)

Resim: https://cms.edn.com/ContentEETimes/Images/15%20Quinnell/Bloggers/UART1.png

Şimdi örnek bir UART iletişiminin lojik analizör görüntüsünü ayrıntılı olarak inceleyelim. Bu resmi anladıktan sonra UART protokolü için anlaşılmadık bir nokta kalmayacaktır.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/Async_logical_analyzer_2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-19-usart-iletisim-protokolu-ve-birimine-giris/attachment/async_logical_analyzer_2/)

Resim: http://www.gammon.com.au/images/Async\_logical\_analyzer\_2.png  _\(Resme tıklayarak tam çözünürlüklü halini görebilirsiniz. \)_

**A :** Burada seri iletişim başlamamıştır ve seri iletişim ayağı HIGH konumdadır. Bu ayak HIGH konumda olduğu sürece de seri iletişim başlamaz.

**B**: Burada seri iletişim ayağı LOW konumuna çekilerek başlama biti alıcıya gönderilir ve seri iletişime hazır olması gerektiği söylenir. Başlama biti 1.5 saat dalgası kadar uzun kalmalıdır. Normal veri bitleri için 1 saat dalgası yeterlidir.

**C**: Burada sıralı olarak ilk bitten başlayarak \(sağdan itibaren\) byte tipindeki 8 bitlik verinin bitleri sırayla gönderilir. Bit bir \(1\) ise HIGH konumda sıfır \(0\) ise LOW konumda bir saat dalgası kadar bekler.

**D**: Stop Bitidir. Burada okumanın bittiği haber verilir.

**E**: Bir sonraki bayt verisinin başlangıç bitidir.

Bu seri iletişim bazen RS-232 olarak da adlandırılsa da RS-232 protokolünün gerilim değerleri ile mikrodenetleyicinin UART protokolünün gerilim değerleri birbirinden farklıdır. Bu farklılıktan dolayı bir mikrodenetleyici bilgisayarın RS-232 portuna doğrudan bağlanamaz. RS-232 portu olan bilgisayar pek kalmasa da bağlamak isteyenler MAX232 entegresini kullanmalıdır.

**Eşlik \(Parity\) Bitleri**

Bu bitleri kullanmak tercihe bağlı olup verinin doğru iletilip iletilmediğini sağlamak için kullanılır. Bu bitler iletilen verinin bitlerinin toplamına göre tek veya çift olur. \(Tek = 1 Çift = 0 \) Bu bite göre verinin doğru olarak iletilip iletilmediğine karar verilir.

USART ise bunun üzerine asenkron iletişimden kaynaklanan senkronizasyon sorununu gidermek için ek bir Saat frekansı hattına sahiptir. AVR ve Arduino projelerinde genellikle UART kullanılmakta olup bu dezavantajına rağmen oldukça stabil sonuçlar alınmaktadır. Bu dersi burada bitirip AVR’nin USART birimine sonraki derste geçelim.

**Kaynaklar**   
Async \(Serial\) peripheral interface – for Arduino, Nick Gammon, http://www.gammon.com.au/forum/?id=10894, Erişim Tarihi: 20.08.2018

USART vs UART: Know the difference, Jacob Beringo, https://www.edn.com/electronics-blogs/embedded-basics/4440395/USART-vs-UART–Know-the-difference, Erişim Tarihi : 20.08.2018

Kapak Resmi : http://10rem.net/media/82343/Windows-Live-Writer\_b3541542a059\_B73E\_image\_21.png

