# Analog-Dijital Çevirici \(ADC\) Yazmaçları

Önceki yazımızda analog ve dijital çeviriciyi anlatmıştık ve en son yazmaçlarda kalmıştık. Şimdi geri kalan yazmaçları anlatalım ve analog-dijital çeviriciyi nasıl kullanacağımızı anlatmakla devam edelim.

**ADCL – ADC Veri Yazmacı \(Düşük Bit\)**

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/adcrs-1024x150.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/adcrs.png)

ADC çevirimi bittiği zaman sonuç bu iki yazmaç içerisinde bulunur. ADC çevirimi 10-bit olmasından dolayı 8-bitlik yazmaçlara sığmamakta ve yazmacın birine 8-bit veri ötekisine ise kalan 2 bitlik veri kaydedilir. O yüzden iki yazmacı da okuyup bu verileri birleştirerek ADC çevirim sonucunu elde ederiz. Bu yazmaçlar sadece okunabilir yazmaçlar olup ADLAR bitinin \(önceki yazıda bahsettik\) durumuna göre sola veya sağa hizalı şekilde verileri depolar.  Bitleri tek tek açıklamaya gerek yok çünkü 0-7 arası tüm bitler ADC çevirim sonucunu barındıran bitlerdir. Bu bitleri okuyup birleştirerek son işlemi yapmış olacağız.

**ADCH – ADC Veri Yazmacı \(Yüksek Bit\)**

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/adch.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/adch.png)

Bu yazmaç okunan ADC değerinin son 2 bitini \(soldan sağa ilk iki biti\) içinde saklar. Bu iki yazmaç veri okuma yazmacıdır ve sadece veri okumaya yarar. ADLAR bitine göre sola hizalandığında bu yazmaç 8-bitlik veri bulundurup diğer yazmaç son iki bitinde \(soldan iki bit\) kalan veriyi barındırır.

**ADCSRB – ADC Denetim ve Durum Yazmacı B** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/adcb-1024x147.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/adcb.png)

Bu yazmaç ADC denetim ve ayar bitlerini bulunduran yazmacın devamı niteliğinde üzerinde bazı ayar bitlerini barındırır.

**Bit 6 – ACME : Analog Karşılaştırıcı Faal**

Bu bit HIGH yapıldığı zaman ve ADC kapatıldığı zaman \(ADCSRA yazmacındaki ADEN biti sıfır olduğunda\) ADC çoklayıcısı analog karşılaştırıcıya negatif giriş seçer. Eğer bu bit sıfır olarak yazılırsa AIN1 ayağı analog karşılaştırıcı için negatif giriş olarak seçilir.

**Bit 2:0 – ADTSn : ADC Otomatik Tetikleyici Kaynağı \[n = 2:0 \]**

ADCSRA yazmacı içindeki ADATE biti HIGH yapıldıysa bu değerler ADC çevirimi için tetikleyici kaynağını belirler. Eğer ADATE sıfır olarak ayarlanırsa ADTS bitleri bir işleve sahip olmaz. Çeviriim seçili kesme bayrağının yükselen kenarında tetiklenir. Eğer ADCSRA içindeki ADEN biti HIGH yapılırsa çevirim başlar. Serbest Çevirim modunda ADC kesme bayrağı HIGH konumda olsa dahi tetiklenme olmaz.  Bu bitlerin görevleri aşağıdaki tablodaki gibidir.

| ADTS \[2:0\] | Tetikleme Kaynağı |
| :--- | :--- |
| 000 | Serbest Çalışma Modu |
| 001 | Analog Karşılaştırıcı |
| 010 | Dış Kesme İsteği 0 |
| 011 | Zamanlayıcı/Sayıcı0 karşılaştırma örtüşmesi A |
| 100 | Zamanlayıcı/Sayıcı0 Taşma |
| 101 | Zamanlayıcı/Sayıcı1 Karşılaştırma Örtüşmesi B |
| 110 | Zamanlayıcı/Sayıcı1 Taşma |
| 111 | Zamanlayıcı/Sayıcı1 Yakalama Olayı |

**DIDR0 – Dijital Giriş Devredışı Bırakma Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/devre-1024x145.png)](http://www.lojikprob.com/wp-content/uploads/2018/08/devre.png)

Bu yazmaca yazılan HIGH bitleri karşılık gelen ADC kanalındaki dijital girişi devre dışı bırakır.

Buraya kadar yazmaç kısmını anlatmış olduk. Şimdi bu yazmaçlar üzerinde nasıl ADC ölçümünün yapıldığını anlatmaya sıra geldi. Bunu da sonraki yazıda anlatacağız.

**Kaynaklar**

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 18.08.2018

Kapak Resmi, https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/ATmega8\_01\_Pengo.jpg/1200px-ATmega8\_01\_Pengo.jpg

