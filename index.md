## Hello There, I'm Silas Udofia

I intend on making most of my projects open source both in hardware development and in my web development, hence all my code will be available here on github
do well to learn from them. also contribute to making it better. my WhatsApp Number ```08160595927```

This is a code written for a 7 segment 4 digit  display  project interfaced with an Arduino nano, the display is built using bright RED LED's. In this project i prefered using the **PLATFORM IO** Extension with **Visual Studio code**. You can still use the regular arduino IDE for this project. 

Click the link to view the project on github. 

[view project here](https://github.com/DigiSoft-blend/Arduino-DigitalClock/blob/main/src/main.cpp)

### The main.cpp code is shown below. Feel free to copy this code to your arduino sketches, make changes as suits your specifications. Note you wouldnt need to include the ```<Arduino.h>``` file when working with the regular Arduino IDE.

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown

#include <Arduino.h>

/*
  Digital Clock

  Dispalys Time using a 4 digit 7 segment display of common anode, 
  also performs counting as well as displays characters ranging from 
  0-F Extending O,P,n,u,U,g,r,S._,- . with blinking entire display

  The circuit:
   The circuit comprises of an Arduino nano with a 4 digit 7 segment display built from descrete LED's.
   the lED's are connected in the common anode configuration and are driven by anode and cathode drivers
   such as UDN2981 and ULN2003.

  created November 29, 2021.
  by Udofia Silas Silas
 

  This example code is in the private domain.
  
*/

#define cath_a 2
#define cath_b 3
#define cath_c 4
#define cath_d 5
#define cath_e 6
#define cath_f 7
#define cath_g 8
/////////////////////////////////////////////////////////
#define anod_1 9
#define anod_2 10
#define anod_3 11
#define anod_4 12
#define anod_5 13 // seconds led
//////////////////////////////////////////////////////////
void init_7seg()
{
  pinMode(cath_a , OUTPUT);
  pinMode(cath_b , OUTPUT);
  pinMode(cath_c , OUTPUT);
  pinMode(cath_d , OUTPUT);
  pinMode(cath_e , OUTPUT);
  pinMode(cath_f , OUTPUT);
  pinMode(cath_g , OUTPUT);
 
  pinMode(anod_1,OUTPUT);
  pinMode(anod_2,OUTPUT);
  pinMode(anod_3,OUTPUT);
  pinMode(anod_4,OUTPUT);
  pinMode(anod_5,OUTPUT); // seconds led
}

//////////////////////////////////////////////////////

const byte segmentPins[] = {cath_a,cath_b,cath_c,cath_d,cath_e,cath_f,cath_g,};
const byte digitPins[]={anod_1, anod_2, anod_3, anod_4, anod_5};
int c;
int i;
//time variables
long count;
int sec ;
int mins = 59;
int hours = 1;

// Variables will change:
int ledState = LOW;             // ledState used to set the LED

// Generally, you should use "unsigned long" for variables that hold time
// The value will quickly become too large for an int to store
unsigned long previousMillis = 0;        // will store last time LED was updated

// constants won't change:
const long interval = 1000;           // interval at which to blink (milliseconds)
//////////////////////////////////////////////////////////////////////////////////////////
void start_timer();
void print_char(char a, short b);
void display_num(int num);
int echo(int a);
void OPEN(int del);
void start_counter( int max_count);
void print_b(int num, int del);
void clock_min(int a);
void clock_hour(int a);

void setup() {
 init_7seg();
}

////////////////////////////////////////////////////////////////////////////////////////

void loop() {
 // start_counter(500);
    start_timer();
  // OPEN(0);
 
}

/////////////////////////////////////////////////////////////////////////////////////////

void OPEN(int del)
{
   for(int x = 0; x < 30; x++){
        for(int i = 0; i < 30; i++){
           print_char('S',0);
           print_char('I',1);
           print_char('L',2);
           print_char('S',3);
        }
       delay(del); 
   }
}

///////////////////////////////////////////////////////////////////////////
void print_char(char a, short b){
  display_num(a);
  digitalWrite(digitPins[b],HIGH);
  delay(2);
  digitalWrite(digitPins[b],LOW);
  delay(2);
}


void start_counter( int max_count)
{
       echo(count);
        if(count == max_count){
          count = count; 
          print_b(count,300);
        }else{
          count++;
      }
 }

void start_timer()
{
  // check to see if it's time to blink the LED; that is, if the difference
  // between the current time and last time you blinked the LED is bigger than
  // the interval at which you want to blink the LED.
  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    // save the last time you blinked the LED
    previousMillis = currentMillis;

   
    // if the LED is off turn it on and vice-versa:
    if (ledState == LOW) {
      ledState = HIGH;
    } else {
      ledState = LOW;
    }

    // set the LED with the ledState of the variable:
    digitalWrite(digitPins[4], ledState);
    sec = sec + 1;
  }
    clock_min(mins);
    clock_hour(hours);
    
      if( sec == 60 ){
          mins = mins + 1;
          sec = 0;
       }
     if( mins == 60 ){
        hours = hours + 1;
        mins = 0;   
     }
 }
 
