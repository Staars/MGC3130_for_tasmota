# MGC3130_for_tasmota
WIP-driver to use a MGC3130-board in TASMOTA (currently tested with an NodeMCU and a SKYWRITER board).

## USAGE:
- copy the .ino file to /sonoff
- wire up SDA and SDL and configure as usual in TASMOTA
- wire up RESET and TRFR to free GPIO's and adjust the code (#define MGC3130_xfer, #define MGC3130_reset)

## EXPECTED BEHAVIOUR:
- after boot gestures (FLICK, EDGE_FLICK, TOUCH, TAP, DOUBLE_TAP) will be sensed and published via MQTT 
- you can select different modes with the COMMANDS, touch will always be sensed and report the the duration in 1/20 seconds.
- the airwheel gesture will be sensed and published as "AW" via MQTT with values between 0 and 1023 - clockwise up
- after entering 3d cursor mode the values for x,y,z will be sensed and published via MQTT with values between 0 and 1023 for x,y. Data is only published, when z is in the upper half (z values are between 0 and 511). 
- at the moment the circle gestures ((COUNTER)CLOCKWISE) must be activated with the COMMAND: SENSOR91 1 (we must wait 250 ms after the start and can not activate it in the init function -> we will deal with it later on)

## COMMANDS: (will likely change in the future)
* SENSOR91 0 - next mode
* SENSOR91 1 - gesture mode 
* SENSOR91 2 - airwheel mode
* SENSOR91 2 - position mode (ATTENTION: this will send a lot of data!)

a possible solution to cycle through the modes only with the sensor by double tapping the centre is using RULES:
rule1 on Tele-MGC3130#DT_C do sensor91 0 endon
or with a "long" touch of a second
rule1 on Tele-MGC3130#TH_C > 20 do sensor91 0 endon

## CONSIDERATIONS:
This is an extemely versatile sensor and the main problem is not to get it to work somehow in TASMOTA, but to make it usable in a sensible way. We can measure and publish all kinds of data in parallel, but this will likely end up in an unusable situation. 
It is important to have a basic understanding of the sensor, to not get confused with seemingly unreasonable messages (DOUBLE TAP triggers a TOUCH (or more than one), then a TAP (after the first lift of the finger) and then a DOUBLE TAP.
The naming conventions of the gestures are according to the data sheets from Microchip, because if we only would have simple FLICKS, it would have made it easy to use: UP, DOWN, LEFT, RIGHT. But we have EDGLE FLICKS and various TOUCHES too, and so the direction would be  ambigous. That's whay we (have to) use NORTH-SOUTH, EAST-WEST ... and NORTH, SOUTH, .... and CENTRE.
To make the MQTT messages not too long, some useful abbreviations have to be found. This ist definitly work in progress.


## TO-BE-DONE:
- Filtering of the TOUCH messages
...










