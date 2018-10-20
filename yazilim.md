# Yazılım

Bundan önceki dört derste tamamen donanım yönünden AVR’ye giriş yaptık. Şimdi işin yazılım boyutunu ele alıyoruz. Bu yazılımlar arasında geliştirme stüdyosu, derleyici, programcı, metin editörü gibi gerekli ya da faydalı yazılımlar var. Bu yazılımların tamamını ücretsiz olarak indirip kullanabiliriz. Atmel’in en büyük özelliklerinden biri de ücretsiz geliştirme ortamı sunmasıdır. Visual Studio tabanlı bu geliştirme ortamı AVR Assembly, C ve C++ dillerinde yazılım geliştirme imkanı sunmaktadır.

> Sadece Assembly değil de AVR Assembly dememizin bir sebebi var. Çünkü Assembly dilinde makine kodu alfanümerik \(harf  ve rakam\) sembollere çevrilmiştir. Bu yüzden her mimarinin kendine ait bir assembly dili vardır. x86 assembly, z80 assembly, PIC assembly gibi ayrı diller olarak zikretmek gerekir. Tek bir assembly dili yoktur.

#### Atmel Studio

Atmel Studio, Atmel’in resmi olarak yayınladığı geliştirme stüdyosudur. Günümüzde 6.2 ve 7 versiyonları kullanılsa da biz 6.2 versiyonunu kullanmaya devam edeceğiz. Dileyen 7. sürümünü kullanabilir arasında fazla fark yoktur.  Atmel Studio’yu aşağıdaki bağlantılardan indirip kurabilirsiniz.

[http://www.microchip.com/mplab/avr-support/avr-and-sam-downloads-archive](http://www.microchip.com/mplab/avr-support/avr-and-sam-downloads-archive)

#### CH340 Sürücüsü

Klon Arduino UNO kartında USB-TTL çevirici olarak CH340 entegresi bulunur. Arduino IDE yüklense dahi bu sürücü resmi olarak tanınmadığı için ayrıca yüklemek gerekir. Bunun için aşağıdaki bağlantıdan sürücüyü indirip yükleyebilirsiniz.  
[https://sparks.gogo.co.nz/assets/\_site\_/downloads/CH34x\_Install\_Windows\_v3\_4.zip](https://sparks.gogo.co.nz/assets/_site_/downloads/CH34x_Install_Windows_v3_4.zip)

#### AVRDUDE

Atmel Studio’da Arduino UNO’yu kullanabilmek için AVRDUDE programına ihtiyaç vardır. Bu programı  C:\AVRDUDE adresine çıkarıyoruz.  
[http://download.savannah.gnu.org/releases/avrdude/avrdude-5.11-Patch7610-win32.zip](http://download.savannah.gnu.org/releases/avrdude/avrdude-5.11-Patch7610-win32.zip)

#### Arduino

Bazen Arduino yazılımlarını bazen de kaynak kodunu AVR üzerinden inceleyebileceğimiz için Arduino derleyicisini indirmekte fayda vardır.  Aşağıdaki bağlantıdan indirebilirsiniz.

[https://www.arduino.cc/en/Main/Software](https://www.arduino.cc/en/Main/Software)

#### Notepad ++

Kod incelemede ve kod yazmada faydası olduğu için sıkça kullanacağımız bir kelime işlem programı olup ücretsiz olarak şu bağlantıdan indirilebilir.

[https://notepad-plus-plus.org/download/v7.5.8.html](https://notepad-plus-plus.org/download/v7.5.8.html)

İleride gerekli başka bir program olduğunda gerektiği zaman bunu dile getireceğiz. Başlangıç için gerekli yazılımlar bu kadarla sınırlıdır.

Kapak Resmi: https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/ATmega8\_01\_Pengo.jpg/1200px-ATmega8\_01\_Pengo.jpg

