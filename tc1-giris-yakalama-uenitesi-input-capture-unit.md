# TC1 Giriş Yakalama Ünitesi \(Input Capture Unit\)

Önceki yazımızda TC1 zamanlayıcısının sayıcı ünitesinden bahsetmiş ve dersi bitirmiştik. Şimdi geri kalan birimlerinden bahselim ve sonrasında yazmaçlara geçelim.

#### **Giriş Yakalama Ünitesi \(Input Capture Unit\)**

TC1 zamanlayıcısı harici olayları birleştiren ve bunlara geçen süre zarfında zaman damgası veren bir ünitedir. Harici sinyal tek ya da çoklu olayları belirtir ve ICP1 ayağından uygulanır. Alternatif olarak analog karşılaştırıcı ünitesinden de yararlanılabilir. Bu zaman damgası frekans, görev döngüsü \(duty-cycle\) ve diğer uygulamaları hesaplamada kullanılabilir.  Alternatif olarak zaman damgaları olay kütüğü \(log\) oluşturmada kullanılabilir.

Giriş yakalama ünitesi aşağıdaki blok diyagramında gösterilmiştir. Gri boyalı parçalar giriş yakalama ünitesinin asıl elemanlarından değildir. Yazmaç ve bitlerdeki “n” harfi zamanlayıcının numarasını temsil eder.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/tc13.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-30-tc1-giris-yakalama-unitesi-input-capture-unit/attachment/tc13/)

Giriş yakalama ayağında \(ICP1\) bir mantıksal seviye değişimi olduğunda \(olay gerçekleştiğinde\) ya da analog karşılaştırıcı çıkışında bir değişiklik olduğunda \(ACO\) bu değişiklik kenar algılayıcısı tarafından saptanır ve yakalama tetiklenir. 16 bitlik sayaç değeri \(TCNT1\) giriş yakalama yazmacına \(ICR1\) yazılır. Giriş yakalama bayrak biti \(ICF\) TCNT1 değerinin kopyalandığı aynı saat çeviriminde bir \(1\) konumuna geçer.  Eğer TIMSK1 yazmacındaki ICIE biti bir \(1\) yapılırsa giriş yakalama bayrak biti giriş yakalama kesmesini yürütür. ICF1 bayrağı kesme yürüdükten sonra otomatik olarak sıfırlanır. ICF bayrağı alternatif olarak yazılımla bir \(1\) yazılarak da sıfırlanabilir.

Giriş yakalama yazmacının \(ICR1\) değerini okumak için öncelikle bunun düşük baytını \(ICR1L\) okumak gerekir. Sonrasında ise yüksek bayt okunur \(ICR1H\). Düşük bayt okunduğu zaman yüksek baytın değeri TEMP yazmacına aktarılır.

ICR1 yazmacı ancak dalga formu üreteci modunda yazılabilir. Bu mod ICR1 yazmacının değerini sayacın TOP değeri olarak belirler. Öncelikle dalga formu üreteci yazmaçlarına değer yazılmalıdır sonra ise ICR1 yazmacına istenilen değer yazılmalıdır. ICR1 yazmacına veri yazarken öncelikle yüksek bayt ICR1H yazmacına sonra ise düşük bayt ICR1L yazmacına yazılmalıdır. Önceki yazıda 16-bitlik yazmaçlara erişimden bahsetmiştik.

**Giriş Yakalama Tetikleme Kaynağı**

Giriş yakalama ünitesinin ana tetikleme kaynağı ICP1 ayağıdır. Ayrıca analog karşılaştırıcının çıkışı da tetikleme kaynağı olarak kullanılabilir. Analog karşılaştırıcı analog karşılaştırıcı denetleme ve durum yazmacı \(ACSR\) yazmacının ACIC biti ile tetikleme kaynağı olarak seçilebilir. Tetikleme kaynağını değiştirmek bir yakalamayı tetikleyebilir. O yüzden giriş yakalama bayrak biti değişimden sonra sıfırlanmalıdır.

Giriş yakalama ayağı \(ICP1\) ve Analog karşılaştırıcı çıkışı \(ACO\) girişleri T1 ayağında olduğu gibi aynı teknikle örneklendirilir. Kenar algılayıcısı da özdeştir. Gürültü önleyici etkinleştirildiğinde kenar yakalayıcısından önce ek bir mantık işlemi yürütülür. Böylelikle sistemi dört sistem saat çevirimi daha bekletir. Gürültü engelleyicinin girişi ve kenar algılayıcı zamanlayıcı dalga formu üreteci moduna alınmadığı sürece etkindir. Giriş yakalama ICP1 ayağını kontrol ederek yazılım ile de yapılabilir.

**Gürültü Engelleyici**

Gürültü engelleyici basit dijital filtreleme işleminde gürültünün engellenmesi için kullanılır. Gürültü engelleyici girişi dört adet örnek alır ve bu dört örneğin kenar algılayıcı tarafından kullanılan çıkışı değiştirecek kadar eşit olmasını denetler. Gürültü engelleyici Giriş Yakalama Gürültü Önleyici bitini değiştirmekle etkin hale getirilebilir. Bu ICNC biti TCCR1B yazmacında bulunmaktadır. Etkin hale getirildiğinde gürültü engelleyicinin çalışması için ekstradan dört saat çevirimi daha kullanıldığından bu kadar bir bekleme söz konusudur.

**Giriş Yakalama Ünitesini Kullanmak** 

Giriş yakalama ünitesini kullanırken en önemli mesele gelen sinyali işleyecek yeterli işlemci kapasitesidir. İki sinyal olayı arasındaki zaman çok önemlidir. Eğer işlemci ICR1 yazmacındaki değeri bir sonraki yakalama olayı gerçekleştiremezse ICR1 üzerine yeni değer yazılır ve yakalama sonucu hatalı olur. Giriş yakalama kesmesini kullanırken ICR1 yazmacı kesme rutininde mümkün olduğu kadar erken okunmalıdır. TOP değerinin etkin olarak değiştirilmesi giriş yakalamada tavsiye edilmez. Harici sinyalin görev döngüsünü ölçmek için tetikleyici kenar her yakalamada değişmelidir. Kenar algılamayı ICR1 yazmacı değiştirildikten hemen sonra yapmak gerekir. Kenar değişiminden sonra Giriş Yakalama Bayrak Biti \(ICF\) yazılım tarafından sıfırlanmalıdır. \(yani mantıksal bir \(1\) yazılması gerekir. \) Frekans ölçümünde ICF bayrağını sıfırlamak gerekmez.

Kaynaklar:

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

Kapak Resmi :https://d3s11pzv7w3h1q.cloudfront.net/wp-content/uploads/IC-ATMEGA8A-PU-2.jpg

