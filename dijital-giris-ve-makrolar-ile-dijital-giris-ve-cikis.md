# Dijital Giriş ve Makrolar ile Dijital Giriş ve Çıkış

Önceki yazımızda operatörler ile bit tabanlı dijital çıkışı anlatmış ve dijital giriş ile makroları bu yazıya bırakmıştık. Artık bu yazıda dijital giriş ve çıkış ile alakalı tüm konuları anlatmış olacağız ve diğer konulara geçeceğiz. Fakat bu konuyu burada bırakmayıp ileride örnek devre ve program üzerinden anlatmaya devam edeceğiz. Makrolar operatörlerle yapılan işleri biraz daha anlaşılabilir kılıp kolaylaştırmak için kullanılan fonksiyonlardır. Bu fonksiyonları hiç kullanmadan da bütün işleri yapmamız mümkündür. Fakat programın okunabilirliği açısından makroların inkar edilemez bir faydası vardır. Burada makro kullanmayı sizin tercihinize bıraksam da dijital çıkışta makro kullanmayıp dijital girişte makro kullanmayı tavsiye ediyorum. Dijital girişte makrosuz bir programlama zaman zaman karışıklıklara sebep olabilir.

**Makrosuz Bit Tabanlı Dijital Giriş** 

Dijital girişte PINx yazmacındaki değeri kullanıyorduk. PINx yazmacındaki değer 8-bitlik olup tek bir bite erişimi değer atama \(=\) yöntemiyle sağlayamıyorduk. Aynı dijital çıkışta olduğu gibi bunda da bitwise operatörler ile tek bir bite erişim sağlayabiliriz. Bunun için **PINx & \(1&lt;&lt;PORTxn\);** komutunu kullanıyoruz. Örneğin D portunun 1 numaralı bacağından bir dijital okuma yapıp değişkene aktaralım.

**degisken = PIND & \(1&lt;&lt;PORTD1\);**

Burada okuyacağımız değer HIGH ve LOW çeşidinden olacağı için sıfırdan farklı bir değer HIGH olarak okunacaktır. Genellikle if gibi karar yapılarında düğme durumu okunduğu için okunan değer değil değerin sıfırdan farklı olup olmaması önemlidir. Bunu kısaltarak yazmak istersek yine aşağıdaki iki şekilde yazabiliriz.

**degisken = PIND & \(1&lt;&lt;PD1\);**

**degisken = PIND & \(1&lt;&lt;1\);**

Son komut üzerinden bu işlemin nasıl çalıştığını anlatalım. Öncelikle “1” değerindeki ikilik sayı “&lt;&lt;” operatörü ile bir bit kadar sola kaydırılır. Böylelikle 1 numaralı bite denk gelen değer yani “00000010” elde edilmiş olur. PIND ise giriş ayaklarının ayak durumunu barındıran yazmaçtır. PIND’deki orjinal değer ile AND işlemine tutulan bizim değerimiz eğer ayak gerçekten “1” konumunda ise sonuç kısmındaki bite “1” değerini atar. Eğer ayak sıfır konumunda ise AND işlemine tabi tutulduğundan o değer “0” olur. Bu durumda belirtilen ayağın durumu öğrenilmiş ve sonuca aktarılmış olur. D portunun birinci ayağı “1” konumunda olsun ve PIND yazmacı ile AND işlemi uygulayarak tablo üzerinden sonuç elde edelim. Birinci tablo PIND, ikinci tablo bizim belirttiğimiz değer sabiti, üçüncü tablo ise sonuç olsun.

| 1 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 |
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Bu durumda 1 değerini elde ederiz. Eğer belirttiğimiz değerdeki bit konumuna denk gelen ayak “0” olsaydı bu sefer 0 değerini elde edecektik. Diğer ayakların bir \(1\) veya sıfır \(0\) olmasının sonuca bir etkisi olmayacaktı. Bu komutu if veya döngü yapıları içerisinde kullanmamız da mümkündür. Aşağıda bir örneğini görebilirsiniz.

if\(**PIND & \(1&lt;&lt;1\)\)** 

{

komutlar;

}

Buraya kadar operatörlerle ilgili dijital giriş ve çıkış konuları hakkında anlatılacak konuları anlatmış olduk. Dijital girişte entegre pull-up dirençlerini faal hale getirmeyi ve tersleme komutunu makrolardan sonra anlatacağız. Makroları çok uzun anlatmıza gerek olmayacak. Şimdi makrolara giriş yapalım.

#### **Makrolar**

Birazdan bahsedeceğimiz makrolar C dilinin kendisinde olmayıp avr-libc’nin içerisinde olan var avr/sfr\_defs.h başlık dosyasında belirtilen makrolardır. Bunlar giriş ve çıkış işlemlerini kolaylaştırmak adına kütüphaneye eklenmiştir. Bu makroları kullanmak yerine operatörleri kullanmak programı daha taşınabilir \(portable\) yapacaktır. Makroların yaptığı iş aslında operatörlerle yapılan komutun farklı bir biçimde yazılmasıdır. Örneğin \_BV\(\) makrosunun tanımı şu şekildedir.

\#define \_BV\(bit\)   \(1 &lt;&lt; \(bit\)\)

Yani bizim \(1&lt;&lt;1\) şeklinde yazdığımız kodu burada \_BV\(1\) olarak yazıyoruz. \_BV\(\) makrosunun açılımı “Bit value” yani “bit değeri” anlamına geldiği için programcı için daha anlaşılır oluyor.

**\_BV\(\) makrosu ile giriş ve çıkış**

