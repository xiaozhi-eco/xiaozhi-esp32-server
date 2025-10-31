# Voice Recognition, Large Language Model, Non-Streaming Speech Synthesis, Streaming Speech Synthesis, Vision Model Performance Testing Tool Usage Guide

1. Create data directory under main/xiaozhi-server
2. Create .config.yaml file in data directory
3. Write your voice recognition, large language model, streaming speech synthesis, vision model parameters in data/config.yaml
For example:
```
LLM:
  ChatGLMLLM:
    # Define LLM API type
    type: openai
    # glm-4-flash is free, but still needs to register and fill in api_key
    # You can find your api key here https://bigmodel.cn/usercenter/proj-mgmt/apikeys
    model_name: glm-4-flash
    url: https://open.bigmodel.cn/api/paas/v4/
    api_key: your chat-glm web key

TTS:

VLLM:

ASR:
```
4. Run performance_tester.py under main/xiaozhi-server directory:
```
python performance_tester.py
```

