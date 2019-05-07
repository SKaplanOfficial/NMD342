---
layout: post
title: "Instilling Emotion"
date: 2019-02-13
excerpt: "Notes and reflection from Sofian's teaching"
tags: [notes, arduino]
comments: false
---

Sofian came in and taught the class a couple of times while Mike was in Cuba. Over the course of three class periods, we learned about and discussed the major concepts of Sofian's own Plaquette library/modified Arduino language. We then used the library to assist us in representing a particular emotion using an LED (or combination of LEDs).

In a general sense, Plaquette tries to make working with Arduino inputs and outputs easier by introducing an object-oriented structure. Although I work mainly in Java, where object-oriented programming is the standard, I did not see much benefit in using Plaquette over creating my own C++ classes (which Arduino supports) for most of the changes, and I found the additional features, like SineOsc generator, more limiting than just incrementing a float variable and observing its effects on the default sin() function. I like the idea of making the Arduino easier to work with and more accessible, but I'm not sure Plaquette accomplishes that (nor does it have to, necessarily, but these are just the thoughts I had while using it).

Sofian had each of us think of an emotion that would be represented with an LED. I wanted to see how someone would/could represent boredom, so I chose that, as I figured it would be easy for someone to think of a way to represent it but relatively difficult to convey that emotion through an LED.

The emotions were then assigned randomly. I ended up with the word "limbic," which Sophie defined as relating to the feeling of being in limbo - i.e. slow, unchanging, stuck. Sofian said we could change the emotion we were representing as long as we put effort into representing it, so I decided to take a slightly different approach and go with emotions that I associate with being stuck/in limbo. The first thing that came to mind was stressed, and I immediately had the idea of using a heartbeat sensor to cause lights to flicker. Although the link from one idea to the next may not seem obvious to some, "stress" and "flickering heart beat" went hand-in-hand in my mind.
