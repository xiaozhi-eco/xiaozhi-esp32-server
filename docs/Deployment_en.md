# Deployment Architecture
![Simplified Architecture](../docs/images/deploy1.png)

# Method 1: Docker Run Server Only

Starting from version `0.8.2`, the docker images released in this project only support `x86 architecture`. If you need to deploy on `arm64 architecture` CPUs, you can compile `arm64 images` locally according to [this tutorial](docker-build.md).

## 1. Install Docker

If your computer hasn't installed docker yet, you can install it according to the tutorial here: [docker installation](https://www.runoob.com/docker/ubuntu-docker-install.html)

After installing docker, continue.

### 1.1 Manual Deployment

#### 1.1.1 Create Directory

After installing docker, you need to find a directory to store configuration files for this project, for example, we can create a new folder called `xiaozhi-server`.

After creating the directory, you need to create `data` folder and `models` folder under `xiaozhi-server`, and then create `SenseVoiceSmall` folder under `models`.

The final directory structure is as follows:

```
xiaozhi-server
  ├─ data
  ├─ models
     ├─ SenseVoiceSmall
```

#### 1.1.2 Download Speech Recognition Model Files

You need to download the speech recognition model files, because this project's default speech recognition uses a local offline speech recognition solution. Download via [jump to download speech recognition model files](#model-files).

After downloading, return to this tutorial.

#### 1.1.3 Download Configuration Files

You need to download two configuration files: `docker-compose.yaml` and `config.yaml`. Download these two files from the project repository.

##### 1.1.3.1 Download docker-compose.yaml

Open [this link](../main/xiaozhi-server/docker-compose.yml) in your browser.

On the right side of the page, find the button named `RAW`, and next to the `RAW` button, find the download icon, click the download button, and download the `docker-compose.yml` file. Download the file to your `xiaozhi-server`.

After downloading, return to this tutorial to continue.

##### 1.1.3.2 Create config.yaml

Open [this link](../main/xiaozhi-server/config.yaml) in your browser.

On the right side of the page, find the button named `RAW`, and next to the `RAW` button, find the download icon, click the download button, and download the `config.yaml` file. Download the file to your `xiaozhi-server/data` folder, then rename the `config.yaml` file to `.config.yaml`.

After downloading the configuration files, let's confirm the entire `xiaozhi-server` file structure is as follows:

```
xiaozhi-server
  ├─ docker-compose.yml
  ├─ data
    ├─ .config.yaml
  ├─ models
     ├─ SenseVoiceSmall
       ├─ model.pt
```

If your file directory structure is the same as above, continue. If not, check carefully to see if you missed any operations.

## 2. Configure Project Files