\_BV\(\) makrosunu yukarıda anlattığım için örnek kod vermekle yetineceğiz.

D portunun 1. bitini HIGH yapmak için,  
**PORTD \|= \_BV\(1\);**  
D portunun 1. bitini LOW yapmak için,  
**PORTD &= !\_BV\(1\);  ya da PORTD &= ~\_BV\(1\);**  
Dijital Okuma Yapmak İçin,  
**PIND & \_BV\(1\);**

Şimdi bu komutları referans için sözdizimsel olarak tekrar yazalım.  
**x : Port Adı n: Ayak Numarası**  
Belli Ayaktan Dijital HIGH çıkış,  
**PORTx \|= \_BV\(n\);**  
Belli Ayaktan Dijital LOW çıkış,  
**PORTx &= ~\_BV\(n\);**  
Belli Ayaktan Dijital Giriş,  
**PINx & \_BV\(n\);**

bit\_is\_set\(\) ve bit\_is\_clear\(\) makroları

Bu makrolar adından da anlaşılacağı üzere bit bitin bir \(1\) yani HIGH mı yoksa sıfır \(0\) yani LOW mu olduğunu denetlemeye yarar. bit\_is\_set\(\)  ve bit\_is\_clear\(\) makroları şu şekildedir,  
**\#define bit\_is\_set\(sfr, bit\)   \(\_SFR\_BYTE\(sfr\) & \_BV\(bit\)\)**  
**\#define bit\_is\_clear\(sfr, bit\)   \(!\(\_SFR\_BYTE\(sfr\) & \_BV\(bit\)\)\)**

Bu makroların birbirinden tek farkı birinde fazladan tersleme “!” \(değil\) operatörü olmasıdır. Yani aynı işlemde sonuç bir \(1\) ise sıfır \(0\), sıfır \(0\) ise bir \(1\) yapmaktadır. Burada makro içinde makroların kullanıldığını görüyoruz. \_BV\(\) makrosunu önceden anlattığımız için anlatmaya gerek olmasa da \_SFR\_BYTE\(sfr\) makrosu burada yenidir. Bu makro belirtilen adresteki veri hücresinin değerini geri döndürür. Yani PINx dediğimizde PINx yazmacının adresine gider ve oradaki değeri verir. Buna benzeyen  \_SFR\_ADDR\(\) makrosu ise adresi geri döndürür. Bu makrolar konumuzu aştığı için bu kadarıyla yetinelim. Kısacası bit\_is\_set\(\) ve bit\_is\_clear\(\) makroları ekstradan bir şey katmayıp bizim operatörler ile yaptığımız işlemi daha anlaşılır hale getirmektedir.

Şimdi bit\_is\_set\(\) ve bit\_is\_clear\(\) makrolarına örnek verelim,

**bit\_is\_set\(PINC,0\)**  
Eğer C portunun 0 numaralı ayağı bir \(1\) ise bir \(1\) değerini geri döndürür. Eğer ayak sıfır \(0\) ise sıfır \(0\) değerini geri döndürür.

**bit\_is\_clear\(PINC,0\)**

Eğer C portunun 0 numaralı ayağı sıfır \(0\) ise bir \(1\) değerini geri döndürür. Eğer ayak bir \(1\) ise sıfır \(0\) değerini geri döndürür.

Bu makroları referans için sözdizimsel olarak verelim,  
**x: Port adı n: ayak numarası** 

**bit\_is\_set\(PINx,n\)** 

**bit\_is\_clear\(PINx,n\)**

Bu makrolar if gibi karar yapıları içerisinde kullanılırsa operatörlere göre çok daha okunabilir kod yazılabilir. Aşağıda buna benzer bir örnek verilmiştir,

**if \(bit\_is\_clear\(PINC,0\)\) { }**

Anlatacağımız makrolar buraya kadardı. İsteseydik sadece makrolar üzerinden giriş ve çıkışı anlatıp ne operatörleri ne de makroların iç yapısını anlatma zahmetine girerdik. Fakat bu tarz yaklaşımlar ezberciliğe zemin hazırladığı için en azından başlangıçta seviyeyi çok basit tutmama gereği duyduk.  Bundan sonra anlatacağımız iki ufak bilgi kaldı onları da kısaca verelim,

**Dahili Pull-UP dirençlerini faal hale getirmek**   
Bunun için yapılması gereken işlem oldukça basittir. Öncelik DDRx yazmacına değer atayarak giriş olarak tanımladığımız portun PORTx yazmacına değer atıyoruz. Örnek aşağıdaki gibidir,  
**DDRB = 0x00; // PortB nin bütün ayakları giriş yapıldı**   
**PORTB = 0xFF; // PORTB nin bütün ayakları PULL-UP konumunda.**

Tersleme / Toggle  
Yanıp sönen led gibi aynı komutla bir \(1\) iken sıfır \(0\) , sıfır \(0\) iken bir \(1\) yapmak istiyorsak bu komutu kullanırız. İki farklı kullanımı vardır ikisi de aşağıda verilmiştir.  
**PORTC = ~PORTC;**

**PORTB ^= 0xFF;**

Burada PORT veya sabit değer yerlerine operatörler veya makrolarla belli bir ayağı belirtebiliriz. Yukarıda bahsettiğimiz mantık burada da geçerlidir.

Kaynaklar:

&lt;avr/sfr\_defs.h&gt;: Special function registers, https://www.programiz.com/c-programming/bitwise-operators, Erişim Tarihi: 18.08.2018

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018

