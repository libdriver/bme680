[English](/README.md) | [ 简体中文](/README_zh-Hans.md) | [繁體中文](/README_zh-Hant.md) | [日本語](/README_ja.md) | [Deutsch](/README_de.md) | [한국어](/README_ko.md)

<div align=center>
<img src="/doc/image/logo.svg" width="400" height="150"/>
</div>

## LibDriver BME680

[![MISRA](https://img.shields.io/badge/misra-compliant-brightgreen.svg)](/misra/README.md) [![API](https://img.shields.io/badge/api-reference-blue.svg)](https://www.libdriver.com/docs/bme680/index.html) [![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](/LICENSE)

BME680は、小型化と低消費電力が求められるモバイルアプリケーションやウェアラブルデバイス向けに設計された統合型環境センサーです。ボッシュ・センサーテックの既存の環境センサーファミリーに、ガス、圧力、湿度、温度に対応する高直線性と高精度のセンサーを初めて統合し、ラインナップを拡充します。

LibDriver BME680は、LibDriverがリリースしたBME680のフル機能ドライバです。圧力、温度、湿度、ガス測定機能を提供します。LibDriverはMISRAに準拠しています。

### 目次

  - [説明](#説明)
  - [インストール](#インストール)
  - [使用](#使用)
    - [example basic](#example-basic)
    - [example gas](#example-gas)
  - [ドキュメント](#ドキュメント)
  - [貢献](#貢献)
  - [著作権](#著作権)
  - [連絡して](#連絡して)

### 説明

/ srcディレクトリには、LibDriver BME680のソースファイルが含まれています。

/ interfaceディレクトリには、LibDriver BME680用のプラットフォームに依存しないIIC, SPIバステンプレートが含まれています。

/ testディレクトリには、チップの必要な機能を簡単にテストできるLibDriver BME680ドライバーテストプログラムが含まれています。

/ exampleディレクトリには、LibDriver BME680プログラミング例が含まれています。

/ docディレクトリには、LibDriver BME680オフラインドキュメントが含まれています。

/ datasheetディレクトリには、BME680データシートが含まれています。

/ projectディレクトリには、一般的に使用されるLinuxおよびマイクロコントローラー開発ボードのプロジェクトサンプルが含まれています。 すべてのプロジェクトは、デバッグ方法としてシェルスクリプトを使用しています。詳細については、各プロジェクトのREADME.mdを参照してください。

/ misraはLibDriver misraコードスキャン結果を含む。

### インストール

/ interfaceディレクトリにあるプラットフォームに依存しないIIC, SPIバステンプレートを参照して、指定したプラットフォームのIIC, SPIバスドライバを完成させます。

/src ディレクトリ、プラットフォームのインターフェイス ドライバー、および独自のドライバーをプロジェクトに追加します。デフォルトのサンプル ドライバーを使用する場合は、/example ディレクトリをプロジェクトに追加します。

### 使用

/example ディレクトリ内のサンプルを参照して、独自のドライバーを完成させることができます。 デフォルトのプログラミング例を使用したい場合の使用方法は次のとおりです。

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

```c
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

### ドキュメント

オンラインドキュメント: [https://www.libdriver.com/docs/bme680/index.html](https://www.libdriver.com/docs/bme680/index.html)。

オフラインドキュメント: /doc/html/index.html。

### 貢献

CONTRIBUTING.mdを参照してください。

### 著作権

著作権（c）2015-今 LibDriver 全著作権所有

MITライセンス（MIT）

このソフトウェアおよび関連するドキュメントファイル（「ソフトウェア」）のコピーを取得した人は、無制限の使用、複製、変更、組み込み、公開、配布、サブライセンスを含む、ソフトウェアを処分する権利を制限なく付与されます。ソフトウェアのライセンスおよび/またはコピーの販売、および上記のようにソフトウェアが配布された人の権利のサブライセンスは、次の条件に従うものとします。

上記の著作権表示およびこの許可通知は、このソフトウェアのすべてのコピーまたは実体に含まれるものとします。

このソフトウェアは「現状有姿」で提供され、商品性、特定目的への適合性、および非侵害の保証を含むがこれらに限定されない、明示または黙示を問わず、いかなる種類の保証もありません。 いかなる場合も、作者または著作権所有者は、契約、不法行為、またはその他の方法で、本ソフトウェアおよび本ソフトウェアの使用またはその他の廃棄に起因または関連して、請求、損害、またはその他の責任を負わないものとします。

### 連絡して

お問い合わせくださいlishifenging@outlook.com。