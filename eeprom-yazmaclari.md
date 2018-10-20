# EEPROM Yazmaçları

Derslere yazmaya başlarken herhangi bir “İçindekiler” listesi veya şablon kullanmadım. Tamamen doğaçlama yürütülen bu dersler zaman ile kendi orjinal formatına sahip oldu. Böyle bir formata sahip olmasını asla kalitesizlik göstergesi olarak görmemek gereklidir. Çünkü diğer yayınlara bakıp onları esas alarak yürüttüğüm bir çalışma özgünlük açısından daha zayıf olacaktı. Çalışmayı diğer ders yazılarındaki seviyede tutup bilginin tamamını vermeyip en basit seviyeden anlatmakla yürütseydim yazması da anlaşılması da daha kolay olacaktı. Biz kolay olmasını, herkese hitap etmesini, anlaşılır olmasını değil faydalı olmasını gözettiğimiz için verilebilecek faydanın azami seviyede olmasına özen gösteriyoruz. Bu yüzden çoğu hususdan feragat ederek bu çalışmayı yürütmekteyiz.

Fark edilirse günümüzde basılan bilişim ve elektronik alanındaki kitapların neredeyse tamamı giriş seviyesinde olmaktadır. İleri seviyede veya alt bir konuda bir kitap pek görülmemektedir. Bu, bu alandaki literatürün en büyük eksikliği olmaktadır. Bir okur aynı konuda onlarca kitap okusa dahi giriş seviyesinde kalıyorsa bu kitapların faydasının sorgulanması gereklidir. Öte taraftan giriş seviyesinde olmayan kaynaklar yeterli ilgiyi görmemektedir. Bu hazırladığımız kaynak hiçbir zaman hak ettiği ilgiyi görmeyecek olsa da biz görevimizi yerine getireceğiz.

AVR derslerini sadece AVR konuları üzerinden yürütmemin bir sebebi var. Bu dersler bittikten sonra dijital elektronik, mikroişlemci mimarisi ve gömülü sistemlerde C dili dersleri gibi başka ders yazısı dizisi yürütmeyi düşünüyorum. AVR dersleri hemen bittikten sonra ise Arduino kaynak kodu incelemesi yapacağımız bir yazı dizimiz olacak. Böylelikle Arduino’dan AVR’ye geçenler aynı zamanda Arduino’nun nasıl yazıldığını da öğrenecek. Arduino’nun kaynak kodunu öğrenerek Arduino fonksiyonlarını ve kütüphanelerini rahatça AVR’ye uyarlayarak kullanabileceksiniz. Bu durumda Arduino’nun hiçbir artısı kalmış olmayacak. Bu yüzden AVR derslerinden sonra yürütülmesi gereken en önemli çalışmanın bu olacağını düşünüyorum.

Şimdi dersimize başlayalım,

Teknik veri kitapçığında EEPROMlar hafıza birimleri üst başlığı altında anlatılmaktadır. Burada tüm konuları anlatma gibi bir gayemiz olsa da Flash ve SRAM gibi birimleri mikroişlemci mimarisini anlattığımız zamanlarda AVR Mimarisi alt başlığında anlatacağız. C dilinde programlama için AVR mimarisini bu seviyede bilmek gerekli değildir. Yine de C dilinde programlama yaparken EEPROM bellekler üzerinde çalışmak diğer alanlara göre biraz daha alt seviyededir, diyebiliriz. Bunu dememizin sebebi burada sadece veri ve kontrol yazmaçları yer almayıp bir de bunun yanında adres bilgisi de yer almaktadır.

Buradan anlaşılacağı üzere denetim, Veri ve Adres yazmacı olmak üzere üç ayrı yazmaç türü vardır.  Şimdi bu yazmaçları tek tek açıklayalım.

**EEARH – EEPROM Adres Yazmacı \(Üst\)**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/eprom1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-42-eeprom-yazmaclari/attachment/eprom1/)

Adres yazmacının üst ve alt olmak üzere iki ayrı yazmaçtan oluşup toplamda 10-bit olmasının sebebi tek bir yazmacın 8-bit olup toplamda 255 baytlık bir veriyi adresleyebilmesinden dolayıdır. ADC konusunda da gördüğümüz üzere 1024 farklı değeri saklayabilmek için 10 bitlik bir alana ihtiyaç vardır. 1024 baytlık bir EEPROM verisini adreslememiz gerektiği için 10-bitlik bir alana ihtiyacımız vardır. Bu yazmaç ise üst iki biti saklamak için kullanılır.

**EEARL – EEPROM Adres Yazmacı \(Alt\)**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/eprom2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-42-eeprom-yazmaclari/attachment/eprom2/)

Bu yazmaç 10 bitlik adres verisinin alt 8 bitini saklamak için kullanılır. EEPROMdan bir veri okuyup yazmadan önce adres değerini bu yazmaca yazmak gereklidir. EEAR bitleri adres  verisi bitleridir.

**EEDR – EEPROM Veri Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/eprom3.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-42-eeprom-yazmaclari/attachment/eprom3/)

Bu yazmaç EEAR yani EEPROM Adres Yazmacının gösterdiği adresteki EEPROM verisini tutan yazmaçtır. Okuma ve yazma işlemleri bu yazmaç üzerinden yapılır. Her adresten 8 bitlik bir değer okunduğu için 16-bit integer, float ve karakter dizisi verilerini okurken adres üzerinde işlem yapmak gereklidir. Tek bir adresten tek bir harf veya bayt verisi okunabilir. EEDR bitleri EEPROM verisini saklayan bitlerdir.

