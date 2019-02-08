# SmartBike-NXP

Weather monitoring system and emergency notifications based on Rapid IoT Prototyping Kit.

<img src="https://i.ibb.co/0j1wBrq/SBA.png" width="1000">

Always use technology to improve the world, if you are a black hat or gray hat hacker please abstain at this point ......... or at least leave your star to make me feel less guilty XP.

# Table of contents

* [Introduction](#introduction)
* [Materials](#materials)
* [NXP Software](#nxp-software)
* [The PCB](#the-pcb)
* [Development](#development)
* [The Final Product](#the-final-product)
* [Comments](#comments)
* [References](#references)

## Introduction:

It is a fact that today the use of bicycles and other multimodal transport is increasing due to various reasons, including traffic, increase of awareness in global warming,etc... 

This is very good for the planet but it puts the users in a continuous latent risk when using it due to factors such as:

- Inadequate climate to exercise due to high pollutants.
- Fatal dangers by motor vehicles such as cars or public transport.
- Danger of falling or an accident due to the fact that the vehicle does not have a safety system for the user (bicycles, unlike cars, do not have an airbag or any protection, duh).

An integral platform connected by cell phone for constant weather monitoring, whenever the user suffers a fall or accident, it can effectively notify relatives or the corresponding authority (or emergency service) with only a button.

The solutions that exist today are SmartWatch based, which provide monitoring of distance traveled but do not provide an emergency warning system or real-time analysis of air quality.

## Materials:

Hardware:
 
 - NXP Rapid IoT Prototyping Kit. x 1
 - A piece of strap or velcro strap.
 - An acrylic layer 3mm thick.
 - Screw (diameter 1.9 mm and length 4mm minimum). x 3
 
 Software:
 - Hosting server (https://infinityfree.net/).
 - Account NXP: http://rapid-iot-studio.nxp.com/ (Software development).
 - Pushetta Account: http://www.pushetta.com/
 - IBM Bluemix account or CloudMQTT. (MQTT Broker).
 Bluemix IBM: https://www.ibm.com/cloud/
 CloudMQTT: https://www.cloudmqtt.com/
 
## NXP Software:

Para el desarollo del software se utilizo Rapid IoT Studio tomando como template el ejemplo "Rapid IoT Weather Station", a este proyecto se le hiceron varios cambios importantes.

- Se elimino la comunicacion BT con el celular debido a que no necesitamos que los datos de los sensores sean mandados a la aplicacion.
- Se le añadieron modulos de comparacion para poder obtener los datos de los sensores con la siguiente configuracion.

<img src="https://i.ibb.co/0YNtJ9X/Capture.png" width="1000">

Los modulos de comparacion tienen que tener las siguientes conexiones para que funcionen correctamente.

- Para temperatura el bloque de comparacion va conectado a la funcion "GetTempStr".
- Para humedad el bloque de comparacion va conectado a la funcion "GetHumidityStr".
- Para presion el bloque de comparacion va conectado a la funcion "GetPressureStr".
- Para luz ambiental el bloque de comparacion va conectado directo al bloque del sensor "TSL2572AmbientLigth".
- Para calidad del aire el bloque de comparacion va conectado directo al bloque del sensor "CCS811AirQuality".

La configuracion de cada bloque de comparacion tiene la siguiente estructura.

<img src="https://i.ibb.co/xgjymqj/bloque1.png" width="150"><img src="https://i.ibb.co/3rCFkTJ/bloque2.png" width="150"><img src="https://i.ibb.co/HThGnBf/bloque3.png" width="150"><img src="https://i.ibb.co/jzV0cGF/bloque4.png" width="150"><img src="https://i.ibb.co/1zjBPkg/bloque5.png" width="150">

## The PCB:



## Development:


## The Final Product:


## Comments:



## References:

All the information about the technology used, and direct references are in our wiki:

Wiki: https://github.com/altaga/SmartBike-NXP/wiki

Top:

* [Table of Contents](#table-of-contents)
