# EEPROM Kütüphanesi

EEPROM konusunu var olan bir kütüphaneyi açıklayarak bitireceğiz. Sadece kütüphane açıklayarak da konuyu bitirebilsek de bu insanı hazırcılığa itecektir. AVR programlarken çoğu kütüphaneyi kendimiz yazmamız, var olan kütüphaneleri de kendimiz düzenlememiz gereklidir. Böylelikle kütüphaneye göre program değil programa göre kütüphane ortaya çıkacaktır. Bu alanda profesyonel bir iş ortaya koymak için bu şarttır.

AVR GCC derleyicisinde hazır olarak eeprom.h başlık dosyası vardır. Dışarıdan bir kütüphane indirmemize gerek yoktur ve Atmel Studio’yu yüklediğimiz andan itibaren bu kullanılabilir. Yine **include** direktifi ile bu başlık dosyasını programa dahil etmemiz gereklidir. Aşağıdaki kodu programın başında kullanarak EEPROM kütüphanesini programımıza dahil ederiz.

```c
#include <avr/eeprom.h>
```

Makro açıklaması ile devam ederken burada atladığımız makroların olduğunu söylememizde fayda var. Bu makrolar IAR AVR derleyicisine uyumluluk için tanımlanmış makrolar olup bizim işimize yaramamaktadır. Yazının sonunda kaynak olarak belirttiğimiz bağlantıdan kütüphanenin tamamını inceleyebilirsiniz.

**eeprom\_busy\_wait\(\)** 

Bu makro EEPROMda okuma ve yazma işleri yürütülürken işlemciyi döngüye sokarak beklemeye alır. Örnek kodlarda belirttiğimiz beklemeyi bu fonksiyonla sağlarız.

**eeprom\_is\_ready\(\)** 

EEPROM yeni bir okuma ve yazma işine hazır olduğunda bu fonksiyon bir \(1\) değerini geri döndürür. Bu fonksiyonun nasıl yazılacağını ise yazmaçları ve örnek kodları incelediğimiz için biliyoruz. Yani istesek kendi EEPROM kütüphanemizi de yazabiliriz.

```c
void eeprom_read_block(void * __dst, const void *__src, size_t __n)
```

 Bu fonksiyon EEPROMdan bir veri bloğunu okuyup SRAM belleğine aktarır. Veri bloğunun boyutu \_\_n ile belirlenir. \_\_n verisine yazılan değer kadar bayt okuması yapılır. \_\_src ise EEPROM adresi olup SRAM ise okunan değerin aktarılacağı adrestir. Veri blokları genellikle karakter dizileri olduğu için aktarım için dizi değişken tanımlanması gerekir. Örnek bir blok okuması aşağıdaki gibi olmalıdır.

```c
#include <avr/eeprom.h>
void main(void)
{
 uint8_t StringVerisi[10];
 eeprom_read_block((void*)&StringVerisi, (const void*)20, 10);
}
```

 Önce aktarılacak değerin adres değeri, sonra eeprom adresi \(burada 20 olarak belirtilmiş 0-1023 arası olacak.\) ve devamında uzunluk sabiti belirtilir. Bu fonksiyon 10 baytlık veriyi 20. adresten başlayarak StringVerisi dizisinin adresine sırayla kopyalar.

```c
uint8_t eeprom_read_byte( const uint8_t * __p)
```

 Bu fonksiyon EEPROMdan bir bayt okur ve bunu 8 bitlik değer olarak döndürür. \_\_p değişkeni okunacak adres verisini bulundurur. Burada “\*” işareti dikkatinizi çekmiştir. Bu işaretçi olarak alınan bir argüman değerini gösterir. Bunun için fonksiyon çağırılırken değerleri çevirmek gereklidir. Örnek kodda bu gösterilmiştir. Örneğin 50. adresteki bayt değerini okuyalım.

```c
#include <avr/eeprom.h>
void main(void)
{
 uint8_t okunandeger;
 okunandeger = eeprom_read_byte((uint8_t*)50);
}
```

```c
uint16_t eeprom_read_word( const uint16_t * __p)
```

 Bu fonksiyon 16 bitlik word tipinde değişken okumayı sağlar. Yukarıda olduğu gibi bu da işaretçi ile belirlendiği için argüman olarak göndereceğimiz sabitin işaretçiye dönüştürülmesi gereklidir. Aşağıdaki örnek kodda 50. adresteki word değişkeni okunmaktadır.

```c
#include <avr/eeprom.h>
void main(void)
{
 uint16_t wordverisi;
 wordverisi = eeprom_read_word((uint16_t*)50);
}
```

Yukarıdaki örneklerde olduğu gibi aşağıdaki fonksiyonların da bu şekilde kullanılması gereklidir.

```c
uint32_t eeprom_read_dword( const uint32_t * __p)
```

 Bu foksiyon 32-bit double word cinsinde değeri okur.

```c
float eeprom_read_float( const float * __p)
```

 Bu fonksiyon float cinsi değer okur.

```c
void eeprom_write_byte( uint8_t * __p, uint8_t __value)
```

 Bu fonksiyon byte cinsinde değer yazar.

```c
void eeprom_write_word( uint16_t * __p, uint16_t __value)
```

 Bu fonksiyon word cinsinde değer yazar.

```c
void eeprom_write_dword( uint32_t * __p, uint32_t __value)
```

 Bu fonksiyon double word cinsinde değer yazar.

```c
void eeprom_write_float( float * __p, float __value)
```

 Bu fonksiyon float cinsinde değer yazar.

```c
void eeprom_write_block( const void * __src, void * __dst, size_t __n)
```

Bu fonksiyon blok değeri yazmak için kullanılır.

Okuma ve Yazma işleminden başka bir de Update \(Güncelleme\) özelliği vardır. Bu özelliğin tek getirisi mevcut değer aynı ise tekrar yazma işleminin yürütülmemesidir. Böylelikle EEPROMun ömrü uzatılmış olur.  Bu özelliğin kullanıldığı fonksiyonların prototipleri aşağıdaki gibidir.

Bayt için,

```c
void eeprom_update_byte( uint8_t * __p, uint8_t __value)
```

 Word için,

```c
void eeprom_update_word( uint16_t * __p, uint16_t __value)
```

 Double word için,

```c
void eeprom_update_dword( uint32_t * __p, uint32_t __value)
```

 Float için,

```c
void eeprom_update_float( float * __p, float __value)
```

 Blok için,

```c
void eeprom_update_block( const void * __src, void * __dst, size_t __n)
```

 Bu fonksiyonların nasıl kullanılacağını örneklerle yukarıda açıkladığımız için tek tek açıklamaya gerek yoktur. Bu konuda anlatılacak pek bir şey kalmadığı için burada bitirelim ve yeni konuya geçelim.

**Kaynaklar**

&lt;avr/eeprom.h&gt;: EEPROM handling, http://nongnu.org/avr-libc/user-manual/group\_\_avr\_\_eeprom.html, Erişim Tarihi : 27.09.2018

