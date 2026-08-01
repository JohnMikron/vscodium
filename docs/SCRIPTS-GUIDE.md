# 🚀 VSCodium LlamaCpp Server Scripts - Ολοκληρωμένος Οδηγός

## 📋 Επισκόπηση

Τα scripts στον φάκελο `scripts/` επιτρέπουν την **εύκολη εκκίνηση** του llama.cpp server με **πλήρη έλεγχο** των ρυθμίσεων, χωρίς να χρειάζεται να θυμάστε πολύπλοκες εντολές.

## 🎯 Χαρακτηριστικά

✅ **Cross-platform**: Windows (.bat), Linux (.sh), macOS (.sh με Metal)  
✅ **Interactive mode**: Αν δεν δοθούν arguments, τα scripts ζητούν είσοδο  
✅ **Auto-detection**: Βρίσκει αυτόματα μοντέλα και εγκαταστάσεις  
✅ **Custom paths**: Υποστήριξη για custom διαδρομές (llama.cpp, CUDA, ROCm)  
✅ **GPU acceleration**: Αυτόματη ενεργοποίηση CUDA/ROCm/Metal  
✅ **Flexible options**: Ρύθμιση θύρας, threads, context size, κλπ.  
✅ **Verbose logging**: Αναλυτικά logs για debugging  

---

## 📦 Εγκατάσταση

### Προαπαιτούμενα

