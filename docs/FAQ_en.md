# FAQ ❓

### 1. Why does Xiaozhi recognize what I say as Korean, Japanese, or English? 🇰🇷

Suggestion: Check if `models/SenseVoiceSmall` already has a `model.pt` file. If not, you need to download it. See here [Download Speech Recognition Model File](Deployment.md#model-files).

### 2. Why does "TTS task error file not found" occur? 📁

Suggestion: Check if you have correctly installed `libopus` and `ffmpeg` libraries using `conda`.

If not installed, install them:

```
conda install conda-forge::libopus
conda install conda-forge::ffmpeg
```

### 3. TTS frequently fails or times out ⏰

Suggestion: If `EdgeTTS` frequently fails, first check if you are using a proxy (VPN). If using one, try closing the proxy and retry. If you're using Volcano Engine's Doubao TTS and it frequently fails, we recommend using the paid version, as the test version only supports 2 concurrent connections.

### 4. Can connect to self-built server via WiFi but not in 4G mode 🔐

Reason: Brother Xia's firmware requires secure connections in 4G mode.

Solutions: There are currently two methods. Choose one:

1. Modify the code. Refer to this video for the solution: https://www.bilibili.com/video/BV18MfTYoE85
2. Use nginx to configure SSL certificates. Refer to the tutorial: https://icnt94i5ctj4.feishu.cn/docx/GnYOdMNJOoRCljx1ctecsj9cnRe

### 5. How to improve Xiaozhi's dialogue response speed? ⚡

This project's default configuration is a low-cost solution. It's recommended that beginners first use the default free models to solve the "can run" problem, then optimize for "run fast". To improve response speed, you can try switching components. Starting from version `0.5.2`, the project supports streaming configuration. Compared to earlier versions, response speed has improved by approximately `2.5 seconds`, significantly improving user experience.

| Module Name | Entry Level Free Settings | Streaming Configuration |
|:---:|:---:|:---:|
| ASR(Speech Recognition) | FunASR(Local) | 👍FunASR(Local GPU mode) |
| LLM(Large Model) | ChatGLMLLM(Zhipu glm-4-flash) | 👍AliLLM(qwen3-235b-a22b-instruct-2507) or 👍DoubaoLLM(doubao-1-5-pro-32k-250115) |
| VLLM(Vision Large Model) | ChatGLMVLLM(Zhipu glm-4v-flash) | 👍QwenVLVLLM(Qwen qwen2.5-vl-3b-instructh) |
| TTS(Speech Synthesis) | ✅LinkeraiTTS(Lingxi streaming) | 👍HuoshanDoubleStreamTTS(Volcano dual-stream speech synthesis) or 👍AliyunStreamTTS(Alibaba Cloud streaming speech synthesis) |
| Intent(Intent Recognition) | function_call(Function calling) | function_call(Function calling) |
| Memory(Memory function) | mem_local_short(Local short-term memory) | mem_local_short(Local short-term memory) |

If you're concerned about the latency of each component, please refer to [Xiaozhi Component Performance Test Report](https://github.com/xinnan-tech/xiaozhi-performance-research). You can follow the test method in the report to actually test in your environment.

### 6. When I speak slowly, Xiaozhi keeps interrupting during pauses 🗣️

Suggestion: In the configuration file, find the following section and increase the value of `min_silence_duration_ms` (for example, change it to `1000`):

```yaml
VAD:
  SileroVAD:
    threshold: 0.5
    model_dir: models/snakers4_silero-vad
    min_silence_duration_ms: 700  # If speaking pauses are long, you can increase this value
```

### 7. Deployment-Related Tutorials
1. [How to perform simplified deployment](./Deployment.md)<br/>
2. [How to perform full module deployment](./Deployment_all.md)<br/>
3. [How to deploy MQTT gateway to enable MQTT+UDP protocol](./mqtt-gateway-integration.md)<br/>
4. [How to automatically pull the latest project code, compile and start](./dev-ops-integration.md)<br/>
5. [How to integrate with Nginx](https://github.com/xinnan-tech/xiaozhi-esp32-server/issues/791)<br/>

### 9. Firmware Compilation-Related Tutorials
1. [How to compile Xiaozhi firmware](./firmware-build.md)<br/>
2. [How to modify OTA address based on Brother Xia's compiled firmware](./firmware-setting.md)<br/>

### 10. Extension-Related Tutorials
1. [How to enable mobile phone registration for Smart Console](./ali-sms-integration.md)<br/>
2. [How to integrate HomeAssistant for smart home control](./homeassistant-integration.md)<br/>
3. [How to enable vision models for photo recognition](./mcp-vision-integration.md)<br/>
4. [How to deploy MCP endpoint](./mcp-endpoint-enable.md)<br/>
5. [How to access MCP endpoint](./mcp-endpoint-integration.md)<br/>
6. [How MCP methods get device information](./mcp-get-device-info.md)<br/>
7. [How to enable voiceprint recognition](./voiceprint-integration.md)<br/>
8. [News plugin source configuration guide](./newsnow_plugin_config.md)<br/>

### 11. Voice Cloning and Local Speech Deployment-Related Tutorials
1. [How to clone voice in Smart Console](./huoshan-streamTTS-voice-cloning.md)<br/>
2. [How to deploy and integrate index-tts local speech](./index-stream-integration.md)<br/>
3. [How to deploy and integrate fish-speech local speech](./fish-speech-integration.md)<br/>
4. [How to deploy and integrate PaddleSpeech local speech](./paddlespeech-deploy.md)<br/>

### 12. Performance Testing Tutorials
1. [Component speed testing guide](./performance_tester.md)<br/>
2. [Regular public test results](https://github.com/xinnan-tech/xiaozhi-performance-research)<br/>

### 13. For more questions, please contact us for feedback 💬

You can submit your questions at [issues](https://github.com/xinnan-tech/xiaozhi-esp32-server/issues).

