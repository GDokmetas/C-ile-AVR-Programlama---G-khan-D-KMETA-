# Dış Kesme Yazmaçları

Dış kesme yazmaçlarını anlatmaya bu yazımızda devam ediyoruz. Şimdi yazmaçlara kaldığımız yerden devam edelim.

**EIMSK – Dış Kesme Maske Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint2.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-39-dis-kesme-yazmaclari/attachment/exint2/)

**Bit 1 – INT1 : Dış kesme talebi 1 etkin**

INT1 biti ve SREG yazmacındaki I biti bir \(1\) olduğunda harici ayak kesmesi etkin hale gelir. Dış kesme denetleme yazmacındaki \(EICRA\) kesme algılama denetim bitleri \(ISC11 ve ISC10\) bu kesmenin yükselen veya düşen kenarda ya da ayak durumuna göre olup olmayacağını kararlaştırır. INT1 ayağı çıkış olarak tanımlansa da bu ayağa uygulanan gerilim kesmeyi yürütecektir.

**Bit 0 – INT0 : Dış kesme talebi 0 etkin**

INT0 biti ve SREG yazmacındaki I biti bir \(1\) olduğunda harici ayak kesmesi etkin hale gelir. Dış kesme denetleme yazmacındaki \(EICRA\) kesme algılama denetim bitleri \(ISC01 ve ISC00\) bu kesmenin yükselen veya düşen kenarda ya da ayak durumuna göre olup olmayacağını kararlaştırır. INT0 ayağı çıkış olarak tanımlansa da bu ayağa uygulanan gerilim kesmeyi yürütecektir.

**EIFR – Dış Kesme Bayrak Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint3.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-39-dis-kesme-yazmaclari/attachment/exint3/)

**Bit 1 – INTF1 : Dış Kesme Bayrağı 1**

INT1 ayağındaki kenar veya mantık durumu değiştiğinde INT1 ayağı kesmeyi tetiklediğinde INTF1 biti bir \(1\) olur. Eğer SREG yazmacındaki I biti ve EIMSK yazmacındaki INT1 biti bir \(1\) yapılırsa işlemci kesme fonksiyonuna atlar. Bu bayrak kesme fonksiyonu yürütüldüğünde otomatik sıfırlanır. Ya da bu bayrağa bir \(1\) yazılarak sıfırlamak mümkündür. Bu bayrak INT1 kesmesi seviye kesmesi olarak ayarlandığında her zaman sıfır \(0\) konumundadır.

**Bit 0 – INTF0 – Dış Kesme Bayrağı 0** 

INT0 ayağındaki kenar veya mantık durumu değiştiğinde INT0 ayağı kesmeyi tetiklediğinde INTF0 biti bir \(1\) olur. Eğer SREG yazmacındaki I biti ve EIMSK yazmacındaki INT0 biti bir \(1\) yapılırsa işlemci kesme fonksiyonuna atlar. Bu bayrak kesme fonksiyonu yürütüldüğünde otomatik sıfırlanır. Ya da bu bayrağa bir \(1\) yazılarak sıfırlamak mümkündür. Bu bayrak INT0 kesmesi seviye kesmesi olarak ayarlandığında her zaman sıfır \(0\) konumundadır.

**PCICR – Ayak Değişimi Kesmesi Denetleme Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint4.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-39-dis-kesme-yazmaclari/attachment/exint4/)

**Bit 2 – PCIE2 : Ayak değişimi kesmesi Etkin 2**

PCIE2 biti ve SREG yazmacındaki I biti bir \(1\) konumuna getirilirse ayak değişimi kesmesi 2 etkinleştirilir. PCINT\[23:16\] ayaklarındaki bir değişim bu kesmeyi yürütür. PCINT\[23:16\] ayakları tek tek PCMSK2 yazmacından etkinleştirilebilir.

**Bit 1 – PCIE1 : Ayak değişimi kesmesi Etkin 1**

PCIE1 biti ve SREG yazmacındaki I biti bir \(1\) konumuna getirilirse ayak değişimi kesmesi 1 etkinleştirilir. PCINT\[14:8\] ayaklarındaki bir değişim bu kesmeyi yürütür. PCINT\[14:8\] ayakları tek tek PCMSK1 yazmacından etkinleştirilebilir.

**Bit 0 – PCIE0 : Ayak değişimi kesmesi Etkin 0**

