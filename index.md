# Infinity Clock
An infinity clock is a clock including an ilusion that make lights look as if it goes on for forever.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Annie/Yanni L. | Forest Ridge | Electrical Engineering | Incoming Sophmore

![Headstone Image](https://github.com/BlueStampEng/BSE_Template_Portfolio/blob/4655d8c4b2f1d0fa5912511d0b39542520b9f88e/branding/BlueStamp-Engineering-Logo-White.png)

# About the Creator
This is Annie.
![WIN_20220722_10_46_37_Pro](https://user-images.githubusercontent.com/108947618/180495279-26b0bb6d-49b5-4e88-afb6-9bee78cea248.jpg)
Some little facts about her is that she...
- Understand little to nothing about coding
- Makes rather terrible desisions thinking they were good ideas
- Is better at building with her hands
- Is a dog lover

Now that you know a little about her, this is the project that she made!

# The Project
Here is what the final project looks like:
![WIN_20220722_10_53_49_Pro](https://user-images.githubusercontent.com/108947618/180496439-646a3e23-b6e8-4c3b-bd36-e2dd94645607.jpg)
  
# Final Milestone
My final milestone is building the physical project of the Infinity Clock. It includes a mirror, a oneway mirror, the LEDs, and the mirror box. The light is reflected from the mirror, to the onsided mirror, to the mirror, creating an ilusion, or virtual image of the mirror, that have a virtual image of another mirror and so on to create the illusion of the "infinity Mirror". Because on of the mirrors is a onesided mirror, you could see through the mirror without seeing your own reflection. the chips and the alduano are behind the mirror and the is a hole at the top to access the button.

[![Final Milestone][(https://res.cloudinary.com/marcomontalbano/image/upload/v1612573869/video_to_markdown/images/youtube--F7M7imOVGug-c05b58ac6eb4c4700831b2b3070cd403.jpg )](https://www.youtube.com/watch?v=F7M7imOVGug&feature=emb_logo "Final Milestone")](https://www.youtube.com/watch?v=O-UU6cGTjDM){:target="_blank" rel="noopener"}

# Second Milestone
My second milestone is the completion is the coding for the Infinity Clock. Truth to be told, I tried to finish the code, but was unable to do so. It was my instructor who helped me finish. So, I understood about less than half of what the code actually does. The following video is me trying to explain it.

[![Third Milestone][(https://res.cloudinary.com/marcomontalbano/image/upload/v1612574014/video_to_markdown/images/youtube--y3VAmNlER5Y-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=y3VAmNlER5Y&feature=emb_logo "Second Milestone")](https://youtu.be/ntdXTXfvaU0){:target="_blank" rel="noopener"}
# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/4FBheR3g7aQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

My first milestone is including all the components of the Clock together. In this case, I can either could the seconds in the clock or change the colors of the clock. A button is wired to the Arduino send in a signal to allow the light to become a different color for a set amount of time, in my milestone video, 8 seconds. This code helps me figure out how to cotinue with my next step, adding the hour and minutes. For my "time" part, I wired the leds to the arduino along with a clock device, "HW-084". Then I put in a code which allowed the pixels to pop up one by one and when it goes past 60, the number that pops up goes back to 1.

# Code
This is the code that I used:
cp```
#include <Wire.h>
#include <ds3231.h>
 
struct ts t; 

int x=0;
int y=150;
int z=0;

int CD = 0;

bool buttonstate=true;

#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif


#define PIN         6

int NUMPIXELSS =60;
int NUMPIXELSM =60;
int NUMPIXELSH =24;

Adafruit_NeoPixel pixels(NUMPIXELSS, PIN, NEO_GRB + NEO_KHZ800);

#define DELAYVAL 300

void setup() {
  // These lines are specifically to support the Adafruit Trinket 5V 16 MHz.
  // Any other board, you can remove this part (but no harm leaving it):
#if defined(__AVR_ATtiny85__) && (F_CPU == 16000000)
  clock_prescale_set(clock_div_1);
#endif
  // END of Trinket-specific code.

  pixels.begin(); // INITIALIZE NeoPixel strip object (REQUIRED)
  
  Serial.begin(115200);

  pinMode(2, INPUT_PULLUP);

  pinMode(13, OUTPUT);
  Wire.begin();
  t.hour=4; 
  t.min=36;
  t.sec=0;
 DS3231_set(t); 

NUMPIXELSS = t.sec;
NUMPIXELSM = t.min;
NUMPIXELSH = t.hour;

  
  pixels.clear();

}

int phour = 0;
int pmin = 0;
int psec = 0;

void printTime() {
  // Get the current time and date from the chip.
  phour = t.hour;
  pmin = t.min;
  psec = t.sec;
  
  if(phour>=12){phour=phour-12;}
  pixels.setPixelColor(5*phour, pixels.Color(255,,147)); //hour color to blue
  pixels.setPixelColor(pmin, pixels.Color(135,90,0));  //min color to red
  pixels.setPixelColor(psec, pixels.Color(0,0,90));  //secs color to green
  
  pixels.show();

  pixels.show();
  if(psec==0)
   { 
    pixels.setPixelColor(pmin-1, pixels.Color(0,0,0));
    pixels.setPixelColor(pmin, pixels.Color(135,90,0));
     pixels.show();
    for(int i=1;i<60;i++)
      {
    pixels.setPixelColor(i, pixels.Color(0,0,0));
      }
    }

   if(pmin==0)
    { 
    pixels.setPixelColor(59, pixels.Color(0,0,0));
    }
    delay (60000);
}

bool isPress = false;

void loop() {
  
  int sensorVal1 = digitalRead(2);
  NUMPIXELSS = t.sec;
  NUMPIXELSM = t.min;
  NUMPIXELSH = t.hour;

  //print out the value of the pushbutton

  Serial.print(sensorVal1);

  while (!isPress) {
    Serial.print("Boff\n");
    pixels.clear();
    for(int i=0; i<60; i++) {
      pixels.setPixelColor(i, pixels.Color(0,0,90));
      pixels.show();   // Send the updated pixel colors to the hardware.
      delay(1000);
      if (digitalRead(2) == LOW) {
        isPress = true;
        break;
      }
      }
    }
  if (isPress) {
    Serial.print("Bon\n");
    isPress = false;
    printTime();
    pixels.show();
    delay(1000);
  }
}  

```
