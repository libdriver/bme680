### 1. Chip

#### 1.1 Chip Info

Chip Name: STM32F407ZGT6.

Extern Oscillator: 8MHz.

UART Pin: TX/RX PA9/PA10.

IIC Pin: SCL/SDA PB8/PB9.

SPI Pin: SCK/MISO/MOSI/CS  PA5/PA6/PA7/PA4.

### 2. Development and Debugging

#### 2.1 Integrated Development Environment

LibDriver provides both Keil and IAR integrated development environment projects.

MDK is the Keil ARM project and your Keil version must be 5 or higher.Keil ARM project needs STMicroelectronics STM32F4 Series Device Family Pack and you can download from https://www.keil.com/dd2/stmicroelectronics/stm32f407zgtx.

EW is the IAR ARM project and your IAR version must be 9 or higher.

#### 2.2 Serial Port Parameter

Baud Rate: 115200.

Data Bits : 8.

Stop Bits: 1.

Parity: None.

Flow Control: None.

#### 2.3 Serial Port Assistant

We use '\n' to wrap lines.If your serial port assistant displays exceptions (e.g. the displayed content does not divide lines), please modify the configuration of your serial port assistant or replace one that supports '\n' parsing.

### 3. BME680

#### 3.1 Command Instruction

1. Show bme680 chip and driver information.

   ```shell
   bme680 (-i | --information)
   ```

2. Show bme680 help.

   ```shell
   bme680 (-h | --help)
   ```

3. Show bme680 pin connections of the current board.

   ```shell
   bme680 (-p | --port)
   ```

4. Run bme680 register test.

   ```shell
   bme680 (-t reg | --test=reg) [--addr=<0 | 1>] [--interface=<iic | spi>]
   ```

5. Run bme680 read test, num means the test times, index is the gas index. 

   ```shell
   bme680 (-t read | --test=read) [--addr=<0 | 1>] [--interface=<iic | spi>] [--index=<index>] [--times=<num>]
   ```

6. Run bme680 read function, num means the read times. 

   ```shell
   bme680 (-e read | --example=read) [--addr=<0 | 1>] [--interface=<iic | spi>] [--times=<num>]
   ```

7. Run bme680 gas function, num means the read times, index is the gas index, ma the is idac ma, degree is the set degree, wait is the wait time in ms.

   ```shell
   bme680 (-e gas | --example=gas) [--addr=<0 | 1>] [--interface=<iic | spi>] [--index=<index>] [--idac=<ma>] [--degree=<degree>] [--wait=<ms>] [--times=<num>]
   ```

#### 3.2 Command Example

```shell
bme680 -i

bme680: chip is Bosch BME680.
bme680: manufacturer is Bosch.
bme680: interface is IIC SPI.
bme680: driver version is 1.0.
bme680: min supply voltage is 1.7V.
bme680: max supply voltage is 3.6V.
bme680: max current is 0.85mA.
bme680: max temperature is 85.0C.
bme680: min temperature is -40.0C.
```

```shell
bme680 -p

bme680: SPI interface SCK connected to GPIOA PIN5.
bme680: SPI interface MISO connected to GPIOA PIN6.
bme680: SPI interface MOSI connected to GPIOA PIN7.
bme680: SPI interface CS connected to GPIOA PIN4.
bme680: SCL connected to GPIOB PIN8.
bme680: SDA connected to GPIOB PIN9.
```

