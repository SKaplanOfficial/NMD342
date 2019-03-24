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

My love of music visualization made selecting a project idea rather easy. From the start, I knew my project was going to involve a speaker, lights, and a melody, but I didn't know how I would involve a servo motor and an analog sensor. I figured I could use a sensor to activate the music, but I wanted something a little bit more exciting than that. I decided to use a force sensor and the servo motor to create a turntable-inspired "needle" and "arm" using the motor's movement

The last thing to do was to select a song. I wanted to do the Star Trek theme, but realized I would have to the whole theme to be fully satisfied. I didn't have the time for that, although I do hope to revisit the idea in the future. Instead, I did the exact opposite: the Star Wars theme, or at least the most recognizable part of it. I reasonable that a recognizable melody would decrease the aggravation caused by the less-than-ideal sound quality of piezo speakers (though I'm not sure this matters much once the same tune plays more than five times in a row).

Next step: programming my idea into existence.


### Code



### Media
<center><iframe src="../assets/img/posts/assignment2.mov" frameborder="0" allowfullscreen></iframe></center>


### Reflection
