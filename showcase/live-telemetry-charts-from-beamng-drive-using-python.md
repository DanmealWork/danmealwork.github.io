---
layout: page
title: "Live Telemetry Charts From BeamNG.drive using Python"
permalink: /showcase/live-telemetry-charts-from-beamng-drive-using-python
collection: do_not_list
---

# Overview
BeamNG.drive is described as a "dynamic soft-body physics vehicle simulator capable of doing just about anything," and prides itself on its accurate physics engine and extremely detailed damage model. This allows users to explore and drive whatever they wish in BeamNG.drive with its various sandbox levels and scenarios. Usually, BeamNG.drive is recognised for its damage model, and indeed, there are many videos on YouTube showcasing cars crashing and being damaged in various ways. However, if we set aside the damage and crashes, we see a simulator that offers a very realistic driving experience. Every car component is simulated and can be adjusted/tuned to match personal driving styles. This, combined with the damage model, provides a real sense of risk and immersion when driving in the game.

I bought BeamNG.drive sometime in 2020 and didn’t play it extensively, occasionally loading it up and driving around, crashing various cars. Very recently, I bought a Thrustmaster racing wheel to upgrade from my Logitech wheel, and since then, I have been playing it a great deal. I downloaded a map for the Nürburgring-Nordschleife and have been lapping it in a GT3-esque car, constantly tuning and adjusting to get a car that fits my driving style. It got me thinking if there was any way that I could pull telemetry data from BeamNG.drive and plot it, to see if it would reveal any areas of the lap I could improve on.

# BeamNG.drive and the OutGauge Protocol:
After conducting some research, it was discovered that BeamNG.drive utilises the OutGauge protocol to send out UDP packets of data that can be received and unpacked. I decided to write a program in Python that would receive the data, unpack it, and then plot live graphs. The code for the program is provided below, accompanied by explanations of each section.

# Full Code
```python
# Plotting imports

from itertools import count
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation


# UDP set up

import socket
import struct


# Create UDP socket.

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)


# Bind to BeamNG OutGauge.

sock.bind(("127.0.0.1", 4444))

sock.setblocking(False)


# Receive UDP data and unpack


def receiveData():
    data = None

    while True:
        try:
            data, fromAddr = sock.recvfrom(4096)
        except socket.error:
            break

    if data is None:
        return None, None, None, None, None, None
    outsim_pack = struct.unpack("I4sH2c7f2I3f16s16si", data)
    gear = outsim_pack[3]
    speed = 2.23694 * outsim_pack[5]
    rpm = outsim_pack[6]
    engTemp = outsim_pack[8]
    fuel = 100 * outsim_pack[9]
    throttle = 100 * outsim_pack[14]
    brake = 100 * outsim_pack[15]
    clutch = 100 * outsim_pack[16]
    # print(f"RPM: {rpm:4.0f} Speed: {speed:3.0f}")
    return throttle, brake, clutch, rpm, speed, fuel


# Real-time graph

plt.style.use("fivethirtyeight")

x_vals = []
y_throttle = []
y_brake = []
y_clutch = []
y_rpm = []
y_speed = []
y_fuel = []

# Settings for subplots (Total number of plots, column position, row position)

fig = plt.figure(figsize=(12, 12))
ax1 = fig.add_subplot(4, 1, 1)
ax2 = fig.add_subplot(4, 1, 2)
ax3 = fig.add_subplot(4, 1, 3)
ax4 = fig.add_subplot(4, 1, 4)


def animate(i):

    throttle, brake, clutch, rpm, speed, fuel = receiveData()

    x_vals.append(next(index))
    y_throttle.append(throttle)
    y_brake.append(brake)
    y_clutch.append(clutch)
    y_rpm.append(rpm)
    y_speed.append(speed)
    y_fuel.append(fuel)

    # Needed to stop graphs changing colours

    ax1.clear()
    ax2.clear()
    ax3.clear()
    ax4.clear()

    # Moving x axis

    if len(x_vals) > 100:
        ax1.set_xlim(x_vals[-100], x_vals[-1])
        ax2.set_xlim(x_vals[-100], x_vals[-1])
        ax3.set_xlim(x_vals[-100], x_vals[-1])
        ax4.set_xlim(x_vals[-100], x_vals[-1])

    # Setting vertical axis limits

    ax1.set_ylim(-5, 105)
    ax2.set_ylim(-150, 10000)
    ax3.set_ylim(-5, 180)
    ax4.set_ylim(-5, 105)

    # Plotting graphs, with labels and colours

    ax1.plot(x_vals, y_throttle, lw=1, color="blue")
    ax1.plot(x_vals, y_brake, lw=1, color="red")
    ax1.plot(x_vals, y_clutch, lw=1, color="green")
    ax2.plot(x_vals, y_rpm, lw=1, color="black")
    ax3.plot(x_vals, y_speed, lw=1, color="black")
    ax4.plot(x_vals, y_fuel, lw=1, color="black")

    ax1.set_ylabel("Throttle, brake, clutch (%)")
    ax2.set_ylabel("RPM")
    ax3.set_ylabel("Speed (MPH)")
    ax4.set_ylabel("Fuel (%)")

    plt.tight_layout()


index = count()
ani = FuncAnimation(fig, animate, interval=10)

plt.show()


# Release the socket.
sock.close()
```