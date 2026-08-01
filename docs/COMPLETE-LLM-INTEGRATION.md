# Complete LLM Integration Guide

## Overview

This guide covers the complete integration of local LLM models into VSCodium, including:
- **llama.cpp** native support for GGUF models
- **Ollama** integration for local/remote models  
- **AI Agents** with tool execution capabilities
- **Complete telemetry removal** with internal detailed logging
- **Bundled models** with proper licensing
- **Manual path configuration** for custom installations

## Table of Contents

1. [Quick Start](#quick-start)
2. [Configuration](#configuration)
3. [llama.cpp Integration](#llamacpp-integration)
4. [Ollama Integration](#ollama-integration)
5. [AI Agents](#ai-agents)
6. [Telemetry & Logging](#telemetry--logging)
7. [Bundled Models](#bundled-models)
8. [Troubleshooting](#troubleshooting)

---

## Quick Start

### 1. Enable Local LLM Support

Open Settings (`Ctrl+,`) and navigate to **Llama.cpp Integration**:

```json
{
  "llamaCppSupport.enabled": true,
  "llamaCppSupport.modelPath": "/path/to/your/model.gguf",
  "llamaCppSupport.contextSize": 65536,
  "llamaCppSupport.gpuLayers": 99,
  "llamaCppSupport.enableThinking": true,
  "llamaCppSupport.enableMultimodal": true
}
```

### 2. Manual Path Configuration (Optional)

If automatic hardware detection fails, manually specify paths:

```json
{
  "llamaCppSupport.customLlamaCppPath": "/opt/llama.cpp",
  "llamaCppSupport.customCudaPath": "/usr/local/cuda",
  "llamaCppSupport.customRocmPath": "/opt/rocm"
}
```

### 3. Using the Chat Interface

1. Open the Chat view (`Ctrl+Shift+A`)
2. Select **"Local LLM (llama.cpp)"** from the participant dropdown
3. Start chatting with your local model!

---

## Configuration

### llama.cpp Support Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `llamaCppSupport.enabled` | boolean | `true` | Enable llama.cpp integration |
| `llamaCppSupport.modelPath` | string | `""` | Path to GGUF model file |
| `llamaCppSupport.customLlamaCppPath` | string | `""` | Custom llama.cpp installation path |
| `llamaCppSupport.customCudaPath` | string | `""` | Custom CUDA toolkit path |
| `llamaCppSupport.customRocmPath` | string | `""` | Custom ROCm path for AMD GPUs |
| `llamaCppSupport.contextSize` | integer | `65536` | Context window size (max: 262144) |
| `llamaCppSupport.gpuLayers` | integer | `99` | Layers to offload to GPU |
| `llamaCppSupport.threads` | integer | `0` | CPU threads (0 = auto) |
| `llamaCppSupport.batchSize` | integer | `512` | Batch size for prompt processing |
| `llamaCppSupport.enableThinking` | boolean | `true` | Show thinking/reasoning output |
| `llamaCppSupport.enableMultimodal` | boolean | `true` | Enable vision/multimodal models |
| `llamaCppSupport.autoDetectHardware` | boolean | `true` | Auto-detect GPU/hardware |
| `llamaCppSupport.bundledModels.enabled` | boolean | `true` | Use bundled models directory |

### Supported Quantizations

All major GGUF quantizations are supported:
- **Q2_K**, **Q3_K_S/M/L**, **Q4_0**, **Q4_1**, **Q4_K_S/M**
- **Q5_0**, **Q5_1**, **Q5_K_S/M**, **Q6_K**, **Q8_0**, **Q8_1**
- **BF16**, **F16**, **F32** (unquantized)

### Ollama Settings

```json
{
  "ollama.enabled": true,
  "ollama.baseUrl": "http://localhost:11434",
  "ollama.defaultModel": "llama3.2",
  "ollama.timeout": 300000
}
```

### Agent Settings

```json
{
  "agent.enabled": true,
  "agent.maxIterations": 10,
  "agent.requireConfirmation": true,
  "agent.autoApprove": ["read_file", "search_files"]
}
```

---

## llama.cpp Integration

### Features

✅ **Native GGUF Support** - Load any GGUF model directly  
✅ **GPU Acceleration** - CUDA, ROCm, Metal, Vulkan  
✅ **Thinking Output** - Display chain-of-thought reasoning  
✅ **Multimodal** - Vision models (LLaVA, Qwen-VL, etc.)  
✅ **Context Caching** - Prompt caching for efficiency  
✅ **Flash Attention** - Optimized attention mechanisms  

### Loading a Model

1. **Download a GGUF model** from HuggingFace or other sources
2. **Configure the path** in settings
3. **The model loads automatically** when you start a chat

Example model paths:
```
/home/user/models/llama-3.2-3b-instruct.Q4_K_M.gguf
/opt/models/qwen2.5-vl-7b-instruct.Q5_K_M.gguf
C:\Models\mistral-7b-instruct-v0.3.Q6_K.gguf
```

### Performance Tips

- **GPU Layers**: Set to 99 for full GPU offloading (if VRAM allows)
- **Context Size**: Match your model's training context for best results
- **Batch Size**: Higher = faster prompts, more VRAM usage
- **Threads**: Leave at 0 for auto-detection, or set to physical cores

---

## Ollama Integration

### Setup

1. Install Ollama from [ollama.com](https://ollama.com)
2. Pull a model: `ollama pull llama3.2`
3. Enable in VSCodium settings

### Available Models

Run `ollama list` to see installed models, or use the model selector in the chat UI.

### Remote Ollama Server

Configure a remote server:
```json
{
  "ollama.baseUrl": "https://ollama.example.com",
  "ollama.apiKey": "your-api-key"
}
```

---

## AI Agents

### Capabilities

AI Agents can autonomously:
- 📁 **Read/Write Files** - With user confirmation for writes
- 🔍 **Search Codebase** - Find files and content
- ⚙️ **Execute Commands** - Run terminal commands safely
- ✏️ **Edit Files** - Apply intelligent code edits

### Tool Confirmation

For safety, dangerous operations require confirmation by default:

```json
{
  "agent.requireConfirmation": true,
  "agent.autoApprove": ["read_file", "search_files"]
}
```

### Example Agent Workflow

```
User: "Refactor the authentication module to use JWT"

Agent:
1. Reads current auth files
2. Analyzes code structure
3. Creates new JWT implementation
4. Writes updated files (with confirmation)
5. Runs tests to verify
```

---

## Telemetry & Logging

### External Telemetry: **DISABLED** ✅

All external telemetry is completely disabled:
- ❌ No crash reports sent externally
- ❌ No usage data collected
- ❌ No performance metrics shared
- ❌ No error logging to external services

### Internal Detailed Logging: **ENABLED** 🔍

Comprehensive internal logging for debugging:

```json
{
  "telemetry.enabled": false,
  "telemetry.crashReporter": false,
  "telemetry.errorLogging": false,
  "telemetry.usageData": false,
  "telemetry.performanceData": false,
  "telemetry.internalDetailedLogging": true,
  "telemetry.logLevel": "verbose",
  "telemetry.logPath": ""
}
```

### Viewing Logs

1. **Output Panel** → Select "Log (Window)" or "Log (Extension Host)"
2. **Command Palette** → "Developer: Show Logs"
3. **Custom Log Path**: Set `telemetry.logPath` to a specific file

### Log Categories

- `[LlamaCpp]` - Model loading, inference, hardware detection
- `[Ollama]` - API requests, model listing
- `[Agent]` - Tool execution, iterations, confirmations
- `[Telemetry]` - Internal logging status

---

## Bundled Models

### Directory Structure

Models and licenses are stored in the installation directory:

```
VSCodium/
├── models/
│   ├── llama-3.2-1b-instruct.Q4_K_M.gguf
│   ├── qwen2.5-coder-1.5b-instruct.Q4_K_M.gguf
│   └── phi-3-mini-4k-instruct.Q4_K_M.gguf
└── licenses/
    ├── llama-3.2-license.txt
    ├── qwen2.5-license.txt
    └── phi-3-license.txt
```

### Enabling Bundled Models

```json
{
  "llamaCppSupport.bundledModels": {
    "enabled": true,
    "modelsDirectory": "models",
    "licensesDirectory": "licenses"
  }
}
```

### Adding New Models

1. Download GGUF model to `models/` directory
2. Add corresponding license to `licenses/` directory
3. Model appears automatically in the model selector

### License Compliance

All bundled models include:
- ✅ Original license text
- ✅ Attribution information
- ✅ Usage restrictions (if any)
- ✅ Model card links

---

## Troubleshooting

### Model Won't Load

**Symptoms**: Error "Failed to load model"

**Solutions**:
1. Verify the GGUF file is not corrupted
2. Check file permissions
3. Ensure sufficient RAM/VRAM
4. Try with `-fit off` if using flash attention
5. Check logs for specific error messages

### GPU Not Detected

**Symptoms**: Running on CPU only despite having GPU

**Solutions**:
1. Disable auto-detection and set manual paths:
   ```json
   {
     "llamaCppSupport.autoDetectHardware": false,
     "llamaCppSupport.customCudaPath": "/usr/local/cuda"
   }
   ```
2. Verify CUDA/ROCm installation
3. Check `nvidia-smi` or `rocm-smi` output
4. Update GPU drivers

### Out of Memory

**Symptoms**: Crash during model load or inference

**Solutions**:
1. Reduce `gpuLayers` (try 50, then decrease)
2. Reduce `contextSize` (try 32768 or lower)
3. Use smaller quantization (Q4 instead of Q8)
4. Close other GPU-intensive applications

### Thinking Output Not Showing

**Symptoms**: Model doesn't display reasoning

**Solutions**:
1. Enable in settings: `"llamaCppSupport.enableThinking": true`
2. Use a model that supports thinking (DeepSeek-R1, QwQ, etc.)
3. Check logs for thinking token parsing errors

### Agent Stuck in Loop

**Symptoms**: Agent keeps iterating without completing

**Solutions**:
1. Reduce `agent.maxIterations` to 5
2. Enable confirmation: `"agent.requireConfirmation": true`
3. Review logs to identify problematic tool calls
4. Provide more specific instructions

### Custom Paths Not Working

**Symptoms**: Manual paths ignored

**Solutions**:
1. Use absolute paths (not relative)
2. Ensure paths exist and are accessible
3. Check for typos in path strings
4. Restart VSCodium after changing paths

---

## Advanced Configuration

### Environment Variables

```bash
export LLAMA_CPP_LIB_PATH=/opt/llama.cpp/lib
export CUDA_HOME=/usr/local/cuda
export ROCM_PATH=/opt/rocm
```

### Custom Build Flags

For advanced users building llama.cpp from source:

```bash
cmake -B build \
  -DLLAMA_CUDA=ON \
  -DLLAMA_FLASH_ATTN=ON \
  -DLLAMA_METAL=OFF \
  -DCMAKE_BUILD_TYPE=Release
```

### Multiple Model Profiles

Create different configurations for different tasks:

```json
{
  "llamaCpp.profiles": [
    {
      "name": "Coding",
      "modelPath": "/models/qwen-coder.Q6_K.gguf",
      "contextSize": 32768,
      "gpuLayers": 99
    },
    {
      "name": "Chat",
      "modelPath": "/models/llama-chat.Q4_K_M.gguf",
      "contextSize": 65536,
      "gpuLayers": 80
    },
    {
      "name": "Vision",
      "modelPath": "/models/qwen-vl.Q5_K_M.gguf",
      "contextSize": 16384,
      "gpuLayers": 99,
      "enableMultimodal": true
    }
  ]
}
```

---

## Security Considerations

⚠️ **Important Security Notes**:

1. **Built-in tools are experimental** - Review tool outputs before trusting
2. **Don't expose to untrusted networks** - Especially with agents enabled
3. **Agent confirmations** - Keep enabled for write/command operations
4. **Model sourcing** - Only download models from trusted sources
5. **API keys** - Store Ollama API keys securely, never commit to version control

---

## Support & Resources

- **Documentation**: `/docs/llama-cpp-integration.md`
- **Quick Start**: `/docs/quick-start-llama.md`
- **Performance**: `/docs/performance-optimization.md`
- **Security**: `/docs/SECURITY-IMPROVEMENTS.md`
- **Issues**: GitHub Issues page
- **Community**: VSCodium Discord/Forum

---

*Last Updated: 2025*
