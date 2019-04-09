# Moodie Controller Board
The Moodie Controller is a custom designed PCB which houses a WeMos D1 Mini as it's brain. The controller itself acts as a power path and strip connector for the D1 Mini. The parts list and soldering requirements have been kept as minimal as possible to keep the barrier to entry low.

The board itself is unregulated to keep the soldering requirements simple. This means you must supply 5V regulated yourself (eg, you can't use a 6V power plug). While the D1 Mini is a 3.3V MCU, it supports 5V input as it is internally regulated.

Max draw at 5A comes with the caveat that it should only be considered for short bursts or flashes, anything over a few seconds at 5A is likely to start melting components. All components used in the original design are rated for 4A continuous.

Voltage smoothing is provided with a big 1000uF capacitor; this should allow reasonably long power leads with minimal fuss. If either power lead or strip length are short a shorter, 500uF cap can be used to shave some height of the finished boards profile.

## Ratings
The key ratings for the board are:

- __Input Voltage:__ 5V
- __Max Draw:__ 5A
- __Max Continuous:__ 4A

## Parts List
The parts list below represents one completed controller board:

- x1 - Moodie Control Board
- x1 - Moodie Control Board Parts Pack
  - x1 - Length of 2.55mm pitch male pin header
  - x1 - 5V~4A, Slotted feet, DC barrel jack
  - x1 - 3 pin spring loaded terminal block
  - x1 - 1000uF Electrolytic Capacitor
  - x4 - Adhesive Feet
- x1 - D1 Mini
- x1 - 5V DC Power Source (1A~5A Max)

## Design Choices
A project is it's design choices. Below are the key decisions that should be called out:

- It is usually the case to connect a resistor from the MCU data pin to the LED strip's data pin. When using D1 Mini, I've found this to be a source of signal issues.

- There are no breakout traces for unused MCU's. To keep soldering as simple as possible while also keeping board size small, only certain pins of the MCU have appropriate solder points.

## Build Steps
The steps below are ordered in a way that makes it easy to work with the board as you solder components to it. You may do it in any order that makes sense to you.

### Prepare the D1 Mini
First, we will add header pins to our D1 Mini, follow the steps below and ensure you consult the visual guide at each step; de-soldering and then re-soldering is never any fun, look twice, solder once.

1. Flip and turn the D1 Mini until the antenna is facing up and away from you. The USB connector will be on the bottom of the board, facing your body.

2. Break off x4, 2-pin sets of male header pins. Prefer losing a pin per cut to save as much of the black guard, per set, as possible; these guides are used to ensure the D1 Mini sits above the main PCB correctly.

3. Using a breadboard, place each set of pins so that you can lay the D1 over it. The pins should be placed with the long side pushed into the breadboard.

4. Place the D1 Mini over the pins and ensure it is level on the breadboard. When the D1 Mini is placed on top the following data pins should have a corresponding header pin:
    
    - RST, A0
    - RT, RX
    - D8, 3V3
    - GND, 5V

5. Solder each pin into place. Be careful with heat as it will start to transfer to the guard. I've found a chisel headed soldering head makes the job much easier as it will heat both the pin ring and header while maintaining stable contact.

Leave the assembled D1 Mini on the breadboard for now, we will use it later.

### Install the D1 Mini
Todo.

1. Flip and turn the Moodie Control Board so that it is facing right side up.

2. Flip and turn the D1 Mini until the antenna is facing up and away from you. The USB connector will be on the bottom of the board, facing your body, the legs will be pointing down.

3. Insert the D1 Mini into the appropriate holes on the board.

4. Solder each leg to it's slot

### Install the spring loaded Terminal Block
The terminal block used is a 4A spring block with a tiny profile and solid mounting.

1. Flip and turn the Moodie Control Board so that it is facing upside down.

2. Locate and insert the Terminal Block, from the underside, into the contacts marked "3,2,1". If the opening of the Terminal Block is facing out, it's legs should align with the slots correctly.

3. As the legs are two small to bend, fix an elastic band or some tape around the Terminal block so that it stays flows when you take your hand from it.

### Install the DC Barrel Jack
Power is provided via a board mounted Barrel Jack. The one used in the original design can support current of up to 5A. 

1. Flip and turn the Moodie Control Board so that it is facing upside down.

2. Locate and insert the DC Barrel Jack, from the underside, into the contacts marked J1. If the opening of the Jack is facing out, it's legs should align with the slots correctly.

3. Bend the legs slightly so that the Jack stays flush when you take your hands off of it.

4. Solder each leg to it's slot

5. Clip excess leg. Be careful with which tool you use for this. Barrel Jack Legs will tear into the soft blade of a wire cutter, rending it useless for wires. Opt for a cutting pliers used for cutting sheet metal or hack saw. If you have nether, leave the legs as are as the padded legs provide enough clearance for them to stay.

### Install the Capacitor
Todo

1. Flip and turn the Moodie Control Board so that it is facing upside down.

2. Locate and insert the DC Barrel Jack, from the underside, into the contacts marked J1. If the opening of the Jack is facing out, it's legs should align with the slots correctly.

3. Bend the legs slightly so that the Jack stays flush when you take your hands off of it.

4. Solder each leg to it's slot

5. Clip excess leg

## Software
```
// Moodie Controller - V1

// Imports
#include <Adafruit_NeoPixel.h>
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <WS2812FX.h>


// Pin to use
#define PIN D8


// LED Type
#define IS_RGBW


// Targets
#define IS_TV
#define IS_KALLAX
#define IS_MIRROR
#define IS_CABINETS_TOP

// WiFi
char ssid[] = "your-ssid";
char pass[] = "your-password";
char auth[] = "";

// Define LED'S and auth-code based on which target we are,
// this keeps maintenance and additional target support easy.
#if defined(IS_TV)
  #define LED'S 60
  auth = "";
#elif defined(IS_KALLAX)
  #define LED'S 60
  auth = "";
#elif defined(IS_MIRROR)
  #define LED'S 69
  auth = "";
#elif defined(IS_CABINETS_TOP)
  #define LED'S 230
  auth = "";
#elif defined(IS_CABINETS_LEFT)
  #define LED'S 33
  auth = "";
#elif defined(IS_CABINETS_RIGHT)
  #define LED'S 66
  auth = "";
#endif


// Declare the strip
#if defined(IS_RGBW)
  WS2812FX ws2812fx = WS2812FX(LEDS, PIN, NEO_GRBW + NEO_KHZ800);
#else
  WS2812FX ws2812fx = WS2812FX(LEDS, PIN, NEO_GRB + NEO_KHZ800);
#endif


// Setting storage
int red = 10;
int green = 10;
int blue = 10;
int white = 10;
int program = 1;
int brightness = 10;


// Sync Hardware to app
BLYNK_CONNECTED () {
  Blynk.syncAll(); 
}


// Program Selector
BLYNK_WRITE (V0) {
  nextProgram(param.asInt());
}


// Brightness Slider
BLYNK_WRITE (V1) {
  int b = param.asInt();

  // Clamp value to 1~100 in case the 
  // incoming values are changed in app. 
  if (b > 100) b = 100;
  if (b < 1) b = 1;

  brightness = b;
  nextBrightness(b);
}


// Red Slide
BLYNK_WRITE (V2) { 
  red = param.asInt();
  
  if (program == 2) {
    solid_custom();
  }
}


// Green Slider
BLYNK_WRITE (V3) { 
  green = param.asInt(); 
  
  if (program == 2) {
    solid_custom(); 
  }
}


// Blue Slider
BLYNK_WRITE (V4) { 
  blue = param.asInt();
  
  if (program == 2) {  
    solid_custom();
  }
}  


// White Slider
BLYNK_WRITE (V5) { 
  white = param.asInt();
  
  if (program == 2) {
    solid_custom();  
  }
}


// Setup function
void setup () {
  Blynk.begin(auth, ssid, pass);
  
  ws2812fx.init();
  ws2812fx.setMode(FX_MODE_STATIC);
  ws2812fx.setSpeed(20);
  ws2812fx.start();
}


// Loop function
void loop () {
  Blynk.run(); 
  ws2812fx.service();
}


void nextBrightness(int b) {
  if (b == 1) ws2812fx.setBrightness(1);
  else ws2812fx.setBrightness((int)((255 / 100) * b));
}


void nextProgram (int p) {
  // Reset the brightness if the 
  // last program was "off"
  if (program == 1) {
    if (p != 1) {
      nextBrightness(brightness);
    }
  }

  program = p;
  
  switch(p) {
    case 1 : off(); 
             break;
    case 2 : solid_custom();
             break;
    case 3 : solid_day_white();
             break;
    case 4 : solid_night_white();
             break;
    case 5 : flags_meath();
             break;
    case 6 : seasonal_halloween();
             break;
  }
}


void off() {
   ws2812fx.resetSegments();
   ws2812fx.setSegment(0, 0, (LEDS - 1), FX_MODE_STATIC, BLACK, 5000, false);
   nextBrightness(1);
}


void solid_custom() {
  ws2812fx.resetSegments();
  ws2812fx.setSegment(0, 0, (LEDS - 1), FX_MODE_STATIC, ws2812fx.Color(red, green, blue, white), 5000, false);
}


void solid_day_white() {
  ws2812fx.resetSegments();
  ws2812fx.setSegment(0, 0, (LEDS - 1), FX_MODE_STATIC, ws2812fx.Color(232, 110, 124, 92), 5000, false);
}


void solid_night_white() {
  ws2812fx.resetSegments();
  ws2812fx.setSegment(0, 0, (LEDS - 1), FX_MODE_STATIC, ws2812fx.Color(196, 8, 123, 60), 5000, false);
}


void flags_meath() {
  int num = LEDS / 4;
  
  ws2812fx.resetSegments();
  ws2812fx.setSegment(0, 0,       num - 1,     FX_MODE_STATIC, GREEN, 5000, false);
  ws2812fx.setSegment(1, num,     num * 2 - 1, FX_MODE_STATIC, GREEN, 5000, false);
  ws2812fx.setSegment(2, num * 2, num * 3 - 1, FX_MODE_STATIC, YELLOW, 5000, false);
  ws2812fx.setSegment(3, num * 3, LEDS - 1,    FX_MODE_STATIC, YELLOW, 5000, false);
}


void seasonal_halloween() {
  int num = LEDS / 4;
  
  ws2812fx.resetSegments();
  ws2812fx.setSegment(0, 0,       num - 1,     FX_MODE_STATIC, PURPLE, 5000, false);
  ws2812fx.setSegment(1, num,     num * 2 - 1, FX_MODE_STATIC, ORANGE, 5000, false);
  ws2812fx.setSegment(2, num * 2, num * 3 - 1, FX_MODE_STATIC, PURPLE, 5000, false);
  ws2812fx.setSegment(3, num * 3, LEDS - 1,    FX_MODE_STATIC, ORANGE, 5000, false);
}
```
