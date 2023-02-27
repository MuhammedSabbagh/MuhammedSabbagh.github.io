---
layout: page
title: Sun-tracking for solar panels using Arduinos
description: this is one of the projects i have done
img: assets/img/13.jpg
importance: 1
category: finished
---

# Prolouge

This project was a portfolio before it got removed from my disk, not that it is a problem i'm gonna share how it was done.

## The idea

the idea was that i didn't want to use any light\optical or any expensive tools\sensors, this means that i have to rely on my humble understanding of how day and night works..

the thing is since getting into lots of complications when dealing with more than one axle, we will resort to only deal with rotating horizontally we will move forward and comeback to this point later.

## Basic understanding of solar and how it works?

the sun (assuming the day is fully clear at noon) gives an enormous amount of energy ~ $$1kW/m^2$$ !

the thing about solar panels is that they generate electricity from Photovoltaic energy which is great renewable, clean, and low maintainance form of electricity where there is no carbon footprint, but comes with a catch; you can benefit at the peak of the panels by having the sun at the most parallel position to the solar panels, this make moving solar panels from say the east to west preferable where every joule of energy from the sun is important like powering cities, or solar pumping where it's neccessary to keep the pump runnning at all time to help watering crops

## how this project works?

since i left the project for year now i don't remember how exactly i worked on it but the key principle is that we have daytime period and this period was devided to 20 moves across the day
this mean roughly 120 degrees is divided over 20 moves making 6 degrees each move, this is briliant and helps increasing the solar panels efficiency across the day

## where did the sketch go?

unfourtunately the Arduino sketch is missing and i couldn't find it nevertheless the code was pretty horrible so it had to be rewritten because of memory leaks, lots of unlimited recursions, bad code habits, ifs, basicly it was off the scale and the proof of concept deemed bad

## is there a plan to re-write?

yes and no

- the concept while good has flows and making it production product isn't a great idea

- i'm looking for more ambitious and more reasonable projects such as for example xinu_avr; in all fairness observing things in Low level is always fun and intruiging.

- if there is a possibility for a better iteration of this project i might start working on it however i won't be using AVR microcontrollers due to how limited they are (they are so small!)
