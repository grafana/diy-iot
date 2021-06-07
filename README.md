# IoT with Grafana
This repository contains information on how to start with IoT using [Arduino](https://www.arduino.cc/), [Grafana](https://grafana.com/oss/grafana/), [Prometheus](https://prometheus.io/) and [Grafana Loki](https://grafana.com/oss/loki/). In this repository, we will use [Grafana Cloud](https://grafana.com/products/cloud/) that includes hosted Grafana, Prometheus and Loki to remove the overhead of installing and maintaining these systems. All of these projects are open source and therefore if you want, you can run them by yourself. 

## Contents
1. Setting up and using Arduino IDE
1. Setting up Grafana Cloud 
1. Using Loki and Prometheus libraries
1. Example of projects built with Arduino and Grafana Cloud

## Setting up and using Arduino IDE
The open source Arduino IDE makes it easy to write code and upload it to the board. The Arduino software is easy to use for beginners, yet flexible enough for advanced users. 

### Installing
To start, we are going to install [Arduino IDE](https://www.arduino.cc/en/Guide), a platform that’s used to write and upload programs to Arduino compatible boards. Follow the installing instruction for your OS.

### Set up Arduino IDE to support ESP32 board
This step is not necessary if Arduino hardware is used. If you decide to use ESP32 development boards _(we are using ESP32 boards in most of the projects listed below)_, we will need to do couple additional steps:

1. If your OS won’t recognize the USB serial automatically, you’ll probably need to install **[CP210x USB to UART Bridge VCP Driver](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)** that lets your computer communicate with your development board.

1. **Add ESP32 boards definition** that adds support for the EP32 board. Go to ***Arduino > Preferences*** and add url https://dl.espressif.com/dl/package_esp32_index.json to the Additional Boards Manager URLs input. This open-source board definition adds support for programming ESP32 boards.
<img src="https://user-images.githubusercontent.com/30407135/120989143-8325d200-c77f-11eb-8818-0f9974678867.png" alt="arduino" height="500"/>
Then go to Tools > Boards > Boards Manager and add ESP32 board manager.
<img src="https://user-images.githubusercontent.com/30407135/120989440-d8fa7a00-c77f-11eb-95b3-05f1a2d7b9c5.png" alt="arduino" height="500"/>
Now, in Tools > Boards, you are able to select your ESP32 board. This tells the Arduino IDE which profile and base libraries to use when compiling the firmware image, and how to flash it to the board. 
<img src="https://user-images.githubusercontent.com/30407135/120989739-2545ba00-c780-11eb-8caf-be2da7ea98ba.png" alt="arduino" height="500"/>

### Using Arduino IDE and uploading program

In the Arduino environment, the programs that we are writing are called sketches. Before uploading the sketches to your development board, make sure that:
- Sketch can be compiled. Click on the ***Verify button*** on top of IDE. Verifying goes through your sketch, checks for errors and compiles it. 
- In the ***Tools > Board*** submenu, you’ve selected the correct type of development port
- In the ***Tools > Port*** submenu, you’ve selected the new correct COM port
- In the ***Tools > Board*** submenu, you’ve selected the correct type of development port

Sketches are then uploaded to development board using ***Upload button*** on top of IDE.

## Set up Grafana Cloud
As mentioned, we are going to be using Grafana Cloud — which comes with hosted Grafana, Grafana Loki, and Graphite — for our data storage and data visualization. The free tier comes with 10,000 series for Prometheus metrics and 50GB for logs in Loki, which is definitely more than enough for simple monitoring solutions. 

To start, we'll visit [Grafana Cloud signup](https://grafana.com/auth/sign-up/create-user) and create a new account. As soon as the account is all set up, we can see the portal with hosted Grafana, Loki, and Prometheus instances.
<img src="https://user-images.githubusercontent.com/30407135/120991670-25df5000-c782-11eb-9226-031dca99ae68.png" alt="grafana cloud" height="500"/>

At this point, or anytime in the future, we can create the API keys for Loki and Prometheus, to publish metrics from the monitoring system to these databases. The API key can be created by clicking on ***API Keys*** in the navigation on the left side. Then we click on ***+ Add API Key*** and create API keys. 
<img src="https://user-images.githubusercontent.com/30407135/120992526-fbda5d80-c782-11eb-86d4-8d1e88df6a2e.png" alt="grafana cloud" />

## Using Loki and Prometheus libraries

## Example of projects built with Arduino and Grafana Cloud
We have created couple of DIY IoT projects that you can easily create by yourself. These project have README that contains all information needed for you to re-create them.

1. [Room comfort monitoring](https://github.com/ivanahuckova/room_comfort_monitoring_grafana)
1. [Sourdough monitoring](https://github.com/ivanahuckova/sourdough_monitoring_grafana)
1. [Avocado plant monitoring](https://github.com/ivanahuckova/avocado_monitoring_grafana)
1. [Candle monitoring](https://github.com/ivanahuckova/candle_monitoring_grafana)

