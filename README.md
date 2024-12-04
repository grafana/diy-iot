# IoT with Arduino and Grafana

This repository contains information on how to start with IoT using [Arduino](https://www.arduino.cc/), [Grafana](https://grafana.com/oss/grafana/), [Prometheus](https://prometheus.io/) and [Grafana Loki](https://grafana.com/oss/loki/). In this repository, we will use [Grafana Cloud](https://grafana.com/products/cloud/) that includes hosted Grafana, Prometheus and Loki to remove the overhead of installing and maintaining these systems. All of these projects are open source and therefore if you want, you can run them by yourself.

## Contents

1. [Setting up and using Arduino IDE](#setting-up-and-using-arduino-ide)
1. [Setting up and using Grafana Cloud](#setting-up-and-using-grafana-cloud)
1. [Sending Metrics](#sending-metrics)
1. [Sending Logs](#sending-logs)
1. [Using Loki and Prometheus libraries](#using-loki-and-prometheus-libraries)
1. [Examples of projects built with Arduino and Grafana Cloud](#examples-of-projects-built-with-arduino-and-grafana-cloud)

## Setting up and using Arduino IDE

The open source Arduino IDE makes it easy to write code and upload it to the board. The Arduino software is easy to use for beginners, yet flexible enough for advanced users.

### Installing

To start, we are going to install [Arduino IDE](https://www.arduino.cc/en/Guide), a platform that’s used to write and upload programs to Arduino compatible boards. Follow the installing instruction for your OS.

### Set up Arduino IDE to support ESP32 board

This step is not necessary if Arduino hardware is used. If you decide to use ESP32 development boards _(we are using ESP32 boards in most of the projects listed below)_, we will need to do couple additional steps:

1. If your OS does not recognize the USB serial automatically, you’ll probably need to install **[CP210x USB to UART Bridge VCP Driver](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)** that lets your computer communicate with your development board.

1. **Add ESP32 boards definition** that adds support for the EP32 board. Go to **_Arduino > Preferences_** and add url https://dl.espressif.com/dl/package_esp32_index.json to the Additional Boards Manager URLs input. This open-source board definition adds support for programming ESP32 boards.
   
   <img src="https://user-images.githubusercontent.com/30407135/120989143-8325d200-c77f-11eb-8818-0f9974678867.png" alt="arduino" width="600"/>
   
   Then go to Tools > Boards > Boards Manager and add ESP32 board manager.
   
   <img src="https://user-images.githubusercontent.com/30407135/120989440-d8fa7a00-c77f-11eb-95b3-05f1a2d7b9c5.png" alt="arduino" width="600"/>
   
   Now, in Tools > Boards, you are able to select your ESP32 board. This tells the Arduino IDE which profile and base libraries to use when compiling the firmware image, and how to flash it to the board.
   
   <img src="https://user-images.githubusercontent.com/30407135/120989739-2545ba00-c780-11eb-8caf-be2da7ea98ba.png" alt="arduino" width="600"/>

### Using Arduino IDE and uploading program

In the Arduino environment, the programs that we are writing are called sketches. Before uploading the sketches to your development board, make sure that:

- Sketch can be compiled. Click on the **_Verify button_** on top of IDE. Verifying goes through your sketch, checks for errors and compiles it.
- In the **_Tools > Board_** submenu, you’ve selected the correct type of development port
- In the **_Tools > Port_** submenu, you’ve selected the new correct COM port

Sketches are then uploaded to development board using **_Upload button_** on top of IDE.

## Setting up and using Grafana Cloud

As mentioned, we are going to be using Grafana Cloud — which comes with hosted Grafana, Grafana Loki, and Graphite — for our data storage and data visualization. The free tier comes with 10,000 series for Prometheus metrics and 50GB for logs in Loki, which is definitely more than enough for simple monitoring solutions.

### Creating account

To start, we'll visit [Grafana Cloud signup](https://grafana.com/auth/sign-up/create-user) and create a new account. As soon as the account is all set up, we can see the portal with hosted Grafana, Loki, and Prometheus instances.

<img src="https://user-images.githubusercontent.com/30407135/120991670-25df5000-c782-11eb-9226-031dca99ae68.png" alt="grafana cloud" width="600"/>

#### Sending Metrics

To set up your project for sending metrics, click on the `Send Metrics` button

<img src="https://user-images.githubusercontent.com/10332331/121828047-63654100-cc8c-11eb-888c-6a3f331ecfea.png" alt="send metrics" width="600"/>

From the next page, first you will want to copy the `Remote Write Endpoint` address

<img src="https://user-images.githubusercontent.com/10332331/121828126-a6bfaf80-cc8c-11eb-818f-dbdb62412fc5.png" alt="send metrics" width="600"/>

And put the value in your `config.h`

```
#define GC_URL "prometheus-blocks-prod-us-central1.grafana.net"
```

Next copy the `Username / Instance ID`

<img src="https://user-images.githubusercontent.com/10332331/121828819-b5a76180-cc8e-11eb-9f37-23dd2406d3f8.png" alt="send metrics" width="600"/>

```
#define GC_USER "137822"
```

Then create an API key by clicking `Generate now`

<img src="https://user-images.githubusercontent.com/10332331/121829204-df14bd00-cc8f-11eb-9829-17e48cacf47d.png" alt="send metrics" width="600"/>

Give the key a meaningful name and ensure the permission includes `metrics:write`.

Then click `Create API Key`

Copy the value into the `GC_PASS` field

```
#define GC_PASS "eyJrIjoiMTkzNDFkMzM2YTNhZTRlNmE4ZDkyMjgzSTBhNGFiYTcwY2VjMzVjNiIsIm4iOiJlc3AzMi10ZXN0LTMiLCJpZCI6NDIwMDY1fQ=="
```

The final result should look something like:

```
#define WIFI_SSID     "your_ssid"
#define WIFI_PASSWORD "your_wifi_password"

#define GC_URL "prometheus-blocks-prod-us-central1.grafana.net"
#define GC_PATH "/api/prom/push"
#define GC_PORT 443
#define GC_USER "137822"
#define GC_PASS "eyJrIjoiMTkzNDFkMzM2YTNhZTRlNmE4ZDkyMjgzSTBhNGFiYTcwY2VjMzVjNiIsIm4iOiJlc3AzMi10ZXN0LTMiLCJpZCI6NDIwMDY1fQ=="
```

#### Sending Logs

Similar to metrics, click the `Sending Logs` button

<img src="https://user-images.githubusercontent.com/10332331/121830077-3ae04580-cc92-11eb-9663-a02e10a27349.png" alt="send logs" width="600"/>

We will take some values from the `Data Source Settings` section starting with the URL

<img src="https://user-images.githubusercontent.com/10332331/121830346-d376c580-cc92-11eb-9157-aa844b9bf723.png" alt="send logs" width="600"/>

```
#define GC_URL "logs-prod-us-central1.grafana.net"
```

Next grab the `User`

<img src="https://user-images.githubusercontent.com/10332331/121830467-1df84200-cc93-11eb-851e-517bf98b484e.png" alt="send logs" width="600"/>

```
#define GC_USER "8435"
```

And now generate an API key by clicking `Generate now`

<img src="https://user-images.githubusercontent.com/10332331/121830638-6f083600-cc93-11eb-8e16-8ba44ddbffaa.png" alt="send logs" width="600"/>

Create a meaningful name and ensure the permission includes `logs:write`.

The final `config.h` should look similar to this

```
#define WIFI_SSID     "your_ssid"
#define WIFI_PASSWORD "your_wifi_pass"

#define GC_URL "logs-prod-us-central1.grafana.net"
#define GC_PORT 443
#define GC_PATH "/loki/api/v1/push"
#define GC_USER "8435"
#define GC_PASS "eyJrIjoiMTkzNDFkMzM2YTNhZTRlNmE4ZDkyMjgzSTBhNGFiYTcwY2VjMzVjNiIsIm4iOiJlc3AzMi10ZXN0LTMiLCJpZCI6NDIwMDY1fQ=="
```

### Creating more API Keys

At this point, or anytime in the future, we can create the API keys for Loki and Prometheus, to publish metrics from the monitoring system to these databases. The API key can be created by clicking on **_API Keys_** in the navigation on the left side. Then we click on **_+ Add API Key_** and create API keys.

<img src="https://user-images.githubusercontent.com/30407135/120992526-fbda5d80-c782-11eb-86d4-8d1e88df6a2e.png" alt="API keys" width="600"/>

### Visualised data in Grafana

Hosted Loki & Prometheus instances are automatically added as data sources in your hosted Grafana. You can find them under the name `grafanacloud-NAME-logs` and `grafanacloud-NAME-prom`. You can use these data sources with data from monitoring solutions to create dashboards or use them in Explore.

## Using Loki and Prometheus libraries

We have created following Arduino libraries to make it easier for you to send data to Prometheus and Loki:

- https://github.com/grafana/prometheus-arduino
- https://github.com/grafana/loki-arduino
- https://github.com/grafana/arduino-prom-loki-transport
- https://github.com/grafana/arduino-snappy-proto

All libraries can be downloaded and used directly through your Arduino IDE Library manager.

Start with opening Arduino IDE. Then go to _Tools > Manage Libraries..._

<img src="https://user-images.githubusercontent.com/30407135/122038232-aed73680-cdd5-11eb-8b54-d02a96f8f76b.png" alt="libraries" width="600"/>

Search for _"Prometheus"_ and install **PrometheusArduino**, **PromLokiTransport**, **SnappyProto** (You may be prompted to install additional libraries, saying yes will be the easiest path, but you can also install the necessary libraries manually, see below)

<img src="https://user-images.githubusercontent.com/30407135/122038308-c1517000-cdd5-11eb-8596-8612eb2248de.png" alt="libraries" width="600"/>

Search for _"Loki"_ and install **GrafanaLoki**

<img src="https://user-images.githubusercontent.com/30407135/122043638-d7fac580-cddb-11eb-92bd-6c548596fa42.png" alt="libraries" width="600"/>

For list of additional libraries that you might need to install, refer to **[PrometheusArduino dependencies](https://github.com/grafana/prometheus-arduino#dependencies)** and **[GrafanaLoki dependencies](https://github.com/grafana/loki-arduino#dependencies)**

For how to use **[PrometheusArduino](https://github.com/grafana/prometheus-arduino)** and **[GrafanaLoki](https://github.com/grafana/loki-arduino)**, refer to their READMEs and examples folder

## Examples of projects built with Arduino and Grafana Cloud

We have created a couple of DIY IoT projects that you can easily create by yourself. These project have README that contains all information needed for you to re-create them.

1. [Room comfort monitoring](https://github.com/ivanahuckova/room_comfort_monitoring_grafana)
1. [Sourdough monitoring](https://github.com/ivanahuckova/sourdough_monitoring_grafana)
1. [Avocado plant monitoring](https://github.com/ivanahuckova/avocado_monitoring_grafana)
1. [Candle monitoring](https://github.com/ivanahuckova/candle_monitoring_grafana)
1. [Dog tracking](https://github.com/slim-bean/sully-tracker)
1. [Time tracking](https://github.com/slim-bean/timefidget)