1. **llama.cpp** εγκατεστημένο:
   - **Windows**: Κατεβάστε από [llama.cpp releases](https://github.com/ggml-org/llama.cpp/releases)
   - **Linux**: Build από source ή package manager
   - **macOS**: `brew install llama.cpp` ή build από source

2. **GGUF Model**: Κατεβάστε ένα μοντέλο σε GGUF format
   - Πηγές: [HuggingFace](https://huggingface.co/models?library=gguf), [Ollama](https://ollama.ai/library)

3. **GPU Drivers** (προαιρετικό αλλά συνιστάται):
   - **NVIDIA**: CUDA Toolkit + drivers
   - **AMD**: ROCm
   - **macOS**: Metal (built-in)

### Τοποθέτηση Μοντέλων

```bash
# Αντιγράψτε τα μοντέλα σας στον φάκελο models/
cp /path/to/your/model.gguf /workspace/models/
```

---

## 🖥️ Χρήση ανά Πλατφόρμα

### Windows

#### Βασική Χρήση
```batch
cd scripts\windows
run-llama-server.bat models\Bonsai-27B-Q1_0.gguf 8070
```

#### Με Επιλογές
```batch
run-llama-server.bat F:\models\llama-3.2-3b-instruct.Q4_K_M.gguf 8080 --gpu-layers 50 --threads 8 --context 32768
```

#### Interactive Mode (χωρίς arguments)
```batch
run-llama-server.bat
```
Θα σας ζητήσει:
- Διαδρομή μοντέλου
- Θύρα (default: 8070)

#### Custom CUDA Path
```batch
set CUDA_PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1
set LLAMA_CPP_PATH=C:\llama.cpp\build\bin\Release
run-llama-server.bat models\model.gguf 8070
```

---

### Linux

#### Βασική Χρήση
```bash
cd /workspace/scripts/linux
./run-llama-server.sh models/Bonsai-27B-Q1_0.gguf 8070
```

#### Με Επιλογές
```bash
./run-llama-server.sh /home/user/models/llama-3.2-3b-instruct.Q4_K_M.gguf 8080 \
    --gpu-layers 50 \
    --threads 8 \
    --context 32768 \
    --batch 1024
```

#### Interactive Mode
```bash
./run-llama-server.sh
```

#### Custom Paths
```bash
export CUDA_PATH=/usr/local/cuda-12.1
export ROCM_PATH=/opt/rocm-5.7
export LLAMA_CPP_PATH=/opt/llama.cpp/build/bin
./run-llama-server.sh models/model.gguf 8070
```

#### Systemd Service (για background execution)
```ini
# /etc/systemd/system/llama-server.service
[Unit]
Description=LlamaCpp Server for VSCodium
After=network.target

[Service]
Type=simple
User=youruser
WorkingDirectory=/workspace
ExecStart=/workspace/scripts/linux/run-llama-server.sh /workspace/models/Bonsai-27B-Q1_0.gguf 8070
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable llama-server
sudo systemctl start llama-server
```

---

### macOS

#### Βασική Χρήση (με Metal GPU)
```bash
cd /workspace/scripts/mac
./run-llama-server-mac.sh models/Bonsai-27B-Q1_0.gguf 8070
```

#### Με Επιλογές
```bash
./run-llama-server-mac.sh /Users/username/models/llama-3.2-3b-instruct.Q4_K_M.gguf 8080 \
    --gpu-layers 50 \
    --threads 10 \
    --context 32768
```

#### Interactive Mode
```bash
./run-llama-server-mac.sh
```

#### Custom Paths
```bash
export LLAMA_CPP_PATH=/opt/homebrew/opt/llama.cpp/bin
./run-llama-server-mac.sh models/model.gguf 8070
```

---

## ⚙️ Όλες οι Επιλογές

| Επιλογή | Περιγραφή | Default | Παράδειγμα |
|---------|-----------|---------|------------|
| `[μοντέλο]` | Διαδρομή προς GGUF αρχείο | **Απαιτείται** | `models/model.gguf` |
| `[θύρα]` | Θύρα HTTP server | `8070` | `8080` |
| `--gpu-layers N` | Layers στη GPU | `99` (όλα) | `50`, `99`, `0` (CPU only) |
| `--threads N` | CPU threads | `6` (Win/Linux), auto (Mac) | `8`, `12` |
| `--batch N` | Batch size | `512` | `1024`, `2048` |
| `--context N` | Context size | `65536` | `32768`, `131072`, `262144` |
| `--host IP` | Host address | `127.0.0.1` | `0.0.0.0` (all interfaces) |

---

## 🔗 Σύνδεση με VSCodium

### Βήμα 1: Εκκίνηση Server
```bash
./run-llama-server.sh models/model.gguf 8070
```

### Βήμα 2: Ρυθμίσεις VSCodium

Προσθέστε στο `settings.json`:
```json
{
  "llamaCppSupport.enabled": true,
  "llamaCppSupport.serverUrl": "http://127.0.0.1:8070",
  "llamaCppSupport.autoConnect": true,
  "llamaCppSupport.customLlamaCppPath": "",  // Προαιρετικό
  "llamaCppSupport.customCudaPath": "",      // Προαιρετικό
  "llamaCppSupport.customRocmPath": ""       // Προαιρετικό
}
```

### Βήμα 3: Χρήση Chat

1. Ανοίξτε το Chat panel: `Ctrl+Alt+I` (Windows/Linux) ή `Cmd+Option+I` (macOS)
2. Επιλέξτε **"LlamaCpp"** από τα available participants
3. Ξεκινήστε να συνομιλείτε!

---

## 🎯 Παραδείγματα Χρήσης

### Μικρό Μοντέλο (3B-7B) σε CPU
```bash
./run-llama-server.sh models/phi-3-mini-4k-instruct.Q4_K_M.gguf 8070 \
    --gpu-layers 0 \
    --threads 8
```

### Μεσαίο Μοντέλο (13B) με Μερικό GPU
```bash
./run-llama-server.sh models/mistral-7b-instruct-v0.3.Q5_K_M.gguf 8070 \
    --gpu-layers 35 \
    --threads 6 \
    --context 32768
```

### Μεγάλο Μοντέλο (27B+) με Πλήρες GPU
```bash
./run-llama-server.sh models/Bonsai-27B-Q1_0.gguf 8070 \
    --gpu-layers 99 \
    --threads 6 \
    --context 65536 \
    --batch 1024
```

### Multimodal/Vision Model
```bash
./run-llama-server.sh models/qwen2-vl-7b-instruct-q4_k_m.gguf 8070 \
    --gpu-layers 99 \
    --mmproj models/qwen2-vl-mmproj-Q8_0.gguf \
    --threads 8
```

### Multiple Models (Different Ports)
```bash
# Terminal 1
./run-llama-server.sh models/llama-3.2-3b-instruct.Q4_K_M.gguf 8070

# Terminal 2
./run-llama-server.sh models/mistral-7b-instruct-v0.3.Q5_K_M.gguf 8071

# Terminal 3
./run-llama-server.sh models/Bonsai-27B-Q1_0.gguf 8072
```

---

## 🐛 Troubleshooting

### "Model file not found"
```bash
# Έλεγχος διαδρομής
ls -la models/*.gguf

# Χρήση απόλυτης διαδρομής
./run-llama-server.sh /absolute/path/to/model.gguf 8070
```

### "llama-server not found"
```bash
# Έλεγχος εγκατάστασης
which llama-server

# Ορισμός custom path
export LLAMA_CPP_PATH=/opt/llama.cpp/build/bin
./run-llama-server.sh models/model.gguf 8070
```

### GPU Not Detected

**NVIDIA:**
```bash
# Έλεγχος CUDA
nvidia-smi
nvcc --version

# Ορισμός CUDA path
export CUDA_PATH=/usr/local/cuda-12.1
```

**AMD:**
```bash
# Έλεγχος ROCm
rocminfo

# Ορισμός ROCm path
export ROCM_PATH=/opt/rocm-5.7
```

**macOS:**
```bash
# Έλεγχος Metal
system_profiler SPDisplaysDataType | grep Metal
```

### Port Already in Use
```bash
# Εύρεση process που χρησιμοποιεί την θύρα
# Linux/macOS
lsof -i :8070

# Windows
netstat -ano | findstr :8070

# Αλλαγή θύρας
./run-llama-server.sh models/model.gguf 8080
```

### Out of Memory
```bash
# Μείωση GPU layers
./run-llama-server.sh models/model.gguf 8070 --gpu-layers 30

# Μείωση context size
./run-llama-server.sh models/model.gguf 8070 --context 32768

# Μείωση batch size
./run-llama-server.sh models/model.gguf 8070 --batch 256
```

### Slow Performance
```bash
# Αύξηση GPU layers
./run-llama-server.sh models/model.gguf 8070 --gpu-layers 99

# Αύξηση threads (CPU only)
./run-llama-server.sh models/model.gguf 8070 --gpu-layers 0 --threads 12

# Χρήση καλύτερης quantization (Q5_K_M ή Q6_K)
```

---

## 📊 Performance Tips

### GPU Memory Requirements

| Model Size | Quantization | VRAM Required | Recommended GPU |
|------------|--------------|---------------|-----------------|
| 3B         | Q4_K_M       | ~2.5 GB       | GTX 1650+       |
| 7B         | Q4_K_M       | ~5 GB         | RTX 3060+       |
| 13B        | Q4_K_M       | ~9 GB         | RTX 3080+       |
| 27B        | Q1_0         | ~10 GB        | RTX 3060 12GB+  |
| 27B        | Q4_K_M       | ~18 GB        | RTX 4090+       |
| 70B        | Q2_K         | ~24 GB        | Dual GPU        |

### Optimal Settings by Hardware

**RTX 3060 12GB:**
```bash
./run-llama-server.sh models/27B-model.gguf 8070 \
    --gpu-layers 99 \
    --threads 6 \
    --context 65536
```

**RTX 4090 24GB:**
```bash
./run-llama-server.sh models/70B-model.gguf 8070 \
    --gpu-layers 99 \
    --threads 8 \
    --context 131072
```

**CPU Only (32GB RAM):**
```bash
./run-llama-server.sh models/13B-model.gguf 8070 \
    --gpu-layers 0 \
    --threads 12 \
    --context 32768 \
    --batch 256
```

**macOS M1/M2/M3 (16GB Unified):**
```bash
./run-llama-server-mac.sh models/13B-model.gguf 8070 \
    --gpu-layers 99 \
    --context 65536
```

---

## 🔒 Ασφάλεια

### Local Only (Default)
Τα scripts τρέχουν μόνο στο `127.0.0.1` από προεπιλογή - **δεν εκτίθενται στο internet**.

### Exposure Warning
⚠️ **ΜΗΝ** εκθέτετε τον server στο internet χωρίς authentication!

Αν χρειάζεστε remote access:
1. Χρησιμοποιήστε SSH tunneling
2. Ρυθμίστε reverse proxy με authentication
3. Χρησιμοποιήστε VPN

### CORS & Built-in Tools
Ο server έχει ενεργοποιημένα CORS και built-in tools για development. **Μην εκθέτετε σε untrusted environments**.

---

## 📄 Άδειες Χρήσης

Βεβαιωθείτε ότι έχετε διαβάσει και αποδεχτεί τις άδειες χρήσης των μοντέλων:
- `licenses/llama-3.2-license.txt` - Meta Llama 3.2
- `licenses/` - Προσθέστε νέες άδειες εδώ

Κάθε μοντέλο έχει τη δική του άδεια χρήσης. Ενημερωθείτε πριν τη χρήση!

---

## 🆘 Υποστήριξη

### Documentation
- `docs/COMPLETE-LLM-INTEGRATION.md` - Πλήρης τεκμηρίωση ενσωμάτωσης
- `docs/TELEMETRY-REMOVAL-COMPLETE.md` - Τεκμηρίωση telemetry removal
- `IMPLEMENTATION_COMPLETE.md` - Σύνοψη υλοποίησης

### Logs
Τα logs εμφανίζονται στο terminal και αποθηκεύονται (αν οριστεί `logPath` στο `product.json`).

### Community
- GitHub Issues: [VSCodium Repository](https://github.com/VSCodium/vscodium/issues)
- llama.cpp Issues: [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp/issues)

---

## 🎉 Έτοιμοι;

```bash
# Γρήγορο ξεκίνημα
cd scripts/linux  # ή windows/mac
./run-llama-server.sh  # Interactive mode
# ή
./run-llama-server.sh models/your-model.gguf 8070
```

**Απολαύστε τα τοπικά AI μοντέλα σας στο VSCodium!** 🚀
