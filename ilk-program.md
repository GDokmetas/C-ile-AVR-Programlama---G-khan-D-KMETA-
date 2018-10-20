# İlk Program

Bundan önceki 6 derste AVR’yi tanıtıp sizi AVR’ye hazırladık. Şimdi ilk programı işletme vakti. Fakat söylememiz gereken çok fazla teorik bilgi olduğu için sadece bir program yapıp sonra teorik bilgileri anlatmakla devam edeceğiz. Teorik bilgilerin bolluğu bu seviyede pek sorun olmayacaktır. Eğer mikrodenetleyici dünyasına yeni girdiyseniz ve pratik eksikliğiniz varsa daha basitleştirilmiş ve yani başlayan dostu olan Arduino’yu deneyebilirsiniz. Yazdığım [Arduino Eğitim Kitabı](https://www.dikeyeksen.com/products/arduino-egitim-kitabi) sıfırdan başlayanları AVR ile ilgili konuları anlayacak seviyeye getirecek düzeydedir. Sağlam bir temel oluşturmadan ileri seviye konuların anlamadan ve ezbere yapılma olasılığı çok fazladır. Ezberciliğin önüne geçmek için mümkün olduğu kadar ince ayrıntısıyla ve herkesin anlayacağı düzeyde açıklayarak fakat bir birikimi olan okuyucuları da sıkmayacak seviyede anlatmaya gayret edeceğim.

C ile AVR programlama mantığını anlamadan oldukça anlaşılmaz ve zor, mantığını anlayarak oldukça anlaşılır ve kolay gelecektir. C dilinde programlamamızın yanında elimizde belli sınırları ve özellikleri olan bir işlemci vardır. Portlar, Kesmeler, Zamanlayıcılar, İletişim, Sigortalar, ADC, PWM gibi konuları öğrendikten sonra geriye fazla bir şey kalmamaktadır. Bu işlemler de  genel olarak belli yazmaçların bit değerlerini okuma, bu yazmaçlara bit değeri yazma ve bu bit değerlerini işlemcide işleme veya kaydetme olarak yapılmaktadır.  Çevre birimleriyle ve dış dünya ile iletişimi çözdükten sonra geri kalan işimiz yazılımsal boyutta olacaktır.

**Arduino UNO’yu Atmel Studio’da Kullanmak**

Arduino UNO kartı Atmel Studio ile doğrudan kullanılamaz. Bunun için Atmel Studio’nun bir özelliği olan **Tool** \(Alet\) olarak tanımlanması gerekir. Fakat ondan da önce AVRDUDE programını yüklememiz gerekir. Bundan öncesinde AVRDUDE programından bahsetmiştik. Okumayanlar [buradan ](http://www.lojikprob.com/avr/c-ile-avr-programlama-5-yazilim/) okuyabilir. C sürücüsünde AVRDUDE adında bir klasör açıp indirdiğimiz arşiv dosyasını oraya çıkardıktan sonra Atmel Studio’yu açıyoruz. **Tools** kısmından **External Tools** sekmesini seçiyoruz.  
![](http://www.lojikprob.com/wp-content/uploads/2018/08/atmel1.png)

Burada açılan pencerede ADD düğmesine tıklayıp yeni bir alet oluşturuyoruz. Bilgiler resimdeki gibi girilmelidir.  
![](http://www.lojikprob.com/wp-content/uploads/2018/08/atmel2.png)

| **Title** | **Arduino UNO \(veya istediğiniz bir başlık\)** |
| :--- | :--- |
| **Command** | **C:\avrdude\avrdude.exe** |
| **Arguments** | **-C “C:\avrdude\avrdude.conf” -p atmega328p -c arduino -P COM9 -b 115200 -U flash:w:”$\(ProjectDir\)Debug\$\(ItemFileName\).hex”:i** |

**Use Output Window kutusunu işaretli değilse işaretliyoruz.** 

**COM9 yerine Aygıt Yöneticisinden baktığımız COM değerini yazıyoruz.**

Özellikle Arguments kısmındaki komutun doğru olarak yazıldığından emin olun. AVRDUDE programına gönderilecek bilgiler burada yer alır ve Arduino UNO’ya göre düzenlenmiştir. Eğer farklı bir kart ya da entegre kullanmayı istiyorsanız argümanların değerlerini değiştirerek programı işletebilirsiniz. Burada Atmel Studio AVRDUDE programı üzerinden Arduino kartına debug klasöründeki .hex dosyasını atacaktır.

Bu işlem ile beraber Atmel Studioda derleyeceğimiz program doğrudan Arduino UNO kartına atılacaktır. Ama bunun kısayolunu oluşturup ekrandaki bir düğmeye basarak işi kolaylaştıralım. Öncelikle şekildeki gibi bir düğme oluşturmak için sağ taraftaki oka tıklayıp Add or Remove Buttons sekmesinden Customize düğmesine tıklıyoruz.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/atmel3.png)

Açılan pencerede **Add Command…** düğmesine tıklıyoruz ve Tools sekmesinden External Command 1’i seçiyoruz.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/atmel4.png)

Böylelikle şimdilik Atmel Studio’da yapacağımız ek bir şey kalmamış oluyor. Şimdi yeni proje açalım ve ilk programı yazıp çalıştıralım.

Öncelikle File/New/ New Project diyoruz.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/atmel5.png)

Karşımıza iki dil ve o dillerdeki proje tipleri çıkıyor. Assembly ve C/C++ dilleri ayrı olduğu gibi C ve C++ projeleri de ayrı olarak açılmaktadır. GCC C Executable Project seçeneğini seçiyoruz ve projemize isim verdikten sonra projeyi açıyoruz.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/atmel6.png)

Proje ekranından sonra karşımıza aygıt seçme ekranı geliyor.  Atmega328P kullanacağımız için arama kısmına Atmega328P yazıyoruz. Ayrıca aygıtı geçtikten sonra teknik veri sayfası \(datasheet\) ve desteklenen araç listesi çıkıyor. Buranın ekran görüntüsünü verme gereği duymadım çünkü yukarıdaki ekran görüntüleri yeteri kadar fazla oldu. Şimdi karşımıza programı yazacağımız .c uzantılı dosya geliyor ve programı yüklemek için yukarıdaki Arduino UNO düğmesine tıklıyoruz.

![](http://www.lojikprob.com/wp-content/uploads/2018/08/atmel7.png)

Şimdilik Atmel Studio’yu kullanmak hakkında vereceğim bilgiler bu kadarı ile sınırlı olacak. Şimdi basit bir yanıp sönen led programı ile Arduino UNO’nun üzerindeki L etiketli ledi yakıp söndürelim.

```c
/*
 * GccApplication1.c
 *
 * Created: 16.8.2018 00:49:23
 *  Author: Gökhan Dökmetaş
 */ 
 
 
#include <avr/io.h>
#define F_CPU 16000000UL
#include "util/delay.h"
int main(void)
{
	DDRB = 0xFF;
	while(1)
	{
		PORTB ^= 0xFF; 
		_delay_ms(1000); 
	}
}
```

Programın açıklamasını burada yapmayacağız. Sadece deneme amaçlı bir program olarak burada bırakalım. İlerleyen derslerde PORT yazmaçlarını ve port manipulasyonunu ayrıntılarıyla açıklayacağız.

Kaynaklar:  
Using Arduino Boards in Atmel Studio, Sepehr Naimi, http://www.microdigitaled.com/AVR/Hardware/Arduino/UsingArduinoBoardsInAtmelStudio.pdf, Erişim Tarihi : 16.08.2018

