[English](/README.md) | [ 简体中文](/README_zh-Hans.md) | [繁體中文](/README_zh-Hant.md) | [日本語](/README_ja.md) | [Deutsch](/README_de.md) | [한국어](/README_ko.md)

<div align=center>
<img src="/doc/image/logo.svg" width="400" height="150"/>
</div>

## LibDriver BME680

[![MISRA](https://img.shields.io/badge/misra-compliant-brightgreen.svg)](/misra/README.md) [![API](https://img.shields.io/badge/api-reference-blue.svg)](https://www.libdriver.com/docs/bme680/index.html) [![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](/LICENSE) 

Der BME680 ist ein integrierter Umweltsensor für mobile Anwendungen und Wearables, bei denen Größe und geringer Stromverbrauch entscheidend sind. Er erweitert die bestehende Familie der Umweltsensoren von Bosch Sensortec um erstmals hochlineare und hochpräzise Sensoren für Gas, Druck, Feuchtigkeit und Temperatur.

LibDriver BME680 ist ein vollfunktionaler Treiber für BME680, der von LibDriver eingeführt wurde. Er bietet Funktionen zur Messung von Druck, Temperatur, Luftfeuchtigkeit und Gasen. LibDriver ist MISRA-kompatibel.

### Inhaltsverzeichnis

  - [Anweisung](#Anweisung)
  - [Installieren](#Installieren)
  - [Nutzung](#Nutzung)
    - [example basic](#example-basic)
    - [example gas](#example-gas)
  - [Dokument](#Dokument)
  - [Beitrag](#Beitrag)
  - [Lizenz](#Lizenz)
  - [Kontaktieren Sie uns](#Kontaktieren-Sie-uns)

### Anweisung

/src enthält LibDriver BME680-Quelldateien.

/interface enthält die plattformunabhängige Vorlage LibDriver BME680 IIC, SPI.

/test enthält den Testcode des LibDriver BME680-Treibers und dieser Code kann die erforderliche Funktion des Chips einfach testen.

/example enthält LibDriver BME680-Beispielcode.

/doc enthält das LibDriver BME680-Offlinedokument.

/Datenblatt enthält BME680-Datenblatt.

/project enthält den allgemeinen Beispielcode für Linux- und MCU-Entwicklungsboards. Alle Projekte verwenden das Shell-Skript, um den Treiber zu debuggen, und die detaillierten Anweisungen finden Sie in der README.md jedes Projekts.

/misra enthält die Ergebnisse des LibDriver MISRA Code Scans.

### Installieren

Verweisen Sie auf eine plattformunabhängige IIC, SPI-Schnittstellenvorlage und stellen Sie Ihren Plattform-IIC, SPI-Treiber fertig.

Fügen Sie das Verzeichnis /src, den Schnittstellentreiber für Ihre Plattform und Ihre eigenen Treiber zu Ihrem Projekt hinzu. Wenn Sie die Standardbeispieltreiber verwenden möchten, fügen Sie das Verzeichnis /example zu Ihrem Projekt hinzu.

### Nutzung

Sie können auf die Beispiele im Verzeichnis /example zurückgreifen, um Ihren eigenen Treiber zu vervollständigen. Wenn Sie die Standardprogrammierbeispiele verwenden möchten, erfahren Sie hier, wie Sie diese verwenden.

#### example basic

```C
#include "driver_bme680_basic.h"

uint8_t res;
uint32_t i;
float temperature_c;
float pressure_pa;
float humidity_percentage;

/* basic init */
res = bme680_basic_init(BME680_INTERFACE_IIC, BME680_ADDRESS_ADO_LOW);
if (res != 0)
{
    return 1;
}

...
    
/* loop */
for (i = 0; i < 3; i++)
{
    /* delay 1000ms */
    bme680_interface_delay_ms(1000);

    /* read data */
    res = bme680_basic_read((float *)&temperature_c, (float *)&pressure_pa, (float *)&humidity_percentage);
    if (res != 0)
    {
        (void)bme680_basic_deinit();

        return 1;
    }

    /* output */
    bme680_interface_debug_print("bme680: %d/%d.\n", i + 1, 3);
    bme680_interface_debug_print("bme680: temperature is %0.2fC.\n", temperature_c);
    bme680_interface_debug_print("bme680: pressure is %0.2fPa.\n", pressure_pa);
    bme680_interface_debug_print("bme680: humidity is %0.2f%%.\n", humidity_percentage);
    
    ...
}

...
    
/* deinit */
(void)bme680_basic_deinit();

return 0;
```
#### example gas

```C
#include "driver_bme680_gas.h"

uint8_t res;
uint32_t i;
float temperature_c;
float pressure_pa;
float humidity_percentage;
float ohms;
uint8_t index = 0;
float idac_ma = 5.0f;
float degree_celsius = 200.0f;
uint16_t gas_wait_ms = 150;

/* gas init */
res = bme680_gas_init(BME680_INTERFACE_IIC, BME680_ADDRESS_ADO_LOW);
if (res != 0)
{
    return 1;
}

...
    
/* loop */
for (i = 0; i < 3; i++)
{
    /* delay 1000ms */
    bme680_interface_delay_ms(1000);

    /* read data */
    res = bme680_gas_read(idac_ma, degree_celsius, gas_wait_ms, index,
                         (float *)&temperature_c, (float *)&pressure_pa, (float *)&humidity_percentage, (float *)&ohms);
    if (res != 0)
    {
        (void)bme680_gas_deinit();

        return 1;
    }

    /* output */
    bme680_interface_debug_print("bme680: %d/%d.\n", i + 1, 3);
    bme680_interface_debug_print("bme680: temperature is %0.2fC.\n", temperature_c);
    bme680_interface_debug_print("bme680: pressure is %0.2fPa.\n", pressure_pa);
    bme680_interface_debug_print("bme680: humidity is %0.2f%%.\n", humidity_percentage);
    bme680_interface_debug_print("bme680: gas index is %d.\n", index);
    bme680_interface_debug_print("bme680: idac is %0.2fmA.\n", idac_ma);
    bme680_interface_debug_print("bme680: degree celsius is %0.1fC.\n", degree_celsius);
    bme680_interface_debug_print("bme680: gas wait is %d ms.\n", gas_wait_ms);
    bme680_interface_debug_print("bme680: gas resistance is %0.2fohms.\n", ohms);
    
    ...
}

...
    
/* deinit */
(void)bme680_gas_deinit();

return 0;
```

### Dokument

Online-Dokumente: [https://www.libdriver.com/docs/bme680/index.html](https://www.libdriver.com/docs/bme680/index.html).

Offline-Dokumente: /doc/html/index.html.

### Beitrag

Bitte beachten Sie CONTRIBUTING.md.

### Lizenz

Urheberrechte © (c) 2015 - Gegenwart LibDriver Alle Rechte vorbehalten



Die MIT-Lizenz (MIT)



Hiermit wird jeder Person kostenlos die Erlaubnis erteilt, eine Kopie zu erhalten

dieser Software und zugehörigen Dokumentationsdateien (die „Software“) zu behandeln

in der Software ohne Einschränkung, einschließlich, aber nicht beschränkt auf die Rechte

zu verwenden, zu kopieren, zu modifizieren, zusammenzuführen, zu veröffentlichen, zu verteilen, unterzulizenzieren und/oder zu verkaufen

Kopien der Software und Personen, denen die Software gehört, zu gestatten

dazu eingerichtet werden, unter folgenden Bedingungen:



Der obige Urheberrechtshinweis und dieser Genehmigungshinweis müssen in allen enthalten sein

Kopien oder wesentliche Teile der Software.



DIE SOFTWARE WIRD "WIE BESEHEN" BEREITGESTELLT, OHNE JEGLICHE GEWÄHRLEISTUNG, AUSDRÜCKLICH ODER

STILLSCHWEIGEND, EINSCHLIESSLICH, ABER NICHT BESCHRÄNKT AUF DIE GEWÄHRLEISTUNG DER MARKTGÄNGIGKEIT,

EIGNUNG FÜR EINEN BESTIMMTEN ZWECK UND NICHTVERLETZUNG VON RECHTEN DRITTER. IN KEINEM FALL DARF DAS

AUTOREN ODER URHEBERRECHTSINHABER HAFTEN FÜR JEGLICHE ANSPRÜCHE, SCHÄDEN ODER ANDERE

HAFTUNG, OB AUS VERTRAG, DELIKT ODER ANDERWEITIG, ENTSTEHEND AUS,

AUS ODER IM ZUSAMMENHANG MIT DER SOFTWARE ODER DER VERWENDUNG ODER ANDEREN HANDLUNGEN MIT DER

SOFTWARE.

### Kontaktieren Sie uns

Bitte senden Sie eine E-Mail an lishifenging@outlook.com.