**EECR – EEPROM Denetim Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/eprom4.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-42-eeprom-yazmaclari/attachment/eprom4/)

Yukarıda adres ve veri yazmaçlarını anlattık. Geriye ise denetim yazmacı kaldı. Denetim yazmacı tek bir işleve sahip olmadığı için her bir bitinin ne işe yaradığını ezberlemenizi istemiyoruz. Fakat gerektiği zaman bu yazdığımız referans kaynağa müracaat edebilmeniz gerekli. Önemli olan bunu ezberlemek değil ne işe yaradığını ve ne yapılabildiğini öğrenmenizdir.

**Bit 5:4 – EEPMn EEPROM Programlama Modu Bitleri \[ n = 1:0 \]**

EEPE biti tetiklendiği zaman hangi programlama modunda işlem yapılacağı bu bitler ile belirlenir. Bu işlemler okuma ve yazma olarak iki ayrı işlem olsa da atomik işlem adı verilen silme ve yazmanın beraber yürütüldüğü bir işlem de vardır. EEPE biti bir \(1\) konumunda olduğu zaman EEPM bitlerine yapılan yazım işlemi görmezden gelinir. Aşağıdaki tabloda EEPM bitlerinin işlevi verilmiştir.

| EEPM\[1:0\] | Programlama Süresi | İşlem |
| :--- | :--- | :--- |
| 00 | 3.4 ms | Silme ve Yazma \(Tek İşlemde\) |
| 01 | 1.8 ms | Silme |
| 10 | 1.8 ms | Yazma |
| 11 | – | Rezerve \(Kullanım Dışı\) |

**Bit 3 – EERIE : EEPROM Hazır Kesmesi Etkin**

Bu biti bir \(1\) yapmak EEPROM’un hazır olduğunda yürütülecek kesmeyi yürür hale getirir. Burada bu kesmenin yürütülmesi için SREG’deki I bitiyle genel kesmelerin etkin olması gereklidir.  Kesmenin nasıl kullanılacağına dair bilgi edinmek için kesmeleri konu aldığımız önceki derslerimize bakabilirsiniz.

**Bit 2 – EEMPE : EEPROM Ana Yazma Etkin**

Bu bit, EEPE bitine bir \(1\) yazıldığında yazma işleminin yapılıp yapılmayacağını belirleyen bir güvenlik anahtarıdır. EEMPE bir \(1\) olduğu zaman EEPE  4 saat çevrimi içinde bir \(1\) olursa seçili adrese veri yazılır. Eğer EEMPE sıfır ise EEPE bitini bir \(1\) yapmanın bir etkisi yoktur. EEMPE biti bir \(1\) yapıldıktan sonra donanım dört saat çevrimi içerisinde bu biti tekrar sıfır \(0\) konumuna getirir.

**Bir 1 – EEPE : EEPROM Yazma Etkin**

Adres ve veri yazmaçları hazır olduğu zaman yazma işleminin gerçekleşmesi için EEPE bitinin bir \(1\) yapılması gereklidir. EEPE biti bir \(1\) yapılmadan önce EEMPE bitinin bir \(1\) yapılması gereklidir. Bu işlem bu sırada yürütülmezse EEPROM yazma işlemi gecikmeye uğrar. Aşağıdaki işlem sırasına göre EEPROM yazma işlemi yürütülmelidir.

* EEPE bitinin sıfır olmasını bekle.
* SPMCSR yazmacındaki SPMEN bitinin sıfır olmasını bekle.
* EEAR yazmacına yeni EEPROM adresini yaz.
* EEDR yazmacına yeni EEPROM verisini yaz.
* EEMPE yazmacına EEPE bitine sıfır \(0\) yazma esnasında bir \(1\) yaz.
* EEMPE yazmacına bir \(1\) yazmanın ardından dört saat çevrimi içerisinde EEPE yazmacına bir \(1\) yaz.

**Bit 0 – EERE : EEPROM Okuma Etkin**

Bu bit sayesinde EEPROMdaki belli bir adresten bir veriyi okuyabiliriz. EEAR yazmacına doğru adres yazıldıktan sonra EERE biti bir \(1\) yapılmalıdır. Böylelikle okuma işlemi tetiklenmiş olur. EEPROM okuma işlemi sadece bir işlemci komutunda yürütülür ve istenilen veriyi anında verir. EEPROM okunduğunda işlemci dört saat çevrimi süresince durdurulur ve sonraki işlemci komutu işletilir. EEPROM okuma işlemi 26368 saat çeviriminde ve 3.3ms zamanda gerçekleşmiş olur.

Fark  edilirse bir baytı okumak için 3 ms kadar bir zaman gereklidir. Bu süre bir veya birkaç bayt seviyesinde görmezden gelinecek kadar az olsa da belleğin ciddi bir bölümünü kaplayan verileri okumak için katlanarak artıp uzun bir bekleme süresi haline gelecektir. Daha hızlı veri okumak için SD Kart veya Flash Bellek kullanmak gerekebilir. Çoğu zaman ihtiyaç duymasak da bunu bilmenizde fayda vardır.

Bu derste bütün EEPROM yazmaçlarını anlatmayı bitirdik ve sıra örnek kodları anlatmaya geldi. Bunu da bir sonraki derste anlatacağız ve konuyu bitireceğiz.

