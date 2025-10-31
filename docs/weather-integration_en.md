# Weather Plugin Usage Guide

## Overview

The weather plugin `get_weather` is one of the core functions of Xiaozhi ESP32 voice assistant, supporting voice-based weather queries for locations nationwide. The plugin is based on the Hefeng Weather API, providing real-time weather and 7-day weather forecasts.

## API Key Application Guide

### 1. Register Hefeng Weather Account

1. Visit [Hefeng Weather Console](https://console.qweather.com/)
2. Register an account and complete email verification
3. Log in to the console

### 2. Create Application and Get API Key

1. After entering the console, click ["Project Management"](https://console.qweather.com/project?lang=zh) → "Create Project"
2. Fill in project information:
   - **Project Name**: e.g., "Xiaozhi Voice Assistant"
3. Click Save
4. After project creation, click "Create Credentials" in that project
5. Fill in credential information:
   - **Credential Name**: e.g., "Xiaozhi Voice Assistant"
   - **Authentication Method**: Select "API Key"
6. Click Save
7. Copy the `API Key` from the credential, this is the first key configuration information

### 3. Get API Host

1. Click ["Settings"](https://console.qweather.com/setting?lang=zh) → "API Host" in the console
2. View the exclusive `API Host` address assigned to you, this is the second key configuration information

The above operations will give you two important pieces of configuration information: `API Key` and `API Host`

## Configuration Methods (Choose One)

### Method 1. If you have deployed the Control Console (Recommended)

1. Log in to the Control Console
2. Go to "Role Configuration" page
3. Select the agent you want to configure
4. Click the "Edit Functions" button
5. Find the "Weather Query" plugin in the parameter configuration area on the right
6. Check "Weather Query"
7. Paste the first configuration `API Key` into the `Weather Plugin API Key` field
8. Paste the second configuration `API Host` into the `Developer API Host` field
9. Save configuration, then save agent configuration

### Method 2. If you only have a single-module xiaozhi-server deployment

Configure in `data/.config.yaml`:

1. Paste the first configuration `API Key` into the `api_key` field
2. Paste the second configuration `API Host` into the `api_host` field
3. Enter your city into the `default_location` field, e.g., `Guangzhou`

```yaml
plugins:
  get_weather:
    api_key: "Your Hefeng Weather API Key"
    api_host: "Your Hefeng Weather API Host Address"
    default_location: "Your default query city"
```

