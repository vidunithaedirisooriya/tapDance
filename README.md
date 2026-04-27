```
  _____      _       ____             ____       _      _   _      ____ U _____ u 
 |_ " _| U  /"\  u U|  _"\ u         |  _"\  U  /"\  u | \ |"|  U /"___|\| ___"|/ 
   | |    \/ _ \/  \| |_) |/        /| | | |  \/ _ \/ <|  \| |> \| | u   |  _|"   
  /| |\   / ___ \   |  __/          U| |_| |\ / ___ \ U| |\  |u  | |/__  | |___   
 u |_|U  /_/   \_\  |_|              |____/ u/_/   \_\ |_| \_|    \____| |_____|  
 _// \\_  \\    >>  ||>>_             |||_    \\    >> ||   \\,-._// \\  <<   >>  
(__) (__)(__)  (__)(__)__)           (__)_)  (__)  (__)(_")  (_/(__)(__)(__) (__) 
```
tapDance is a library to detect taps with the touchRead functionality of the ESP32 boards(however, ESP32 S2/S3 are NOT supported). This library can distinguish single taps, double taps and holds. This is acheived by running a FreeRTOS task that polls the touchRead of the touchPin.

How To Use:
-

create a tapDance object:
```
tapDance objectName(touchPin, contactThreshold, restThreshold, onTap, onDoubleTap, onHold);
```
touchPin:         any IO pin on the ESP32. [Random Nerd Tutorial on what pins are touchPins](https://randomnerdtutorials.com/esp32-touch-pins-arduino-ide/)

contactThreshold: touchRead of the touchPin is always above this threshold when it is not touched. refer [callibration](callibration)

restThreshold:    touchRead of the touchPin is always below this threshold when it is touched. refer [callibration](callibration)

onTap:            the name of the function that should be executed when the user single taps the touch pin. If recognising a single tap is unnecessary, pass `NULL`. refer [examples](examples/example1.cpp)

onDoubleTap:      the name of the function that should be executed when the user double taps the touch pin. If recognising a double tap is unnecessary, pass `NULL`. refer [examples](examples/example1.cpp)

onHold:           the name of the function that should be executed when the user has held the touch pin. If recognising a hold is unnecessary, pass `NULL`. refer [examples](examples/example1.cpp)


Initialise the object.
```
objectName.init();
```
This should be placed in the setup section of your code. 

There are no functions to be called in the loop. The relevant function will automatically be called when a touch is detected.


Working Principle:
-
[how touchRead changes when touched](forDemonstrationPurposesInReadme.png)
Above is graph of very noisy touchRead(touchPin) against time and it shows how the value of touchRead changes. restThreshold is placed so that the value of touchRead is always above it when not touched. And contactThreshold is placed so that the touchRead value is below the threshold when touched. The code recognises a touch when the value goes from something above teh restThreshold to something below the restThreshold.


callibration:
-
The above explained restThreshold can change drastically based on the circuitry, and minorly based on things like atmospheric conditions. Therefore, the restThreshold best be callibrated for your specific circuit. Moreover, the contactTheshold may also vary, based on factors such as wearing gloves, having some kind of coating on top of the touch surface. These simple [sketches](forCallibration) shall help you find the Thresholds for your use case.

The timing regarding polling frequency, the time the pin should be held to register a hold and the maximum time between taps that can be allowed for double taps are defined in the beginning of tapDance.cpp




vidunithaedirisooriya 2026