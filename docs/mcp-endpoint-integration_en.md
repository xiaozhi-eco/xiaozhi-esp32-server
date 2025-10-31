# MCP Endpoint Usage Guide

This tutorial uses Brother Xia's open-source mcp calculator function as an example to introduce how to integrate your custom mcp service into your endpoint.

The premise of this tutorial is that your `xiaozhi-server` has already enabled the MCP endpoint function. If you haven't enabled it yet, you can first follow [this tutorial](./mcp-endpoint-enable.md) to enable it.

# How to Integrate a Simple MCP Function Like Calculator Function into an Agent

### If You Are Using Full Module Deployment

If you are using full module deployment, you can enter the Control Console, Agent Management, click `Configure Role`, on the right side of `Intent Recognition`, there is an `Edit Functions` button.

Click this button. In the pop-up page, at the bottom, there will be `MCP Endpoint`. Normally, it will display the agent's `MCP Endpoint Address`. Next, let's extend a calculator function based on MCP technology for this agent.

This `MCP Endpoint Address` is very important, you will use it later.

### If You Are Using Single Module Deployment

If you are using single module deployment and have already configured the MCP endpoint address in the configuration file, then normally, when starting the single module deployment, it will output logs like this:
```
250705[__main__]-INFO-Initializing MCP Endpoint: wss://2662r3426b.vicp.fun/mcp_e 
250705[__main__]-INFO-Sending MCP Endpoint initialization message
250705[__main__]-INFO-MCP Endpoint connection successful
250705[__main__]-INFO-MCP Endpoint initialization successful
250705[__main__]-INFO-Unified tool handler initialization complete
250705[__main__]-INFO-MCP Endpoint server information: name=Calculator, version=1.9.4
250705[__main__]-INFO-Number of MCP Endpoint supported tools: 1
250705[__main__]-INFO-All MCP Endpoint tools retrieved, client ready
250705[__main__]-INFO-Tool cache refreshed
250705[__main__]-INFO-Currently supported function list: [ 'get_time', 'get_lunar', 'play_music', 'get_weather', 'handle_exit_intent', 'calculator']
```

As above, the `ws://192.168.1.25:8004/mcp_endpoint/mcp/?token=abc` in `MCP Endpoint is` is your `MCP Endpoint Address`.

This `MCP Endpoint Address` is very important, you will use it later.

## Step 1: Download Brother Xia's MCP Calculator Project Code

Open Brother Xia's [Calculator Project](https://github.com/78/mcp-calculator) in your browser,

After opening, find a green button on the page that says `Code`, click it, then you will see a button that says `Download ZIP`.

Click it to download the source code zip file of this project. After downloading to your computer, extract it. At this time, its name may be `mcp-calculator-main`.
You need to rename it to `mcp-calculator`. Next, we enter the project directory with the command line and install dependencies.


```bash
# Enter project directory
cd mcp-calculator

conda remove -n mcp-calculator --all -y
conda create -n mcp-calculator python=3.10 -y
conda activate mcp-calculator

pip install -r requirements.txt
```

## Step 2: Start

Before starting, copy the MCP endpoint address from your Control Console agent.

For example, my agent's MCP address is:
```
ws://192.168.1.25:8004/mcp_endpoint/mcp/?token=abc
```

Start entering commands:

```bash
export MCP_ENDPOINT=ws://192.168.1.25:8004/mcp_endpoint/mcp/?token=abc
```

After entering, start the program:

```bash
python mcp_pipe.py calculator.py
```

### If You Are Using Control Console Deployment

If you are using Control Console deployment, after starting, go to the Control Console again, click Refresh MCP connection status, and you will see your extended function list.

### If You Are Using Single Module Deployment

If you are using single module deployment, when the device connects, it will output logs like this, indicating success.

```
250705 -INFO-Initializing MCP Endpoint: wss://2662r3426b.vicp.fun/mcp_e 
250705 -INFO-Sending MCP Endpoint initialization message
250705 -INFO-MCP Endpoint connection successful
250705 -INFO-MCP Endpoint initialization successful
250705 -INFO-Unified tool handler initialization complete
250705 -INFO-MCP Endpoint server information: name=Calculator, version=1.9.4
250705 -INFO-Number of MCP Endpoint supported tools: 1
250705 -INFO-All MCP Endpoint tools retrieved, client ready
250705 -INFO-Tool cache refreshed
250705 -INFO-Currently supported function list: [ 'get_time', 'get_lunar', 'play_music', 'get_weather', 'handle_exit_intent', 'calculator']
```

If it contains `'calculator'`, it means the device can call the calculator tool based on intent recognition.

