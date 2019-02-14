---
layout: post
title: "Programming With Arduino"
date: 2019-02-13
excerpt: "Notes on various aspects of working with microcontrollers and electricity"
tags: [notes, arduino, resources, electricity, microcontrollers]
comments: false
---

Notes on resources related to working with Arduinos.

## Digital Input & Output
### Digital Inputs
- Simplest activites to sense are those that are true or false, on or off
- 1 -> HIGH, 0 -> LOW
- Define what a pin is to be used for in setup(): pinMode(n, INPUT/OUTPUT)
- Reading and writing data: digitalRead(n), digitalWrite(n, HIGH/LOW)

### Digital Outputs
- Arduino only inputs/outputs very little current (~10 microamps). HIGH/LOW change voltage. To change current, use a resister (decrease) or a transistor/relay (increase).
- A relay is a switch that is controlled by electric current, consist of two pieces of ferrous metal near a coil of wire. When current is passed through, the metal pieces move together, thereby making a switch.
- Relays allow for one circuit to control another, but are slowed compared to transistors
- Transistors can act as very fast switches as well as current amplifiers


## Electricity Basics
### Definitions
- Electricity: Flow of electrical energy through conductive materials
- Electrical circuit: A system comprised of a power source and components (load) that convert electrical energy into other forms of energy
- Sensors: Convert other forms of energy into electrical energy
- Actuators: Convert electrical energy into other forms of energy
- Electronics: Reading changins in electrical properties as information
- Transduction: Changing one form of energy into another
- Transducers: Devices that change one form of energy into another
- Voltage: Measure of the difference in electrical potential energy between two points
- Current: Measure of the magnitude of the flow of electrons through a point
- Resistance: Measure of a material's abilitity to oppose the flow of electrons
- All of the electrical energy of a circuit must be used by the load
- Short circuit: A circuit with no load, dangerous
- Schematic diagrams: Show the relationship between components in a circuit, represents flow of electricity instead of actual spatial relation
- Ground: Point of lowest energy
- Conductors: Materials through which current moves freely
- Insulators: Materials that prevent the flow of current
- Capacitors: Store electricity while current is flowing, then release it once the current stops, sometimes polarized, explode if more voltage is applied than they can handle
- Diodes: Permit flow of current in one direction while blocking it in the other
- Thermistors: Temperature-dependent resistors

### Relationships
- V = IR, Volts = Amps * Ohms
- P = IV, Watts = Amps * Volts
- Series: Total resistance = R1 + R2 + R3 + ...
- Parallel: 1/Total resistance = 1/R1 + 1/R2 + 1/R3 + ...
- Total voltage around path of a circuit is zero
- Resistors divide voltage when in series and amperage when in parallel
- Parallel: Total current = I1 + I2 + ...


## DC Power Supplies
