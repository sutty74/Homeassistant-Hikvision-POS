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

Example 1 shows a very simple timer sending the sun.sun state to the DVR as filter rule 1


![nodered example1](https://user-images.githubusercontent.com/53712651/94610575-694ef980-0298-11eb-8dde-91e788822d94.png)


Each node was setup as follows

![nodered attributes](https://user-images.githubusercontent.com/53712651/94612503-1cb8ed80-029b-11eb-9408-7e241374dd84.png)


Else you can import the code below directly into node-RED

''' [{"id":"d5a6bc5c.a7bf8","type":"tcp request","z":"7a9b006b.6cd62","server":"192.168.0.3","port":"1031","out":"time","splitc":"0","name":"Send Event to DVR Rule 1","x":740,"y":120,"wires":[[]]},{"id":"6afaa498.6496ec","type":"inject","z":"7a9b006b.6cd62","name":"30 second Timer","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"30","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":190,"y":120,"wires":[["d9e3a5d4.147978"]]},{"id":"d9e3a5d4.147978","type":"api-render-template","z":"7a9b006b.6cd62","name":"Sun template","server":"b999d14.8e2393","template":"The sun is {{ states('sun.sun') }} ","resultsLocation":"payload","resultsLocationType":"msg","templateLocation":"template","templateLocationType":"msg","x":430,"y":120,"wires":[["d5a6bc5c.a7bf8"]]},{"id":"f06698bc.fc02e8","type":"comment","z":"7a9b006b.6cd62","name":"Example 1  (Simple Recurring message)","info":"","x":240,"y":60,"wires":[]},{"id":"b999d14.8e2393","type":"server","z":"","name":"Home Assistant","legacy":false,"addon":true,"rejectUnauthorizedCerts":true,"ha_boolean":"y|yes|true|on|home|open","connectionDelay":true,"cacheJson":true}] '''


Example 2 shows how i simply expanded on this to display more than one event in sequence.

![nodered example 2](https://user-images.githubusercontent.com/53712651/94613678-e11f2300-029c-11eb-8964-2247c78e23ee.png)

Node-RED code

	[{"id":"cdc3ac16.68dfe","type":"inject","z":"7a9b006b.6cd62","name":"40 sec timer","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"40","crontab":"","once":true,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":120,"y":240,"wires":[["9a1bb87.1a43e48","4be4091f.c11568"]]},{"id":"9a1bb87.1a43e48","type":"api-render-template","z":"7a9b006b.6cd62","name":"Pond temp template","server":"b999d14.8e2393","template":" Pond Temp\n  {{ states('sensor.pond_temperature') }}C ","resultsLocation":"payload","resultsLocationType":"msg","templateLocation":"template","templateLocationType":"msg","x":340,"y":280,"wires":[["179c9b4f.ec0415"]]},{"id":"4be4091f.c11568","type":"delay","z":"7a9b006b.6cd62","name":"","pauseType":"delay","timeout":"10","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":240,"y":340,"wires":[["108976a6.894099","ee90276c.0c5a98"]]},{"id":"108976a6.894099","type":"api-render-template","z":"7a9b006b.6cd62","name":"Pond clarity template","server":"b999d14.8e2393","template":" Clarity \n   {{ states('sensor.tds_ppm_rounded') }} ppm ","resultsLocation":"payload","resultsLocationType":"msg","templateLocation":"template","templateLocationType":"msg","x":540,"y":340,"wires":[["179c9b4f.ec0415"]]},{"id":"ee90276c.0c5a98","type":"delay","z":"7a9b006b.6cd62","name":"","pauseType":"delay","timeout":"10","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":400,"y":400,"wires":[["e9b5442f.6f2058","66fa6cc4.571de4"]]},{"id":"e9b5442f.6f2058","type":"api-render-template","z":"7a9b006b.6cd62","name":"Pond water level template","server":"b999d14.8e2393","template":" Water Level \n{{ states('sensor.pondwater_level_percent') }} % ","resultsLocation":"payload","resultsLocationType":"msg","templateLocation":"template","templateLocationType":"msg","x":690,"y":400,"wires":[["179c9b4f.ec0415"]]},{"id":"66fa6cc4.571de4","type":"delay","z":"7a9b006b.6cd62","name":"","pauseType":"delay","timeout":"10","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":560,"y":460,"wires":[["cb185088.e3676"]]},{"id":"cb185088.e3676","type":"api-render-template","z":"7a9b006b.6cd62","name":"Outside Temp template","server":"b999d14.8e2393","template":"Outside Temp {{ states('sensor.outside_temp') }}C ","resultsLocation":"payload","resultsLocationType":"msg","templateLocation":"template","templateLocationType":"msg","x":790,"y":460,"wires":[["179c9b4f.ec0415"]]},{"id":"179c9b4f.ec0415","type":"tcp request","z":"7a9b006b.6cd62","server":"192.168.0.3","port":"1031","out":"time","splitc":"0","name":"Send Event to DVR Rule 1","x":1000,"y":280,"wires":[[]]},{"id":"fc134a06.701048","type":"comment","z":"7a9b006b.6cd62","name":"Example 2 (Rotation of events)","info":"","x":190,"y":200,"wires":[]},{"id":"b999d14.8e2393","type":"server","z":"","name":"Home Assistant","legacy":false,"addon":true,"rejectUnauthorizedCerts":true,"ha_boolean":"y|yes|true|on|home|open","connectionDelay":true,"cacheJson":true}]










