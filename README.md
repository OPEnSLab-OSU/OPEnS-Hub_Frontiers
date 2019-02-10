# OPEnS-Hub_Frontiers
[![DOI](https://zenodo.org/badge/155015932.svg)](https://zenodo.org/badge/latestdoi/155015932)

This is the Data, CAD, and Code archive for the OPEnS Hub Project for access by Frontiers



[Project Page](http://www.open-sensing.org/lora-hub)

[Project Loom Repository](https://github.com/OPEnSLab-OSU/InternetOfAg) that this is a instance/subset of 

## Development Boards


This project used the Adafruit line of Development boards called [Feathers](https://www.adafruit.com/feather?gclid=EAIaIQobChMIz5DNofCb4AIVWB6tBh2Vhga1EAAYASAAEgLMxPD_BwE) and [FeatherWings](https://www.adafruit.com/category/814)


Specifically 
* [Adafruit's M0 LoRa RFM95 Feather](https://www.adafruit.com/product/3178) 
* [Adafruit's Ethernet FeatherWing ](https://www.adafruit.com/product/3201) 
* [Adafruit's Adalogger FeatherWing (Real-time Clock and SD Card)](https://www.adafruit.com/product/2922) 



## Bill of Materials
[BOM](https://github.com/OPEnSLab-OSU/OPEnS-Hub_Frontiers/tree/master/BOM)


## Getting Started

Details on setting up Arduino IDE and Code can be found [here](https://github.com/OPEnSLab-OSU/OPEnS-Hub_Frontiers/tree/master/Arduino_and_Loom_Setup)

Details on using the Code  once setup (with examples) can be found [here](https://github.com/OPEnSLab-OSU/InternetOfAg/blob/master/ReadMe_Using_Loom.md)

## Low Power & Real-Time Clock Support

Our dependancy library supports two RTC devices:

- [Adafruit DS3231](https://learn.adafruit.com/adafruit-ds3231-precision-rtc-breakout/)
- [Adafruit PCF8523](https://learn.adafruit.com/adafruit-adalogger-featherwing)

Currently, interrupts (for waking the microprocessor from sleep) are only supported via the DS3231, with the PCF8523 mostly being used in conjunction with an SD card (as Adafruit Adalogger featherwing has PCF8523 and SD card holder) to provide timestamps.

The Loom Library has support for both devices for returning strings of the date, time, or weekday. Additional timer functions are provided for the DS3231.

#### RTC and Low Power Dependencies

- [DS3231 Extended Library](https://github.com/FabioCuomo/FabioCuomo-DS3231)
- [Low Power Library](https://github.com/rocketscream/Low-Power)
- [Enable Interrupt](https://github.com/GreyGnome/EnableInterrupt)

**NOTE:** To use the DS3231 extended library with the Feather M0,
the following line must be added to `RTClibExtended.h`:

```cpp
#define _BV(bit) (1 << (bit))
```

#### Sleep Modes

Project Loom supports two sleep modes for the Feather M0 and one sleep mode for the Feather 32u4.
Here are some details on the various modes:

| Mode           | Supported board | Current Draw |
| -------------- | --------------- | ------------ |
| Idle\_2        | Feather M0      | ~5 mA        |
| Standby        | Feather M0      | ~0.7 mA      |


#### Standby Operation

Due to some incompatibilities between Standby mode and falling interrupts, a very particular
scheme must be followed to use Standby mode on the Feather M0.  The following code is an
example of how standby mode can be set up on the M0 with a wakeup interrupt on pin 11:

``` cpp
void setup() {
    pinMode(11, INPUT_PULLUP);
    bool OperationFlag = false;
    delay(10000); //It's important to leave a delay so the board can more easily
                  //be reprogrammed
}

void loop() {
    if (OperationFlag) {

        // Whatever you want the board to do while awake goes here

        OperationFlag = false; //reset the flag
    }

    attachInterrupt(digitalPinToInterrupt(11), wake, LOW);

    LowPower.standby();
}

void wake() {
    OperationFlag = true;
    detachInterrupt(digitalPinToInterrupt(11)); //detach the interrupt in the ISR so that
                                                //multiple ISRs are not called
}
```


## Supporting Files

- [Adafruit References](https://github.com/OPEnSLab-OSU/OPEnS-Hub_Frontiers/tree/master/Adafruit%20Reference)
- [CAD Files](https://github.com/OPEnSLab-OSU/OPEnS-Hub_Frontiers/tree/master/CAD)
- [Field Data](https://github.com/OPEnSLab-OSU/OPEnS-Hub_Frontiers/tree/master/Field%20Data)
- [Images](https://github.com/OPEnSLab-OSU/OPEnS-Hub_Frontiers/tree/master/Images)
- [Wiring](https://github.com/OPEnSLab-OSU/OPEnS-Hub_Frontiers/tree/master/Wiring)
- [PushingBox Guide](https://github.com/OPEnSLab-OSU/OPEnS-Hub_Frontiers/tree/master/PushingBox)