```shell
bme680 -t reg --addr=0 --interface=iic

bme680: chip is Bosch BME680.
bme680: manufacturer is Bosch.
bme680: interface is IIC SPI.
bme680: driver version is 1.0.
bme680: min supply voltage is 1.7V.
bme680: max supply voltage is 3.6V.
bme680: max current is 0.85mA.
bme680: max temperature is 85.0C.
bme680: min temperature is -40.0C.
bme680: start register test.
bme680: bme680_set_interface/bme680_get_interface test.
bme680: set interface spi.
bme680: check interface ok.
bme680: set interface iic.
bme680: check interface ok.
bme680: bme680_set_addr_pin/bme680_get_addr_pin test.
bme680: set addr pin low.
bme680: check addr pin ok.
bme680: set addr pin high.
bme680: check addr pin ok.
bme680: bme680_set_temperature_oversampling/bme680_get_temperature_oversampling test.
bme680: set temperature oversampling skip.
bme680: check temperature oversampling ok.
bme680: set temperature oversampling x1.
bme680: check temperature oversampling ok.
bme680: set temperature oversampling x2.
bme680: check temperature oversampling ok.
bme680: set temperature oversampling x4.
bme680: check temperature oversampling ok.
bme680: set temperature oversampling x8.
bme680: check temperature oversampling ok.
bme680: set temperature oversampling x16.
bme680: check temperature oversampling ok.
bme680: bme680_set_pressure_oversampling/bme680_get_pressure_oversampling test.
bme680: set pressure oversampling skip.
bme680: check pressure oversampling ok.
bme680: set pressure oversampling x1.
bme680: check pressure oversampling ok.
bme680: set pressure oversampling x2.
bme680: check pressure oversampling ok.
bme680: set pressure oversampling x4.
bme680: check pressure oversampling ok.
bme680: set pressure oversampling x8.
bme680: check pressure oversampling ok.
bme680: set pressure oversampling x16.
bme680: check pressure oversampling ok.
bme680: bme680_set_humidity_oversampling/bme680_get_humidity_oversampling test.
bme680: set humidity oversampling skip.
bme680: check humidity oversampling ok.
bme680: set humidity oversampling x1.
bme680: check humidity oversampling ok.
bme680: set humidity oversampling x2.
bme680: check humidity oversampling ok.
bme680: set humidity oversampling x4.
bme680: check humidity oversampling ok.
bme680: set humidity oversampling x8.
bme680: check humidity oversampling ok.
bme680: set humidity oversampling x16.
bme680: check humidity oversampling ok.
bme680: bme680_set_mode/bme680_get_mode test.
bme680: set mode forced.
bme680: check mode ok.
bme680: set mode sleep.
bme680: check mode ok.
bme680: bme680_set_filter/bme680_get_filter test.
bme680: set filter off.
bme680: check filter ok.
bme680: set filter coeff 1.
bme680: check filter ok.
bme680: set filter coeff 3.
bme680: check filter ok.
bme680: set filter coeff 7.
bme680: check filter ok.
bme680: set filter coeff 15.
bme680: check filter ok.
bme680: set filter coeff 31.
bme680: check filter ok.
bme680: set filter coeff 63.
bme680: check filter ok.
bme680: set filter coeff 127.
bme680: check filter ok.
bme680: bme680_set_spi_wire/bme680_get_spi_wire test.
bme680: set spi wire 3.
bme680: check spi wire ok.
bme680: set spi wire 4.
bme680: check spi wire ok.
bme680: bme680_set_spi_wire_3_data_interrupt/bme680_get_spi_wire_3_data_interrupt test.
bme680: enable spi wire 3 data interrupt.
bme680: check spi wire 3 data interrupt ok.
bme680: disable spi wire 3 data interrupt.
bme680: check spi wire 3 data interrupt ok.
bme680: bme680_get_new_data_status test.
bme680: new data status is true.
bme680: bme680_get_gas_measuring_status test.
bme680: gas measuring status is false.
bme680: bme680_get_measuring_status test.
bme680: get measuring status is false.
bme680: bme680_get_gas_measuring_index test.
bme680: gas measuring index is 0x00.
bme680: bme680_get_gas_valid_status test.
bme680: gas valid status is false.
bme680: bme680_get_heater_stability_status test.
bme680: heater stability status is false.
bme680: bme680_set_heat_off/bme680_get_heat_off test.
bme680: disable heat off.
bme680: check heat off ok.
bme680: enable heat off.
bme680: check heat off ok.
bme680: bme680_set_run_gas/bme680_get_run_gas test.
bme680: enable run gas.
bme680: check run gas ok.
bme680: disable run gas.
bme680: check run gas ok.
bme680: bme680_set_convert_index/bme680_get_convert_index test.
bme680: set convert index 0.
bme680: check convert index ok.
bme680: set convert index 1.
bme680: check convert index ok.
bme680: set convert index 2.
bme680: check convert index ok.
bme680: set convert index 3.
bme680: check convert index ok.
bme680: set convert index 4.
bme680: check convert index ok.
bme680: set convert index 5.
bme680: check convert index ok.
bme680: set convert index 6.
bme680: check convert index ok.
bme680: set convert index 7.
bme680: check convert index ok.
bme680: set convert index 8.
bme680: check convert index ok.
bme680: set convert index 9.
bme680: check convert index ok.
bme680: bme680_set_idac_heat/bme680_get_idac_heat test.
bme680: set index 0 idac heat 0xEE.
bme680: check idac heat ok.
bme680: set index 1 idac heat 0xDE.
bme680: check idac heat ok.
bme680: set index 2 idac heat 0xB8.
bme680: check idac heat ok.
bme680: set index 3 idac heat 0x5F.
bme680: check idac heat ok.
bme680: set index 4 idac heat 0xF3.
bme680: check idac heat ok.
bme680: set index 5 idac heat 0x65.
bme680: check idac heat ok.
bme680: set index 6 idac heat 0x3C.
bme680: check idac heat ok.
bme680: set index 7 idac heat 0x0C.
bme680: check idac heat ok.
bme680: set index 8 idac heat 0x84.
bme680: check idac heat ok.
bme680: set index 9 idac heat 0xB8.
bme680: check idac heat ok.
bme680: bme680_set_resistance_heat/bme680_get_resistance_heat test.
bme680: set index 0 resistance heat 0xD5.
bme680: check resistance heat ok.
bme680: set index 1 resistance heat 0x16.
bme680: check resistance heat ok.
bme680: set index 2 resistance heat 0x09.
bme680: check resistance heat ok.
bme680: set index 3 resistance heat 0x03.
bme680: check resistance heat ok.
bme680: set index 4 resistance heat 0xED.
bme680: check resistance heat ok.
bme680: set index 5 resistance heat 0xDF.
bme680: check resistance heat ok.
bme680: set index 6 resistance heat 0xBF.
bme680: check resistance heat ok.
bme680: set index 7 resistance heat 0xFB.
bme680: check resistance heat ok.
bme680: set index 8 resistance heat 0x90.
bme680: check resistance heat ok.
bme680: set index 9 resistance heat 0xB2.
bme680: check resistance heat ok.
bme680: bme680_set_gas_wait/bme680_get_gas_wait test.
bme680: set index 0 gas wait 0x1E.
bme680: check gas wait ok.
bme680: set index 1 gas wait 0xBB.
bme680: check gas wait ok.
bme680: set index 2 gas wait 0xFA.
bme680: check gas wait ok.
bme680: set index 3 gas wait 0xC8.
bme680: check gas wait ok.
bme680: set index 4 gas wait 0x2C.
bme680: check gas wait ok.
bme680: set index 5 gas wait 0xF9.
bme680: check gas wait ok.
bme680: set index 6 gas wait 0xA7.
bme680: check gas wait ok.
bme680: set index 7 gas wait 0xCF.
bme680: check gas wait ok.
bme680: set index 8 gas wait 0x2B.
bme680: check gas wait ok.
bme680: set index 9 gas wait 0xCA.
bme680: check gas wait ok.
bme680: bme680_resistance_heat_convert_to_register test.
bme680: 167.35C convert to register 0x45.
bme680: bme680_gas_wait_convert_to_register test.
bme680: 118ms convert to register 0x5D.
bme680: bme680_idac_heat_convert_to_register/bme680_idac_heat_convert_to_data test.
bme680: idac heat ma before is 4.10mA.
bme680: idac heat reg is 0x3E.
bme680: idac heat ma after is 4.00mA.
bme680: bme680_soft_reset test.
bme680: check soft reset ok.
bme680: finish register test.
```

