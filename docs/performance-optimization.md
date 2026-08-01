# Performance Optimization Guide for Llama.cpp in VSCodium

Maximize your local LLM performance with these optimization techniques.

## Hardware Detection

### Automatic GPU Detection

VSCodium automatically detects your GPU and suggests optimal settings:

```
Device Info Detected:
  - CUDA0: NVIDIA GeForce RTX 3060 (12287 MiB)
  - CPU: Intel(R) Core(TM) i7-4930K CPU @ 3.40GHz
```

### Manual Configuration

If auto-detection fails, manually set:

```json
{
  "llamaCpp.gpuLayers": 99,
  "llamaCpp.threads": 6,
  "llamaCpp.nBatch": 512
}
```

## Memory Management

### VRAM Optimization

| GPU VRAM | Max Model Size | Recommended Quant |
|----------|---------------|-------------------|
| 6GB | 7B | Q4_K_M |
| 8GB | 13B | Q4_K_M |
| 12GB | 27B | Q4_K_M |
| 16GB | 35B | Q5_K_M |
| 24GB | 70B | Q4_K_M |
| 48GB+ | 70B+ | Q6_K/Q8_0 |

### System RAM Requirements

For CPU fallback or large context:
- Minimum: 16GB
- Recommended: 32GB
- Optimal: 64GB+

## Context Window Settings

### Understanding the Warning

```
W llama_context: n_ctx_seq (64768) < n_ctx_train (262144) -- 
  the full capacity of the model will not be utilized
```

This means you're using less context than the model was trained with.

### Optimal Context Sizes

| Use Case | Context Size | Memory Required |
|----------|-------------|-----------------|
| Quick chat | 8192 | ~2GB |
| Code assistance | 32768 | ~4GB |
| Document analysis | 65536 | ~8GB |
| Full book analysis | 131072 | ~16GB |
| Maximum capability | 262144 | ~32GB |

## Batch Processing

### Batch Size Tuning

```json
{
  "llamaCpp.batchSize": 512,      // Prompt processing
  "llamaCpp.ubatchSize": 256       // Generation batch
}
```

**Guidelines:**
- Larger batches = faster prompt processing
- Too large = OOM errors
- Start at 512, adjust based on VRAM

## Thread Configuration

### CPU Thread Optimization

```bash
# Physical cores only (no hyperthreading)
n_threads = physical_cores
n_threads_batch = physical_cores
```

**Example for i7-4930K (6 cores, 12 threads):**
```json
{
  "llamaCpp.threads": 6,
  "llamaCpp.batchThreads": 6
}
```

## Caching Strategies

### Prompt Cache

Enabled by default with 8GB limit:
```json
{
  "llamaCpp.enableCache": true,
  "llamaCpp.cacheSizeGB": 8
}
```

**Benefits:**
- Faster repeated queries
- Conversation history retention
- Reduced model reloads

### Model Warm-up

First load includes warm-up run (~2-5 seconds):
```json
{
  "llamaCpp.warmup": true  // Default: true
}
```

Disable for faster startup (first query will be slower):
```json
{
  "llamaCpp.warmup": false
}
```

## Advanced Optimizations

### Flash Attention

For supported GPUs (RTX 30xx+):
```json
{
  "llamaCpp.flashAttention": true
}
```

### Memory Mapping

For large models (>20GB):
```json
{
  "llamaCpp.memoryMap": true,
  "llamaCpp.mmapThreshold": 0.8
}
```

### Speculative Decoding

When using draft models:
```json
{
  "llamaCpp.speculativeDecoding": true,
  "llamaCpp.draftModel": "/path/to/draft-model.gguf",
  "llamaCpp.draftLayers": 16
}
```

## Multimodal Optimization

### Vision Models (Qwen-VL, etc.)

```json
{
  "llamaCpp.enableMultimodal": true,
  "llamaCpp.imageMinTokens": 1024,
  "llamaCpp.maxImageTokens": 4096
}
```

**Note:** Qwen-VL requires minimum 1024 image tokens for grounding tasks.

## Monitoring & Debugging

### Enable Verbose Logging

```json
{
  "llamaCpp.verbose": true,
  "llamaCpp.logLevel": 3
}
```

Log levels:
- 0: Errors only
- 1: Warnings
- 2: Info
- 3: Debug (verbose)

### Performance Metrics

Watch for these metrics in logs:
```
load_time: 12.5s     # Model loading
eval_time: 45ms/token # Generation speed
memory_used: 8.2GB   # Total memory usage
```

## Troubleshooting Performance

### Slow Generation (<10 tokens/s)

**Checklist:**
- [ ] GPU layers set to max (99)
- [ ] Model fits in VRAM
- [ ] No thermal throttling
- [ ] Using quantized model (Q4_K_M or similar)

### High Memory Usage

**Solutions:**
1. Reduce context size
2. Lower GPU layers
3. Use higher quantization
4. Enable memory mapping

### OOM (Out of Memory) Errors

**Immediate fixes:**
```json
{
  "llamaCpp.gpuLayers": 50,
  "llamaCpp.contextSize": 32768,
  "llamaCpp.batchSize": 256
}
```

## Benchmark Examples

### RTX 3060 12GB (Optimal)
```
Model: Mistral-7B-Q4_K_M
Context: 65536
GPU Layers: 99
Speed: ~45 tokens/s
Memory: 6.2GB VRAM
```

### CPU Only (i7-4930K)
```
Model: Mistral-7B-Q4_K_M
Context: 32768
Threads: 6
Speed: ~3 tokens/s
Memory: 8.5GB RAM
```

### RTX 4090 24GB (Maximum)
```
Model: Qwen-32B-Q5_K_M
Context: 131072
GPU Layers: 99
Speed: ~35 tokens/s
Memory: 22.1GB VRAM
```

## Quick Reference Card

```
┌─────────────────────────────────────────────┐
│  OPTIMAL SETTINGS BY HARDWARE              │
├─────────────────────────────────────────────┤
│  RTX 3060 (12GB):                          │
│    gpuLayers: 99, context: 65536           │
│                                            │
│  RTX 4070 (12GB):                          │
│    gpuLayers: 99, context: 65536           │
│                                            │
│  RTX 4090 (24GB):                          │
│    gpuLayers: 99, context: 131072          │
│                                            │
│  CPU Only (16GB RAM):                      │
│    gpuLayers: 0, threads: phys_cores       │
│    context: 32768                          │
└─────────────────────────────────────────────┘
```
