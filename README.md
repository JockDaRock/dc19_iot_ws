# DevNet Create 2019 WS - IoT 101 (MacOS)

## Install Dependencies for Mac

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

`$ brew install python`

`$ brew cask install docker`

`$ brew install git`

`$ pip3 install esptool adafruit-ampy paho-mqtt`

## IoT 101 Baseline

link to Google Slides https://docs.google.com/presentation/d/11vLKOqJNSnMwer-fsB9PZ_pIcQjXSQbKxRkvYduRPTo/edit?usp=sharing


## Docker Dash and MQTT / Websockets Broker

`$ mkdir dash-ws`

`$ cd dash-ws`

Next, we need to download the source code from GitHub that we will be using to build our dashboards and other tools.

```

$ git clone https://github.com/JockDaRock/mqtt-docker-lab-code

$ git clone https://github.com/JockDaRock/freeboard-lab-code

```

### Now it is time to build the app from our source code

Change directories into our first code base

```
$ cd mqtt-docker-lab-code
```

Then, using the docker engine we will build our mqtt broker app.

```
$ docker build -t mqtt-brk:latest .
```

Once that is done building, we will run our application on our laptops.

```
$ docker run -d -p 1883:1883 -p 9001:9001 mqtt-brk:latest

$ docker ps
```

Next, we will build the dashboard application we will connect to for our MQTT application.

First, we need to get back out of our current directory and change into our freeboard app directory.

```
$ cd ..

$ cd freeboard-lab-code

```

Next, we need to build out freeboard image.

```
$ docker build -t dc-freeboard:latest .
```

Now, we are going to run our application on our laptop.

```
$ docker run -d -p 8585:80 dc-freeboard:latest

$ docker ps
```

Our applications are running! We can now start connecting our dashboard to our MQTT broker app.

### Now it is time to connect our dash and datasource together.

We need to open up our browser to a couple of sites.

Let's open our browser to `http://www.hivemq.com/demos/websocket-client/`.

Then, we will open an additional browser tab to `http://127.0.0.1:8585`.

#### Data Connections

* Now, in the Freeboard Dashboard app, under **Datasources**, click the add link.
* In the drop down menu, Select type **MQTT**
* Name your first pane of Data, **Engine Data**
* replace the default TOPIC with **devnet/dash**
* change the server variable to **127.0.0.1**
* change the port number to **9001**
* change _USE ENCRYPTION_ to **NO**
* then click save.

Our data source is now connected to our dashboard.

From our Websocket client we will send the following message on devnet/dash.

```
{"Data": {"RPM": 35}}
```

### Building our dashboard widgets

On the same dashboard site, click the **ADD PANE** button.

* Inside the pane, click the **+** button.
* For TYPE, select Text.
* For Title, type RPM Text.
* For value click DataSource, and then add the data source we connected.
* Click SAVE.

We have now created our first real-time widget on our dashboard. Let's practice a few more.

## MQTT

Create a directory for us to work in.

`cd $HOME`

`$ mkdir iot-prot0`

`$ cd iot-prot0`

Now let's clone the code we will need in this workshop.

`$ git clone https://github.com/JockDaRock/iot-protocols`

Change directories into the our Python code base

`cd ./iot-protocols/mqtt`

Next we will run our python program.

`python mqtt_sub.py`

Now, we need to open a separate terminal from where our mqtt_sub.py file is running.

And then we will run the python publisher application.

`python mqtt_pub.py`

## WebSockets

In your browser open the following url http://www.hivemq.com/demos/websocket-client/

Use `broker.hivemq.com` for our host for Websockets

## Device Time

Click the following link http://blog.sengotta.net/wp-content/uploads/2015/11/CH341SER_MAC-1.4.zip

Connect your device to your USB port.

Use the following command to connect our terminal to the device

`$ screen /dev/tty.wchusbserial1410 115200'

`>>> import webrepl_setup`

`>>> import network`
`>>> routercon = network.WLAN(network.STA_IF)`

`>>>routercon.active()`

`>>> routercon.active(True)`
`>>> routercon.connect('<your_Wifi_SSID>', '<your_WiFi_passw>')`

`>>> routercon.isconnected()`

Disconnect and reconnect the device

Now we will try pushing some code files to device

Let's test to make sure everything is working correctly

`$ ampy -d 0.5 -p /dev/cu.wchusbserial1410 -b 115200 ls`

Now let's download the code repo we will be working from.

`$ cd $HOME`
`$ git clone https://github.com/JockDaRock/esp-device-test`

`cd esp-device-test`

Now let's push some of the basic code and do a few changes as well.

`$ ampy -d 0.5 -p /dev/cu.wchusbserial1410 -b 115200 put boot.py`

`$ ampy -d 0.5 -p /dev/cu.wchusbserial1410 -b 115200 put main.py`




