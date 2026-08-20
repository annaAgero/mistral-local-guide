# Running Mistral Locally on Windows 11

A comprehensive guide for running Mistral AI models locally on your Dell XPS 15 7590 with Windows 11 Business.

## Your System Specs

- **OS:** Windows 11 Business (Build 26200)
- **Processor:** Intel Core i9-9880H (8 cores, 16 logical processors)
- **RAM:** 32 GB (7.27 GB currently available)
- **System:** Dell XPS 15 7590

These specs are **ideal for running Mistral locally**. ✅

---

## Quick Start: Ollama (Recommended for Beginners)

### Step 1: Download Ollama

1. Visit [ollama.ai](https://ollama.ai)
2. Click "Download" and select **Windows**
3. Run the installer and follow the setup wizard
4. Ollama will start automatically after installation

### Step 2: Pull Mistral Model

1. Open **PowerShell** or **Command Prompt**
2. Run:
   ```bash
   ollama pull mistral
   ```
   This downloads the Mistral 7B model (~4.1 GB) — perfect for your system

### Step 3: Run Mistral

```bash
ollama run mistral
```

You'll see a prompt where you can type questions and get responses locally.

### Step 4: Use via API

Ollama runs a local API on `http://localhost:11434`. You can use it with Python:

```python
import requests
import json

response = requests.post(
    "http://localhost:11434/api/generate",
    json={
        "model": "mistral",
        "prompt": "Why is the sky blue?",
        "stream": False
    }
)

print(response.json()["response"])
```

---

## Alternative: LLaMA.cpp (CPU Optimized)

Good if you want maximum CPU efficiency.

### Step 1: Download & Setup

1. Visit [llama.cpp releases](https://github.com/ggerganov/llama.cpp/releases)
2. Download `llama-cpp-win-x64.zip` (Windows x64 build)
3. Extract to a folder, e.g., `C:\llama.cpp`

### Step 2: Download Mistral Model

1. Visit [Hugging Face - Mistral 7B GGUF](https://huggingface.co/TheBloke/Mistral-7B-GGUF)
2. Download a quantized version:
   - **Q4_K_M** (recommended) — ~4.4 GB, good balance of speed/quality
   - **Q5_K_M** — ~5.2 GB, higher quality
3. Save to `C:\llama.cpp\models\mistral.gguf`

### Step 3: Run Mistral

Open PowerShell and run:

```bash
cd C:\llama.cpp
.\main.exe -m models\mistral.gguf -p "Why is the sky blue?" -n 256
```

---

## Advanced: Python Setup (HuggingFace Transformers)

For developers who want fine-tuning or custom integration.

### Step 1: Install Python

1. Download [Python 3.10+](https://www.python.org/downloads/)
2. During installation, **check "Add Python to PATH"**
3. Verify: Open PowerShell and run `python --version`

### Step 2: Install Dependencies

```bash
pip install transformers torch accelerate
```

### Step 3: Run Mistral

Create a file `run_mistral.py`:

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "mistralai/Mistral-7B-Instruct-v0.1"

# Load tokenizer and model
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(model_name, device_map="auto")

# Generate response
prompt = "Why is the sky blue?"
inputs = tokenizer(prompt, return_tensors="pt")
outputs = model.generate(**inputs, max_length=200)

print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

Run it:
```bash
python run_mistral.py
```

---

## Performance Tips for Your System

### 1. Monitor Available Memory
Before running Mistral, ensure you have at least **8-10 GB free**:
```bash
# Check available RAM in PowerShell
Get-ComputerInfo | Select TotalPhysicalMemory, FreePhysicalMemory
```

### 2. Enable GPU Acceleration (if available)

Your Dell XPS 15 likely has NVIDIA GPU. To enable CUDA:

**Ollama:** 
- Ollama automatically detects NVIDIA GPU — no extra setup needed

**LLaMA.cpp:**
- Download the CUDA-enabled build instead of CPU-only
- Run with: `.\main.exe -m models\mistral.gguf -p "prompt" -n 256 -ngl 32`

**Python/Transformers:**
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### 3. Optimize Response Speed

- Use **quantized models** (Q4, Q5) for faster inference
- Lower `max_tokens` to reduce generation time
- Run during off-peak CPU usage times

### 4. Manage Disk Space

Mistral models typically need:
- **7B model:** 4-5 GB
- Additional dependencies: 2-3 GB
- **Total recommended free space:** 15-20 GB

Current free space on your system: 7.27 GB. **Consider freeing up more space** before downloading large models.

---

## Troubleshooting

### Issue: "Out of Memory" Error
**Solution:** 
- Use smaller quantization (Q3_K instead of Q5_K)
- Close other applications
- Reduce `max_tokens` parameter
- Enable GPU acceleration

### Issue: Slow Response Generation
**Solution:**
- Enable GPU acceleration (see above)
- Use quantized models (Q4_K_M)
- Reduce context length
- Check CPU usage — close background apps

### Issue: Ollama Won't Start
**Solution:**
- Restart the Ollama service
- Reinstall Ollama
- Check Windows Defender isn't blocking it

### Issue: CUDA Not Detected
**Solution:**
- Verify NVIDIA GPU with `nvidia-smi` (install NVIDIA drivers if needed)
- Reinstall CUDA toolkit from NVIDIA website

---

## Recommended Setup for Your System

Based on your specs, I recommend:

**Best for ease:** **Ollama** + Mistral 7B
- Minimal setup
- Works out-of-the-box
- Good performance on CPU, better with GPU

**Best for control:** **LLaMA.cpp** + Q4_K_M quantization
- CPU-optimized
- Lightweight
- Faster inference

**Best for development:** **Python + HuggingFace** (once you free up disk space)
- Full customization
- Easy fine-tuning
- Best for integration into projects

---

## Next Steps

1. **Choose a setup method** (I recommend starting with Ollama)
2. **Free up disk space** to at least 15 GB
3. **Download and test** your first model
4. **Benchmark performance** and optimize as needed

Good luck! 🚀
