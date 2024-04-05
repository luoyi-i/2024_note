
## doc
https://docs.platformio.org/en/stable/tutorials/espressif32/arduino_debugging_unit_testing.html

## hello world
```
#include <Arduino.h>

void setup()
{
    Serial.begin(9600);
}

void loop()
{
    Serial.println("Hello world!");
    delay(1000);
}
```
