# Huidu HD-WF4

Huidu HD-WF4 LED controller card with ESP32-S3

I just got my Huidu HD-WF4 controller card and four 64x32 P4 fullcolor LED displays and realized that version v7.0.1-1 I have is powered by an ESP32-S3! To there might be an option to get CircuitPython installed on it in the future. This repository is intended to collect my findings.

## Using software from mrfaptastic

In one of the [issues posted in his repository](https://github.com/mrfaptastic/ESP32-HUB75-MatrixPanel-DMA/issues/433) we find some pictures and a pin config (reverse engineered by [hn](https://github.com/hn) in early 2023).

![picture of board closeup](https://user-images.githubusercontent.com/11134981/230600947-7caa452a-efea-4c9e-9074-29b69c5665de.jpg)

``` c
#define WF2_X1_R1_PIN 2
#define WF2_X1_R2_PIN 3
#define WF2_X1_G1_PIN 6
#define WF2_X1_G2_PIN 7
#define WF2_X1_B1_PIN 10
#define WF2_X1_B2_PIN 11
#define WF2_X1_E_PIN 21

#define WF2_X2_R1_PIN 4
#define WF2_X2_R2_PIN 5
#define WF2_X2_G1_PIN 8
#define WF2_X2_G2_PIN 9
#define WF2_X2_B1_PIN 12
#define WF2_X2_B2_PIN 13
#define WF2_X2_E_PIN -1        // Currently unknown, so X2 port will not work (yet) with 1/32 scan panels

#define WF2_A_PIN 39
#define WF2_B_PIN 38
#define WF2_C_PIN 37
#define WF2_D_PIN 36
#define WF2_OE_PIN 35
#define WF2_CLK_PIN 34
#define WF2_LAT_PIN 33

#define WF2_BUTTON_TEST     17  // Test key button on PCB, 1=normal, 0=pressed
#define WF2_LED_RUN_PIN     40  // Status LED on PCB
#define WF2_BM8563_I2C_SDA  41  // RTC BM8563 I2C port
#define WF2_BM8563_I2C_SCL  42
#define WF2_USB_DM_PIN 19
#define WF2_USB_DP_PIN 20

#define PANEL_RES_X 64     // Number of pixels wide of each INDIVIDUAL panel module. 
#define PANEL_RES_Y 32     // Number of pixels tall of each INDIVIDUAL panel module.
#define PANEL_CHAIN 2      // Total number of panels chained one to another

HUB75_I2S_CFG::i2s_pins _pins_x1 = {WF2_X1_R1_PIN, WF2_X1_G1_PIN, WF2_X1_B1_PIN, WF2_X1_R2_PIN, WF2_X1_G2_PIN, WF2_X1_B2_PIN, WF2_A_PIN, WF2_B_PIN, WF2_C_PIN, WF2_D_PIN, WF2_X1_E_PIN, WF2_LAT_PIN, WF2_OE_PIN, WF2_CLK_PIN};
HUB75_I2S_CFG::i2s_pins _pins_x2 = {WF2_X2_R1_PIN, WF2_X2_G1_PIN, WF2_X2_B1_PIN, WF2_X2_R2_PIN, WF2_X2_G2_PIN, WF2_X2_B2_PIN, WF2_A_PIN, WF2_B_PIN, WF2_C_PIN, WF2_D_PIN, WF2_X2_E_PIN, WF2_LAT_PIN, WF2_OE_PIN, WF2_CLK_PIN};

HUB75_I2S_CFG mxconfig(
  PANEL_RES_X,   // module width
  PANEL_RES_Y,   // module height
  PANEL_CHAIN,   // Chain length
  _pins_x1       // pin mapping for port X1
);
```

The HD-WF2 looks similar to my HD-WF4 except the 4 connectors instead of 2.

![HD-WF2](https://user-images.githubusercontent.com/4750719/253481802-1cd8e3f3-a1f7-4472-ae2e-ff60b7bb92f5.png) 

## Install Circuitpython

TBD

## Create output on one LED display 64x32

TBD

## Get displayio.root_group to show the REPL prompt

TBD

## Compile a Firmware that uses one display 64x32 as main board.DISPLAY

TBD

## Reinstall the original firmware from Huidu

The company has an official product page: https://www.huidu.cn/product_132.html

The official link for specification is still for the older v6.0.1 which used a different CPU. [Here is the link](https://huidu-cn.oss-ap-southeast-1.aliyuncs.com/eData/File/Specifications/Single_color/WFX_series/HD-WF4%20Specification%20V6.0.1.pdf)

## Hardware

- CPU ESP32-S3 [datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)
- six 74HC573 octal transparent D-type latches (probably only used as level-shifters) [datasheet](https://www.ti.com/lit/ds/symlink/cd54hc573.pdf)
- cFeon QH64A-104HIP X217E01 2322HSA a 64 MBit (8,192 kByte) flash chip [datasheet](https://www.xmcwh.com/uploads/207/XM25QH64C.pdf) - connected as Quad SPI?
- BM8563 2333CD CMOS real time clock/calendar over i2c [datasheet](https://www.xmcwh.com/uploads/207/XM25QH64C.pdf)

Using an USB-C to USB-A cable does not power the board, so I probably need a USB-A to USB-A cable (outside of specifications) to upload any firmware. The USB-port should be directly connected to the ESP32-S3 since it supports USB natively.

