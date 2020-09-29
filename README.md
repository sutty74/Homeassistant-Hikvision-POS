# Hassio-Hikvision-pos

## Description 
Integration of Homeassistant events/ messages into Hikvision DVR/NVR via POS (Point of Sales).

## My Reasoning

I am by no means a programmer and i have never used github before, i do not have much experience with YAML or Python. There is probably many other ways to easier and tidyier ways to accomplish the same task but i put this together with the knowledge i have obtained through my CCTV knowledge and google which is always everyones best friend.

I put this project together as we like to watch our pond from the CCTV system we have installed, i have installed a few sensors to watch for :-
	Pond temperature
	Water clarity
	Water depth
Rather than have all this information shown over several pieces of software screens i decided to utilise the platform IVMS-4200. IVMS will also enable me to search for homeassistant events and view the synced video stream so i decided to intergrate the two.

IVMS-4200 is a good free viewing platform for Hikvision CCTV system, it has plenty of features which include a great search function for POS

This can be easily expanded to include any homeassistant event and further thoughts include POS triggered recordings.

## Requirements

Conmpatible Hikvision DVR/ NVR

Not all Hikvison recorders have the POS function so please check compatibility. I have seen POS on most NVR's such as the "I" series NVR and some DVR such as the DS-7316HUHI-K4 on which this has been tested on.
You Must have administrator access to the recorder to program the POS function

**IVMS-4200**
The windows version 2 and version 3 supports POS
The MAC version must be version 2.0.0.5 or higher?

I have tested this on the above versions.

Node Red

I used the ingress version of node red, through HASS-OS but any i assume can be used 

# Getting started
I will be using IVMS 4200 version 3.1.1 to program and setup.

Installing and setting up your recorder within IVMS is not part of this tutorial so we will jump straight into remote configuration of the recorder.

## IVMS Step 1

From the recorder remote configuration menu in maintenance and management of IVMS scroll to the bottom of the window and select POS, then select the connection mode sub-menu.

![ivms pos option](https://user-images.githubusercontent.com/53712651/94496365-17509a00-01ec-11eb-97a9-5cc1af0e1805.PNG)

We will setup the connection modes first.

Starting with filter rule 1 select the connection mode as TCP,

Select a port, it can be any port but please make sure it's not currently in use by homeassistant,

Enter the IP address of your homeassistant server.

Click Save

## IVMS Step 2

Next select Filter Rule and setup as shown in the diagram below.

![ivms pos rule](https://user-images.githubusercontent.com/53712651/94603910-d362a100-028e-11eb-8a2c-f887fa47248a.PNG)

## IVMS Step 3

You will now need to setup the channel filter rule.

![ivms pos setup](https://user-images.githubusercontent.com/53712651/94604494-b24e8000-028f-11eb-8eb4-039fbe5a4c66.png)

Starting from the top, select which camera you want to overlay the POS on.

Select the Filter Rule ID, in this case it's 1

Select the POS Font Size

Make sure the POS Text overlay is ticked

Overlay mode, you can choose either Scroll or Page Turn.

The duration you want to show the POS text for.

OSD Colour (I haven't been able to select any other colour than white)

Timeout 5.

In the video window draw a box where you want to show the POS text to be displayed. As an additional step i setup a small privacy mask on the camera in the same position as the POS draw box, this helped display the POS text better.

Triggered Camera, for now select the camera channel you are currently displaying.



I had to reboot my recorder before it would accept any connections on the programmed port, this included when i altered or added any additional connections.


## Node Red