PCIE0 biti ve SREG yazmacındaki I biti bir \(1\) konumuna getirilirse ayak değişimi kesmesi 0 etkinleştirilir. PCINT\[7:0\] ayaklarındaki bir değişim bu kesmeyi yürütür. PCINT\[7:0\] ayakları tek tek PCMSK0 yazmacından etkinleştirilebilir.

**PCIFR – Ayak Değişimi Kesmesi Bayrak Yazmacı**

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint5.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-39-dis-kesme-yazmaclari/attachment/exint5/)

**Bit 2 – PCIF2 : Ayak Değişimi Kesme Bayrağı 2** 

PCINT \[23:16\] ayaklarından birinde mantıksal değişim olduğunda kesme talebi tetiklenir ve bu durumda PCIF2 biti bir \(1\) konumuna geçer.

**Bit 1 – PCIF1 : Ayak Değişimi Kesme Bayrağı 1**

PCINT \[14:8\] ayaklarından birinde mantıksal değişim olduğunda kesme talebi tetiklenir ve bu durumda PCIF1 biti bir \(1\) konumuna geçer.

**Bit 0 – PCIF0 : Ayak Değişimi Kesme Bayrağı 0**

PCINT \[7:0\] ayaklarından birinde mantıksal değişim olduğunda kesme talebi tetiklenir ve bu durumda PCIF0 biti bir \(1\) konumuna geçer.

**PCMSK2 – Ayak Değişimi Maske Yazmacı 2** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint6.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-39-dis-kesme-yazmaclari/attachment/exint6/)

**Bit 0, 1 , 2, 4, 5, 6, 7 – PCINT 16, PCINT17, PCINT18, PCINT19, PCINT20, PCINT21, PCINT22, PCINT23 : Ayak değişimi Etkinleştirme Biti**

Bu yazmaç hangi ayaklardan kesme gerçekleştireceğini belirler. Bu bitlerden biri ya da birkaçı ve PCICR yazmacındaki PCIE2 biti bir \(1\)  yapılırsa uygun ayaktaki değişmeye göre kesme gerçekleşir. PCMSK2 yazmacındaki bitler sıfır \(0\) yapılırsa o kesme devre dışı bırakılır.

**PCMSK1 – Ayak Değişimi Maske Yazmacı 1** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint7.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-39-dis-kesme-yazmaclari/attachment/exint7/)

**Bit 0, 1, 2, 3, 4, 5, 6 – PCINT8, PCINT9, PCINT10, PCINT11, PCINT12, PCINT13, PCINT14 : Ayak Değişimi Etkinleştirme Biti**

Bu yazmaç hangi ayaklardan kesme gerçekleştireceğini belirler. Bu bitlerden biri ya da birkaçı ve PCICR yazmacındaki PCIE1 biti bir \(1\)  yapılırsa uygun ayaktaki değişmeye göre kesme gerçekleşir. PCMSK1 yazmacındaki bitler sıfır \(0\) yapılırsa o kesme devre dışı bırakılır.

**PCMSK0 – Ayak Değişimi Maske Yazmacı 0** 

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/exint8.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-39-dis-kesme-yazmaclari/attachment/exint8/)

**Bit 7:0 – PCINTn : Ayak Değişimi Etkinleştirme Biti \[ n = 7:0 \]**

Bu yazmaç hangi ayaklardan kesme gerçekleştireceğini belirler. Bu bitlerden biri ya da birkaçı ve PCICR yazmacındaki PCIE0 biti bir \(1\)  yapılırsa uygun ayaktaki değişmeye göre kesme gerçekleşir. PCMSK0 yazmacındaki bitler sıfır \(0\) yapılırsa o kesme devre dışı bırakılır.

Bütün dış kesme yazmaçlarını bitirdiğimize göre şimdi dış kesmelerin kullanıldığı bir örneği açıklayarak konumuzu bitirelim. Bu devrede dışarıdan bağlı bir düğme ve bir porta bağlı olan toplam 8 adet led bulunur. Düğmeye bastıkça bu ledler sıra ile yanacaktır. Programın en önemli özelliği ise bağlı olan düğmenin dış kesme ile kullanılmasıdır. Devremizin şeması aşağıdaki gibidir.

