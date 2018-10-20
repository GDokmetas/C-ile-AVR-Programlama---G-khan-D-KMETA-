# Zamanlayıcı ve Sayıcıların Özeti

Atmega mikrodenetleyicilerdeki zamanlayıcı ünitelerinin oldukça karmaşık olduğunu anlatmamıza gerek yoktur. On derstir anlatmaya çalıştığımız ve daha bitiremediğimiz bu konu kolay kolay kavranacak bir konu da değildir. Basit bekleme işlemlerinden karmaşık PWM işlemlerine kadar kullanılan bu üniteler prensipte basit fakat uygulamada zordur.

Buraya kadar anlatmadığımız konulardan biri de TC2 zamanlayıcısının asenkron olarak kullanımı ve gerçek zaman saati \(RTC\) uygulamasıydı. Ayrıca PWM konusuna daha geçiş yapmayıp zamanlayıcıları normal ve CTC modda anlatmaya devam ettik. Bunları şimdilik ertelemek zorundayız.

Zamanlayıcılar ana programdan bağımsız olarak işleyebilen ve mikroişlemciden bağımsız çalışan aygıtlardır. Mikroişlemci ile bağlantıları kesmeler ve yazmaçlardır. Kesmeler mikroişlemci üzerinde oluşturdukları etki, yazmaçlar ise mikroişlemcinin denetlediği birimlerdir. Kesme durumunda mikroişlemciyi bir işi yapmaya zorlarken geri kalan işlemlerde mikroişlemci yazmaçlar vasıtası ile denetleme ve okuma/yazma işlemlerini yapar.

Zamanlayıcıların ana parçaları sayaç değeri yazmacı ve karşılaştırma değeri yazmaçlarıdır. Buradaki değerler üzerinden sayaçlar çeşitli işlemleri yapar. Ayrıca ayar yazmaçları ile zamanlayıcının nasıl çalışacağı belirlenir. Giriş ve çıkış ayakları ile zamanlayıcılar dış dünya ile iletişime geçer.

Zamanlayıcılar çalışmak için mikroişlemciler gibi belli bir saat frekansına ihtiyaç duyar. AVR zamanlayıcıları bunu işlemcinin saat hızından edinir. Böylelikle işlemci ile aynı hızda çalışan bir zamanlayıcı elde etmiş oluruz. Bunu ön derecelendirici \(prescaler\) adı veren yapı ile bölüp yavaşlatmamız mümkündür. İşlemci hızında çalışan bir zamanlayıcı çoğu uygulama için oldukça hızlıdır.

Zamanlayıcıda zaman hesabı yapmak için **Zaman = 1 / Frekans** formülünü kullanırız. Örneğin 100Hz frekans girişinde zamanımız 1/100’den 0.01 yani 10 mili saniye olarak bulunur. Frekans/Zaman dönüşümü oldukça önemlidir ve sıkça kullanılır.

Zamanlayıcıda belli bir süre için kaça kadar sayması gerektiğini ise şu formülle buluruz,

**Sayaç Değeri = \( 1/Frekans\) / \(1 / Saat Hızı\) – 1**

Örneğin saniyenin yirmide biri kadar bir süreyi 1MHz frekansta çalışan işlemcide elde etmek için şöyle bir formül uygulanır.

Sayaç Değeri = \(1/20\) / \(1/1000000\) – 1  
=  0.05 / 0.000001 -1  
= 50000 – 1  
= 49999  
Yani sayacımız 49999’a kadar sayıp sonra tekrar sıfırlamalıdır. Bu karşılaştırma değerini ise karşılaştırma yazmacına bizim yazmamız gereklidir. Böylelikle sayaç değer yazmacı ile karşılaştırma değer yazmacı eşitlendiğinde taşma meydana gelir ve kesme yürütülür.

Ön derecelendiriciler ile bu hızlı sayan zamanlayıcıyı yavaşlatıp esneklik sağlatırız. Bunun için gerekli yazmaç bitleri ayarlanmadan önce aşağıdaki formülteki gibi bir hesaplama yapılır.

**Zaman Çözünürlüğü = 1 / \(Frekans / Ön derecelendirici\)** 

Örneğin ön derecelendirici olmaksızın \( yani 1 iken\) 1MHz saat hızında çözünürlük 1 mikrosaniye iken, ön derecelendirici 8 olduğunda 8 mikro saniye, 64 olduğunda 64 mikrosaniye, 256 olduğunda 256 mikrosaniye, 1024 olduğunda ise 1024 mikrosaniyedir. Yani saatimiz bir çevirimde bir tık atarken artık 1024 çevirimde bir tık atıyor. Böylelikle saati yavaşlatmış oluyoruz. Saatin her atmasında sayaç yazmacı değeri bir artmaktadır.

Şimdi ön derecelendiriciler ile beraber sayaç değeri hesaplamayı gösterelim. Aşağıdaki formülde hedeflenen sayaç değerini ön derecelendiricilerle elde etme gösterilmiştir.

**İstenilen Sayaç Değeri = \( 1 / Hedef Frekans \) / \( Ön derecelendirici / Frekans \)  -1**

Ya da

**Sayaç Değeri = \( Giriş Frekansı / Ön derecelendirici \* Hedef Frekans \) – 1** 

Şimdi 1 saniyelik bekleme için ön derecelendirici durumuna göre 1MHz’de ne kadar bir değer gerektiğine bakalım.

| Ön derecelendirici değeri | İstenilen Sayaç Değeri |
| :--- | :--- |
| 1 | 999999 |
| 8 | 124999 |
| 64 | 15624 |
| 256 | 3905.25 |
| 1024 | 975.5625 |

Küsüratlı sayılar kullanılamadığı için sadece bilgi vermek için yazılmıştır. Bu durumda mümkün olan tek değer 16 bit sayıcı için 64 değeridir.

Zamanlama işlemleri bu formüllere göre yapılır ve ön derecelendirici, zamanlama modu ayarları yazmaç bitlerine müdahale ile yapılır. Buraya kadar normal modu, CTC modunu \(karşılaştırmalı mod\), kesmeli CTC modunu örneklerle açıkladık. Temel zamanlayıcı fonksiyonlarını buraya kadar anlatabildiğimizi umuyoruz. Artık zamanlayıcıları burada bırakalım ve yeni konumuza  geçelim.

