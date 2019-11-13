---
layout: page
title: 'Arduino SDK'
---

The Arduino SDK facilitates WiFi-enabled Arduinos to connect to the dcd hub and update the values of the thing's property.

# Getting Started

Before we start, make sure you have followed the instruction to create DCD hub account from [Getting Started](https://datacentricdesign.org/docs/getting-started) page.

If you have already install Arduino IDE, you can directly skip to step 2.

## Step 1: Download and Install Arduino IDE

Select and download the latest version of the Arduino IDE from the link below. (Arduino IDE is an open-source programming text editor, which allows you to write and upload the code to the Arduino board.)

Follow the step in the link : [https://www.arduino.cc/en/main/software](https://www.arduino.cc/en/main/software)

## Step 2: Install the DCD SDK and other dependent libraries

Because the Arduino SDK relies on a few other libraries to work with, You need to install those libraries first. The libraries you need to installs are:

1. WiFiNINA: enables WiFi enabled Arduino to connect to the internet.
2. ArduinoMqttClient: This library allows you to connect to any MQTT server and send a message as the topic.
3. DCD SDK for Arduino: The SDK to interact with DCD hub.

To install these libraries into the Arduino IDE, go to `Arduino IDE -> Tools -> Manage Libraries.`

On the library manager windows appear on the screen, search for WiFiNINA and install the latest version of the library from there.

![Install WiFiNINA to Arduino Library Manager](/assets/res/arduino_wifiNiNA.png)

Follow the above same steps to install `ArduinoMqttClient` library and then our `DCD SDK for Arduino`.

![Install DCDHub SDK to Arduino Library Manager](/assets/res/arduino_dcdHub.png)

## Step 3: Explore one of the given example sketch

Once you have installed these three libraries, you can go to the example folder and open the given SDK example sketch, which shows basic interactions with the DCD hub.

`Arduino IDE -> File -> Examples -> DCD SDK Arduino -> examples -> Arduino_nano33_MQTT.`

## Step4: Code Description

In the sketch, first, go to the `arduno_secrets.h` write your wifi `SSID`, `SSID_PASSWORD`, `THING_ID` and `THING_TOKEN.`
(Here the Thing_ID and Thing_TOKEN are the same you derived from the getting started page.)

Next, go to the example code.

In the first two lines of the code (line 9-10), we are including DCD SDK library and our secret_credential file.

```c++
#include "arduino_secrets.h"
#include <dcd_hub_arduino.h>
```

Then we are defining the DCD class object from the library (line 12).

```c++
dcd_hub_arduino dcdHub;
```

In the arduino `setup()` function, we will connect to the DCD hub using a
connect function. We will provide our credentials derived from the "arduino_secrets.h" in this function as parameters.

```c++
dcdHub.connect(SECRET_SSID, SECRET_PASS, THING_ID, THING_TOKEN, "Arduino Nano 33 IoT");
```

Make sure to change the last parameter "Arduino Nano 33 IoT", which is your client_id on the hub.

This function will allow Arduino to connect to wifi and the MQTT server to send messages from Arduino to the server.

In the `loop()` function, we will first call the `keep_alive_mqtt()` to make sure that it will keep the server connection alive for the rest of the operation.

```c++
dcdHub.keep_alive_mqtt();
```

Then we have some value arrays `(float)` with `one`, `two`, and `three-dimension` values.

```c++
  float value[] = {random(5)};
  float value2[] = {random(80), random(25)};
  float value3[] = {random(80), random(25), random(60)};
```

You can use a similar structure-based for other kinds of value types derived from different sensors.

For example:

```c++
float AccelerometerValue [] = (aX, aY, aZ);
```

And then, we will call the `update_property()` functions to update the particular value of the thing's property.

```c++
dcdHub.update_property("random-02d3",value, 1);
dcdHub.update_property("my-random-property3-e0cf",value2, 2);
dcdHub.update_property("my-random-property2-563b",value3, 3);
```

In this function, the first parameters will be our `property_id`, which you can get from the DCD hub dashboard.

The second parameter in the function is your sensor `value`, which you want to update on the hub. This value can be just a float number or an array of float numbers. And the third and last parameter is the `dimension` of your value as an integer. e.g. 1, 2, 3.

## Step 5: Upload the code on the Arduino and open the serial monitor to see the response.

![Output on Arduino Serial](/assets/res/arduino_response.gif)

You can check more examples with Accelerometer and FSR Sensor from the Example provided with Arduino DCD hub SDK.
