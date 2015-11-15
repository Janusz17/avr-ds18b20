# AVR Dallas1820
This is a library for controlling temperature sensor DS1820 with AVR.

# Features
 - Structures for sensor configuration
 - Functions for reading temperatures, ROMs
 - Functions for matching ROMs

# Examples:

### Reading temperature

```c
#include <stdio.h>

#include <avr/io.h>
#include <util/delay.h>

#include "Dallas1820/Dallas1820.h"

int main( )
{
    float Temperature = 0.0f;

    //Make sensor pin output (optional)
    DDRD = (1 << 7);

    //Sensor configuration (port direction register, port output register, port input register and mask)
    OneWireConfiguration Thermometer = { &DDRD, &PORTD, &PIND, ( 1 << 7 ) };

    Dallas1820ReadROM( &Thermometer ); //Read ROM

    while ( 1 )
    {
        //Request Dallas 1820
        Dallas1820Request( &Thermometer );

        //Read temperature
        Temperature = Dallas18B20ToCelcius( Dallas18B20Read( &Thermometer ) );

        //Use data
        printf( "%f\n", (double) Temperature );

        //Wait 1s
        for ( unsigned char i = 0; i < 100; i++ )
            _delay_ms( 10 );
    }
    return 0;
}

```