[![](http://www.lojikprob.com/wp-content/uploads/2018/09/INT0_0.png)](http://www.lojikprob.com/avr/c-ile-avr-programlama-39-dis-kesme-yazmaclari/attachment/int0_0/)

Resim : http://www.avr-tutorials.com/sites/default/files/INT0\_0.png

Şimdi kodu vererek bu kodu satır satır anlatalım.

```c
/*
 *  Yazar : AVR Tutorials
 *  Website: www.AVR-Tutorials.com
   Düzenleyen : Gökhan Dökmetaş
*/
 
#include <avr/io.h>
#include <avr/interrupt.h>
 
#define F_CPU 4000000UL
#include <util/delay.h>
 // Buraya kadar bildiğimiz kodlar. İşlemci 4MHz'de çalışacak.
#define DataPort	PORTC	// PORTC dataport olarak tanımlanıyor. 
#define DataDDR		DDRC    // Port adlarına böyle isim verebiliriz.
 
//INT0 için kesme fonksiyonu. Kesme yürütüldüğünde işletilecek kodlar.
ISR(INT0_vect)
{
	unsigned char i, temp;
 
	_delay_ms(500); // Düğme arkını engellemek için bekleme fonksiyonu.
 
	temp = DataPort;	// DataPort'un değerini ver. 
 
	/* Bu döngü dataporttaki ledleri beş kere yakar.*/
	for(i = 0; i<5; i++)
	{
		DataPort = 0x00;
		_delay_ms(500);	// 500 ms bekle
		DataPort = 0xFF;
		_delay_ms(500);	// 500 ms bekle
	}
 
	DataPort = temp;	//Dataporta eski değerini ver. (0)
}	
 
int main(void)
{
	DDRD = 1<<PD2;		// ,INT0 kesmesi için kullanılan PD2 ayağı giriş
	PORTD = 1<<PD2;		// Dahili pull-up dirençlerini etkinleştir.
 
	DataDDR = 0xFF;		// Dataportu çıkış yap
	DataPort = 0x01;	// 1 değerini yükle. 
 
	EIMSK = 1<<INT0;		// INT0 kesmesini etkinleştir
	EICRA = 1<<ISC01 | 1<<ISC00;	// Yükselen kenarda INT0 kesmesi yürüyecek
 
	sei();				//Genel kesmeleri etkinleştir.
 
    while(1)
    {
		if(DataPort >= 0x80)
			DataPort = 1;
		else
			DataPort = DataPort << 1;	// Birer bit sola kaydır
 
		_delay_ms(500);	// 500 mili saniye bekle
    }
```

Bu program kodlardan anlaşıldığı üzere normal çalışma döngüsünde devreye bağlı olan sekiz ledi sağdan sola sırayla 500 mili saniye aralıklarla yakacaktır. 0x80 değeri 0b10000000 değerine denk geldiği için 8. led yandığı zaman port tekrar 1 değerine çekilip ilk ledin yanması sağlanacaktır. Böylelikle bu kayan ışık programı sürekli olarak çalışacaktır. Bu sürekli çalışan programı kesmek için INT0 kesmesi kullanılmıştır. Bu kesmeyi yürütmek için PD2 ayağı giriş olarak tanımlanmış, buna bir düğme bağlanmış ve dahili pull-up özelliği etkinleştirilmiştir. Böylelikle düğmeye basıldığında kesme yürütülecektir. Dış kesmeler için kullanılan iki komut ve kesme fonksiyonu vardır. Geri kalan komutlar temel giriş ve çıkış özelliğinde olduğu için derslerin en başında anlatılmıştır.

**ISR\(INT0\_vect\)**  Bu kesme fonksiyonu INT0 kesmesi yürütüldüğünde işletilecek kodları içinde bulundurur.

**GICR = 1&lt;&lt;INT0;**  GICR yazmacının INT0 biti bir \(1\) yapılarak INT0 kesmesi etkinleştirilir.

**EICRA = 1&lt;&lt;ISC01 \| 1&lt;&lt;ISC00;** Kesmenin ne zaman yürütüleceği EICRA yazmacının bitleri ile kararlaştırılır.

Bir sonraki derste yeni bir konuya geçeceğiz. Görüşmek üzere.

Kaynaklar:

AVR C Programming of External Interrupt, http://www.avr-tutorials.com/interrupts/avr-external-interrupt-c-programming, Erişim Tarihi : 02.09.2018

ATmega328P – Microchip Technology , http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P\_Datasheet.pdf, Erişim Tarihi: 25.08.2018

