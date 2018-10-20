# RESET \(Yeniden Başlatma\)

Yeniden başlatma \(Reset\) bilgisayarlardan tanıdık olduğumuz üzere kararsızlaşan bir sistemi tekrar kararlı hale sokmak için yürütülen bir düzenleme eylemidir. Yeniden başlatma özelliği olmayan makinalarda bile bunu el ile yapmamız mümkün olmaktadır. Kapatıp açmak, çoğu elektronik aleti onarmak için başvurduğumuz ilk yöntemlerden biridir.

Yeniden başlatma sürecinde giriş ve çıkış yazmaçları sıfırlanır, program ise reset vektöründen itibaren çalışmaya başlar. Bilgisayarlarda yeniden başlatma işletim sisteminden veya reset düğmesinden gerçekleştirildiği gibi AVR mikrodenetleyicilerde de çeşitli yeniden başlatma kaynakları vardır. Bu kaynaklar vasıtasıyla çeşitli yollardan yeniden başlatma işlemini gerçekleştirmiş oluruz.

Aşağıdaki listede yeniden başlatma kaynakları açıklanmıştır.

* Açılış Reseti \(Power-on Reset\) : Mikrodenetleyiciye çalışması için güç verildiği zaman ve bu güç de açılış reseti eşiğini geçtiği zaman yeniden başlatma işi gerçekleştirilir.
* Harici Reset : RESET ayağı belli bir süre sıfır \(0\) yapıldığında mikrodenetleyici kendini yeniden başlatır.
* WDT Sistem Reseti : Bekçi zamanlayıcısı periyodu geçtiği zaman gerçekleştirilen yeniden başlatmadır.
* Kararma \(Brown-out\) Reseti: Besleme gerilimi kararma eşiğinin altına düştüğünde gerçekleştirilen yeniden başlatmadır.

Aşağıdaki şemada reset mantık devresinin blok diyagramını görüyoruz.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/rst11.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-58-reset-yeniden-baslatma/attachment/rst11/)

**MCUSR – Mikrodenetleyici Durum Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/rst2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-58-reset-yeniden-baslatma/attachment/rst2/)

**Bit 3 – WDRF – Bekçi Zamanlayıcısı Sistem Reset Bayrak Biti**

Bekçi zamanlayıcısı sistem yeniden başlatma gerçekleştiğinde bu bit bir \(1\) konumuna gelir. Açılış yeniden başlatımında veya sıfır \(0\) yazılmasında bu bit eski haline gelir.

**Bit 2 – BORF : Kararma Reset Bayrak Biti**

Bu bit Brown-out reseti gerçekleştiğinde bir \(1\) konumuna gelir. Açılış reseti ile veya sıfır \(0\) yazma ile eski haline alır.

**Bit 1 – EXTRF : Harici Reset Bayrağı** 

Harici Reset durumunda bu bit bir \(1\) konumuna gelir. Açılış reseti ile veya sıfır \(0\) yazma ile eski haline alır.

**Bit 0 – PORF – Power-on Reset Bayrağı** 

Açılış resetidir. Sıfır \(0\) yazma ile eski haline alır.

Reset konusu bayağı kısa olduğu için bu kadarını anlatabildik. Bir sonraki derste bekçi zamanlayıcısını \(watch-dog timer\) ele alacağız.

_**Kaynaklar**_

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

