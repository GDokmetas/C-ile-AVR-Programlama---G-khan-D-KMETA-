# UART Kütüphanesi

avr-libc kütüphanesinde bir örnek projede yer alan usart kütüphanesi olsa da AVR’de bu tarz kütüphanelerin resmi sürümleri Arduino’da olduğu gibi yoktur. Bizim yapmamız gereken teknik veri sayfasını \(datasheet\) okumak ve bunun üzerinden C/C++ dilinde kod yazmaktır. Fakat AVR’nin açık kaynak programcılık anlayışında olan bir kullanıcı kitlesi olduğu için kullanıcılar tarafından yazılan kütüphaneler internette ücretsiz kullanıma sunulmaktadır. Biz de yazdığı kütüphaneleri AVR topluluğu tarafından yaygınca kullanılan bir geliştiricinin kütüphanesini inceleyip kütüphane referansını size anlatacağız. Amatör kütüphaneler oldukça fazla olsa da kaliteli ve taşınabilir \(portable\) kütüphane yazmak usta işidir. Bu inceleyeceğimiz kütüphane de genel olarak taşınabilirliği ve işlevi yüksek bir kütüphanedir. Bu kütüphanenin fonksiyonlarını tek tek açıklayacağız. Öncelikle kütüphane dosyalarını aşağıdaki bağlantıdan indirip bilgisayarımızda açalım.

