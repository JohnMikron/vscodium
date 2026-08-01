# Quick Start Guide: Using Llama.cpp in VSCodium

Get started with local LLM models in VSCodium in 5 minutes!

## Prerequisites

- VSCodium with llama.cpp integration (v1.95+)
- A GGUF format model file
- GPU with CUDA support (optional but recommended)

## Step 1: Download a Model

Recommended starter models:

| Model | Size | Quality | Download |
|-------|------|---------|----------|
| Llama-3.2-3B | 3B | Good for quick tasks | [HuggingFace](https://huggingface.co/bartowski/Llama-3.2-3B-Instruct-GGUF) |
| Qwen2.5-7B | 7B | Balanced | [HuggingFace](https://huggingface.co/bartowski/Qwen2.5-7B-Instruct-GGUF) |
| Mistral-7B | 7B | Great general purpose | [HuggingFace](https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.2-GGUF) |

Download the **Q4_K_M** quantization for best balance of quality and speed.

## Step 2: Configure VSCodium

1. Open VSCodium
2. Press `Ctrl+,` (or `Cmd+,` on Mac) to open Settings
3. Search for "llama"
4. Set the following:

```json
{
  "llamaCpp.enabled": true,
  "llamaCpp.modelPath": "/path/to/your/model.gguf",
  "llamaCpp.gpuLayers": 99,
  "llamaCpp.contextSize": 65536
}
```

### Path Examples:
- **Windows**: `F:\models\mistral-7b.Q4_K_M.gguf`
- **Linux**: `/home/user/models/mistral-7b.Q4_K_M.gguf`
- **macOS**: `/Users/username/models/mistral-7b.Q4_K_M.gguf`

## Step 3: Start Chatting

1. Open Chat view: `Ctrl+Shift+I` (or `Cmd+Shift+I`)
2. Select **"Local LLM"** from participants
3. Type your question and press Enter

## Troubleshooting

### "Model not loading"
- Check the file path is correct
- Ensure the file is in GGUF format
- Verify file integrity (re-download if needed)

### "Out of memory"
- Reduce `llamaCpp.gpuLayers` to 50 or lower
- Reduce `llamaCpp.contextSize` to 32768
- Close other GPU-intensive applications

### "Very slow responses"
- Increase `llamaCpp.gpuLayers` (ideally to 99)
- Ensure model fits entirely in VRAM
- Use a smaller model or higher quantization

## Next Steps

- Read the full [Llama.cpp Integration Guide](llama-cpp-integration.md)
- Explore different models on [HuggingFace](https://huggingface.co/models?library=gguf)
- Join our [Gitter community](https://gitter.im/VSCodium/Lobby) for help

## Example Session

```
You: Explain quantum entanglement in simple terms

Local LLM: Quantum entanglement is like having two magic coins...
[thinking]
Let me break this down step by step...

When two particles become entangled, they share a connection 
that persists regardless of distance. Think of it as...
```

Happy coding with AI assistance! 🚀