void print_b(int num, int del){
   for(int x = 0; x < del/10; x++){
        for(int i = 0; i < del/10; i++){
          echo(num);
        }
       delay(del); 
   }
}
 

void display_num(int num){
 switch(num){
  case 0:
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], LOW);
  break;
  case 1:
    digitalWrite(segmentPins[0], LOW);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], LOW);
    digitalWrite(segmentPins[6], LOW);
  break;  
  case 2:
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], LOW);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], LOW);
    digitalWrite(segmentPins[6], HIGH);
  break;
   case 3:
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], LOW);
    digitalWrite(segmentPins[6], HIGH);
  break;
   case 4:
    digitalWrite(segmentPins[0], LOW);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
   case 5:
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
  case 6:
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
  case 7:
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], LOW);
    digitalWrite(segmentPins[6], LOW);
  break;
  case 8:
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
  case 9:
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
   case 'A':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
   case 'B':
    digitalWrite(segmentPins[0], LOW);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
   case 'C':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], LOW);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], LOW);
  break;
   case 'D':
    digitalWrite(segmentPins[0], LOW);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], LOW);
    digitalWrite(segmentPins[6], HIGH);
  break;
   case 'E':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], LOW);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
   case 'F':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], LOW);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
  case 'O':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], LOW);
  break;
  case 'P':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], LOW);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;
  case 'N':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], LOW);
  break;
  case 'U':
    digitalWrite(segmentPins[0], LOW);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], LOW);
  break;
  case 'G':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
 break;   
  case 'R':
    digitalWrite(segmentPins[0], LOW);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], LOW);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], LOW);
    digitalWrite(segmentPins[6], HIGH);
  break;  
  case 'S':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;   
   case 'H':
    digitalWrite(segmentPins[0], LOW);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], LOW);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], HIGH);
  break;   
   case 'L':
    digitalWrite(segmentPins[0], LOW);
    digitalWrite(segmentPins[1], LOW);
    digitalWrite(segmentPins[2], LOW);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], HIGH);
    digitalWrite(segmentPins[5], HIGH);
    digitalWrite(segmentPins[6], LOW);
  break; 
   case 'Q':
    digitalWrite(segmentPins[0], HIGH);
    digitalWrite(segmentPins[1], HIGH);
    digitalWrite(segmentPins[2], HIGH);
    digitalWrite(segmentPins[3], HIGH);
    digitalWrite(segmentPins[4], LOW);
    digitalWrite(segmentPins[5], LOW);
    digitalWrite(segmentPins[6], HIGH);
  break;     
 } 
}

///////////////////////////////////////////////////////////////////////////
int echo(int a){
  int num = a;
  int temp = num / 1000;
  num = num % 1000;
  display_num(temp);
  digitalWrite(digitPins[0],HIGH);
  delay(2);
  digitalWrite(digitPins[0],LOW);
  delay(2);

  temp = num / 100;
  num = num % 100;
  display_num(temp);
  digitalWrite(digitPins[1],HIGH);
  delay(2);
  digitalWrite(digitPins[1],LOW);
  delay(2);

  temp = num / 10;
  display_num(temp);
  digitalWrite(digitPins[2],HIGH);
  delay(2);
  digitalWrite(digitPins[2],LOW);
  delay(2);

  temp = num % 10;
  display_num(temp);
  digitalWrite(digitPins[3],HIGH);
  delay(2);
  digitalWrite(digitPins[3],LOW);
  delay(2);
}


void clock_min(int a){
 int num = a;
 int temp ;
  temp = num / 10;
  display_num(temp);
  digitalWrite(digitPins[2],HIGH);
  delay(2);
  digitalWrite(digitPins[2],LOW);
  delay(2);

  temp = num % 10;
  display_num(temp);
  digitalWrite(digitPins[3],HIGH);
  delay(2);
  digitalWrite(digitPins[3],LOW);
  delay(2);
}

void clock_hour(int a){
 int num = a;
 int temp ;
  temp = num / 10;
  display_num(temp);
  digitalWrite(digitPins[0],HIGH);
  delay(2);
  digitalWrite(digitPins[0],LOW);
  delay(2);

  temp = num % 10;
  display_num(temp);
  digitalWrite(digitPins[1],HIGH);
  delay(2);
  digitalWrite(digitPins[1],LOW);
  delay(2);
}

```


### Support or Contact

Having trouble with code? will help you sort it out.
