# ESP32 Firmware Compilation

## Step 1: Prepare Your OTA Address

If you are using version 0.3.12 of this project, whether it's a simple Server deployment or full-module deployment, you will have an OTA address.

Since simple Server deployment and full-module deployment have different OTA address setup methods, please select the specific method below:

### If You're Using Simple Server Deployment
At this time, please open your OTA address in your browser, for example, my OTA address:
```
http://192.168.1.25:8003/xiaozhi/ota/
```

If it displays "OTA interface is running normally, websocket address sent to devices is: ws://xxx:8000/xiaozhi/v1/"

You can use the built-in `test_page.html` to test if it can connect to the websocket address output by the OTA page.

If it cannot be accessed, you need to modify the `server.websocket` address in the configuration file `.config.yaml`, restart and test again until `test_page.html` can access normally.

After success, please proceed to Step 2

### If You're Using Full-Module Deployment
At this time, please open your OTA address in your browser, for example, my OTA address:
```
http://192.168.1.25:8002/xiaozhi/ota/
```

If it displays "OTA interface is running normally, websocket cluster count: X". Then proceed to Step 2.

If it displays "OTA interface is not running normally", it's probably because you haven't configured the `Websocket` address in the `Control Console`. Then:

- 1. Use super administrator to log in to Control Console

- 2. Click `Parameter Management` in the top menu

- 3. Find `server.websocket` item in the list, enter your `Websocket` address. For example, mine is:

```
ws://192.168.1.25:8000/xiaozhi/v1/
```

After configuration, refresh your OTA interface address in your browser to see if it's normal. If it's still not normal, confirm again that Websocket is started normally and whether the Websocket address is configured.

## Step 2: Configure Environment

First, follow this tutorial to configure the project environment ["Windows Setting Up ESP IDF 5.3.2 Development Environment and Compiling Xiaozhi"](https://icnynnzcwou8.feishu.cn/wiki/JEYDwTTALi5s2zkGlFGcDiRknXf)

## Step 3: Open Configuration File

After configuring the compilation environment, download Brother Xia's xiaozhi-esp32 project source code,

Download Brother Xia's [xiaozhi-esp32 project source code](https://github.com/78/xiaozhi-esp32) from here.

After downloading, open `xiaozhi-esp32/main/Kconfig.projbuild` file.

## Step 4: Modify OTA Address

Find the `default` content of `OTA_URL`, change `https://api.tenclass.net/xiaozhi/ota/` to your own address. For example, my interface address is `http://192.168.1.25:8002/xiaozhi/ota/`, change the content to this.

Before modification:
```
config OTA_URL
    string "Default OTA URL"
    default "https://api.tenclass.net/xiaozhi/ota/"
    help
        The application will access this URL to check for new firmwares and server address.
```

After modification:
```
config OTA_URL
    string "Default OTA URL"
    default "http://192.168.1.25:8002/xiaozhi/ota/"
    help
        The application will access this URL to check for new firmwares and server address.
```

## Step 5: Set Compilation Parameters

Set compilation parameters

```
# Enter xiaozhi-esp32 root directory in terminal command line
cd xiaozhi-esp32
# For example, my board is esp32s3, so set compilation target as esp32s3, if your board is other model, please replace with corresponding model
idf.py set-target esp32s3
# Enter menu configuration
idf.py menuconfig
```

After entering menu configuration, then enter `Xiaozhi Assistant`, set `BOARD_TYPE` to your board's specific model
Save and exit, return to terminal command line.

## Step 6: Compile Firmware

```
idf.py build
```

## Step 7: Package bin Firmware

```
cd scripts
python release.py
```

After the packaging command above is executed, firmware file `merged-binary.bin` will be generated in the `build` directory under the project root directory.
This `merged-binary.bin` is the firmware file to be burned to the hardware.

Note: If after executing the second command, you encounter a "zip" related error, please ignore this error, as long as the firmware file `merged-binary.bin` is generated in the `build` directory, it won't have much impact on you, please continue.

## Step 8: Burn Firmware
   Connect the esp32 device to the computer, use chrome browser, open the following URL:

```
https://espressif.github.io/esp-launchpad/
```

Open this tutorial, [Flash Tool/Web Burning Firmware (Without IDF Development Environment)"](https://ccnphfhqs21z.feishu.cn/wiki/Zpz4wXBtdimBrLk25WdcXzxcnNS).
Scroll to: `Method 2: ESP-Launchpad Browser Web Burning`, start from `3. Burn Firmware/Download to Development Board`, follow the tutorial to operate.

After successful burning and successful networking, wake up Xiaozhi through wake word, pay attention to console information output by the server.

## FAQ
Here are some common questions for reference:

[1. Why does Xiaozhi recognize what I say as Korean, Japanese, or English?](./FAQ.md)

[2. Why does "TTS task error file not found" occur?](./FAQ.md)

[3. TTS frequently fails or times out](./FAQ.md)

[4. Can connect to self-built server via WiFi but not in 4G mode](./FAQ.md)

[5. How to improve Xiaozhi's dialogue response speed?](./FAQ.md)

[6. When I speak slowly, Xiaozhi keeps interrupting during pauses](./FAQ.md)

[7. I want to control lights, air conditioners, remote shutdown through Xiaozhi](./FAQ.md)