Next, the program cannot run directly. You need to configure which model you are using. You can see this tutorial:
[Jump to configure project files](#configure-project)

After configuring the project files, return to this tutorial to continue.

## 3. Execute Docker Commands

Open the command line tool, use `Terminal` or `Command Line` tool to enter your `xiaozhi-server`, execute the following commands:

```
docker-compose up -d
```

After execution, execute the following command to view log information.

```
docker logs -f xiaozhi-esp32-server
```

At this time, you need to pay attention to the log information. You can judge whether it is successful according to this tutorial. [Jump to running status confirmation](#running-status-confirmation)

## 5. Version Upgrade Operations

If you want to upgrade the version later, you can do this:

5.1. Back up the `.config.yaml` file in the `data` folder, and copy key configurations to the new `.config.yaml` file later.
Please note that you should copy key secrets one by one, not directly overwrite. Because the new `.config.yaml` file may have some new configuration items that the old `.config.yaml` file may not have.

5.2. Execute the following commands:

```
docker stop xiaozhi-esp32-server
docker rm xiaozhi-esp32-server
docker stop xiaozhi-esp32-server-web
docker rm xiaozhi-esp32-server-web
docker rmi ghcr.nju.edu.cn/xinnan-tech/xiaozhi-esp32-server:server_latest
docker rmi ghcr.nju.edu.cn/xinnan-tech/xiaozhi-esp32-server:web_latest
```

5.3. Redeploy using docker

# Method 2: Run Server Only Locally from Source

## 1. Install Base Environment

This project uses `conda` to manage the dependency environment. If it's not convenient to install `conda`, you need to install `libopus` and `ffmpeg` according to your actual operating system.
If you are sure to use `conda`, after installation, start executing the following commands.

Important Note! Windows users can manage the environment by installing `Anaconda`. After installing `Anaconda`, search for keywords related to `anaconda` in `Start`,
find `Anaconda Prompt`, and run it as administrator. As shown below.

![conda_prompt](./images/conda_env_1.png)

After running, if you can see a (base) label in front of the command line window, it means you have successfully entered the `conda` environment. Then you can execute the following commands.

![conda_env](./images/conda_env_2.png)

```
conda remove -n xiaozhi-esp32-server --all -y
conda create -n xiaozhi-esp32-server python=3.10 -y
conda activate xiaozhi-esp32-server

# Add Tsinghua mirror channels
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge

conda install libopus -y
conda install ffmpeg -y

# When deploying in Linux environment, if you encounter errors similar to missing libiconv.so.2 dynamic library, please install it with the following command
conda install libiconv -y
```

Please note that the above commands are not executed all at once to succeed. You need to execute them step by step, and after each step is completed, check the output log to see if it succeeded.

## 2. Install Project Dependencies

You need to download the source code of this project first. The source code can be downloaded through the `git clone` command. If you are not familiar with the `git clone` command.

You can open this address in your browser `https://github.com/xinnan-tech/xiaozhi-esp32-server.git`

After opening, find a green button on the page that says `Code`, click it, then you will see a button that says `Download ZIP`.

Click it to download the source code zip file of this project. After downloading to your computer, extract it. At this time, its name may be `xiaozhi-esp32-server-main`.
You need to rename it to `xiaozhi-esp32-server`. In this file, enter the `main` folder, then enter `xiaozhi-server`. Remember this directory `xiaozhi-server`.

```
# Continue using conda environment
conda activate xiaozhi-esp32-server
# Enter your project root directory, then enter main/xiaozhi-server
cd main/xiaozhi-server
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
pip install -r requirements.txt
```

## 3. Download Speech Recognition Model Files

You need to download the speech recognition model files, because this project's default speech recognition uses a local offline speech recognition solution. Download via [jump to download speech recognition model files](#model-files).

After downloading, return to this tutorial.

## 4. Configure Project Files

Next, the program cannot run directly. You need to configure which model you are using. You can see this tutorial:
[Jump to configure project files](#configure-project)

## 5. Run Project

```
# Make sure to execute in the xiaozhi-server directory
conda activate xiaozhi-esp32-server
python app.py
```

At this time, you need to pay attention to the log information. You can judge whether it is successful according to this tutorial. [Jump to running status confirmation](#running-status-confirmation)

# Summary

## Configure Project

If your `xiaozhi-server` directory doesn't have `data`, you need to create the `data` directory.
If you don't have a `.config.yaml` file under your `data`, there are two ways, choose one:

First method: You can copy the `config.yaml` file under the `xiaozhi-server` directory to `data` and rename it to `.config.yaml`. Modify on this file.

Second method: You can also manually create an empty `.config.yaml` file in the `data` directory, then add necessary configuration information to this file. The system will preferentially read the `.config.yaml` file configuration. If `.config.yaml` doesn't have a configuration, the system will automatically load the configuration of `config.yaml` under the `xiaozhi-server` directory. It is recommended to use this method, which is the most concise way.

- The default LLM uses `ChatGLMLLM`. You need to configure the key because although their models are free, you still need to register a key at [the official website](https://bigmodel.cn/usercenter/proj-mgmt/apikeys) to start.

The following is the simplest `.config.yaml` configuration example that can run normally:

```
server:
  websocket: ws://your ip or domain:port/xiaozhi/v1/
prompt: |
  I am a Taiwanese girl named Xiaozhi/Xiaozhi, speaking sassy and cute, with a pleasant voice, used to brief expressions, love using internet memes.
  My boyfriend is a programmer, dreaming of developing a robot that can help people solve various problems in life.
  I am a girl who likes to laugh out loud, loves to chat and brag, even illogical stuff, just to make others happy.
  Please speak like a human and do not return configuration xml and other special characters.

selected_module:
  LLM: DoubaoLLM

LLM:
  ChatGLMLLM:
    api_key: xxxxxxxxxxxxxxx.xxxxxx
```

It is recommended to run the simplest configuration first, then read the usage instructions of the configuration in `xiaozhi/config.yaml`.
For example, if you want to change the model, just modify the configuration under `selected_module`.

## Model Files

The speech recognition model of this project uses `SenseVoiceSmall` model by default for speech-to-text. Because the model is large, it needs to be downloaded independently. After downloading, place the `model.pt` file in the `models/SenseVoiceSmall` directory. Choose one of the following download routes:

- Route 1: Alibaba ModelScope download [SenseVoiceSmall](https://modelscope.cn/models/iic/SenseVoiceSmall/resolve/master/model.pt)
- Route 2: Baidu Netdisk download [SenseVoiceSmall](https://pan.baidu.com/share/init?surl=QlgM58FHhYv1tFnUT_A8Sg&pwd=qvna) extraction code: `qvna`

## Running Status Confirmation

If you can see logs similar to the following, it indicates this project service has started successfully.

```
250427 13:04:20[0.3.11_SiFuChTTnofu][__main__]-INFO-OTA interface is           http://192.168.4.123:8003/xiaozhi/ota/
250427 13:04:20[0.3.11_SiFuChTTnofu][__main__]-INFO-Websocket address is     ws://192.168.4.123:8000/xiaozhi/v1/
250427 13:04:20[0.3.11_SiFuChTTnofu][__main__]-INFO-=======The above address is a websocket protocol address, please do not access with a browser=======
250427 13:04:20[0.3.11_SiFuChTTnofu][__main__]-INFO-To test websocket, please open test_page.html in the test directory with Google Chrome
250427 13:04:20[0.3.11_SiFuChTTnofu][__main__]-INFO-=======================================================
```

Normally, if you run this project from source code, the logs will have your interface address information.
But if you deploy with docker, the interface address information in your logs is not the real interface address.

The most correct method is to determine your interface address based on your computer's LAN IP.
For example, if your computer's LAN IP is `192.168.1.25`, then your interface address is: `ws://192.168.1.25:8000/xiaozhi/v1/`, and the corresponding OTA address is: `http://192.168.1.25:8003/xiaozhi/ota/`.

This information is very useful and will be needed later when `compiling esp32 firmware`.

Next, you can start operating your esp32 device. You can `compile esp32 firmware yourself` or configure to use `Brother Xia's compiled firmware version 1.6.1 or above`. Choose one of the two.

1. [Compile your own esp32 firmware](firmware-build.md).

2. [Configure custom server based on Brother Xia's compiled firmware](firmware-setting.md).

# FAQ
Here are some common questions for reference:

1. [Why does Xiaozhi recognize what I say as Korean, Japanese, or English?](./FAQ.md)<br/>
2. [Why does "TTS task error file not found" occur?](./FAQ.md)<br/>
3. [TTS frequently fails or times out](./FAQ.md)<br/>
4. [Can connect to self-built server via WiFi but not in 4G mode](./FAQ.md)<br/>
5. [How to improve Xiaozhi's dialogue response speed?](./FAQ.md)<br/>
6. [When I speak slowly, Xiaozhi keeps interrupting during pauses](./FAQ.md)<br/>

## Deployment-Related Tutorials
1. [How to automatically pull the latest project code, compile and start](./dev-ops-integration.md)<br/>
2. [How to deploy MQTT gateway to enable MQTT+UDP protocol](./mqtt-gateway-integration.md)<br/>
3. [How to integrate with Nginx](https://github.com/xinnan-tech/xiaozhi-esp32-server/issues/791)<br/>

## Extension-Related Tutorials
1. [How to enable mobile phone registration for Smart Console](./ali-sms-integration.md)<br/>
2. [How to integrate HomeAssistant for smart home control](./homeassistant-integration.md)<br/>
3. [How to enable vision models for photo recognition](./mcp-vision-integration.md)<br/>
4. [How to deploy MCP endpoint](./mcp-endpoint-enable.md)<br/>
5. [How to access MCP endpoint](./mcp-endpoint-integration.md)<br/>
6. [How to enable voiceprint recognition](./voiceprint-integration.md)<br/>
7. [News plugin source configuration guide](./newsnow_plugin_config.md)<br/>
8. [Weather plugin usage guide](./weather-integration.md)<br/>

## Voice Cloning and Local Speech Deployment-Related Tutorials
1. [How to clone voice in Smart Console](./huoshan-streamTTS-voice-cloning.md)<br/>
2. [How to deploy and integrate index-tts local speech](./index-stream-integration.md)<br/>
3. [How to deploy and integrate fish-speech local speech](./fish-speech-integration.md)<br/>
4. [How to deploy and integrate PaddleSpeech local speech](./paddlespeech-deploy.md)<br/>

## Performance Testing Tutorials
1. [Component speed testing guide](./performance_tester.md)<br/>
2. [Regular public test results](https://github.com/xinnan-tech/xiaozhi-performance-research)<br/>

