# Voiceprint Recognition Enable Guide

This tutorial contains 3 parts:
- 1. How to deploy the voiceprint recognition service
- 2. How to configure the voiceprint recognition interface when deploying the full module
- 3. How to configure voiceprint recognition in simplified deployment

# 1. How to Deploy the Voiceprint Recognition Service

## Step 1: Download Voiceprint Recognition Project Source Code

Open the [Voiceprint Recognition Project Address](https://github.com/xinnan-tech/voiceprint-api) in your browser.

After opening, find a green button on the page that says `Code`, click it, then you will see a button that says `Download ZIP`.

Click it to download the source code zip file of this project. After downloading to your computer, extract it. At this time, its name may be `voiceprint-api-main`.
You need to rename it to `voiceprint-api`.

## Step 2: Create Database and Tables

Voiceprint recognition needs to rely on the `mysql` database. If you have previously deployed the `Control Console`, it means you have already installed `mysql`. You can share it.

You can try using the `telnet` command on the host machine to see if you can normally access the `mysql` port `3306`.
```
telnet 127.0.0.1 3306
```

If you can access port 3306, please ignore the following content and go directly to Step 3.

If you cannot access it, you need to recall how your `mysql` was installed.

If your mysql was installed by yourself using an installation package, it means your `mysql` has network isolation. You may need to first solve the problem of accessing mysql's port `3306`.

If your `mysql` was installed through this project's `docker-compose_all.yml`. You need to find the `docker-compose_all.yml` file where you created the database at that time and modify the following content:

Before modification:
```
  xiaozhi-esp32-server-db:
    ...
    networks:
      - default
    expose:
      - "3306:3306"
```

After modification:
```
  xiaozhi-esp32-server-db:
    ...
    networks:
      - default
    ports:
      - "3306:3306"
```

Note that change `expose` under `xiaozhi-esp32-server-db` to `ports`. After changing, you need to restart. The following is the command to restart mysql:

```
# Enter the folder where your docker-compose_all.yml is located, for example, mine is xiaozhi-server
cd xiaozhi-server
docker compose -f docker-compose_all.yml down
docker compose -f docker-compose.yml up -d
```

After starting, use the `telnet` command on the host machine again to see if you can normally access mysql port `3306`.
```
telnet 127.0.0.1 3306
```

Normally, this should allow access.

## Step 3: Create Database and Tables

If your host machine can normally access the mysql database, create a database named `voiceprint_db` and a `voiceprints` table on mysql.

```
CREATE DATABASE voiceprint_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE voiceprint_db;

CREATE TABLE voiceprints (
    id INT AUTO_INCREMENT PRIMARY KEY,
    speaker_id VARCHAR(255) NOT NULL UNIQUE,
    feature_vector LONGBLOB NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_speaker_id (speaker_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

## Step 4: Configure Database Connection

Enter the `voiceprint-api` folder and create a folder named `data`.

Copy `voiceprint.yaml` in the `voiceprint-api` root directory to the `data` folder and rename it to `.voiceprint.yaml`

Next, you need to focus on configuring the database connection in `.voiceprint.yaml`.

```
mysql:
  host: "127.0.0.1"
  port: 3306
  user: "root"
  password: "your_password"
  database: "voiceprint_db"
```

Note! Since your voiceprint recognition service is deployed using docker, `host` needs to be filled in with the `LAN IP of the machine where your mysql is located`.

Note! Since your voiceprint recognition service is deployed using docker, `host` needs to be filled in with the `LAN IP of the machine where your mysql is located`.

Note! Since your voiceprint recognition service is deployed using docker, `host` needs to be filled in with the `LAN IP of the machine where your mysql is located`.

## Step 5: Start the Program

This project is a very simple project. It is recommended to use docker to run. However, if you don't want to run with docker, you can refer to [this page](https://github.com/xinnan-tech/voiceprint-api/blob/main/README.md) to run from source code. The following is the docker running method:

```
# Enter this project source code root directory
cd voiceprint-api

# Clear cache
docker compose -f docker-compose.yml down
docker stop voiceprint-api
docker rm voiceprint-api
docker rmi ghcr.nju.edu.cn/xinnan-tech/voiceprint-api:latest

# Start docker container
docker compose -f docker-compose.yml up -d
# View logs
docker logs -f voiceprint-api
```

At this time, the logs will output logs similar to the following:
```
250711 INFO-🚀 Start: Production environment service startup (Uvicorn), listening address: 0.0.0.0:8005
250711 INFO-============================================================
250711 INFO-Voiceprint interface address: http://127.0.0.1:8005/voiceprint/health?key=abcd
250711 INFO-============================================================
```

Please copy out the voiceprint interface address:

Since you are deploying with docker, you must not directly use the above address!

Since you are deploying with docker, you must not directly use the above address!

Since you are deploying with docker, you must not directly use the above address!

First copy out the address and put it in a draft. You need to know what your computer's LAN IP is. For example, my computer's LAN IP is `192.168.1.25`, then
originally my interface address
```
http://127.0.0.1:8005/voiceprint/health?key=abcd

```

should be changed to
```
http://192.168.1.25:8005/voiceprint/health?key=abcd
```

After making the changes, please use your browser to directly access the `Voiceprint Interface Address`. When the browser displays code similar to this, it means it's successful:
```
{"total_voiceprints":0,"status":"healthy"}
```

Please keep the modified `Voiceprint Interface Address` for use in the next step.

# 2. How to Configure Voiceprint Recognition in Full Module Deployment

## Step 1: Configure Interface
If you are deploying the full module, use the administrator account to log in to the Control Console, click `Parameter Dictionary` at the top, select `Parameter Management` function.

Then search for parameter `server.voice_print`. At this time, its value should be `null`.
Click the Modify button, paste the `Voiceprint Interface Address` obtained in the previous step into `Parameter Value`. Then save.

If it can be saved successfully, everything is going well and you can go to the agent to check the effect. If it fails, it means the Control Console cannot access the voiceprint recognition. It's most likely a network firewall or incorrect LAN IP.

## Step 2: Set Agent Memory Mode

Enter your agent's role configuration, set memory to `Local Short-Term Memory`, and make sure to enable `Report Text + Voice`.

## Step 3: Chat with Your Agent

Power on your device, then chat with it using normal speaking speed and tone.

## Step 4: Set Voiceprint

In the Control Console, `Agent Management` page, in the agent panel, there is a `Voiceprint Recognition` button, click it. At the bottom, there is a `New` button. You can perform voiceprint registration for what someone says.
In the pop-up dialog, it's recommended to fill in the `Description` attribute. It can be this person's profession, personality, hobbies. This helps the agent to analyze and understand the speaker.

## Step 3: Chat with Your Agent

Power on your device, ask it, "Do you know who I am?" If it can answer correctly, it means the voiceprint recognition function is working normally.

# 3. How to Configure Voiceprint Recognition in Simplified Deployment

## Step 1: Configure Interface

Open the `xiaozhi-server/data/.config.yaml` file (create if needed), then add/modify the following content:

```
# Voiceprint Recognition Configuration
voiceprint:
  # Voiceprint Interface Address
  url: Your voiceprint interface address
  # Speaker Configuration: speaker_id, Name, Description
  speakers:
    - "test1,张三,张三是一个程序员"
    - "test2,李四,李四是一个产品经理"
    - "test3,王五,王五是一个设计师"
```

Paste the `Voiceprint Interface Address` obtained in the previous step into `url`. Then save.

Add `speakers` parameters as needed. Note that the `speaker_id` parameter will be used later for voiceprint registration.

## Step 2: Register Voiceprint
If you have already started the voiceprint service, open `http://localhost:8005/voiceprint/docs` in your local browser to view the API documentation. Here we only explain how to use the API for voiceprint registration.

The voiceprint registration API address is `http://localhost:8005/voiceprint/register`, request method is POST.

The request header needs to include Bearer Token authentication. The token is the part after `?key=` in the `Voiceprint Interface Address`. For example, if my voiceprint registration address is `http://127.0.0.1:8005/voiceprint/health?key=abcd`, then my token is `abcd`.

The request body includes speaker ID (speaker_id) and WAV audio file (file). Example request:

```
curl -X POST \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -F "speaker_id=your_speaker_id_here" \
  -F "file=@/path/to/your/file" \
  http://localhost:8005/voiceprint/register
```

Here, `file` is the audio file of the speaker speaking, and `speaker_id` needs to be consistent with the `speaker_id` in Step 1 configuration. For example, if I need to register Zhang San's voiceprint, the `speaker_id` for Zhang San in `.config.yaml` is `test1`, then when I register Zhang San's voiceprint, the `speaker_id` in the request body is `test1`, and `file` is Zhang San's speech audio file.

## Step 3: Start Service

Start the Xiaozhi server and voiceprint service, then you can use it normally.