[http://homepage.hispeed.ch/peterfleury/uartlibrary.zip](http://homepage.hispeed.ch/peterfleury/uartlibrary.zip)

Kütüphanenin referans kılavuzu yazıldığı için kütüphaneyi açıp tek tek kodları incelemek zorunda değiliz. O yüzden öncelikle değer tanımlamaları ve makrolar ile başlayalım ve sonra fonksiyonlara geçelim.

**\#define UART\_BAUD\_SELECT\( baudRate, xtalCpu \)   \(\(\(xtalCpu\) + 8UL \* \(baudRate\)\) / \(16UL \* \(baudRate\)\) -1UL\)**

Bu makro UART iletişim için baud oranınnı belirlemek için kullanılır. Önceki yazımızda baud oranını nasıl yazmaçlara yazmak için bir işlem uyguladığımızı anlatmıştık. Aynı işlem burada da uygulanmaktadır fakat xtalCpu ve baudRate adında iki değerden bahsedilmektedir. Bunlar bizim belirleyeceğimiz değerler olup xtalCpu sistemin saat hızı \(16000000UL gibi\) baudRate ise baud oranıdır \(9600 gibi\).

**\#define UART\_BAUD\_SELECT\_DOUBLE\_SPEED\( baudRate, xtalCpu \)   \( \(\(\(\(xtalCpu\) + 4UL \* \(baudRate\)\) / \(8UL \* \(baudRate\)\) -1UL\)\) \| 0x8000\)**

Bu makro yukarıdaki makronun aynısı olup Atmega’nın çift hız modunda kullandırır.

**\#define UART\_RX\_BUFFER\_SIZE   32**

UART iletişimde alıcının tampon belleğinin kaç bayt olacağını belirler. Bu değer 2’nin katları olmalıdır.

**\#define UART\_TX\_BUFFER\_SIZE   32**

UART iletişimde vericinin tampon belleğinin kaç bayt olacağını belirler. Bu değer 2’nin katları olmalıdır.

**void uart\_init\(unsigned int baudrate\)**

Bu fonksiyon UART birimini hazır hale getirmeye yarar. Argüman olarak aldığı baudrate ise baud oranıdır. UART\_BAUD\_SELECT\(\) makrosu ile aldığı baudrate değerini işlemci yazmaçlarına yazdırır.

**unsigned int uart\_getc \( void \)**

By fonksiyon tampon bellekteki bayt verisini almaya yarar. Yani UART okuma bu fonksiyon tarafından gerçekleştirilir. Bu fonksiyon değer olarak ilk sekiz bitte tampondan okunan veriyi son sekiz bitte \(üst baytta\) ise durum mesajını geri döndürür. Durum mesajları şu şekildedir,

* **0** : UART biriminden veri alımı başarılır
* **UART\_NO\_DATA** : Alınacak bir veri yok.
* **UART\_BUFFER\_OVERFLOW** : Tampondan veri taşması. Yeterince hızlı veri okunmadığı için tampondaki bazı veriler kayboldu.
* **UART\_OVERRUN\_ERROR** : UART’da overrun durumu oluştu. UART’ın UDR yazmacındaki mevcut karakter kesme tarafından yeni karakter gelmeden önce okunamadı. Bir veya birkaç karakter kayboldu.
* **UART\_FRAME\_ERROR** : Veri çerçevesi hatası

**void uart\_putc \( unsigned char data \)**

Gönderilecek veri tamponuna bir baytlık veri eklemeye yarar. Aldığı argüman unsigned char yani byte değerinde olup 0-255 arasıdır.

**void uart\_puts \( const char\* s \)**

Gönderilecek veri tampon belleğine harf dizisi verisi koymaya yarar .

**void uart\_puts\_p \( const char\* s\)**

Program hafızasına yerleştirilmiş harf dizisi verisini göndermeye yarar.

**void uart1\_init \(unsigned int baudrate\)**

İkinci uart birimi olan ATmega mikrodenetleyiciler için ikinci UART’ı tanımlamaya yarar.

**unsigned int uart1\_getc \(void\)**  
**void uart1\_putc \( unsigned char data \)**  
**void uart1\_put2 \( const char\* s\)**  
**void uart1\_puts\_p \(conts char\* s\)**

Bu fonksiyonlar UART1 için tanımlanmıştır ve yukarıda aynı addaki fonksiyonlarla aynı görevi gerine getirdiği için açıklama gereği duymuyoruz.  Şimdi kütüphanenin test programını inceleyelim.

```c
/*************************************************************************
Title:    Example program for the Interrupt controlled UART library
Author:   Peter Fleury <pfleury@gmx.ch>   http://tinyurl.com/peterfleury
File:     $Id: test_uart.c,v 1.7 2015/01/31 17:46:31 peter Exp $
Software: AVR-GCC 4.x
Hardware: AVR with built-in UART/USART

DESCRIPTION:
          This example shows how to use the UART library uart.c

*************************************************************************/
#include <stdlib.h>
#include <avr/io.h>
#include <avr/interrupt.h>
#include <avr/pgmspace.h>

#include "uart.h"


/* CPU saat hızını belirle değilse hata mesajı ver. */
#ifndef F_CPU
#error "F_CPU undefined, please define CPU frequency in Hz in Makefile"
#endif

/* Buradan UART baud oranını belirliyoruz */
#define UART_BAUD_RATE      9600      


int main(void)
{
    unsigned int c;
    char buffer[7];
    int  num=134;

    
    /*
     *  Initialize UART library, pass baudrate and AVR cpu clock
     *  with the macro 
     *  UART_BAUD_SELECT() (normal speed mode )
     *  or 
     *  UART_BAUD_SELECT_DOUBLE_SPEED() ( double speed mode)
     */
    uart_init( UART_BAUD_SELECT(UART_BAUD_RATE,F_CPU) ); 
    
    /*
     * kesmeleri etkinleştiriyoruz
     */
    sei();
    
    /*
     *  Transmit string to UART
     *  The string is buffered by the uart library in a circular buffer
     *  and one character at a time is transmitted to the UART using interrupts.
     *  uart_puts() blocks if it can not write the whole string to the circular 
     *  buffer
     */
    uart_puts("String stored in SRAM\n");
    
    /*
     * Transmit string from program memory to UART
     */
    uart_puts_P("String stored in FLASH\n");
    
        
    /* 
     * Use standard avr-libc functions to convert numbers into string
     * before transmitting via UART
     */     
    itoa( num, buffer, 10);   // convert interger into string (decimal format)         
    uart_puts(buffer);        // and transmit string to UART

    
    /*
     * Transmit single character to UART
     */
    uart_putc('\r');
    
    for(;;)
    {
        /*
         * Get received character from ringbuffer
         * uart_getc() returns in the lower byte the received character and 
         * in the higher byte (bitmask) the last receive error
         * UART_NO_DATA is returned when no data is available.
         *
         */
        c = uart_getc();
        if ( c & UART_NO_DATA )
        {
            /* 
             * no data available from UART 
             */
        }
        else
        {
            /*
             * new data available from UART
             * check for Frame or Overrun error
             */
            if ( c & UART_FRAME_ERROR )
            {
                /* Framing Error detected, i.e no stop bit detected */
                uart_puts_P("UART Frame Error: ");
            }
            if ( c & UART_OVERRUN_ERROR )
            {
                /* 
                 * Overrun, a character already present in the UART UDR register was 
                 * not read by the interrupt handler before the next character arrived,
                 * one or more received characters have been dropped
                 */
                uart_puts_P("UART Overrun Error: ");
            }
            if ( c & UART_BUFFER_OVERFLOW )
            {
                /* 
                 * We are not reading the receive buffer fast enough,
                 * one or more received character have been dropped 
                 */
                uart_puts_P("Buffer overflow error: ");
            }
            /* 
             * send received character back
             */
            uart_putc( (unsigned char)c );
        }
    }
    
}
```

Burada yukarıdan itibaren başlayarak kodları Türkçe açıklayalım. Öncelikle 17. satırda uart.h başlık dosyasını programımıza ekliyoruz. Sonrasında ise F\_CPU değerini tanımlamamız gerektiği için \(delay kütüphanesinde olduğu gibi\) bir hata mesajı ile bu denetlenmiş. Bunu da 21. ve 23. satırlar arasında görebiliriz. UART\_BAUD\_RATE değerini bizim tanımlamamız gerektiği için 26. satırda uart baud oranı değeri 9600 olarak tanımlanmış. 43. satırda uart\_init fonksiyonunun kullanımına örnek verilmiş ve bizim önceden belirlediğimiz değerler argüman olarak alınmış. 48. satırda ilk gördüğümüz sei\(\) fonksiyonu kullanılmıştır. Bu fonksiyon kesmeleri etkinleştirmek için  kullanılır. Kütüphane UART kesmeleri ile çalıştığı için bunu açmamız gerekir. 57. satırda uart\_puts\(\) fonksiyonu ile SRAM’da tutulan string verisi gönderilmiştir. 62. satırdaki uart\_puts\_p\(\) fonksiyonu ile de program hafızasında tutulan string verisi gönderilmiştir. 69. ve 70. satırlarda ise integer yani tam sayı değerinin nasıl string değerine dönüştürüleceği ve bunun gönderileceği gösterilmiştir. 76. satırda ise tek bir karakter verisinin UART ile nasıl gönderileceği gösterilmiştir.

87. satırda unsigned int olarak tanımlanan c değişkenine gelen UART verisi yüklenmiştir. 88. satırda kontrol yapıları ile c değişkeninde durum veya hata mesajları denetlenmiş ve buna göre kodların işletilmesi için örnek karar yapıları verilmiştir.

125. satırda ise alınan veriyi tekrar göndermek için bir kod yazılmıştır.

UART kütüphanesini bu örnek programdaki fonksiyonların sözdizimine ve kullanılışına bakılarak kullanmamız gereklidir. Şimdilik AVR’nin USART birimi ile ilgili anlatacaklarımızın sonuna geldik. Bir sonraki konuda görüşmek üzere.

