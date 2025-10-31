# Xiaozhi ESP32 Open-Source Server and HomeAssistant Integration Guide

[TOC]

-----

## Introduction

This document will guide you on how to integrate ESP32 devices with HomeAssistant.

## Prerequisites

- `HomeAssistant` has been installed and configured
- The model I chose is: Free ChatGLM, which supports function call function calling

## Before Starting (Required)

### 1. Get HA Network Address Information

Please access your Home Assistant network address. For example, if my HA address is 192.168.4.7 and the port is the default 8123, open in browser:

```
http://192.168.4.7:8123
```

> **Manual HA IP Address Lookup Method (only when xiaozhi-esp32-server and HA are deployed on the same network device [e.g., same WiFi])**:
>
> 1. Enter Home Assistant (frontend).
>
> 2. Click **Settings** in the lower left corner → **System** → **Network**.
>
> 3. Scroll to the bottom `Home Assistant Website` area, in `Local Network`, click the `eye` button to see the IP address currently in use (e.g., `192.168.1.10`) and network interface. Click `Copy Link` to copy directly.
>
>    ![image-20250504051716417](images/image-ha-integration-01.png)

Or, if you have set up a directly accessible Home Assistant OAuth address, you can also access directly in browser:

```
http://homeassistant.local:8123
```

### 2. Log in to `Home Assistant` to Get Development Key

Log in to `HomeAssistant`, click `Avatar in lower left corner -> Personal`, switch to `Security` tab, scroll to the bottom `Long-lived Access Tokens`, generate an api_key, and copy and save it. Subsequent methods all require this api key and it only appears once (tip: You can save the generated QR code image and scan it later to extract the api key again).

## Method 1: Xiaozhi Community-Built HA Calling Function

### Function Description

- If you need to add new devices later, this method requires manually restarting the `xiaozhi-esp32-server service` to update device information ** (important)**.

- You need to ensure you have integrated `Xiaomi Home` in HomeAssistant and imported Xiaomi devices into `HomeAssistant`.

- You need to ensure the `xiaozhi-esp32-server Control Console` can work normally.

- My `xiaozhi-esp32-server Control Console` and `HomeAssistant` are deployed on the same machine on another port, version is `0.3.10`.

  ```
  http://192.168.4.7:8002
  ```

### Configuration Steps

#### 1. Log in to `HomeAssistant` to Organize Device List You Need to Control

Log in to `HomeAssistant`, click `Settings in lower left corner`, then go to `Devices & Services`, then click `Entities` at the top.

Then search for the switch you want to control in entities. After results appear, click one in the list. A switch interface will appear.

In the switch interface, try clicking the switch to see if it opens/closes with our click. If it works, it means it's normally connected.

Then find the settings button on the switch panel, click it to view the `Entity ID` of this switch.

Open a notepad and organize a piece of data in this format:

Location + English comma + Device Name + English comma + `Entity ID` + English semicolon

For example, I'm at the company, I have a toy light, its identifier is switch.cuco_cn_460494544_cp1_on_p_2_1, so I write this data:

```
Company,Toy Light,switch.cuco_cn_460494544_cp1_on_p_2_1;
```

Of course, in the end I might operate two lights, my final result is:

```
Company,Toy Light,switch.cuco_cn_460494544_cp1_on_p_2_1;
Company,Desk Light,switch.iot_cn_831898993_socn1_on_p_2_1;
```

This string of characters, we call it "Device List String" and it needs to be saved, it will be useful later.

#### 2. Log in to `Control Console`

![image-20250504051716417](images/image-ha-integration-06.png)

Use the administrator account to log in to the `Control Console`. In `Agent Management`, find your agent, then click `Configure Role`.

Set Intent Recognition to `Function Call` or `LLM Intent Recognition`. At this time, you will see `Edit Functions` on the right. Click the `Edit Functions` button, a `Function Management` dialog will pop up.

In the `Function Management` dialog, you need to check `HomeAssistant Device Status Query` and `HomeAssistant Device Status Modification`.

After checking, click `HomeAssistant Device Status Query` in `Selected Functions`, then configure your `HomeAssistant` address, key, and device list string in `Parameter Configuration`.

After editing, click `Save Configuration`. At this time, the `Function Management` dialog will hide. Then click Save Agent Configuration.

After saving successfully, you can wake up the device to operate.

#### 3. Wake Up Device to Control

Try telling the ESP32, "Turn on XXX light"

## Method 2: Xiaozhi Using Home Assistant's Voice Assistant as an LLM Tool

### Function Description

- This method has a relatively serious drawback—**This method cannot use the function_call plugin capabilities of the Xiaozhi open-source ecosystem**, because using Home Assistant as the LLM tool for Xiaozhi will transfer intent recognition capability to Home Assistant. However, **this method can experience native Home Assistant operation functions, and Xiaozhi's chat capability remains unchanged**. If you really mind this, you can use [Method 3](##Method-3-Using-Home-Assistants-MCP-Service-(Recommended)), which is also supported by Home Assistant and can best experience Home Assistant functionality.

### Configuration Steps:

