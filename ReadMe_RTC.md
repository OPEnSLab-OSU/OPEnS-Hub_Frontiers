# Project Loom: Library Overview



### Notes on Internet Connectivity

**Ethernet MAC Addresses:** 

If you are not on a network where MAC addresses are managed / locked if not explicitly approved, then you will need obtain a valid MAC address from your IT department. Once you have a MAC address, it can be set in the code with something like `byte mac[] = {0x98, 0x76, 0xB6, 0x10, 0x61, 0xD6};` It would generally be worth testing a new MAC address in basic example code first to ensure that will in fact work.

## Real-Time Clock Support

The Loom library supports two RTC devices:

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
| SLEEP\_FOREVER | Feather 32U4    | Untested     |

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

#### Setting the Time from the Internet

Loom supports setting the RTC time based on the time obtained from an NTP server.

#### UTC Time

Loom supports the usage of either local or UTC time.



## Using the Loom Library

Details on setting up Arduino IDE and Loom can be found [here](https://github.com/OPEnSLab-OSU/InternetOfAg/tree/master/Arduino_and_Loom_Setup)

Details on using Loom once setup (with examples) can be found [here](https://github.com/OPEnSLab-OSU/InternetOfAg/blob/master/ReadMe_Using_Loom.md)

#### 