```shell
bme680 -t read --addr=0 --interface=iic --times=3

bme680: chip is Bosch BME680.
bme680: manufacturer is Bosch.
bme680: interface is IIC SPI.
bme680: driver version is 1.0.
bme680: min supply voltage is 1.7V.
bme680: max supply voltage is 3.6V.
bme680: max current is 0.85mA.
bme680: max temperature is 85.0C.
bme680: min temperature is -40.0C.
bme680: start read test.
bme680: temperature is 27.05C.
bme680: pressure is 100117.32pa.
bme680: humidity is 63.56%.
bme680: gas index is 0.
bme680: gas resistance is 28742.22ohms.
bme680: temperature is 27.06C.
bme680: pressure is 100117.18pa.
bme680: humidity is 63.41%.
bme680: gas index is 0.
bme680: gas resistance is 21955.69ohms.
bme680: temperature is 27.07C.
bme680: pressure is 100117.12pa.
bme680: humidity is 63.20%.
bme680: gas index is 0.
bme680: gas resistance is 20062.96ohms.
bme680: finish read test.
```

```shell
bme680 -e read --addr=0 --interface=iic --index=0 --times=3

bme680: 1/3.
bme680: temperature is 27.07C.
bme680: pressure is 100119.45Pa.
bme680: humidity is 63.76%.
bme680: 2/3.
bme680: temperature is 27.07C.
bme680: pressure is 100119.38Pa.
bme680: humidity is 63.76%.
bme680: 3/3.
bme680: temperature is 27.07C.
bme680: pressure is 100119.41Pa.
bme680: humidity is 63.75%.
```

