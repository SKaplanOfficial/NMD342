---
layout: post
title: "Assignment One: Input"
date: 2019-02-13
excerpt: "Digital and Analog Input/Output"
tags: [arduino, assignment, lab, digital, analog, sensors, programming ]
comments: false
---

# Digital Input and Output With an Arduino
The goal of this lab was to use sensors to control a servo motor and a piezo speaker. The lab used an analog input, such as a force sensor, to control the motion of the servo. Below you'll find notes from the lab, a description of the project inspired by the lab, the fully commented code, and a video of the project.

## Notes
- Concepts apply to any microcontroller
- GPIO (General Purpose Input-Output) pins
  - Most fundamental connections for any microcontroller


# Analog In With an Arduino

## Notes

## Notes
- Servos
  - Easiest way to start making motion with a microcontroller
  - Only turn 180 degrees
  - Not all have the same wiring colors
  - Require a row of 3 male headers to attach to a breadboard
  - Utilize the Servo library (Servo.h)

This lab didn't introduce many new concepts, so the notes are not very extensive. Instead, the lab promoted the use of previously gained knowledge.


## The Project
### Description
Once I had the basic outline of a servo project provided by the lab, I started to think of what I could do that would be inspiring to others, challenging to me, and consistent with my topics of interest. Although attempting to meet these goals can often be a source of adversity, I am satisfied with my results.

My love of music visualization made selecting a project idea rather easy. From the start, I knew my project was going to involve a speaker, lights, and a melody, but I didn't know how I would involve a servo motor and an analog sensor. I figured I could use a sensor to activate the music, but I wanted something a little bit more exciting than that. I decided to use a force sensor and the servo motor to create a turntable-inspired "needle" and "arm." Music would activate ones the servo exerted enough force on the sensor. Once the music stopped, the servo would return to its original position to signal the end of the interaction.

The last thing to do was to select a song. I wanted to do the Star Trek theme, but realized I would have to the whole theme to be fully satisfied. I didn't have the time for that, although I do hope to revisit the idea in the future. Instead, I did the exact opposite: the Star Wars theme, or at least the most recognizable part of it. I reasonable that a recognizable melody would decrease the aggravation caused by the less-than-ideal sound quality of piezo speakers (though I'm not sure this matters much once the same tune plays more than five times in a row).

Next step: programming my idea into existence.


### Code
My program had two files. One file contained the main logic of the project while the other contained a list of definitions for pitches to be used for music creation. This file, titled pitches.h, is publicly available <a href="https://www.arduino.cc/en/Tutorial/ToneMelody">here</a>.

The main code can be seen below:

```
//-- Included Files --//
#include <Servo.h>      // Servo motor library
#include "pitches.h"    // Music note definiitions

//-- Servo Variables --//
Servo servoMotor;       // Instantiate servo library
int servoPin = 3;       // Motor is connected to pin 3
int previousAngle = 0;  // Previous angle of the motor

//-- Music Variables --//
// Star Wars theme in note form
int melody[] = {
  NOTE_G3, NOTE_G3, NOTE_G3, NOTE_C4, NOTE_G4,
  NOTE_F4, NOTE_E4, NOTE_D4, NOTE_C5, NOTE_G4,
  NOTE_F4, NOTE_E4, NOTE_D4, NOTE_C5, NOTE_G4,
  NOTE_F4, NOTE_E4, NOTE_F4, NOTE_D4
};

// Duration of each note in the theme
int noteDurations[] = {
  4, 4, 4, 2, 2, 4, 4, 4, 2, 2, 4, 4, 4, 2, 2, 4, 4, 4, 1
};


//-- Program Start --//
void setup() {
  // Set pins 5, 6, 7, and 8 as output pins for visualization LEDS
  pinMode(5, OUTPUT);
  pinMode(6, OUTPUT);
  pinMode(7, OUTPUT);
  pinMode(8, OUTPUT);

  // Set pins 9, 10, and 11 as output pins for RGB leds
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  pinMode(11, OUTPUT);

  // Begin serial output
  Serial.begin(9600);

  // Attach servo pin to the motor object
  servoMotor.attach(servoPin);
}


//-- Main Loop --//
void loop() {
  // Value of potentiometer on pin A0
  int potVal = analogRead(0);

  // Activate lights according to potVal
  if (potVal < 200) {
    // Low value -> First light
    analogWrite(6, HIGH);
    digitalWrite(7, LOW);
    digitalWrite(8, LOW);

  } else if (potVal < 600) {
    // Medium value -> Second light
    analogWrite(6, LOW);
    digitalWrite(7, HIGH);
    digitalWrite(8, LOW);

  } else if (potVal < 1000) {
    // High value -> Last light
    analogWrite(6, LOW);
    digitalWrite(7, LOW);
    digitalWrite(8, HIGH);

  } else {
    // Others -> No lights
    analogWrite(6, LOW);
    digitalWrite(7, LOW);
    digitalWrite(8, LOW);
  }

  // Map the value of potVal to between 0 and 60
  int servoAngle = map(potVal, 0, 1028, 0, 60);

  // Rotate the servo motor if reasonable
  if (abs(servoAngle - previousAngle) > 5) {
    servoMotor.write(servoAngle);
    previousAngle = servoAngle;
  }


  // Value of force sensor on pin A1
  int forceVal = analogRead(1);

  if (forceVal > 270) {
    // If forceVal is large enough...
    map(forceVal, 0, 1024, 0, 255); // Map the value to between 0 and 255

    // Loop through notes in the melody
    for (int thisNote = 0; thisNote < 19; thisNote++) {
      // Play the tone for the set duration
      int noteDuration = 1000 / noteDurations[thisNote];
      tone(5, melody[thisNote], noteDuration);

      // Deactivate all lights
      for (int i=6; i<9; i++){
        digitalWrite(i, LOW);
      }

      // Activate the light that represents that tone
      int light = map(log10(melody[thisNote]*1000), 1, 6, 6, 10);
      digitalWrite(light, HIGH);

      // Activate all lights when the melody ends
      if (thisNote == 18) {
        digitalWrite(6, HIGH);
        digitalWrite(7, HIGH);
        digitalWrite(8, HIGH);
      }

      // To distinguish the notes, set a minimum time between them
      int pauseBetweenNotes = noteDuration * 1.30;
      delay(pauseBetweenNotes);

      // Stop the tone
      noTone(5);
    }
  } else {
    // If forceVal is small, do not play a tone
    analogWrite(5, 0);
  }


  // Value of photoresistor on pin A2
  int lightVal = analogRead(2);

  if (lightVal < 100) {
    // If lightVal is small (very little light)..

    if ((millis()/500)%2 == 0) {
      // Blink RGB led between colors every half millisecond
      analogWrite(11, 255);
      analogWrite(9, 0);
    } else {
      analogWrite(9, 255);
      analogWrite(11, 0);
    }

    // Activate all other lights
    analogWrite(6, HIGH);
    digitalWrite(7, HIGH);
    digitalWrite(8, HIGH);
  } else {
    // Otherwise, deactivate RGB light
    analogWrite(9, 0);
    analogWrite(10, 0);
    analogWrite(11, 0);
  }
}
```


### Media
An early version of the project can be seen below. Unfortunately, I do not have a video of the final version at this time. In the future I intend to rebuild the project in order to document the completed version.

In this version, everything works properly except the actual visualization using LEDs. Although the LEDs turn on when the music starts, they do not activate and deactivate according to the different tones. This was fixed in a later version.

<center><video style="border-radius: 5px;" width="80%" src="../assets/img/posts/assignment2.mov" controls></video></center>


### Reflection
This project is a small representation of a larger idea. As I already mentioned, I've always enjoyed music visualization, and now I am able to make a physical representation of it. With a greater amount of power and processing power, it would be possible to have a 3D LED matrix like the one seen <a href="https://0x7d.com/photos/rgb-led-cube-rev-b-patterns/img_4761.jpg">here</a> that responds to music. This would be a fascinating installation and I'm hoping I can make it one day. Servos wouldn't be involved in such a project, though.

Servos do, of course, have their use, and I look forward to working with them more one day. I've already compiled a list of ideas that may work well for future projects:
- Robotics
  - Walking animal, such as a cat or dog
  - Arm
- Music
  - Automated piano
  - Real record player / turntable

I'm not sure if any of these ideas will take shape one day, but it's nice to keep a record of my thinking regardless.