#### 1. Configure Home Assistant's Large Model Voice Assistant

**You need to have previously configured Home Assistant's voice assistant or large model tool.**

#### 2. Get Home Assistant's Language Assistant Agent ID

1. Enter the Home Assistant page. Click `Developer Assistant` on the left.
2. In the opened `Developer Assistant`, click the `Actions` tab (as shown in operation 1), in the action option bar of the page, find or enter `conversation.process` and select `Conversation: Process` (as shown in operation 2).

![image-20250504043539343](images/image-ha-integration-02.png)

3. Check the `Agent` option in the page, and in the highlighted `Conversation Agent`, select the voice assistant name you configured in step 1, as shown in the figure. I configured `ZhipuAi` and selected it.

![image-20250504043854760](images/image-ha-integration-03.png)

4. After selecting, click `Switch to YAML Mode` at the bottom left of the form.

![image-20250504043951126](images/image-ha-integration-04.png)

5. Copy the agent-id value, for example, in the figure mine is `01JP2DYMBDF7F4ZA2DMCF2AGX2`(for reference only).

![image-20250504044046466](images/image-ha-integration-05.png)

6. Switch to `xiaozhi-esp32-server` `config.yaml` file, find Home Assistant in the LLM configuration, set your Home Assistant network address, API key, and the agent_id you just queried.
7. Modify `selected_module` attribute's `LLM` to `HomeAssistant`, `Intent` to `nointent` in the `config.yaml` file.
8. Restart the `xiaozhi-esp32-server service` to use it normally.

## Method 3: Using Home Assistant's MCP Service (Recommended)

### Function Description

- You need to have previously integrated and installed HA integration —— [Model Context Protocol Server](https://www.home-assistant.io/integrations/mcp_server/) in Home Assistant.

- This method and Method 2 are both officially provided solutions by HA. The difference from Method 2 is that you can normally use the open-source plugins of `xiaozhi-esp32-server`, while allowing you to freely use any LLM large model that supports function_call function.

### Configuration Steps

#### 1. Install Home Assistant's MCP Service Integration

Official integration website —— [Model Context Protocol Server](https://www.home-assistant.io/integrations/mcp_server/)..

Or follow the manual steps below.

> - Go to the Home Assistant page **[Settings > Devices & Services](https://my.home-assistant.io/redirect/integrations)**.
>
> - In the lower right corner, select **[Add Integration](https://my.home-assistant.io/redirect/config_flow_start?domain=mcp_server)** button.
>
> - Select **Model Context Protocol Server** from the list.
>
> - Follow the on-screen instructions to complete setup.

#### 2. Configure Xiaozhi Open Source Server MCP Configuration Information


Enter the `data` directory and find the `.mcp_server_settings.json` file.

If you don't have a `.mcp_server_settings.json` file in your `data` directory,
- Copy the `mcp_server_settings.json` file from the `xiaozhi-server` folder root directory to the `data` directory, and rename it to `.mcp_server_settings.json`
- Or [download this file](https://github.com/xinnan-tech/xiaozhi-esp32-server/blob/main/main/xiaozhi-server/mcp_server_settings.json), download to the `data` directory, and rename it to `.mcp_server_settings.json`


Modify this part of the content in `"mcpServers"`:

```json
"Home Assistant": {
      "command": "mcp-proxy",
      "args": [
        "http://YOUR_HA_HOST/mcp_server/sse"
      ],
      "env": {
        "API_ACCESS_TOKEN": "YOUR_API_ACCESS_TOKEN"
      }
},
```

Note:

1. **Replace Configuration:**
   - Replace `YOUR_HA_HOST` in `args` with your HA service address. If your service address already includes https/http (e.g., `http://192.168.1.101:8123`), just fill in `192.168.1.101:8123`.
   - Replace `YOUR_API_ACCESS_TOKEN` in `env` with your development key api key obtained earlier.

2. **If you add configuration after the last item in `"mcpServers"` and there is no new `mcpServers` configuration after that, you need to remove the trailing comma `,`**, otherwise it may fail to parse.

**Final effect reference (reference as follows)**:

```json
 "mcpServers": {
    "Home Assistant": {
      "command": "mcp-proxy",
      "args": [
        "http://192.168.1.101:8123/mcp_server/sse"
      ],
      "env": {
        "API_ACCESS_TOKEN": "abcd.efghi.jkl"
      }
    }
  }
```

#### 3. Configure Xiaozhi Open Source Server System Configuration

1. **Select any LLM large model that supports function_call as Xiaozhi's LLM chat assistant (but don't select Home Assistant as LLM tool)**. The model I chose is: Free ChatGLM, which supports function call, but sometimes it's not very stable. If you want to pursue stability, it is recommended to set LLM to: DoubaoLLM, the specific model_name used is: doubao-1-5-pro-32k-250115.

2. Switch to `xiaozhi-esp32-server` `config.yaml` file, set your LLM large model configuration, and adjust `Intent` in `selected_module` configuration to `function_call`.

3. Restart the `xiaozhi-esp32-server service` to use it normally.

