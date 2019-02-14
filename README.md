# SmartBike-NXP

Weather monitoring system and emergency notifications based on Rapid IoT Prototyping Kit.

<img src="https://i.ibb.co/0j1wBrq/SBA.png" width="1000">

Always use technology to improve the world, if you are a black hat or gray hat hacker please abstain at this point ......... or at least leave your star to make me feel less guilty XP.

# Table of contents

* [Introduction](#introduction)
* [Materials](#materials)
* [NXP Software](#nxp-software)
* [WebPages and MQTT](#webpages-and-mqtt)
* [Pushetta notifications](#pushetta-notifications)
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
 - Hosting server: https://infinityfree.net/.
 - Account NXP: http://rapid-iot-studio.nxp.com/ (Software development).
 - Pushetta Account: http://www.pushetta.com/
 - IBM Bluemix account or CloudMQTT. (MQTT Broker).
 Bluemix IBM: https://www.ibm.com/cloud/
 CloudMQTT: https://www.cloudmqtt.com/
 
## NXP Software:

For the development of the software we use the Rapid IoT Studio using the template "Rapid IoT Weather Station" as template.

- The BT communication with the cell phone was eliminated because we do not need the data from the sensors to be sent to the application.
- Comparison modules were added in order to obtain the data from the sensors with the following configuration.

<img src="https://i.ibb.co/0YNtJ9X/Capture.png" width="1000">

The comparison modules have to have the following connections to work correctly.

- For temperature, the comparison block is connected to the "GetTempStr" function.
- For humidity the comparison block is connected to the "GetHumidityStr" function.
- To press the comparison block is connected to the "GetPressureStr" function.
- For ambient light, the comparison block is connected directly to the sensor block "TSL2572AmbientLigth".
- For air quality, the comparison block is connected directly to the sensor block "CCS811AirQuality".

The configuration of each comparison block has the following structure.

<img src="https://i.ibb.co/xgjymqj/bloque1.png" width="170"><img src="https://i.ibb.co/3rCFkTJ/bloque2.png" width="170"><img src="https://i.ibb.co/HThGnBf/bloque3.png" width="170"><img src="https://i.ibb.co/jzV0cGF/bloque4.png" width="170"><img src="https://i.ibb.co/1zjBPkg/bloque5.png" width="170">

The comparison blocks do not have the "Set Purlpe LED On" and "Set Yellow LED On" option set, we will have to modify the skill code "Set White LED On" and "Toogle Red LED" with the following.

<img src="https://i.ibb.co/0r9KsPB/Code1.png" width="500">

The code:

    ATMO_Status_t EmbeddedNxpRpkRgbLed_setWhiteOn(ATMO_Value_t *in, ATMO_Value_t *out) 
    {
    RGB_Led_Set_State(RGB_LED_BRIGHT_HIGH, RGB_LED_COLOR_PURPLE);
    return ATMO_Status_Success;
    }

    ATMO_Status_t EmbeddedNxpRpkRgbLed_toggleRed(ATMO_Value_t *in, ATMO_Value_t *out) 
    {
       RGB_Led_Set_State(RGB_LED_BRIGHT_HIGH, RGB_LED_COLOR_YELLOW);
       return ATMO_Status_Success;
    }

For the app, the following configuration is used to make the emergency call.

<img src="https://i.ibb.co/WBdyRL0/app1.png" width="600"><img src="https://i.ibb.co/mz98yt5/app2.png" width="250">

## WebPages and MQTT:

For the configuration of the URL we will obtain our location, we will create an account in https://infinityfree.net/ and create two web pages that contain the codes in the folder "WebPages".

<img src="https://i.ibb.co/C677Tx4/web1.png" width="360"><img src="https://i.ibb.co/kmmdMW4/web2.png" width="300">

The geolocation can be done in two different ways, the first uses the geolocation provided by our device through mobile data or WiFi. This is the most accurate but requires that the user accept that their browser on the cell phone allows to determine the location.

    <script>
    var x = document.getElementById("demo");

    function getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition);
      } else { 
        x.innerHTML = "Geolocation is not supported by this browser.";
      }
    }

    function showPosition(position) {
      x.innerHTML = "Latitude: " + position.coords.latitude + 
      "<br>Longitude: " + position.coords.longitude;
    }
    </script>

The second way to get the geolocation is by IP location, through a GET Request by an AJAX script to a web page that provides IP location as http://extreme-ip-lookup.com/json/, in the example of location.html is designed with IP localization, but it is possible to change it easily with the previous script.

    <script>
     $.ajax({
        dataType: "json",
        url: "http://extreme-ip-lookup.com/json/",
        data: data,
        success: function (data)
        {
        var datav = JSON.parse(JSON.stringify(data))
        doStuff(datav.lat, datav.lon) 
    </script>

    <script>
    function doStuff(mylat, mylong) 
    {
      if (document.getElementById("GeoAPI")) 
      {
        document.getElementById("GeoAPI").innerHTML = "<iframe style=\"width: 400px; height: 400px\" frameborder=\"0\" scrolling=\"no\" marginheight=\"0\" marginwidth=\"0\" src=\"//maps.google.com/?ll=" + mylat + "," + mylong + "&z=16&output=embed\"></iframe>";
      }
    }
    </script>


Once both pages are created, we will have to create our MQTT broker, which we will create from any of the following two options.

CloudMQTT: https://www.cloudmqtt.com/ 
IBM Bluemix: https://www.ibm.com/internet-of-things 

For the configuration of broker we recommend to follow the manual, which explains in detail how to do it.

Link of the manual of Watson IoT Platform: https://github.com/altaga/The-Ultimate-IBM-Watson-IoT-Platform-Guide

Through MQTT we will send our latitude and longitude to the web page, in order to be able to deploy it on the web or send notifications to Pushetta.

## Pushetta notifications:

Once the broker is configured successfully, only the pushetta configuration will be needed to send the notifications to any person we want.

As a first step we will have to create an account at http://www.pushetta.com/ and once this is created we will create a channel to send notifications.

<img src="https://i.ibb.co/QDdDN3g/push.png" width="800">

When the channel for notifications is created, we will go to the tab of the dashboard, we will find its APIKEY, this will use later to send notifications with python.

<img src="https://i.ibb.co/HTSPTQZ/api.png" width="600">

Because Javascript only supports Pushetta combined with NodeJS, we had to perform an additional process to make notifications through pushetta, using a virtual machine in Watson Studio running in the cloud, the process to create it is as follows.

1.- Create an account in Bluemix IBM: https://www.ibm.com/cloud/.

2.- Create the WatsonStudio service.

<img src = "https://i.ibb.co/YQzB3d9/watsonstudio.png" width = "400">
3.- Open WatsonStudio service.

<img src = "https://i.ibb.co/QC8G5C1/studio2.png" width = "400">
4.- Create a new project.

<img src = "https://i.ibb.co/3ydmSL0/studio3.png" width = "400">
5.- Select the Standard package.

<img src = "https://i.ibb.co/h7SF6Ty/studio4.png" width = "400">
6.- Put any name to the project.

<img src = "https://i.ibb.co/p4NJryK/studio5.png" width = "400">
7.- Go to the "Enviroments" tab.

<img src = "https://i.ibb.co/G0xYh6w/studio6.png" width = "400">
8.- Select Default Python 3.5 Free.

<img src = "https://i.ibb.co/ypz6kxJ/studio7.png" width = "400">
9.- Select "New notebook".

<img src = "https://i.ibb.co/8XcBDxH/studio8.png" width = "400">
10.- Put any name.

<img src = "https://i.ibb.co/cFQnpzh/studio9.png" width = "400">

11.- With this we will have configured our python notebook in the cloud to run the code that will be in the "Python Code" folder.
<img src="https://i.ibb.co/3dzTP8V/studio10.png" width="400">

To make the python application work, we will use the same credentials that we use in the MQTT broker and the APIKEY that we obtained from pushetta, once we execute the code we can close the window and our application will be running in the cloud.

NOTE: Due to the limitations of the free plans the application can not run for more than 100 hours in the month and also the application will be turned off every 12 hours, we simply recommend turning it on every time you go out to exercise.

## The Final Product:

For the final product they decided to use these values because they are the maximum recommended values for exercise.

| Sensor              | Color      | Max Value    |
|---------------------|----------- |--------------|
| **Temperature**     | Red        | 30 ℃        | 
| **Humidity**        | Green      | 80 %rH       | 
| **Pressure**        | Blue       | 800 hPa      |
| **Ambient Light**   | Purple     | 2000 lx      | 
| **AirQuality**      | Yellow     | 15.0 ppb     |  

An acrylic base was placed on the Rapid IoT Prototyping Kit and a watch strap to be placed on a bicycle, however during the experiments we noticed that it was also possible to use it as a watch, as shown in the following images.

<img src="https://i.ibb.co/9pBBgRG/IMG-2432.jpg" width="420"><img src="https://i.ibb.co/N18sBRB/IMG-2436.jpg" width="420">

Video: Click on the image:

[![SBA](https://i.ibb.co/0j1wBrq/SBA.png)](https://youtu.be/m8FgXPyPd38)

Sorry github does not allow embed videos.

## Comments:

In this project we discovered that it was very easy to program the limits of the sensors in the NXP Studio, besides that its simplicity to make the connections between the nodes facilitated the programming of the application, also thanks to our experience with HTML5 we were able to program easily web applications to perform the task of geolocation and communication by MQTT.

## References:

All the information about the technology used, and direct references are in our wiki:

Wiki: https://github.com/altaga/SmartBike-NXP/wiki

Top:

* [Table of Contents](#table-of-contents)
