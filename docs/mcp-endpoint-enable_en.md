# MCP Endpoint Deployment and Usage Guide

This tutorial contains 3 parts:
- 1. How to deploy the MCP endpoint service
- 2. How to configure the MCP endpoint in full module deployment
- 3. How to configure the MCP endpoint in single module deployment

# 1. How to Deploy the MCP Endpoint Service

## Step 1: Download MCP Endpoint Project Source Code

Open the [MCP endpoint project address](https://github.com/xinnan-tech/mcp-endpoint-server) in your browser.

After opening, find a green button on the page that says `Code`, click it, then you will see a button that says `Download ZIP`.

Click it to download the source code zip file of this project. After downloading to your computer, extract it. At this time, its name may be `mcp-endpoint-server-main`.
You need to rename it to `mcp-endpoint-server`.

## Step 2: Start the Program

This is a very simple project, it is recommended to use docker to run. However, if you don't want to run with docker, you can refer to [this page](https://github.com/xinnan-tech/mcp-endpoint-server/blob/main/README_dev.md) to run from source code. The following is the docker running method:

```
# Enter this project source code root directory
cd mcp-endpoint-server

# Clear cache
docker compose -f docker-compose.yml down
docker stop mcp-endpoint-server
docker rm mcp-endpoint-server
docker rmi ghcr.nju.edu.cn/xinnan-tech/mcp-endpoint-server:latest

# Start docker container
docker compose -f docker-compose.yml up -d
# View logs
docker logs -f mcp-endpoint-server
```

At this time, the logs will output logs similar to the following:
```
250705 INFO-=====The following addresses are Control Console/Single Module MCP Endpoint addresses====
250705 INFO-Control Console MCP Parameter Configuration: http://172.22.0.2:8004/mcp_endpoint/health?key=abc
250705 INFO-Single Module Deployment MCP Endpoint: ws://172.22.0.2:8004/mcp_endpoint/mcp/?token=def
250705 INFO-=====Please choose to use according to specific deployment, please do not leak to anyone======

```

Please copy the two interface addresses:

Since you are deploying with docker, you must not use the above addresses directly!

Since you are deploying with docker, you must not use the above addresses directly!

Since you are deploying with docker, you must not use the above addresses directly!

First copy the addresses and put them in a draft. You need to know what your computer's LAN IP is. For example, my computer's LAN IP is `192.168.1.25`, then
my original interface addresses
```
Control Console MCP Parameter Configuration: http://172.22.0.2:8004/mcp_endpoint/health?key=abc
Single Module Deployment MCP Endpoint: ws://172.22.0.2:8004/mcp_endpoint/mcp/?token=def
```

should be changed to
```
Control Console MCP Parameter Configuration: http://192.168.1.25:8004/mcp_endpoint/health?key=abc
Single Module Deployment MCP Endpoint: ws://192.168.1.25:8004/mcp_endpoint/mcp/?token=def
```

After making the changes, please use your browser to directly access the `Control Console MCP Parameter Configuration`. When the browser displays code similar to this, it means it's successful:
```
{"result":{"status":"success","connections":{"tool_connections":0,"robot_connections":0,"total_connections":0}},"error":null,"id":null,"jsonrpc":"2.0"}
```

Please keep the above two `interface addresses`, you will need them in the next step.

# 2. How to Configure MCP Endpoint in Full Module Deployment

If you are deploying the full module, use the administrator account to log in to the Control Console, click `Parameter Dictionary` at the top, select `Parameter Management` function.

Then search for parameter `server.mcp_endpoint`. At this time, its value should be `null`.
Click the Modify button, paste the `Control Console MCP Parameter Configuration` obtained in the previous step into the `Parameter Value`. Then save.

If it can be saved successfully, everything is going well, you can go to the agent to check the effect. If it fails, it means the Control Console cannot access the MCP endpoint. It's most likely a network firewall or incorrect LAN IP address.

# 3. How to Configure MCP Endpoint in Single Module Deployment

If you are deploying a single module, find your configuration file `data/.config.yaml`.
Search for `mcp_endpoint` in the configuration file. If not found, add the `mcp_endpoint` configuration. For example, mine is like this:
```
server:
  websocket: ws://your ip or domain:port/xiaozhi/v1/
  http_port: 8002
log:
  log_level: INFO

# There may be more configurations here...

mcp_endpoint: your endpoint websocket address
```
At this time, please paste the `Single Module Deployment MCP Endpoint` obtained in `How to Deploy the MCP Endpoint Service` into `mcp_endpoint`. Like this:

```
server:
  websocket: ws://your ip or domain:port/xiaozhi/v1/
  http_port: 8002
log:
  log_level: INFO

# There may be more configurations here...

mcp_endpoint: ws://192.168.1.25:8004/mcp_endpoint/mcp/?token=def
```

After configuration, starting the single module will output logs like this:
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

If it contains `'calculator'`, it means the device can call the calculator tool based on intent recognition.

