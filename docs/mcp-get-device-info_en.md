# How MCP Methods Get Device Information

This tutorial will guide you on how to use MCP methods to get device information.

Step 1: Customize your `agent-base-prompt.txt` file

Copy the content of the `agent-base-prompt.txt` file in the xiaozhi-server directory to your `data` directory and rename it to `.agent-base-prompt.txt`.

Step 2: Modify the `data/.agent-base-prompt.txt` file, find the `<context>` tag, and add the following code content in the tag content:
```
- **Device ID：** {{device_id}}
```

After adding, the `<context>` tag content in your `data/.agent-base-prompt.txt` file should look like this:
```
<context>
[Important! The following information is provided in real-time, no need to call tools to query, please use directly:]
- **Device ID：** {{device_id}}
- **Current time：** {{current_time}}
- **Today's date：** {{today_date}} ({{today_weekday}})
- **Today's lunar date：** {{lunar_date}}
- **User's city：** {{local_address}}
- **Local weather for next 7 days：** {{weather_info}}
</context>
```

Step 3: Modify the `data/.config.yaml` file, find the `agent-base-prompt` configuration. Content before modification:
```
prompt_template: agent-base-prompt.txt
```

Change to:
```
prompt_template: data/.agent-base-prompt.txt
```

Step 4: Restart your xiaozhi-server service.

Step 5: In your mcp method, add a parameter named `device_id`, type `string`, description `Device ID`.

Step 6: Wake up Xiaozhi again and have it call the mcp method to check if your mcp method can get the `Device ID`.

