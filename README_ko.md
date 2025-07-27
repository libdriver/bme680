[English](/README.md) | [ 简体中文](/README_zh-Hans.md) | [繁體中文](/README_zh-Hant.md) | [日本語](/README_ja.md) | [Deutsch](/README_de.md) | [한국어](/README_ko.md)

<div align=center>
<img src="/doc/image/logo.svg" width="400" height="150"/>
</div>

## LibDriver BME680

[![MISRA](https://img.shields.io/badge/misra-compliant-brightgreen.svg)](/misra/README.md) [![API](https://img.shields.io/badge/api-reference-blue.svg)](https://www.libdriver.com/docs/bme680/index.html) [![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](/LICENSE)

BME680은 크기와 낮은 전력 소비가 중요한 모바일 애플리케이션 및 웨어러블 기기를 위해 설계된 통합 환경 센서입니다. Bosch Sensortec의 기존 환경 센서 제품군에 가스, 압력, 습도 및 온도 센서를 위한 고선형성 및 고정밀 센서를 최초로 통합하여 확장했습니다.

LibDriver BME680은 LibDriver에서 출시한 BME680의 모든 기능을 갖춘 드라이버입니다. 압력, 온도, 습도 및 가스 측정 기능을 제공합니다. LibDriver는 MISRA 표준을 준수합니다.

### 콘텐츠

  - [설명](#설명)
  - [설치](#설치)
  - [사용](#사용)
    - [example basic](#example-basic)
    - [example gas](#example-gas)
  - [문서](#문서)
  - [기고](#기고)
  - [저작권](#저작권)
  - [문의하기](#문의하기)

### 설명

/src 디렉토리에는 LibDriver BME680의 소스 파일이 포함되어 있습니다.

/interface 디렉토리에는 LibDriver BME680용 플랫폼 독립적인 IIC, SPI버스 템플릿이 포함되어 있습니다.

/test 디렉토리에는 LibDriver BME680드라이버 테스트 프로그램이 포함되어 있어 칩의 필요한 기능을 간단히 테스트할 수 있습니다.

/example 디렉토리에는 LibDriver BME680프로그래밍 예제가 포함되어 있습니다.

/doc 디렉토리에는 LibDriver BME680오프라인 문서가 포함되어 있습니다.

/datasheet 디렉토리에는 BME680데이터시트가 있습니다.

/project 디렉토리에는 일반적으로 사용되는 Linux 및 마이크로컨트롤러 개발 보드의 프로젝트 샘플이 포함되어 있습니다. 모든 프로젝트는 디버깅 방법으로 셸 스크립트를 사용하며, 자세한 내용은 각 프로젝트의 README.md를 참조하십시오.

/misra 에는 LibDriver misra 코드 검색 결과가 포함됩니다.

### 설치

/interface 디렉토리에서 플랫폼 독립적인 IIC, SPI버스 템플릿을 참조하여 지정된 플랫폼에 대한 IIC, SPI버스 드라이버를 완성하십시오.

/src 디렉터리, 플랫폼용 인터페이스 드라이버 및 자체 드라이버를 프로젝트에 추가합니다. 기본 예제 드라이버를 사용하려면 /example 디렉터리를 프로젝트에 추가합니다.

### 사용

/example 디렉터리의 예제를 참조하여 자신만의 드라이버를 완성할 수 있습니다. 기본 프로그래밍 예제를 사용하려는 경우 사용 방법은 다음과 같습니다.

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

### 문서

온라인 문서: [https://www.libdriver.com/docs/bme680/index.html](https://www.libdriver.com/docs/bme680/index.html).

오프라인 문서: /doc/html/index.html.

### 기고

CONTRIBUTING.md 를 참조하십시오.

### 저작권

저작권 (c) 2015 - 지금 LibDriver 판권 소유

MIT 라이선스(MIT)

이 소프트웨어 및 관련 문서 파일("소프트웨어")의 사본을 얻은 모든 사람은 이에 따라 무제한 사용, 복제, 수정, 통합, 출판, 배포, 2차 라이선스를 포함하여 소프트웨어를 처분할 수 있는 권리가 부여됩니다. 소프트웨어의 사본에 대한 라이선스 및/또는 판매, 그리고 소프트웨어가 위와 같이 배포된 사람의 권리에 대한 2차 라이선스는 다음 조건에 따릅니다.

위의 저작권 표시 및 이 허가 표시는 이 소프트웨어의 모든 사본 또는 내용에 포함됩니다.

이 소프트웨어는 상품성, 특정 목적에의 적합성 및 비침해에 대한 보증을 포함하되 이에 국한되지 않는 어떠한 종류의 명시적 또는 묵시적 보증 없이 "있는 그대로" 제공됩니다. 어떤 경우에도 저자 또는 저작권 소유자는 계약, 불법 행위 또는 기타 방식에 관계없이 소프트웨어 및 기타 소프트웨어 사용으로 인해 발생하거나 이와 관련하여 발생하는 청구, 손해 또는 기타 책임에 대해 책임을 지지 않습니다.

### 문의하기

연락주세요lishifenging@outlook.com.