```shell
bme680 -e gas --addr=0 --interface=iic --index=0 --idac=5.0 --degree=200.0 --wait=150 --times=3

bme680: 1/3.
bme680: temperature is 27.06C.
bme680: pressure is 100117.84Pa.
bme680: humidity is 63.52%.
bme680: gas index is 0.
bme680: idac is 5.00mA.
bme680: degree celsius is 200.0C.
bme680: gas wait is 150 ms.
bme680: gas resistance is 20209.67ohms.
bme680: 2/3.
bme680: temperature is 27.07C.
bme680: pressure is 100117.73Pa.
bme680: humidity is 63.38%.
bme680: gas index is 0.
bme680: idac is 5.00mA.
bme680: degree celsius is 200.0C.
bme680: gas wait is 150 ms.
bme680: gas resistance is 19583.15ohms.
bme680: 3/3.
bme680: temperature is 27.08C.
bme680: pressure is 100117.81Pa.
bme680: humidity is 63.16%.
bme680: gas index is 0.
bme680: idac is 5.00mA.
bme680: degree celsius is 200.0C.
bme680: gas wait is 150 ms.
bme680: gas resistance is 19059.81ohms.
```

```shell
bme680 -h

Usage:
  bme680 (-i | --information)
  bme680 (-h | --help)
  bme680 (-p | --port)
  bme680 (-t reg | --test=reg) [--addr=<0 | 1>] [--interface=<iic | spi>]
  bme680 (-t read | --test=read) [--addr=<0 | 1>] [--interface=<iic | spi>] [--index=<index>] [--times=<num>]
  bme680 (-e read | --example=read) [--addr=<0 | 1>] [--interface=<iic | spi>] [--times=<num>]
  bme680 (-e gas | --example=gas) [--addr=<0 | 1>] [--interface=<iic | spi>] [--index=<index>] [--idac=<ma>]
         [--degree=<degree>] [--wait=<ms>] [--times=<num>]

Options:
      --addr=<0 | 1>                 Set the chip iic address.([default: 0])
      --degree=<degree>              Set the heater expected temperature in degree celsius.([default: 200.0])
  -e <read | gas>, --example=<read | gas>
                                     Run the driver example.
  -h, --help                         Show the help.
  -i, --information                  Show the chip information.
      --idac=<ma>                    Set the heater idac current in mA.([default: 5.0])
      --index=<index>                Set the heater index and it range is 0 - 9.([default: 0])
      --interface=<iic | spi>        Set the chip interface.([default: iic])
  -p, --port                         Display the pin connections of the current board.
  -t <reg | read>, --test=<reg | read>
                                     Run the driver test.
      --times=<num>                  Set the running times.([default: 3])
      --wait=<ms>                    Set the heater wait time in ms.([default: 150])
```

