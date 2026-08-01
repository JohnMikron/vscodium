# Llama.cpp Integration in VSCodium

This document describes the integrated llama.cpp support for running local LLM models directly within VSCodium.

## Features

- **Native Integration**: Run GGUF format models without external extensions
- **GPU Acceleration**: Automatic CUDA/Metal support for compatible hardware
- **Multimodal Support**: Vision-language models with image understanding
- **Thinking Mode**: Display reasoning/thinking output from models
- **Full Offline Operation**: Complete offline mode with no external connections

## Configuration

### Basic Settings

Access settings via `File > Preferences > Settings` and search for "Llama.cpp":

```json
{
  "llamaCpp.enabled": true,
  "llamaCpp.modelPath": "/path/to/your/model.gguf",
  "llamaCpp.contextSize": 65536,
  "llamaCpp.gpuLayers": 99,
  "llamaCpp.threads": 0,
  "llamaCpp.batchSize": 512,
  "llamaCpp.enableThinking": true,
  "llamaCpp.enableMultimodal": true
}
```

### Advanced Settings

#### Context Size
- Default: 65536 tokens
- Maximum: 262144 tokens (model dependent)
- Warning: Using less than model's training context reduces capabilities

#### GPU Layers
- Set to 99 for full GPU offloading
- Reduce if encountering OOM errors
- Auto-detection available on first run

#### Threads
- 0 = auto-detect based on CPU cores
- Recommended: Physical cores only (no hyperthreading)

## Supported Model Quantizations

- Q4_K_M (recommended balance)
- Q5_K_M (higher quality)
- Q6_K (near-lossless)
- Q8_0 (maximum quality)

## Usage

### Chat Interface

1. Open Chat view (`Ctrl+Shift+I` or `Cmd+Shift+I`)
2. Select "Local LLM" participant
3. Ask questions naturally

### Context Window Warning

If you see: `n_ctx_seq < n_ctx_train -- the full capacity of the model will not be utilized`

Increase context size in settings:
```json
"llamaCpp.contextSize": 131072
```

### Multimodal Models

For vision-language models (like Qwen-VL):
```json
"llamaCpp.enableMultimodal": true
```

Note: Qwen-VL models require minimum 1024 image tokens for grounding tasks.
Add `--image-min-tokens 1024` if experiencing accuracy issues.

## Offline Mode

Enable complete offline operation:

```json
{
  "offlineMode.enabled": true,
  "offlineMode.disableExtensions": true,
  "offlineMode.disableGallery": true
}
```

This disables:
- Extension marketplace connections
- Telemetry
- Update checks
- External API calls

## Performance Tips

1. **Model Loading**: First load may take time; subsequent loads use cache
2. **Prompt Cache**: Enabled by default (8GB limit)
3. **Warm-up**: Empty run performed on startup (disable with `--no-warmup`)
4. **Speculative Decoding**: Available for compatible model pairs

## Troubleshooting

### Out of Memory
- Reduce `gpuLayers`
- Reduce `contextSize`
- Close other GPU applications

### Slow Generation
- Increase `gpuLayers`
- Ensure model fits in VRAM
- Check thermal throttling

### Model Not Loading
- Verify GGUF file integrity
- Check path is absolute
- Ensure quantization is supported

## Security Notes

⚠️ **Warning**: Built-in tools are experimental
- Do not expose server to untrusted environments
- CORS proxy is enabled by default for local development
- These features may change in future versions

## Example Configurations

### RTX 3060 12GB (Recommended)
```json
{
  "llamaCpp.contextSize": 65536,
  "llamaCpp.gpuLayers": 99,
  "llamaCpp.threads": 6,
  "llamaCpp.batchSize": 512
}
```

### CPU Only (i7-4930K)
```json
{
  "llamaCpp.contextSize": 32768,
  "llamaCpp.gpuLayers": 0,
  "llamaCpp.threads": 6,
  "llamaCpp.batchSize": 256
}
```

### Maximum Context (64GB+ RAM)
```json
{
  "llamaCpp.contextSize": 262144,
  "llamaCpp.gpuLayers": 99,
  "llamaCpp.threads": 8,
  "llamaCpp.batchSize": 1024
}
```

## Model Recommendations

| Use Case | Model Size | Quantization | VRAM Required |
|----------|-----------|--------------|---------------|
| Quick responses | 7B-14B | Q4_K_M | 6-8 GB |
| Balanced | 20B-35B | Q4_K_M | 12-16 GB |
| High quality | 70B+ | Q4_K_M | 24+ GB |

## Resources

- [llama.cpp Documentation](https://github.com/ggml-org/llama.cpp)
- [GGUF Format Spec](https://github.com/ggml-org/llama.cpp/blob/master/docs/gguf.md)
- [HuggingFace Models](https://huggingface.co/models?library=gguf)
