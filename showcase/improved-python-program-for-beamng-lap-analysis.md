---
layout: page
title: "Improved Python Program for BeamNG Lap Analysis"
permalink: /showcase/improved-python-program-for-beamng-lap-analysis
collection: do_not_list
---

# Overview
The previously published section introduced BeamNG lap analysis using a Python program which took in the UDP packets sent out by BeamNG, converted them into a usable format and then either plotted the data or displayed a value of a parameter. This program was a good introduction into plotting data with Python - using Matplotlib for the graph plotting and Tkinter for the GUI. Unlike the previous section, this section does not show any rigorous lap analysis, but rather focuses on the Python program itself.

The end of that section noted one main problem I had - the plotting speed of the graphs. More live graphs would, understandably, slow down the framerate of the graphs. This was not desired, as fine details might be missed in the graphs that would help improve my driving. I mentioned trying to run the program over multiple threads or using a different programming language (such as c++) that generally performs faster than Python. What I ended up doing instead was still using Python, but using a new GUI package and a new plotting package - PyQt6 and pyqtgraph.

PyQt6 is a comprehensive GUI package that can be used to build complex GUIs and applications, and also allows for use of pyqtgraph for graph plotting instead of matplotlib. As stated on the pyqtgraph website (https://www.pyqtgraph.org/), pyqtgraph is suitable for graphing that requires rapid plot updates - exactly what I was looking for. 
