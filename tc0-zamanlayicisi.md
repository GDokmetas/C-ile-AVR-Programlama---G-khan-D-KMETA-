# TC0 Zamanlayıcısı

Zamanlayıcılar oldukça uzun bir konu olduğu için bizi epey meşgul edecektir. Öncelikle zamanlayıcıları teknik veri sayfasından özetle tanıyıp sonrasında yazmaçları öğrenerek işe başlayalım. Elimizde tek bir zamanlayıcı yerine üç ayrı zamanlayıcı bulunduğu için bunları tek tek sırayla ele almamız gereklidir. Teknik veri sayfasında da bu üç zamanlayıcı ayrı başlıklar altında uzunca açıklanmıştır. Oldukça uzun sürecek konumuza TC0 zamanlayıcısını anlatmakla başlayalım.

Önceki yazımızda özelliklerini verdiğimiz TC0 modülü genel amaçlı 8-bit zamanlayıcı/sayıcı modülüdür. Ayrıca bağımsız karşılaştırma üniteleri ve PWM desteği mevcuttur. Basitleştirilmiş blok diyagramında görüleceği üzere CPU erişimli giriş ve çıkış yazmaçları, giriş ve çıkış bitleri, giriş ve çıkış ayakları koyu renkte gösterilmiştir. Aygıtın yazmaçları ve bit özelliklerine ise yazmaçları açıkladığımız sırada geleceğiz. TC0 modülünün blok diyagramı aşağıdaki gibidir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc0.png)](http://www.lojikprob.com/diger/c-ile-avr-programlama-26-tc0-zamanlayicisi/attachment/tc0/)

Resim: ATmega328P – Microchip Technology ,sf. 126,http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

Şema üzerinden incelediğimizde kabaca bir adet kontrol birimi, saat birimi ve zamanlayıcı biriminden oluştuğunu görüyoruz. Ayrıca pek çok yazmacın yer aldığını söylememiz mümkündür. Bu yazmaçların fazla olmasının yanında üç adet ayak çıkışı da vardır. Bunlar biri giriş olup Tn olarak isimlendirilmiştir. İki adet ise çıkış ayağı yer alıp dalga üretecine bağlıdır. Timer/Counter biriminin içinde yer alan TCNT yazmacı kontrol yapısına bağlı olduğu gibi veri yoluna da bağlıdır. Buradan bunun zamanlayıcı değerini barındıran yazmaç olduğunu söylememiz mümkündür.  Ayrıca alt tarafta yine ICPn olarak bir adet giriş ayağı daha bulunmaktadır. Şemayı yazmaçları anladıktan sonra tekrar gözden geçirdiğimizde çok daha anlaşılır olacaktır. Şimdi konumuza devam edelim.

Zamanlayıcıları anlatırken kullanılan üç tabir vardır. Bunlar alt \(bottom\), azami \(max\) ve üst \(top\) olarak isimlendirilir. Bunların anlamını sırasıyla açıklayalım.

**BOTTOM**: Sayıcı sıfır olduğunda BOTTOM noktasına varır.

**MAX** : Sayıcı 0xFF olduğunda azami değerine ulaşır. \(16-bit için 0xffff\)

**TOP** : Sayıcı sayım sırasında kendi en yüksek değerine ulaştığında TOP noktasına ulaşır. TOP değeri MAX değerine veya OCR1A yazmacındaki değere sabitlenebilir.

TC0 modülü ile ilgili geri kalan bilgiler yazmaçlar üzerinden anlatıldığı için öncelikle yazmaçları anlatalım.

**TCCR0A – TC0 Denetim Yazmacı** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/t1.png)](http://www.lojikprob.com/diger/c-ile-avr-programlama-26-tc0-zamanlayicisi/attachment/t1/)

**Bit 7:6 – COM0An : A kanalı için karşılaştırma çıkış modu \[ n = 1:0 \]**

Bu bitler karşılaştırma çıkışı ayağının \(OC0A\) davranışını belirler. Eğer COM0A bitlerinden biri bir \(1\) yapılırsa OC0A çıkışı normal port fonksiyonunu geçersiz kılar. Çıkış sürücüsünü etkinleştirmek için veri yönü yazmacı \(DDR\) ‘da OC0A ya karşılık gelen ayak bir \(1\) yapılmak zorundadır. OC0A bir ayağa bağlandığında COM0A\[1:0\] bitleri WGM0\[2:0\] bit ayarına bağımlıdır. Aşağıdaki tabloda WGM0\[2:0\] bitlerinin normal ya da CTC moduna alındığındaki COM0A bitlerinin durumunu görüyoruz.

**Karşılaştırma çıkış modu \(PWMsiz\)**

| **COM0A1** | **COM0A0** | **Açıklama** |
| :--- | :--- | :--- |
| 0 | 0 | Normal Port Çalışması, OC0A bağlı değil. |
| 0 | 1 | Karşılaştırma Eşleşmesinde OC0A’yı Aç/Kapa |
| 1 | 0 | Karşılaştırma Eşleşmesinde OC0A’yı Temizle |
| 1 | 1 | Karşılaştırma Eşleşmesinde OC0A’yı Aç |

Aşağıdaki tablo WGM0\[1:0\] bitleri hızlı PWM moduna alındığındaki COM0A \[1:0\] bitlerinin fonksiyonunu açıklar.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>COM0A1</b>
      </th>
      <th style="text-align:left"><b>COM0A0</b>
      </th>
      <th style="text-align:left"><b>Açıklama</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">0</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">Normal Port Çalışması, OC0A bağlı değil.</td>
    </tr>
    <tr>
      <td style="text-align:left">0</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">
        <p>WGM02 = 0 : Normal port çalışması, OC0A bağlı değil.</p>
        <p>WGM02 = 1 OC0A’yı karşılaştırma eşleşmesinde Aç/Kapa</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">Karşılaştırma eşleşmesinde OC0a’yı temizle ve OC0A’yı BOTTOM’da aç. (terslememe
        modu)</td>
    </tr>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">OC0A’yı karşılaştırma eşleşmesinde aç, OC0A’yı BOTTOM’da temizle.</td>
    </tr>
  </tbody>
