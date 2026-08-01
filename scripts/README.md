# VSCodium LlamaCpp Server Scripts

Αυτός ο φάκελος περιέχει scripts για την εκκίνηση του llama.cpp server με προσαρμοσμένες ρυθμίσεις.

## 📁 Δομή Φακέλων

```
scripts/
├── windows/
│   └── run-llama-server.bat      # Windows batch script
├── linux/
│   └── run-llama-server.sh       # Linux bash script
└── mac/
    └── run-llama-server-mac.sh   # macOS bash script (Metal GPU)
```

## 🚀 Χρήση

### Windows

```batch
run-llama-server.bat [μοντέλο] [θύρα] [επιλογές]
```

**Παραδείγματα:**
```batch
run-llama-server.bat models\Bonsai-27B-Q1_0.gguf 8070
run-llama-server.bat F:\models\llama-3.2-3b-instruct.Q4_K_M.gguf 8080 --gpu-layers 50
run-llama-server.bat models\mistral-7b-instruct-v0.3.Q5_K_M.gguf 8070 --threads 8 --context 32768
```

### Linux/macOS

```bash
./run-llama-server.sh [μοντέλο] [θύρα] [επιλογές]
```

**Παραδείγματα:**
```bash
./run-llama-server.sh models/Bonsai-27B-Q1_0.gguf 8070
./run-llama-server.sh /home/user/models/llama-3.2-3b-instruct.Q4_K_M.gguf 8080 --gpu-layers 50
./run-llama-server.sh models/mistral-7b-instruct-v0.3.Q5_K_M.gguf 8070 --threads 8 --context 32768
```

## ⚙️ Επιλογές

| Επιλογή | Περιγραφή | Default |
|---------|-----------|---------|
| `[μοντέλο]` | Διαδρομή προς το GGUF αρχείο μοντέλου | - |
| `[θύρα]` | Θύρα HTTP server | 8070 |
| `--gpu-layers N` | Αριθμός layers στη GPU | 99 (όλα) |
| `--threads N` | Αριθμός CPU threads | 6 (Windows/Linux), auto (macOS) |
| `--batch N` | Batch size | 512 |
| `--context N` | Context size | 65536 |
| `--host IP` | Host address | 127.0.0.1 |

## 🔧 Custom Διαδρομές

Τα scripts υποστηρίζουν custom διαδρομές μέσω environment variables:

### Windows (Batch)
```batch
set LLAMA_CPP_PATH=C:\llama.cpp\build\bin\Release
set CUDA_PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.0
run-llama-server.bat models\model.gguf 8070
```

### Linux/macOS (Bash)
```bash
export LLAMA_CPP_PATH=/opt/llama.cpp/build/bin
export CUDA_PATH=/usr/local/cuda-12.0
export ROCM_PATH=/opt/rocm
./run-llama-server.sh models/model.gguf 8070
```

## 🔗 Σύνδεση με VSCodium

Μόλις τρέξει ο server, το VSCodium μπορεί να συνδεθεί αυτόματα:

1. **Ρυθμίσεις VSCodium:**
   ```json
   {
     "llamaCppSupport.enabled": true,
     "llamaCppSupport.serverUrl": "http://127.0.0.1:8070",
     "llamaCppSupport.autoConnect": true
   }
   ```

2. **Chat Participant:**
   - Ανοίξτε το Chat panel (`Ctrl+Alt+I` ή `Cmd+Option+I`)
   - Επιλέξτε "LlamaCpp" από τα available participants
   - Ξεκινήστε να συνομιλείτε!

## 📝 Σημειώσεις

- **GPU Acceleration**: Τα scripts ενεργοποιούν αυτόματα GPU acceleration αν βρεθεί CUDA (NVIDIA), ROCm (AMD), ή Metal (macOS)
- **Interactive Mode**: Αν δεν δοθούν arguments, τα scripts θα ζητήσουν interactive είσοδο
- **Verbose Logging**: Όλα τα scripts τρέχουν με `--verbose` για detailed logs
- **Chat Template**: Χρησιμοποιείται το `chatml` template ως default

## 🐛 Troubleshooting

### "Model file not found"
- Ελέγξτε ότι η διαδρομή είναι σωστή
- Χρησιμοποιήστε απόλυτη διαδρομή αν χρειάζεται

### "llama-server not found"
- Εγκαταστήστε το llama.cpp ή ορίστε το `LLAMA_CPP_PATH`
- Windows: Κατεβάστε από https://github.com/ggml-org/llama.cpp/releases
- Linux: Build από source ή χρησιμοποιήστε package manager

### GPU not detected
- **NVIDIA**: Εγκαταστήστε CUDA Toolkit και drivers
- **AMD**: Εγκαταστήστε ROCm
- **macOS**: Το Metal είναι built-in, βεβαιωθείτε ότι έχετε recent macOS version

### Port already in use
- Αλλάξτε θύρα: `run-llama-server.bat model.gguf 8080`
- Κλείστε άλλα processes που χρησιμοποιούν την ίδια θύρα

## 📄 Άδειες Χρήσης

Βεβαιωθείτε ότι έχετε διαβάσει και αποδεχτεί τις άδειες χρήσης των μοντέλων που χρησιμοποιείτε.
Οι άδειες βρίσκονται στον φάκελο `licenses/`.
