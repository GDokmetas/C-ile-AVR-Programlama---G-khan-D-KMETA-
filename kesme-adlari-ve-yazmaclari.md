# Kesme Adları ve Yazmaçları

Önceki derste kesmelere bir giriş yapmıştık. Kesme konusu çok uzun olmadığı için şimdi kesme adlarını ve yazmaçlarını vererek iç kesmeleri bitireceğiz. Ondan sonra konumuz dış kesmeler olacak. Şimdi kesme adlarını ve yazmaçlarını vererek iç kesmeleri bitirelim.

**Kesme Adları** 

| **1** | **RESET** | **Reset kesmesi, harici ayak, açılışta, kararma resetinde ve WTC resette** |
| :--- | :--- | :--- |
| **2** | **INT0** | **Harici Kesme İsteği 0** |
| **3** | **INT1** | **Harici kesme İsteği 0** |
| **4** | **PCINT0** | **Ayak değişme kesme isteği 0** |
| **5** | **PCINT1** | **Ayak değişme kesme isteği 1** |
| **6** | **PCINT2** | **Ayak değişme kesme isteği 2** |
| **7** | **WDT** | **WDT Zamanlayıcısı Kesmesi** |
| **8** | **TIMER2\_COMPA** | **Zamanlayıcı 2 Karşılaştırma Eşleşmesi A** |
| **9** | **TIMER2\_COMPB** | **Zamanlayıcı 2 Karşılaştırma Eşleşmesi B** |
| **10** | **TIMER2\_OVF** | **Zamanlayıcı 2 Taşma Kesmesi** |
| **11** | **TIMER1\_CAPT** | **Zamanlayıcı 1 Yakalama Olayı**  |
| **12** | **TIMER1\_COMPA** | **Zamanlayıcı 1 Karşılaştırma Eşleşmesi A** |
| **13** | **TIMER1\_COMPB** | **Zamanlayıcı 1 Karşılaşma Eşleşmesi B** |
| **14** | **TIMER1\_OVF** | **Zamanlayıcı 1 Taşma Kesmesi** |
| **15** | **TIMER0\_COMPA** | **Zamanlayıcı 0 Karşılaştırma Eşleşmesi A** |
| **16** | **TIMER0\_COMPB** | **Zamanlayıcı 1 Karşılaştırma Eşleşmesi B** |
| **17** | **TIMER0\_OVF** | **Zamanlayıcı 1 Taşma** |
| **18** | **SPI STC** | **SPI Transfer Tamamlandı** |
| **19** | **USART\_RX** | **USART Alım Tamamlandı** |
| **20** | **USART\_UDRE** | **USART Veri Yazmacı Boş** |
| **21** | **USART\_TX** | **USART Gönderim Tamamlandı**  |
| **22** | **ADC** | **ADC Çevirim Tamamlandı** |
| **23** | **EE READY** | **EEPROM Hazır** |
| **24** | **ANALOG COMP** | **Analog Karşılaştırıcı** |
| **25** | **TWI** | **I2C Arayüzü** |
| **26** | **SPM READY** | **SPM Hazır** |

**MCUCR Yazmacıo**

| Yazmacın Biti | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Bit Adı | —BOŞ— | BODS | BODSE | PUD | —BOŞ— | —BOŞ— | IVSEL | IVCE |
|  Erişim |  |  Y/O |  Y/O |  Y/O |  |  | Y/O | Y/O |

**Bit 6 – BODS : BOD Uyku**

BODS biti uyku sırasında BOD’u devre dışı bırakmak için bir \(1\) yapılmalıdır. Brown-Out Detection \(Kararma Saptaması\) mikrodenetleyiciye giden besleme geriliminin belli bir seviyeyi aşacak kadar kararması yani düşmesidir. Bu sayede yeterli gücü alamayan mikrodenetleyici kararsızlığa sebebiyet vermemek için kendini yeniden başlatır.

**Bit 5 – BODSE : BOD Uyku Etkin**

Bu bit BODS denetleme bitinin ayarını etkin hale getirir.

**Bit 4 – PUD : Pull-UP devre dışı**

Bu bir bir \(1\) yapıldığında temel giriş ve çıkış portlarının dahili pull-up özelliği devre dışı bırakılır.

**Bit 1 – IVSEL : Kesme Vektör Seçimi**

Eğer bu bir sıfır \(0\) yapılırsa kesme vektörleri flash program hafızasının başına yerleştirilir. Eğer bir \(1\) yapılırsa kesme vektörleri ön yükleyici bölümnün başına yüklenir.

**Bit 0 – IVCE : Kesme Vektör Değişikliği Etkin**

IVSEL bitindeki değişikliği uygulamak için IVCE biti bir \(1\) yapılmalıdır.

Buraya kadar kesmeler hakkında gereken bilgileri anlattık. Sonraki konumuz ise dış kesmeler olacaktır. Sonraki derste görüşmek üzere.

Kaynaklar:

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

