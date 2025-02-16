---
layout: page
title: "BeamNG Tuning Using Telemetry Data"
permalink: /showcase/beamng-tuning-using-telemetry-data
collection: do_not_list

comparison-2: 1dSYReRj66VAmOojPNjEAgLSST8bt6d-e/preview
---

<style>
.image-container {
    width: 50%;
    height: 50%
}
</style>

# Overview
Last year I used a Python script to pull live data from BeamNG and plot it. As described [here](live-telemetry-charts-from-beamng-drive-using-python), BeamNG can use the OutGauge protocol to send out UDP packets that can be received by something and unpacked. At the time, I thought BeamNG could only send a limited amount of data - anything relating to specific details of the car (brake temperatures, downforce, engine damage, etc.), I thought, wasn’t possible. 

It turns out that BeamNG is able to send out whatever data it holds about a particular vehicle (the list is very, very long). By editing the OutGauge.lua file found in the BeamNG game files, it’s possible to send data that I thought was impossible to send. This discovery therefore opened up endless possibilities of what could be sent to my Python program.

The original program plotted throttle, brake and clutch input (one graph), speed, RPM and fuel load. The update to this project aimed to build a program that could be used to analyse driving and see what changes to the tuning could be made to help improve lap times.

The contents of this section are as follows:

- Description of the car to be tuned and how it handles generally
- Explanation of the Python Program
- Analysis of best laps

Before delving into the contents of this section, I can’t claim to be extremely knowledgeable regarding race car dynamics. However, I hope that this section will be an enjoyable read, even if some parts might not be entirely accurate.

# The Car In Question
<div class="image-container" style="margin-bottom: 15px">
    <div>
        <img alt="Car" src="/assets/img/beamng-tuning-using-telemetry-data/car.png" />
    </div>    
</div>

The car used in this project was the Civetta Scintilla Race and appears to be somewhat based on GT3 specifications (but would not fit in the GT3 class). It features a 5.0L V10 engine, which can produce up to 738BHP at 8850rpm. Full tuning details (before and after) are shown in the appendix.

Before performing an analysed pre-tune lap, I spent some time driving around the Hirochi Raceway circuit map to get a feel for the circuit and how the untuned car handles. 

The best phrase I can use to best describe how the untuned car felt to drive: ‘unplanted’. It often felt quite ‘floaty’ when encountering successive steering changes (going through a chicane, for example). Furthermore, it was incredibly easy to use too much power when exiting a corner and lose control. I found myself either reducing throttle input or going to a higher gear when exiting most corners, from fear of losing control. It felt as though I had to wait for the car to be fully straight before committing to full throttle. 

# Python Program
The program builds upon the previous by adding more graphs and data. New features include: a steering input graph, changing the speed graph to display an integer value, current gear, brake temperatures, downforce over each wheel and oil/coolant temperatures.

Despite not being a fully fledged program (compared to professional racing teams), the aim is to provide enough data to see where improvements can be made throughout a lap.

The program works by: opening a UDP socket to receive the incoming data from BeamNG, receiving the data, unpacking it into a suitable format, passing unpacked data to another function which handles the graph plotting and live integer updates and finally the GUI package Tkinter is used to provide the GUI.

The graphs themselves were plotted using matplotlib and were, essentially, embedded into the Tkinter GUI. The text on the right was created using Tkinter labels. Initially, the labels are blank but the aforementioned function configures the labels with text and constantly updates them with the correct values. 

As discussed in the previous post, plotting four simultaneous graphs caused the graphs to plot slower than what I liked. An improvement to this program uses the Python Multiprocessing package. Before implementing this, a single processor thread was responsible for the entire program. Now, the data receiving and data plotting are assigned to different threads, allowing for extra performance. The x-axis labels were also removed, which improved graph plotting speed.

<div style="margin-bottom: 15px">
    <div>
        <img alt="Car" src="/assets/img/beamng-tuning-using-telemetry-data/graph.png" />
    </div>    
</div>

# Lap Comparisons

{% include googleDrivePlayer.html id=page.comparison-2 %}


In this section, setup 1 refers to the car before tuning and setup 2 refers to the car after tuning.

## Turn 1
I braked at roughly the same point on each lap. Despite many attempts, there’s still time to be found here. I expected the car to hold ~100 mph through the corner, but with both setups, it struggled to stay tight to the kerbs. With setup 2, I had to ease off the throttle slightly to allow more rotation and keep to the kerb. With setup 1, I was more hesitant on the throttle (fear of losing control), meaning I carried less speed through the corner.

<div style="margin-bottom: 15px">
    <div>
        <img alt="Car" src="/assets/img/beamng-tuning-using-telemetry-data/1.png" />
    </div>    
</div>
Approach to Turn 1

<div style="margin-bottom: 15px">
    <div>
        <img alt="Car" src="/assets/img/beamng-tuning-using-telemetry-data/2.png" />
    </div>    
</div>
Going through Turn 1

## Turns 2, 3, and 4
My goal was to ‘straightline’ these turns as much as possible to minimise lateral load changes and reduce sliding. At Turn 2, setup 1 was slightly ahead, but with setup 2, I had more confidence on the throttle and didn’t lose much time.

Turn 3 felt similar with both setups, though with setup 2, I was in second gear to allow extra rotation. The data shows I was able to apply more throttle between Turns 2 and 3 with setup 2.

For Turn 4, I aimed to place the inside tyres over the kerb to slingshot the car around. On both setups, I had to be careful with the throttle, as the right rear tyre was on the grass.

Exiting Turn 4 was tricky since the crest reduced downforce. Too much throttle here would cause the rear tyres to lose traction. With setup 2, I was able to go full throttle with some steering input, whereas with setup 1, I had to wait until the car was mostly straight before applying full power.

<div style="margin-bottom: 15px">
    <div>
        <img alt="Car" src="/assets/img/beamng-tuning-using-telemetry-data/3.png" />
    </div>    
</div>
Turn 2

<div style="margin-bottom: 15px">
    <div>
        <img alt="Car" src="/assets/img/beamng-tuning-using-telemetry-data/4.png" />
    </div>    
</div>
Turn 3

<div style="margin-bottom: 15px">
    <div>
        <img alt="Car" src="/assets/img/beamng-tuning-using-telemetry-data/5.png" />
    </div>    
</div>
Turn 4

<div style="margin-bottom: 15px">
    <div>
        <img alt="Car" src="/assets/img/beamng-tuning-using-telemetry-data/6.png" />
    </div>    
</div>
Exit of Turn 4