</table>Aşağıdaki tablo WGM0\[2:0\]bitlerinin faz düzeltmeli PWM modundayken COM0A\[1:0\]bitlerinin işleyişini gösterir.

| COM0A1 | COM0A0 | Açıklama |
| :--- | :--- | :--- |
| 0 | 0 | Normal Port işleyişi, OC0A bağlı değil. |
| 0 | 1 |  WGM02 = 0 : Normal Port işleyişi, OC0A bağlı değil. WGM02 = 1 : OC0A’yı karşılaştırma eşleşmesine Aç/Kapa |
| 1 | 0 | OC0A’yı karşılaştırma eşleşmesinde yukarı sayarken temizle ve OC0A’yı BOTTOM’a ayarla. |
| 1 | 1 | OC0A’yı karşılaştırma eşleşmesinde yukarı sayarken ayarla ve OC0A yı aşağı sayarken temizle. |

**Bit 5:4 – COM0Bn : Kanal için karşılaştırma çıkış modu B \[ n = 1:0 \]**

Bu bitler çıkış karşılaştırma ayağının \(OC0B\) davranışını belirler. Eğer bir yada iki COM0B\[1:0\] bitleri bir \(1\) yapılırsa OCB0B o ayağın port işleyişini geçersiz kılar.  OC0B ayağa bağlandığında COM0B\[1:0\] fonksiyonu WGM0\[2:0\] bitine bağlı halde çalışır. Aşağıdaki tablo WGM0\[2:0\]bitlerinin normal CTC modundayken COM0B\[1:0\] bitlerinin fonksiyonunu vermiştir.

| **COM0B1** | **COM0B0** | **Açıklama** |
| :--- | :--- | :--- |
| 0 | 0 | Normal Port Çalışması, OC0B bağlı değil. |
| 0 | 1 | Karşılaştırma Eşleşmesinde OC0B’yı Aç/Kapa |
| 1 | 0 | Karşılaştırma Eşleşmesinde OC0B’yı Temizle |
| 1 | 1 | Karşılaştırma Eşleşmesinde OC0B’yı Aç |

Aşağıdaki tablo WGM0\[1:0\] bitleri hızlı PWM moduna alındığındaki COM0B \[1:0\] bitlerinin fonksiyonunu açıklar.

| **COM0B1** | **COM0B0** | **Açıklama** |
| :--- | :--- | :--- |
| 0 | 0 |  Normal Port Çalışması, OC0A bağlı değil. |
| 0 | 1 | Rezerve |
| 1 | 0 | Karşılaştırma eşleşmesinde OC0a’yı temizle ve OC0A’yı BOTTOM’da aç. \(terslememe modu\) |
| 1 | 1 | OC0A’yı karşılaştırma eşleşmesinde aç, OC0A’yı BOTTOM’da temizle. |

Aşağıdaki tablo WGM0\[2:0\]bitlerinin faz düzeltmeli PWM modundayken COM0B\[1:0\]bitlerinin işleyişini gösterir.

| COM0A1 | COM0A0 | Açıklama |
| :--- | :--- | :--- |
| 0 | 0 | Normal Port işleyişi, OC0A bağlı değil. |
| 0 | 1 |  Rezerve |
| 1 | 0 | OC0B’yi karşılaştırma eşleşmesinde yukarı sayarken temizle ve OC0B’yi BOTTOM’a ayarla. |
| 1 | 1 | OC0B’yi karşılaştırma eşleşmesinde yukarı sayarken ayarla ve OC0B’yi aşağı sayarken temizle. |

**Bit 1:0 – WGM0n : Dalga üreteci modu \[ n = 1:0\]**

TCCR0B yazmacındaki WGM02 bitiyle beraber kullanıldığında bu bitler sayıcının sayma sırasını denetler, azami \(TOP\) değerin kaynağını ve hangi tip dalga formunun kullanılacağını belirler. Çalışma modları olarak Normal, Karşılaşma eşleşmesinde zamanlayıcıyı temizleme \(CTC\) ve iki tip PWM modu kullanılabilir. Dalga formu üreteci modunun bit açıklaması aşağıdaki tablodaki gibidir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tbb.png)](http://www.lojikprob.com/diger/c-ile-avr-programlama-26-tc0-zamanlayicisi/attachment/tbb/)

Resim: ATmega328P – Microchip Technology , sf. 140,http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

Şimdilik TC0 modülünün sadece ilk yazmacını anlattık. Geriye anlatılacak pek çok başlık olduğu gibi daha 2 adet zamanlayıcı modülü daha var. Şimdilik dersi burada bitirelim ve sonraki derse geçelim

Kaynaklar :

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

