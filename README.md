# Hassio-Hikvision-pos

# Description 
Integration of Homeassistant events/ messages into Hikvision DVR/NVR via POS (Point of Sales).

# My Reasoning

I am by no means a programmer and i have never used github before, i do not have much experience with YAML or Python. There is probably many other ways to easier and tidyier ways to accomplish the same task but i put this together with the knowledge i have obtained through my CCTV knowledge and google which is always everyones best friend.

I put this project together as we like to watch our pond from the CCTV system we have installed, i have installed a few sensors to watch for :-
	Pond temperature
	Water clarity
	Water depth
Rather than have all this information shown over several pieces of software screens i decided to utilise the platform IVMS-4200. IVMS will also enable me to search for homeassistant events and view the synced video stream so i decided to intergrate the two.

IVMS-4200 is a good free viewing platform for Hikvision CCTV system, it has plenty of features which include a great search function for POS

This can be easily expanded to include any homeassistant event and further thoughts include POS triggered recordings.

# Tools Required

Conmpatible Hikvision DVR/ NVR
	Not all Hikvison recorders have the POS function so please check compatibility. I have seen POS on most NVR's such as the "I" series NVR and some DVR such as the DS-7316HUHI-K4 on which this has been tested on.
	You Must have administrator access to the recorder to program the POS function

IVMS4200 
The windows version 2 and version 3 supports POS
The MAC version must be version 2.0.0.5 or higher?

I have tested this on the above versions.

Node Red

I used the ingress version of node red, through HASS-OS but any i assume can be used 

# Getting started


