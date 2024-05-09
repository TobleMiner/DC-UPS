DC uninterruptible power supply
===============================

This repository contains the design files for a DC uninterruptible power
supply well suited to run network appliances like small switches, home
routes and WLAN access points.

**I would not recommend ordering RevC yet as it is still untested.**  
**I would not recommend ordering RevB yet as there are still some issues with the current design.
See [Rev B issues](https://github.com/TobleMiner/DC-UPS#rev-b-issues) below.**

![Render of the device](assets/render_front_small.png)

# Features

 - 3 DC outputs
 - Selectable output voltage (9V/12V/12.5V/15V)
 - USB-C power output (5V only, 3A)
 - Builtin, easy to replace batteries (2x Li-Ion 18650)
 - Builtin Prometheus-exporter with comprehensive metrics
 - Open Source firmware (ESP32-based, esp-idf framework)
 - Easy software updates via back USB port

# Construction

The mechanical construction of the DC UPS is based on 4 assemblies:
 - Hammond Manufacturing 1455L1201BK aluminum case
 - Main PCB
 - Aluminum core PCB front panel
 - Aluminum core PCB rear panel

The three PCBs have been optimized for manufacturing through JLCPCB.  
The main PCB is also designed to be suitable for assembly through JLCPCB.  

The relevant fabrication files are located in `fab`.  

For final assembly it is recommended to countersink the screw holes in
front and rear panel. This will allow the screws included with the case to
sit flush.

## Main PCB assembly

When using the JLCPCB assembly service only a very limited number of
components needs to be assembled by hand:
 - Battery holder B1
 - OLED display U17

## Thermals

For better thermal management I would highly recommend to install a thick,
generously sized thermal pad below U4 and L2 contacting the bottom case if
using the UPS DC outputs beyond 40W output power.

# Firmware

The DC UPS is running Open Source firmware. Firmware source can be found on
GitHub here: [https://github.com/TobleMiner/dc-ups-firmware](https://github.com/TobleMiner/dc-ups-firmware)

# Why?

For a long time I was running the core of my home network (a cable modem,
router and switch) off a normal line-interactive AC UPS.  
This was never ideal for a number of reasons. First of all the UPS was
using sealed lead acid batterys. The UPS did not seem to treat those too
kindly and ended up degrading them beyond the point of useful capacity
within 2 - 5 years, depending on quality of the battery.  
Also I noticed that the UPS was always getting somewhat warm, suggesting
high idle power consumption. In this particular setup the core network
components only use about 12W. The AC UPS alone already used 10W while
idle. Quite the waste of power.  
Another worrying factor was overall system reliability. With the AC UPS
there were three seperate wall warts in use, each powering a single
device. Powering all network compoenents with DC directly this is down
to just one wall wart at the power input of the UPS itself.  
Additionally I was never quite happy with the monitoring options offered
by the AC UPS. While it was equipped with a USB port for monitoring the
available metrics were minimal. The DC UPS offers an Ethernet port and
hosts a Prometheus exporter offering all kinds of metrics relating to
power input and output, temperatures, battery health and battery
management internals.  
Finally the DC UPS is a lot smaller than available AC UPS options. The
following picture shows a comparison between the AC UPS I have been using
before and the DC UPS:  

![Size comparison between AC UPS and DC UPS. DC UPS is way smaller](assets/size_comparison_small.jpg)

# Rev B issues

Rev B is still more of a prototype than I'd like.
This is the list of currently know issues:
 - DCDC U4 startup is too slow on power failure, remove Q13
 - R47 and R48 are too high resistance for effective testing
