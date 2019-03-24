---
layout: post
title: "Assignment Two: Servos"
date: 2019-02-21
excerpt: "Small-scale Music Visualization With Moving Parts"
tags: [arduino, assignment, lab, servo, servo motor, programming ]
comments: false
---

# Servo Motor Control With an Arduino
The goal of this lab was to use sensors to control a servo motor and a piezo speaker. The lab used an analog input, such as a force sensor, to control the motion of the servo. Below you'll find notes from the lab, a description of the project inspired by the lab, the fully commented code, and a video of the project.


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
#include <Servo.h>
#include "pitches.h"

Servo servoMotor;
int servoPin = 3;

int previousAngle = 0;

int melody[] = {
  NOTE_G3, NOTE_G3, NOTE_G3, NOTE_C4, NOTE_G4,
  NOTE_F4, NOTE_E4, NOTE_D4, NOTE_C5, NOTE_G4,
  NOTE_F4, NOTE_E4, NOTE_D4, NOTE_C5, NOTE_G4,
  NOTE_F4, NOTE_E4, NOTE_F4, NOTE_D4
};

int noteDurations[] = {
  4, 4, 4, 2, 2, 4, 4, 4, 2, 2, 4, 4, 4, 2, 2, 4, 4, 4, 1
};

void setup() {
  pinMode(5, OUTPUT);
  pinMode(6, OUTPUT);
  pinMode(7, OUTPUT);
  pinMode(8, OUTPUT);

  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  pinMode(11, OUTPUT);
  Serial.begin(9600);
  servoMotor.attach(servoPin);
}

void loop() {
  int potVal = analogRead(0);
  //  analogWrite(11, potVal);

  if (potVal < 200) {
    analogWrite(6, HIGH);
    digitalWrite(7, LOW);
    digitalWrite(8, LOW);
  } else if (potVal < 600) {
    analogWrite(6, LOW);
    digitalWrite(7, HIGH);
    digitalWrite(8, LOW);
  } else if (potVal < 1000) {
    analogWrite(6, LOW);
    digitalWrite(7, LOW);
    digitalWrite(8, HIGH);
  } else {
    analogWrite(6, LOW);
    digitalWrite(7, LOW);
    digitalWrite(8, LOW);
  }

  int servoAngle = map(potVal, 0, 1028, 0, 60);
  if (abs(servoAngle - previousAngle) > 5) {
    servoMotor.write(servoAngle);
    previousAngle = servoAngle;
  }

  int forceVal = analogRead(1);
  //Serial.println(forceVal);

  if (forceVal > 270) {
    map(forceVal, 0, 1024, 0, 255);
    for (int thisNote = 0; thisNote < 19; thisNote++) {
      int noteDuration = 1000 / noteDurations[thisNote];
      tone(5, melody[thisNote], noteDuration);

      int light = map(log10(melody[thisNote]*1000), 1, 6, 6, 10);
      Serial.println(light);
      for (int i=6; i<9; i++){
        digitalWrite(i, LOW);
      }

      digitalWrite(light, HIGH);

      if (thisNote == 18) {
        digitalWrite(6, HIGH);
        digitalWrite(7, HIGH);
        digitalWrite(8, HIGH);
      }

      // to distinguish the notes, set a minimum time between them.
      // the note's duration + 30% seems to work well:
      int pauseBetweenNotes = noteDuration * 1.30;
      delay(pauseBetweenNotes);
      // stop the tone playing:
      noTone(5);
    }
  } else {
    analogWrite(5, 0);
  }


  int forceVal2 = analogRead(2);
  //Serial.println(forceVal2);
  if (forceVal2 < 100) {

    if ((millis()/500)%2 == 0) {
      analogWrite(11, 255);
      analogWrite(9, 0);
    } else {
      analogWrite(9, 255);
      analogWrite(11, 0);
    }

    analogWrite(6, HIGH);
    digitalWrite(7, HIGH);
    digitalWrite(8, HIGH);
  } else {
    analogWrite(9, 0);
    analogWrite(10, 0);
    analogWrite(11, 0);
  }
}
```


### Media
<center><video width="80%" src="../assets/img/posts/assignment2.mov"></video></center>


### Reflection
