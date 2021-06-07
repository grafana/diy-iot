# IoT with Grafana
This repository contains information on how to start with IoT using [Arduino](https://www.arduino.cc/), [Grafana](https://grafana.com/oss/grafana/), [Prometheus](https://prometheus.io/) and [Grafana Loki](https://grafana.com/oss/loki/). In this repository, we will use [Grafana Cloud](https://grafana.com/products/cloud/) that includes hosted Grafana, Prometheus and Loki to remove the overhead of installing and maintaining these systems. All of these projects are open source and if you want, you can run them by yourself. 

## Contents
1. Set up Arduino
1. Set up Grafana Cloud 
1. Using Loki and Prometheus libraries
1. Example of projects built with Arduino and Grafana Cloud

## Set up Arduino
Arduino is an open-source electronics platform based on easy-to-use hardware and software. The Arduino software is easy-to-use for beginners, yet flexible enough for advanced users. 

### Installing
To start, you’re going to have to install [Arduino IDE](https://www.arduino.cc/en/software), a platform that’s used to write and upload programs to Arduino compatible boards. 

### Set up Arduino IDE to support ESP32 board
This step is not necessary if you are using Arduino hardware. If you decide to use ESP32 development boards that are compatible with Arduino, you need to do couple additional steps:

1. If your OS won’t recognize the USB serial automatically, you’ll probably need to install **[CP210x USB to UART Bridge VCP Driver]**(https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers) that lets your computer communicate with your development board.

1. **Add ESP32 boards definition** that adds support for your EP32 board. Go to _Arduino > Preferences_ and add url https://dl.espressif.com/dl/package_esp32_index.json to the Additional Boards Manager URLs input. This open-source board definition adds support for programming ESP32 boards.
![image](https://user-images.githubusercontent.com/30407135/120989143-8325d200-c77f-11eb-8818-0f9974678867.png)
Then go to _Tools > Boards > Boards Manager_ and add ESP32 board manager.
![image](https://user-images.githubusercontent.com/30407135/120989440-d8fa7a00-c77f-11eb-95b3-05f1a2d7b9c5.png)
Now, in _Tools > Boards_, you are able to select your ESP32 board. This tells the Arduino IDE which profile and base libraries to use when compiling the firmware image, and how to flash it to the board. 
![image](https://user-images.githubusercontent.com/30407135/120989739-2545ba00-c780-11eb-8caf-be2da7ea98ba.png)




