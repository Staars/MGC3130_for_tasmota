# MGC3130_for_tasmota
WIP-driver to use a MGC3130-board in TASMOTA (currently tested with an NodeMCU and a SKYWRITER board).

## USAGE:
- copy the .ino file to /sonoff
- wire up SDA and SDL and configure as usual in TASMOTA
- wire up RESET and TRFR to free GPIO's and adjust the code (#define MGC3130_xfer, #define MGC3130_reset)

## EXPECTED BEHAVIOUR:
- after boot gestures (FLICK, EDGE_FLICK, TOUCH, TAP, DOUBLE_TAP) will be sensed and published via MQTT 
- the airwheel gesture will be sensed and published as "Rot" via MQTT with values between 0 and 1023
- after entering 3d cursor mode the values for x,y,z will be sensed and published via MQTT with values between 0 and 1023
- after stopping airwheel mode circle gestures ((COUNTER)CLOCKWISE)

## COMMANDS: (will likely change in the future)
* SENSOR91 0 - stop cursor mode (enable gestures)
* SENSOR91 1 - start cursor mode (disable gestures)
* SENSOR91 2 - stop airwheel mode (enable circle gestures: CW and CCW) are published

## CONSIDERATIONS:
This is an extemely versatile sensor and the main problem is not to get it to work somehow in TASMOTA, but to make it usable in a sensible way. We can measure and publish all kinds of data in parallel, but this will likely end up in an unusable situation. 
It is important to have a basic understanding of the sensor, to not get confused with seemingly unreasonable messages (DOUBLE TAP triggers a TOUCH (or more than one), then a TAP (after the first lift of the finger) and then a DOUBLE TAP.
The naming conventions of the gestures are according to the data sheets from Microchip, because if we only would have simple FLICKS, it would have made it easy to use: UP, DOWN, LEFT, RIGHT. But we have EDGLE FLICKS and various TOUCHES too, and so the direction would be  ambigous. That's whay we (have to) use NORTH-SOUTH, EAST-WEST ... and NORTH, SOUTH, .... and CENTRE.
To make the MQTT messages not too long, some useful abbreviations have to be found. This ist definitly work in progress.


## TO-BE-DONE:
- Filtering of the TOUCH messages
- Rework of the commands -> make useful different modes (e.g. split gestures and airwheel)
...










