[English](/README.md) | [ 简体中文](/README_zh-Hans.md) | [繁體中文](/README_zh-Hant.md) | [日本語](/README_ja.md) | [Deutsch](/README_de.md) | [한국어](/README_ko.md)

<div align=center>
<img src="/doc/image/logo.svg" width="400" height="150"/>
</div>

## LibDriver BME680

[![MISRA](https://img.shields.io/badge/misra-compliant-brightgreen.svg)](/misra/README.md) [![API](https://img.shields.io/badge/api-reference-blue.svg)](https://www.libdriver.com/docs/bme680/index.html) [![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](/LICENSE)

BME680是一款集成環境感測器，專為移動應用和可穿戴設備設計，其中尺寸和低功耗至關重要。 它通過首次集成氣體、壓力、濕度和溫度的高線性和高精度感測器，擴展了博世Sensortec現有的環境感測器系列。

LibDriver BME680是LibDriver推出的BME680全功能驅動，該驅動提供大氣壓強，溫度，濕度和氣體讀取等功能並且它符合MISRA標準。

### 目錄

  - [說明](#說明)
  - [安裝](#安裝)
  - [使用](#使用)
    - [example basic](#example-basic)
    - [example gas](#example-gas)
  - [文檔](#文檔)
  - [貢獻](#貢獻)
  - [版權](#版權)
  - [聯繫我們](#聯繫我們)

### 說明

/src目錄包含了LibDriver BME680的源文件。

/interface目錄包含了LibDriver BME680與平台無關的IIC、SPI總線模板。

/test目錄包含了LibDriver BME680驅動測試程序，該程序可以簡單的測試芯片必要功能。

/example目錄包含了LibDriver BME680編程範例。

/doc目錄包含了LibDriver BME680離線文檔。

/datasheet目錄包含了BME680數據手冊。

/project目錄包含了常用Linux與單片機開發板的工程樣例。所有工程均採用shell腳本作為調試方法，詳細內容可參考每個工程裡面的README.md。

/misra目錄包含了LibDriver MISRA程式碼掃描結果。

### 安裝

參考/interface目錄下與平台無關的IIC、SPI總線模板，完成指定平台的IIC、SPI總線驅動。

將/src目錄，您使用平臺的介面驅動和您開發的驅動加入工程，如果您想要使用默認的範例驅動，可以將/example目錄加入您的工程。

### 使用

您可以參考/example目錄下的程式設計範例完成適合您的驅動，如果您想要使用默認的程式設計範例，以下是它們的使用方法。

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

### 文檔

在線文檔: [https://www.libdriver.com/docs/bme680/index.html](https://www.libdriver.com/docs/bme680/index.html)。

離線文檔: /doc/html/index.html。

### 貢獻

請參攷CONTRIBUTING.md。

### 版權

版權 (c) 2015 - 現在 LibDriver 版權所有

MIT 許可證（MIT）

特此免費授予任何獲得本軟件副本和相關文檔文件（下稱“軟件”）的人不受限制地處置該軟件的權利，包括不受限制地使用、複製、修改、合併、發布、分發、轉授許可和/或出售該軟件副本，以及再授權被配發了本軟件的人如上的權利，須在下列條件下：

上述版權聲明和本許可聲明應包含在該軟件的所有副本或實質成分中。

本軟件是“如此”提供的，沒有任何形式的明示或暗示的保證，包括但不限於對適銷性、特定用途的適用性和不侵權的保證。在任何情況下，作者或版權持有人都不對任何索賠、損害或其他責任負責，無論這些追責來自合同、侵權或其它行為中，還是產生於、源於或有關於本軟件以及本軟件的使用或其它處置。

### 聯繫我們

請聯繫lishifenging@outlook.com。
