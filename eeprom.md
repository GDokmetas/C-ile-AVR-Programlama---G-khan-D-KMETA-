# EEPROM

EEPROM bellekler elektrik ile yazılıp silinebildiğinden üzerinde defalarca okuma ve yazma işlemi yapılabilir. Diğer eski ROM belleklerde bu bir defaya mahsus programlama veya ışık ile silme ve elektrikle programlama şeklinde olduğu için pek kullanışlı değildir. Çalışma ortamında entegre sadece kullanmaya mahsus olup üretim esnasında programlanır. ROM ve PROM bellekler bir Atari kaseti yapmak için uygun olsa da bizim yapacağımız projeler genellikle kontrol ve otomasyon devreleri olduğu için kullanıcının belirlediği ayar verileri ve diğer önemli verileri sürekli olarak okuyup yazmamız gerekecektir.  
EEPROMlar elektronik dünyasında oldukça yaygın olarak kullanılan hafıza birimleridir. Okuma ve yazma hızları biraz düşük kaldığı için günümüzde yüksek işlem gücü ve kapasite gerektiren uygulamalarda FLASH  bellekler kullanılsa da 8-bit seviyesinde en uygun hafıza birimleri EEPROMlardır.

ROM, PROM, EPROM ve EEPROM olarak zamanla gelişen bu hafıza üniteleri RAM belleklerden oldukça farklı bir yapıya sahiptir. ROM bellek kalıcı bellek olmasından dolayı aynı zamanda fiziksel bellek özelliği taşır. RAM bellekte bilgi sadece elektrikten ibaret iken ROM bellekte farklı statüye geçip fiziksel olarak kaydedilir. Bu fiziksel kayıt aynı zamanda elektrikle de okunup yazılabilir özelliktedir. Kısacası EEPROM bellek aynı bir flash bellek gibi kullanıcı verilerinin kalıcı olarak depolandığı hafıza birimidir. Uygulama esnasında kaydedilen bu veriler programdan bağımsızdır.

ATmega328P entegresinde 1KB EEPROM bulunur ve bu EEPROMu programlamak ve okumak için özel EEPROM yazmaçları vardır. Bu yazmaçların belli bitlerine belli verileri yazarak okuma işlemini yaparız. Bizim için iki önemli değer vardır. Birincisi adres ikincisi ise veridir. EEPROMlarda her veri 8 bitlik adreslere bağlı veri hücrelerinde tutulur. Bu veri hücrelerindeki veriyi okumak için adres bilgisi şarttır. Bu adresleri aynı bir çizgili defterin satırları gibi düşünebiliriz. Bu satırlar defterde numaralı olmasa da burada numaralandırılmış ve 0, 1, 2, 3 vd. olarak devam etmektedir. Her satırda ise 8 bitlik yani 0-255 arası bir sayı değeri veya harf saklanabilir. Bu saklanan veri yıllar boyu üzerine bir veri yazılmadıkça orada bulunur. Cihaz yıllar boyu çalışmasa dahi silinmez. Program hafızası FLASH bellek olup EEPROMdan daha üstün bir teknolojide olsa da uygulama esnasında program hafızası ROM bellek olarak çalışır. Yani sadece program esnasında yazılıp sonradan yazılıp silinemez. Elimizde sadece okunabilir kalıcı bellek olarak program hafızası bulunduğu için mikrodenetleyicilerde EEPROM bellekleri genellikle kullanıcı belleği olarak kullanırız.

Yarı iletken üreticileri harici entegre olarak EEPROM bellekleri de üretmektedir. Bu bellekler SPI, I2C, Seri veya Paralel iletişim protokolleriyle mikroişlemci ve mikrodenetleyiciler tarafından kullanılabilmektedir. ATmega mikrodenetleyicilerde dahili EEPROM bulunması bizi ekstra bir masraftan kurtaracaktır.

EEPROMlar hakkında aşırı derece teknik bilgiye ihtiyacımız yoktur. Çünkü prensibi ve kullanması oldukça basittir. EEPROM üretmeyeceğimiz için bu kadarını bilmemiz yeterli olur. Bir sonraki derste her zaman olduğu gibi yazmaçları anlatarak konumuza devam edeceğiz ve sonrasında örnek kodları inceleyeceğiz.

_**Kaynaklar**_

DÖKMETAŞ, Gökhan, “_Arduino Eğitim Kitabı”,_ İstanbul, 2016

