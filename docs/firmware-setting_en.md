# Configure Custom Server Based on Brother Xia's Compiled Firmware

## Step 1: Confirm Version
Burn Brother Xia's pre-compiled [Firmware version 1.6.1 and above](https://github.com/78/xiaozhi-esp32/releases)

## Step 2: Prepare Your OTA Address
If you follow the tutorial to use full-module deployment, you should have an OTA address.

At this time, please open your OTA address in your browser, for example, my OTA address:
```
https://2662r3426b.vicp.fun/xiaozhi/ota/
```

If it displays "OTA interface is running normally, websocket cluster count: X". Then proceed.

If it displays "OTA interface is not running normally", it's probably because you haven't configured the `Websocket` address in the `Control Console`. Then:

- 1. Use super administrator to log in to Control Console

- 2. Click `Parameter Management` in the top menu

- 3. Find `server.websocket` item in the list, enter your `Websocket` address. For example, mine is:

```
wss://2662r3426b.vicp.fun/xiaozhi/v1/
```

After configuration, refresh your OTA interface address in your browser to see if it's normal. If it's still not normal, confirm again that Websocket is started normally and whether the Websocket address is configured.

## Step 3: Enter Network Configuration Mode
Enter the machine's network configuration mode, click "Advanced Options" at the top of the page, enter your server's `ota` address, click save. Restart the device.

![Please refer to OTA address settings](../docs/images/firmware-setting-ota.png)

## Step 4: Wake Up Xiaozhi and View Log Output

Wake up Xiaozhi, see if the log outputs normally.


## FAQ
Here are some common questions for reference:

[1. Why does Xiaozhi recognize what I say as Korean, Japanese, or English?](./FAQ.md)

[2. Why does "TTS task error file not found" occur?](./FAQ.md)

[3. TTS frequently fails or times out](./FAQ.md)

[4. Can connect to self-built server via WiFi but not in 4G mode](./FAQ.md)

[5. How to improve Xiaozhi's dialogue response speed?](./FAQ.md)

[6. When I speak slowly, Xiaozhi keeps interrupting during pauses](./FAQ.md)

[7. I want to control lights, air conditioners, remote shutdown through Xiaozhi](./FAQ.md)

