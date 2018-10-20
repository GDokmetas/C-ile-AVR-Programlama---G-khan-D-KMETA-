# TC1 Üniteleri

TC1 zamanlayıcısı başlığı altında zamanlayıcı ünitelerini teknik veri kitapçığından anlatmaya devam ediyoruz.

**Çıkış Karşılaştırma Üniteleri**

16-bit karşılaştırıcı devamlı olarak TCNT1 ve çıkış karşılaştırma yazmacını \(OCR1x\) karşılaştırır. Eğer TCNT, OCR1x’e eşit ise karşılaştırıcı eşleşme sinyali verir. Bu eşleşme sinyali çıkış karşılaştırma bayrak bitinde yani TIFR1 yazmacının OCFx bitinde görülür. OCFx bayrak biti kesme yürüdüğünde otomatik olarak sıfırlanır. Buna yazılımla bir \(1\) yazarak sıfırlamak da mümkündür. Dalga formu üreteci bu eşleşme sinyalini çıkış üretmek için kullanır. TOP ve BOTTOM sinyalleri dalga boyu üreteci tarafından uç durumları denetlemek için bazı çalışma modlarında kullanılır.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc14.png)](http://www.lojikprob.com/diger/c-ile-avr-programlama-30-tc1-uniteleri/attachment/tc14/)

Resim: ATmega328P – Microchip Technology , sf. 158, http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

Yukarıda blok diyagramı verilen çıkış karşılaştırma ünitesinde griye boyalı kısımlar ünitenin gerçek elemanları değildir.

OCR1x yazmacı toplamda 20 PWM modundan birini kullandığı zaman çift tamponlu modda olur. Normal ve karşılaştırmadan sonra temizle \(CTC\) modunda çift tamponlama devre dışıdır. Çift tamponlama etkin olduğunda işlemcinin OCR1x tampon yazmacına erişimi vardır. Çift tamponlama devre dışı olduğunda işlemci bu yazmaca doğrudan erişir.

OCR1x \(Tampon veya karşılaştırma\) yazmacındaki değer ancak bir yazma işlemi ile değiştirilebilir. Zamanlayıcı TCNT1 ve ICR1 yazmacındaki olduğu gibi otomatik olarak güncellemez. OCR1x yazmacının yüksek bayt verisi TEMP yazmacından okunmaz. Burada yüksek bayt önce yazılmalıdır. \(OCR1xH\)

PWM modunda değilken FOC1x bitine bir \(1\) yazarak karşılaştırıcının çıkışı zorla yürütülebilir. Zorla yürütünce OCF1x bayrağı bir \(1\) konumuna gelmez ve zamanlayıcıyı sıfırlamaz.

TCNT1 yazmacına işlemci tarafından gerçekleştirilen tüm yazım işlemi sonraki saat çevrimindeki karşılaştırma eşleşmesini engeller.

**Çıkış karşılaştırma ünitesini kullanmak**

TCNT1’e yazılan bir veri bir saat çevirimi kadar karşılaştırma eşleşmelerini durdurur. Eğer TCNT1 yazmacına yazılan veri OCR1x değerine eşit ise karşılaştırma eşleşmesi kaçırılır ve yanlış dalga formu üretimine sebep olur. TCNT1 yazmacına PWM modunda TOP değer yazılmamalıdır. TOP için karşılaştırma eşleşmesi görmezden gelinir ve sayaç 0xFFFF değerine doğru devam eder. Aynı şekilde sayaç aşağı doğru sayıyorsa BOTTOM değeri sayaca yazılmamalıdır.

OC1x ayarı DDR yazmacında ayak çıkış olarak tanımlanmadan önce yapılmalıdır. OC1x değerini ayarlamanın en kolay yolu FOC1x bitlerini normal modda kullanmaktır. OC1x yazmacı dalga formu değişikliğinde bile değeri korur.

**Karşılaştırma Eşleşme Çıkış Ünitesi**

Çıkış karşılaştırma modu biti \(TCCR1A yazmacındaki COM1x\[1:0\] bitleri\) iki işleve sahiptir. Dalga formu üreteci bu bitleri sonraki çıkış karşılaştırma eşleşmesinde \(OC1x\) belirleyici bit olarak kullanır.  İkinci olarak bu bitler OC1x ayağının çıkış kaynağını belirlemeye yarar.Aşağıdaki şemada TCCR1A yazmacının COM1x \[1:0\] bitlerinin ayarlarına göre değişen mantıksal devre gösterilmiştir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc111.png)](http://www.lojikprob.com/diger/c-ile-avr-programlama-30-tc1-uniteleri/attachment/tc111/)

Resim : ATmega328P – Microchip Technology , sf. 160, http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

Burada “n” ile belirtilen aygıt numarasıdır. “x” ile belirtilen ise çıkış karşılaştırma ünitesidir \(A/B\). Eğer TCCR1A.COM1x bitleri bir yapılırsa genel giriş ve çıkış işlevi çıkış karşılaştırma \(OC1x\) tarafından geçersiz kılınır. OC1x ayağının giriş ve çıkış denetimi yine DDR yazmacındadır. OC1x kullanılmadan önce DDR yazmacının uygun düşen ayağı çıkış yapılmalıdır.

**Karşılaştırma çıkış modu ve dalga formu üretimi**

Dalga boyu üreteci TCCR1A yazmacındaki COM1x bitlerini normal, CTC ve PWM modlarında farklı olarak kullanır. Tüm modlar için bu bitleri sıfır \(0\) yapmak herhangi bir işlev olmayacağını belirtir. Bu bitleri değiştirmek bir sonraki karşılaştırma eşleşmesine kadar faaliyete geçirmeyecektir. PWM olmayan modda TCRR1C yazmacının FOC1x biti ile bu durum hemen gerçekleştirilebilir.

Buraya kadar TC1 zamanlayıcısının iç yapısını ve ünitelerini anlattık. Bir seferde okuyup anlamanızı beklemiyoruz. Önemli olan okuyup zamanlayıcılar hakkında fikir edinmenizdir. Sonraki derste zamanlayıcı çalışma modlarından bahsedeceğiz ve yine datasheet üzerinden anlatmaya devam edeceğiz.

Kaynaklar

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

