# Kesmeler

Kesmeler gömülü sistemlerde kullanılan en faydalı ve en temel özelliklerden biridir. Kesmeyi kısaca tarif etmek gerekirse işlemciyi dışarıdan müdahale ile durdurup özel bir fonksiyonu yürüten yapılardır, diyebiliriz. Kesmeler donanımsal olduğu için ek bir işlem gücü gerektirmez. Bu seviyedeki mikroişlemcilerin en büyük kısıtlılıklarından biri aynı anda iki işlemi birden yapamamalarıdır. Aynı anda iki işlem birden yürütülmediği için bunu dışarıdan denetleyecek ve işlem sırasını gözetecek bir yapıya ihtiyaç vardır. Örneğin mikrodenetleyiciye bir düğme bağladığımızda bunun durumunu yazılım boyu denetlememiz gerekecektir. Bu denetleme sürecinde farklı kodlar işletilmeyeceği gibi farklı kodların işletilmesi için de denetleme işleminin belli bir zaman yapılmaması gerekir. Bazı durumlarda mikrodenetleyici bu denetleme işlemleri ile gereksiz yere meşgul olur ve yavaşlar. Aynı zamanda bu denetleme işlemleri çoğalınca programın kararlılığı oldukça zayıflar. Bunun için bu durumları denetleyecek ve mikroişlemciyi dışarıdan yönlendirecek aygıta ihtiyaç vardır.

Bu durum aynı biz bir iş yaparken telefonun çalması ve bu dış uyarana karşılık bizim işimizi bölüp telefonu cevaplamamız gibidir. Telefonu sessize alıp telefonun çalıp çalmadığını her dakika başı denetlemek yerine telefonun sesine göre karar vermek nasıl mantıklı bir seçim ise mikrodenetleyicilerde kesmeleri kullanmak da o derece mantıklı bir seçim olacaktır. Basit programlarda ve her zaman ihtiyaç duyulmayan yerlerde \(mikroişlemcinin işlem hızına güvenimizden dolayı\) kesme kullanmayabiliriz. Ama kesmeleri muhakkak bilmemiz gereklidir.

Kesmeler donanım ve yazılım kesmeleri olarak ikiye ayrılsa da yazılım kesmeleri genellikle işletim sistemlerinin özel görevleri için kullanılıp AVR mikrodenetleyicilerde yer almaz. O yüzden AVR mikrodenetleyicilerde sadece donanım kesmeleri kullanılır. Bu kesmelerin adları, adresleri ve kaynakları \(kesmeye sebep olan olay\) mikrodenetleyicinin teknik veri sayfasında yer alır. C dilinde kesmeleri kullanmak oldukça kolaydır çünkü AVR’nin interrupts.h adında başlık dosyası mevcuttur. Burada kesme fonksiyonlarını yazıp sonrasında işletilmesini istediğimiz kodu arasına koyabiliriz. Kesme fonksiyonları özel bir adla adlandırılır, Kesme Servis Rutini \(Interrupt Service Routine\) olarak adlandırılan bu fonksiyon kısaca ISR olarak ifade edilir.  Kesme biti bayrakları her kesme yürüdükten sonra sıfırlandığı için tekrar sıfırlanmaya ihtiyacı yoktur.

Kesmelerin nasıl kullanıldığını önceden zamanlayıcı örnek kodlarında göstermiştik fakat sadece kullanımından ibaretti. Diğer birimlerde yazmaç olduğu gibi kesmelerde de yazmaçlar mevcuttur. Bu yazmaçları da konumuz süresince anlatacağız.

Bir kesmenin yürütülmesi için öncelikle şu adımlar izlenmelidir.

* AVR’nin genel kesme biti etkin olmalıdır. SREG yazmacındaki I biti bu görevi üstlenmektedir. Bu bit mikrodenetleyici açıldığında sıfır \(0\) konumundadır. Bizim sonradan kesmeleri etkinleştirmemiz gereklidir.
* Kesme kaynağının etkinleştirme biti etkin olmalıdır. Önceden anlattığımız gibi bu kesmeleri etkinleştirme bitleri birim yazmaçlarının içerisinde yer alabilmektedir. Bu bitler özel bir kesmeyi etkinleştirmek için kullanılır. Genel kesmelerin etkin olması tüm kesmelerin etkin olacağı anlamına gelmemektedir. O yüzden hangi kesmeyi kullanmak istiyorsak o kesmeyi elle etkinleştirmemiz gereklidir.
* Kesme şartı sağlanmalıdır. Yani etkinleştirdiğimiz kesmenin bir iş yapması gereklidir. Boş yere bir kesmeyi çalıştırmanın anlamı yoktur. Onu da kesme fonksiyonuna yazdığımız kodlarla yapıyoruız.

AVR mikrodenetleyiciler kesmedeyken kesmeye gidemez. Kesme yürütüldüğü sırada kesmeler devre dışı bırakılır. Bu yüzden kesme içinde kesme gerçekleşmesi mümkün değildir. O yüzden kesme fonksiyonlarını çok uzun tutmamak atlanılan kesmelerin oluşmasını önleyecektir. Çoğu kesme fonksiyonu kısa görevleri yapmak için yürütüldüğünden böyle bir durumun olması pek fazla sıkıntı doğurmayacaktır.

Genel kesmeleri etkinleştirmek ve devre dışı bırakmak için avr/interrupt.h başlık dosyasını aldıktan sonra iki fonksiyon kullanıyoruz. sei\(\) \(SEt Interrupt Bit\) genel kesmeleri etkinleştirirken cli\(\) \(CLear Interrupt Bit\) genel kesmeleri devre dışı bırakır. Bazı hassas uygulamaların kesmelerle bölünmesini istemeyebiliriz. Bu durumda kesmeleri devre dışı bırakıp mikrodenetleyicinin o süre zarfında ancak bizim istediğimiz kodları işlemesini sağlayabiliriz.

Kesmeler yürütüldüğü zaman ana program yürümeyeceği için kısa zaman zarfında oldukça fazla kesme yürütmek ana programın yavaşlamasına sebep olur. Bunu dikkate alarak gereğinden fazla kesme kullanmamak lazımdır.

Kesme fonksiyonları ile ana fonksiyon arasında kullanılan değişkenler volatile veya global \(genel\) olmalıdır. Kesme fonksiyonları ana programdan ayrı yürüdüğü için ana programın içindeki değişkenlere erişimi yoktur. Şimdi örnek bir kesme fonksiyonunu verelim ve konumuzu bitirelim.

```c
ISR ( kesme_vektoru )
{
komutlar;
}
```

 İşte programa kesmeleri eklemek bu denli kolaydır. Sonraki dersimizde teknik veri sayfası üzerinden kesmeleri inceleyeceğiz ve AVR mikrodenetleyicisinde dahili ve harici olmak üzere kaç kesme olduğunu anlatacağız. Zamanlayıcılardan sonra kesmeleri bitirince geriye kalan konuların yükü oldukça azalacaktır. Kesme konusu zamanlayıcılar kadar zor olmasa da bilinmesi gereken başlıca konulardan ve mikrodenetleyicinin en temel birimlerinden olduğu için üzerinde durulması gerekir. Örneğin SPI ya da I2c birimini bilmeden yine pek çok iş yapılabilir ama kesmeleri bilmeden çoğu iş eksik kalır. Sonraki derste görüşmek üzere.

