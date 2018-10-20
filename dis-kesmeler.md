# Dış Kesmeler

Önceki iki dersimizde kesmelere giriş yapmış ve iç kesmeleri anlatmıştık. Şimdi daha uzun bir konu olan dış kesmelere giriş yapacağız ve ardından kesme konusunu bitireceğiz.

Dış kesmeler adından da anlaşılacağı üzere mikrodenetleyicinin iç birimlerinde gerçekleşmeyip dışarıdan bir uyaranla gerçekleşen kesmelere denir. Örneğin mikrodenetleyicinin ayağına bir düğme bağladığımızda bunun sebep olduğu kesme dış kesme iken mikrodenetleyicinin USART veri alışverişinde kullandığı kesmeler iç kesmedir. Kısacası dış kesme dediğimiz INT ya da PCINT ayaklarına uygulanan elektriğin sebep olduğu kesmedir. Böylece mikrodenetleyici kesmeler ile sadece kendi birimleri ile değil dış dünya ile de etkileşime geçebilir. Bu ayakları dersin başlarında verdiğimiz şemadan görebilirsiniz.

Ayak değişimi kesme talebi 2 \(PCI2\) PCINT\[23:16\] ayaklarından herhangi biri değişime uğrarsa yürütülür. Ayak değişimi kesme talebi 1 \(PCI1\) ise PCINT \[14:8\] ayaklarından herhangi biri değişime uğrarsa yürütülür.  Ayak değişimi kesme talebi 0 \(PCI0\) ise PCINT\[7:0\] ayakları değişikliğe uğradığında yürütülür. PCINT ayaklarındaki ayak değişimi kesmesi asenkron olarak algılanır. Bu yüzden bu kesmeleri uyku modundayken uyandırmak için kullanabiliriz.

Dış kesmeler düşen kenarda veya yükselen kenarda tetiklenebilir. Bu ayar dış kesme denetim yazmacı A \(EICRA\) yazmacından yapılır. Ayak değişimi kesme zamanlaması diyagramı aşağıdaki gibidir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-38-dis-kesmeler/attachment/exint/)

Resim: ATmega328P – Microchip Technology , sf. 88,  http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

#### Yazmaçlar

**EICRA – Dış Kesme Denetim Yazmacı A** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint1.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-38-dis-kesmeler/attachment/exint1/)

**Bit 3:2 – ISC1n Kesme Algılama Denetimi \[ n=1:0 \]**

Dış kesme 1’in INT1 ayağından yürütülmesi için SREG yazmacının I bayrak biti ve uygun kesme maskesi bir \(1\) konumunda olmalıdır. Seviye ve kenarlara göre INT1 kesmesinin yürütülme tablosu aşağıda verilmiştir. INT1 ayağının değeri kenarı tespit etmeden önce algılanır. Bu kesmenin yürütülmesi için bu ayağa giden sinyalin en azından bir saat çevirimi uzunluğunda olması gereklidir. Daha kısa uzunlukta olan sinyallerin bir garantisi yoktur. Eğer düşük seviye kesme seçildiyse düşük seviyenin kesmeyi yürütecek kadar uzun sürmesi gerekir.

| **Değer** | **Açıklama** |
| :--- | :--- |
| 00 | INT1 LOW seviyesinde kesme yürütür |
| 01 | INT1’deki herhangi bir değişiklik kesme yürütür. |
| 10 | INT1’in düşen kenarı kesme yürütür. |
| 11 | INT1’in yükselen kenarı kesme yürütür. |

**Bit 1:0 – ISC0n : Kesme Algılama Denetimi 0 \[ n = 1:0 \]**

Dış kesme 0’ın INT1 ayağından yürütülmesi için SREG yazmacının I bayrak biti ve uygun kesme maskesi bir \(1\) konumunda olmalıdır.Seviye ve kenarlara göre INT0 kesmesinin yürütülme tablosu aşağıda verilmiştir. INT0 ayağının değeri kenarı tespit etmeden önce algılanır.

| Değer | Açıklama |
| :--- | :--- |
| 00 | INT0 LOW seviyesinde kesme yürütür |
| 01 | INT0’daki herhangi bir değişiklik kesme yürütür. |
| 10 | INT0’ın düşen kenarı kesme yürütür. |
| 11 | INT0’in yükselen kenarı kesme yürütür. |

Sonraki yazımızda dış kesme yazmaçlarına devam edeceğiz ve bu konuyu bitireceğiz.

Kaynaklar:

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

