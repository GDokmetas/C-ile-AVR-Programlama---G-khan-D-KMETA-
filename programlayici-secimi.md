# Programlayıcı Seçimi

Programlayıcılar bir mikrodenetleyici platformunun olmazsa olmazlarıdır. Programlayıcıya erişim olmadığı sürece elimizdeki mikrodenetleyicilere program atmamız mümkün olmaz. O yüzden bir mikrodenetleyici platformuna giriş yapmadan önce uygun programlayıcıyı temin etmek gerekir. Ülkemizde Atmel’in resmi programlayıcılarını elektronik sitelerinde pek göremesem de klon programlayıcıları rahatça bulabiliyorum. Ayrıca Aliexpress’de orjinal olmasa da uygun fiyata klon programlayıcıları temin etmek mümkün. Programlayıcının yanında hata ayıklama özelliği \(debugger\) olan cihazlar da vardır. Bunların klonu olmadığı gibi fiyatları da normal programlayıcılara göre oldukça yüksektir. Hata ayıklama özelliğinin olması programcıya büyük kolaylık sağlasa da başlangıçta bu özellikten feragat edebiliriz. Klonu çıkmış bir hata ayıklayıcı olsa da Atmel Studio 4 ile kullanılabildiğinden ondan bahsetme gereği duymuyoruz. Şimdilik iki klon programlayıcıyı tanıtmakla yetinelim.

**usbASP**

En uygun fiyatlı AVR programlayıcısı olup rahatlıkla bulunabilir. Üçüncü parti bir programlayıcı olsa da çoğu yazılım ile uyumludur. Atmel Studio ile doğrudan bir uyumluluğu olmasa da sonradan araç olarak eklenebilir. Bazı modellerinde 3.3V-5V arası seçim yapan bir anahtar vardır. Çoğunda ise 5 voltta çalışmaktadır. 5 voltta çalışması 3.3 voltluk devreler için sıkıntı doğurabilir. Gücünü USB’den alır ve bağlandığı devreye doğrudan bu beslemeyi sağlar.  İstenirse PCB görüntüsü alınıp elde de yapılabilir. Devresi paylaşıldığından pek çok üretici kendi programlayıcılarını üretmiştir.

usbASP sürücülerine ve devre şemasına şu bağlantıdan ulaşabilirsiniz.

[https://www.fischl.de/usbasp/](https://www.fischl.de/usbasp/)

![](http://www.lojikprob.com/wp-content/uploads/2018/08/New-USBASP-USBISP-AVR-Programmer-Free-Driver-USBASP-USBISP-AVR-51-AVR-ISP-Downloader-Programmer-for.jpg_640x640-300x300.jpg)

Resim: https://ae01.alicdn.com/kf/HTB1rtGASpXXXXayapXXq6xXFXXXG/New-USBASP-USBISP-AVR-Programmer-Free-Driver-USBASP-USBISP-AVR-51-AVR-ISP-Downloader-Programmer-for.jpg\_640x640.jpg

**AVRISP mkII**

Atmel’in resmi programlayıcısı olan bu cihazın klonları üretilmiştir. usbASP kadar uygun olmasa da yine de makul fiyatlara bulabileceğiniz bu cihaz Atmel Studio ile beraber kullanılabilir. usbASP’ye göre içinde pek çok özellik barındırır. Hata ayıklama özelliği olmasa da devrenin gerilimini otomatik ölçen sistemi, devreden ayrı beslemesi, flash ve eeprom programlayabilmesi, kısa devre koruması vardır.

[![](http://www.lojikprob.com/wp-content/uploads/2018/08/ATAVRISP2-300x300.jpg)](http://www.lojikprob.com/wp-content/uploads/2018/08/ATAVRISP2.jpg)

Resim: https://www.microchip.com/\_ImagedCopy/ATAVRISP2.jpg

İleri seviye bir programcı ve hata ayıklayıcı arayan Atmel ICE ürününe göz atabilir. Biz şimdilik hobiciler ve öğrenciler tarafından alınabilecek olan programcılardan bahsetmekle yetinelim. Bir sonraki konumuzda gerekli ve tavsiye ettiğimiz yazılımlardan bahsedeceğiz.

Kaynaklar:  
AVRISP MkII, Microchip, https://www.microchip.com/developmenttools/ProductDetails/atavrisp2, Erişim Tarihi: 15.08.2015  
usbASP, Thomas Fischl, https://www.fischl.de/usbasp/, Erişim Tarihi: 15.08.2015  
Kapak Resmi: https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/ATmega8\_01\_Pengo.jpg/1200px-ATmega8\_01\_Pengo.jpg

