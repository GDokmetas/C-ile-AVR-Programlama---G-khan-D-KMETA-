# EEPROM Örnek Kod İncelemesi

 AVR Programlarken çoğu zaman dahili bir kütüphane olmasa da EEPROM kütüphanesi derleyicinin içinde gelmektedir. Aslında bu kütüphaneyi kullanıp bütün bu yazmaçlardan ve bitlerden kurtulmamız mümkün olsa da biz bu alanda literatür oluşturacak seviyede bir ders dizisi yazma gayretinde olduğumuz için işi mümkün olduğunca alt seviyede tutmaya çalışacağız. Böylelikle anlaşılmadık bir nokta kalmayacak. Şimdi teknik veri kitapçığında verilen örnek EEPROM okuma ve yazma kodlarını satır satır inceleyelim.

```c
unsigned char EEPROM_read(unsigned int uiAddress)
{
/* Önceki okuma ve yazmanın bitmesini bekle */
while(EECR & (1<<EEPE))
;
/* Adres değerini güncelle */
EEAR = uiAddress;
/* Okuma işlemini başlat. */
EECR |= (1<<EERE);
/* Veri yazmacını değer olarak döndür */
return EEDR;
}
```

Okuma işlemini anlatmakla işe başlayalım. Okuma işlemi oldukça basit olup adresteki değeri döndürmekten ibarettir. Bu adres verisi **unsigned int uiAddress** olarak programcı tarafından fonksiyon çağrılırken yazılır.

**while\(EECR & \(1&lt;&lt;EEPE\)\)**   EECR Yazmacındaki EEPE biti bir olduğu sürece program sonsuz döngüye sokulur. EEPE biti eğer bir okuma veya yazma işlemi yürütülüyorsa bir \(1\) konumunda olup işlem bitmesinin ardından donanım tarafından sıfır \(0\) konumuna çekilir. Yeni bir işlemin yürütülmesi için öncelikle önceki işlemin bitmesi gereklidir. Bu yüzden bu kontrol döngüsünü en başa alıyoruz.

**EEAR = uiAddress;**  Aynı kodun Assembly dilindeki halini incelediğimde EEARH ve EEARL yazmaçlarına ayrı ayrı değer atandığını gördüm. Fakat C dilinde programlama yaparken sadece EEAR yazmacında değer atamak yeterli oluyor. Normalde üst ve alt yazmaçlara ayrı ayrı değer atanması gerekse de burada böyle bir kolaylık getirilmiştir.

**EECR \|= \(1&lt;&lt;EERE\);**  EERE biti bir \(1\) yapılarak okuma işlemi yürütülür. Yazmaçları açıkladığımız önceki derste bu bitin okuma işlemini başlatmakla görevli olduğunu açıklamıştık.

**return EEDR;**  EEPROM Veri Yazmacı değer olarak döndürülür. Bu yazmaçta okuma işlemi bittikten sonra belirtilen adresteki okunan veri yer alır.

Sırada ise yazma işleminin yürütüldüğü bir fonksiyon var. Bunu da yukarıda olduğu gibi satır satır inceleyelim.

```c
void EEPROM_write(unsigned int uiAddress, unsigned char ucData)
{
/* Önceki yazma işleminin bitmesini bekle*/
while(EECR & (1<<EEPE))
;
/* Adres ve Veri yazmaçlarını ayarla*/
EEAR = uiAddress;
EEDR = ucData;
/*EEMPE bitini bir yap */
EECR |= (1<<EEMPE);
/* EEPE bitini bir yaparak yazmaya başlat.*/
EECR |= (1<<EEPE);
}
```

Görüldüğü gibi bu fonksiyon **void** ile başlıyor. Çünkü yazma işleminde herhangi bir değer okumayacağız. Öncelikle 10 bitlik adres verisini **unsigned int uiAddress** olarak, yazılacak veriyi ise **unsigned char ucData** diye argüman olarak alıyoruz.

**while\(EECR & \(1&lt;&lt;EEPE\)\) ;**  EECR Yazmacındaki EEPE biti bir olduğu sürece program sonsuz döngüye sokulur. EEPE biti eğer bir okuma veya yazma işlemi yürütülüyorsa bir \(1\) konumunda olup işlem bitmesinin ardından donanım tarafından sıfır \(0\) konumuna çekilir. Yeni bir işlemin yürütülmesi için öncelikle önceki işlemin bitmesi gereklidir. Bu yüzden bu kontrol döngüsünü en başa alıyoruz.

**EEAR = uiAddress;**    Programcı tarafından belirlenen 10 bitlik adres verisi EEAR yani EEPROM Adres Yazmacına yazılır. Yazma işleminden önce muhakkak doğru adresin yazılması gereklidir. Bu adresi ise programcı çakışmadan belirlemek zorundadır. Hangi verinin hangi adreste bulunacağı keyfimize bağlıdır. Önemli olan bu değerlerin üst üste gelip birbiri üzerine yazılmamasıdır.

**EEDR = ucData;**  char olarak bu veri yazılsa da bizim bir baytlık veri olarak anlamamız gereklidir. Bu değerlerin muhakkak işaretsiz \(unsigned\) olması gerektiğini söylememize gerek yoktur.

**EECR \|= \(1&lt;&lt;EEMPE\);** EEPROMa yazma işlemini yapmadan önce bu güvenlik bitini bir yapmamız gereklidir. Kontağı açıp marşa basmak gibi bir işlem olduğu için önce kontağı açmamız gereklidir. Bu görevi bu bit yerine getirmektedir.

**EECR \|= \(1&lt;&lt;EEPE\);** Bu bit ile yazma işlemini yürütmeye başlarız. Bütün bu hazırlıktan sonra son noktayı bu bite bir \(1\) yazarak koyarız ve işlem yürütülür.

Bir sonraki dersimizde EEPROM kütüphane fonksiyonlarını açıklayacağız ve konuyu bitireceğiz.

