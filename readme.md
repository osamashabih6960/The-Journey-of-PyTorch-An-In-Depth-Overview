<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:EE4C2C,50:FF6B35,100:f5a623&height=220&section=header&text=The%20Journey%20of%20PyTorch&fontSize=44&fontColor=ffffff&fontAlignY=38&desc=An%20In-Depth%20Overview&descSize=18&descAlignY=58&animation=fadeIn" width="100%"/>

<br/>

[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)](https://pytorch.org)
[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![CUDA](https://img.shields.io/badge/CUDA-11.8+-76B900?style=for-the-badge&logo=nvidia&logoColor=white)](https://developer.nvidia.com/cuda-toolkit)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)]()

<br/>

[![Stars](https://img.shields.io/github/stars/osamashabih6960/The-Journey-of-PyTorch-An-In-Depth-Overview?style=social)](https://github.com/osamashabih6960/The-Journey-of-PyTorch-An-In-Depth-Overview/stargazers)
[![Forks](https://img.shields.io/github/forks/osamashabih6960/The-Journey-of-PyTorch-An-In-Depth-Overview?style=social)](https://github.com/osamashabih6960/The-Journey-of-PyTorch-An-In-Depth-Overview/network/members)
[![Watchers](https://img.shields.io/github/watchers/osamashabih6960/The-Journey-of-PyTorch-An-In-Depth-Overview?style=social)](https://github.com/osamashabih6960/The-Journey-of-PyTorch-An-In-Depth-Overview/watchers)

<br/>

> *"The best way to understand deep learning is to build it — one tensor at a time."*

<br/>

<img src="https://avatars.githubusercontent.com/osamashabih6960" width="72" style="border-radius:50%"/>

**👨‍💻 Osama Shabih**
🎓 Jamia Hamdard University &nbsp;·&nbsp; 🔬 Deep Learning Researcher &nbsp;·&nbsp; 🔥 PyTorch Enthusiast

</div>

---

## 🧭 Table of Contents

| # | Topic |
|---|-------|
| 01 | [🌟 Introduction](#01--introduction) |
| 02 | [⚙️ Installation & Setup](#02--installation--setup) |
| 03 | [🧱 Tensors — The Core](#03--tensors--the-core) |
| 04 | [🔁 Autograd & Computational Graphs](#04--autograd--computational-graphs) |
| 05 | [🏗️ Building Neural Networks](#05--building-neural-networks) |
| 06 | [📉 Loss Functions](#06--loss-functions) |
| 07 | [⚡ Optimizers & Schedulers](#07--optimizers--schedulers) |
| 08 | [🔄 The Complete Training Loop](#08--the-complete-training-loop) |
| 09 | [📦 Datasets & DataLoaders](#09--datasets--dataloaders) |
| 10 | [🧠 CNN · RNN · Transformers](#10--cnn--rnn--transformers) |
| 11 | [🔀 Transfer Learning](#11--transfer-learning) |
| 12 | [💾 Save & Load Models](#12--save--load-models) |
| 13 | [🚀 GPU Acceleration](#13--gpu-acceleration) |
| 14 | [📦 Deployment](#14--torchscript--deployment) |
| 15 | [✅ Best Practices & Checklist](#15--best-practices--checklist) |

---

## 01 · Introduction

PyTorch is an **open-source deep learning framework** developed by Meta AI (FAIR), powering everything from cutting-edge research to production AI at massive scale.

### 🗺️ Evolution Timeline

```
  2002          2011          2016          2023
   │             │             │             │
 Torch  ──►  Torch7  ──►  PyTorch  ──►  PyTorch 2.x
  (C)         (Lua)        (Python)    (torch.compile)
```

### ✅ Why PyTorch?

- **Dynamic computation graphs** (define-by-run) — debug with `print()` and `pdb`
- **Fully Pythonic** — no graph compilation step required
- **Dominant in research** — ~75% of ML papers use PyTorch
- **Production-ready** — TorchScript + ONNX + `torch.compile`
- **PyTorch 2.x** delivers up to **2× speedup** via kernel fusion (TorchInductor)

### PyTorch vs TensorFlow

| Feature | PyTorch | TensorFlow |
|:--------|:-------:|:----------:|
| Graph Type | Dynamic (eager) | Static + Eager |
| Debugging | ✅ Pythonic | ⚠️ Complex |
| Research Adoption | ⭐ Dominant | Popular |
| Mobile/Edge | TorchMobile | TFLite |
| Compiler | ✅ `torch.compile` (2.x) | ✅ XLA |

---

## 02 · Installation & Setup

**pip — CPU only**
```bash
pip install torch torchvision torchaudio
```

**pip — CUDA 11.8**
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

**pip — CUDA 12.1**
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

**conda — CUDA 12.1**
```bash
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

**Verify (Google Colab / local)**
```python
import torch
print(torch.__version__)          # e.g. 2.3.0+cu121
print(torch.cuda.is_available())  # True if GPU available
```

---

## 03 · Tensors — The Core

Multi-dimensional arrays that can live on CPU or GPU. Everything in PyTorch is a tensor.

```python
import torch

# ── Creation ──────────────────────────────────────────────────────
x   = torch.tensor([[1.0, 2.0], [3.0, 4.0]])   # from list
z   = torch.zeros(3, 4)                          # all zeros
o   = torch.ones(3, 4)                           # all ones
r   = torch.rand(3, 4)                           # uniform [0,1)
rn  = torch.randn(3, 4)                          # N(0,1) normal
ar  = torch.arange(0, 10, 2)                     # [0, 2, 4, 6, 8]
ls  = torch.linspace(0, 1, 5)                    # [0.0, 0.25 ... 1.0]
eye = torch.eye(3)                               # 3×3 identity

# ── Properties ────────────────────────────────────────────────────
print(x.shape)     # torch.Size([2, 2])
print(x.dtype)     # torch.float32
print(x.device)    # cpu
print(x.ndim)      # 2

# ── Operations ────────────────────────────────────────────────────
a = torch.randn(2, 3)
b = torch.randn(3, 4)

c = a @ b                        # matmul → (2, 4)
d = a + 1                        # broadcast scalar
e = torch.cat([a, a], dim=0)     # concat rows → (4, 3)
f = torch.stack([a, a], dim=0)   # new dim    → (2, 2, 3)

# ── Reshape & View ────────────────────────────────────────────────
t  = torch.arange(12)
t2 = t.view(3, 4)           # same memory, new shape
t3 = t.reshape(2, 6)        # may copy if non-contiguous
t4 = t2.permute(1, 0)       # transpose → (4, 3)
t5 = t2.unsqueeze(0)        # add batch dim → (1, 3, 4)
```

> 💡 **Tip:** Always check `.shape` after every operation. 90% of PyTorch bugs are shape mismatches — especially the batch dimension!

---

## 04 · Autograd & Computational Graphs

PyTorch builds a **dynamic computation graph** on every forward pass and computes gradients via reverse-mode autodiff.

```python
import torch

# ── Basic gradient ────────────────────────────────────────────────
x = torch.tensor([2.0], requires_grad=True)
y = x ** 3 + 2 * x           # y = x³ + 2x

y.backward()                  # compute dy/dx
print(x.grad)                 # tensor([14.])  →  dy/dx = 3x² + 2 = 14

# ── No-grad context (inference / eval) ───────────────────────────
with torch.no_grad():
    y_eval = x ** 2           # graph NOT built → faster + less VRAM

# ── CRITICAL — zero grads every iteration ─────────────────────────
optimizer.zero_grad()         # without this, gradients ACCUMULATE!
```

> ⚠️ **Critical:** Always call `optimizer.zero_grad()` at the start of every training iteration. Without this, gradients accumulate across batches and your model will diverge.

---

## 05 · Building Neural Networks

```python
import torch.nn as nn
import torch.nn.functional as F

class DeepNet(nn.Module):
    def __init__(self, input_dim, hidden_dim, output_dim, dropout=0.4):
        super().__init__()
        self.fc1  = nn.Linear(input_dim, hidden_dim)
        self.bn1  = nn.BatchNorm1d(hidden_dim)
        self.fc2  = nn.Linear(hidden_dim, hidden_dim // 2)
        self.bn2  = nn.BatchNorm1d(hidden_dim // 2)
        self.fc3  = nn.Linear(hidden_dim // 2, output_dim)
        self.drop = nn.Dropout(p=dropout)

    def forward(self, x):
        x = self.drop(F.relu(self.bn1(self.fc1(x))))
        x = self.drop(F.relu(self.bn2(self.fc2(x))))
        return self.fc3(x)    # raw logits — no softmax here

model = DeepNet(784, 256, 10)
trainable = sum(p.numel() for p in model.parameters() if p.requires_grad)
print(f"Trainable params: {trainable:,}")
```

### 📌 Common Layers Reference

| Layer | Purpose |
|:------|:--------|
| `nn.Linear(in, out)` | Fully connected / dense layer |
| `nn.Conv2d(in, out, k)` | 2D spatial convolution |
| `nn.BatchNorm1d/2d` | Batch normalisation |
| `nn.LayerNorm(dim)` | Layer normalisation |
| `nn.Dropout(p)` | Regularisation via random zeroing |
| `nn.Embedding(vocab, dim)` | Token / word embeddings |
| `nn.LSTM(in, hidden)` | Recurrent sequence layer |
| `nn.MultiheadAttention` | Self-attention mechanism |
| `nn.TransformerEncoderLayer` | Full transformer block |

---

## 06 · Loss Functions

```python
import torch.nn as nn

# Multi-class classification  (input = raw logits, NOT softmax)
criterion = nn.CrossEntropyLoss(label_smoothing=0.1)

# Binary classification  (sigmoid applied internally)
criterion = nn.BCEWithLogitsLoss(pos_weight=torch.tensor([2.0]))

# Regression
criterion = nn.MSELoss()        # L2 — sensitive to outliers
criterion = nn.L1Loss()         # MAE — robust to outliers
criterion = nn.SmoothL1Loss()   # Huber — best of both

# Metric learning
criterion = nn.TripletMarginLoss(margin=1.0)

# Usage
logits = model(x_batch)                # (B, num_classes)
loss   = criterion(logits, y_batch)    # y_batch = class indices (Long)
print(f"Loss: {loss.item():.4f}")
```

---

## 07 · Optimizers & Schedulers

```python
import torch.optim as optim

# SGD + Nesterov momentum — great for CNNs
opt = optim.SGD(model.parameters(), lr=0.01,
                momentum=0.9, weight_decay=1e-4, nesterov=True)

# Adam — robust default for most tasks
opt = optim.Adam(model.parameters(), lr=1e-3, betas=(0.9, 0.999))

# AdamW — decoupled weight decay (recommended for Transformers) ⭐
opt = optim.AdamW(model.parameters(), lr=1e-3, weight_decay=0.01)

# ── Schedulers ────────────────────────────────────────────────────

# Cosine annealing — smooth decay (most popular)
scheduler = optim.lr_scheduler.CosineAnnealingLR(opt, T_max=50)

# OneCycle — warmup + cosine decay in one shot (fastest convergence)
scheduler = optim.lr_scheduler.OneCycleLR(
    opt, max_lr=1e-2,
    steps_per_epoch=len(train_loader), epochs=30
)

# Reduce on plateau — adaptive decay when val loss stagnates
scheduler = optim.lr_scheduler.ReduceLROnPlateau(
    opt, mode='min', factor=0.5, patience=5, verbose=True
)
```

---

## 08 · The Complete Training Loop

> ⚠️ **Critical:** Call `model.train()` before training and `model.eval()` before validation — this controls BatchNorm and Dropout behaviour.

```python
def train_one_epoch(model, loader, criterion, optimizer, device, scaler=None):
    model.train()
    total_loss, correct, total = 0.0, 0, 0

    for X, y in loader:
        X, y = X.to(device), y.to(device)

        optimizer.zero_grad()                              # 1️⃣  Clear gradients

        with autocast():
            logits = model(X)                              # 2️⃣  Forward pass
            loss   = criterion(logits, y)                  # 3️⃣  Compute loss
        scaler.scale(loss).backward()                      # 4️⃣  Backprop (scaled)
        scaler.unscale_(optimizer)
        torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
        scaler.step(optimizer)                             # 5️⃣  Update weights
        scaler.update()

        total_loss += loss.item() * X.size(0)
        correct    += (logits.argmax(1) == y).sum().item()
        total      += X.size(0)

    return total_loss / total, correct / total


# ── Main Loop ─────────────────────────────────────────────────────
for epoch in range(1, 51):
    tr_loss, tr_acc = train_one_epoch(model, train_loader, criterion, optimizer, device, scaler)
    va_loss, va_acc = evaluate(model, val_loader, criterion, device)
    scheduler.step()

    print(f"Epoch [{epoch:02d}/50]  Train → loss: {tr_loss:.4f}  acc: {tr_acc:.3f}")

    if va_acc > best_val_acc:
        best_val_acc = va_acc
        torch.save(model.state_dict(), "best_model.pth")
        print(f"  ✅ New best saved  (val_acc = {best_val_acc:.4f})")
```

---

## 09 · Datasets & DataLoaders

```python
from torch.utils.data import Dataset, DataLoader

class TabularDataset(Dataset):
    def __init__(self, X, y):
        self.X = torch.tensor(X, dtype=torch.float32)
        self.y = torch.tensor(y, dtype=torch.long)

    def __len__(self):           return len(self.X)
    def __getitem__(self, idx):  return self.X[idx], self.y[idx]

# ── DataLoader (Optimal Settings) ─────────────────────────────────
train_loader = DataLoader(
    train_ds,
    batch_size=64,
    shuffle=True,
    num_workers=4,          # parallel CPU loading
    pin_memory=True,        # faster CPU→GPU transfer
    persistent_workers=True # keep workers alive between epochs
)
```

---

## 10 · CNN · RNN · Transformers

<details>
<summary>🖼️ CNN — Image Classification</summary>

```python
class ConvNet(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.features = nn.Sequential(
            nn.Conv2d(3, 32, 3, padding=1), nn.BatchNorm2d(32), nn.ReLU(True), nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 3, padding=1), nn.BatchNorm2d(64), nn.ReLU(True), nn.MaxPool2d(2),
            nn.AdaptiveAvgPool2d((4, 4)),
        )
        self.classifier = nn.Sequential(
            nn.Flatten(),
            nn.Linear(64*4*4, 512), nn.ReLU(True), nn.Dropout(0.5),
            nn.Linear(512, num_classes),
        )

    def forward(self, x):
        return self.classifier(self.features(x))
```

</details>

<details>
<summary>📝 LSTM — Sequence Classification</summary>

```python
class LSTMClassifier(nn.Module):
    def __init__(self, vocab_size, embed_dim, hidden, n_layers, n_class):
        super().__init__()
        self.emb  = nn.Embedding(vocab_size, embed_dim, padding_idx=0)
        self.lstm = nn.LSTM(embed_dim, hidden, n_layers,
                            batch_first=True, dropout=0.3, bidirectional=True)
        self.fc   = nn.Linear(hidden * 2, n_class)

    def forward(self, x):
        out, _ = self.lstm(self.emb(x))
        return self.fc(out.mean(dim=1))
```

</details>

<details>
<summary>🤖 Transformer — Text Classification</summary>

```python
class TransformerClassifier(nn.Module):
    def __init__(self, vocab_size, d_model=512, nhead=8,
                 num_layers=6, num_classes=10, max_len=512):
        super().__init__()
        self.emb     = nn.Embedding(vocab_size, d_model)
        self.pos     = nn.Embedding(max_len, d_model)
        enc_layer    = nn.TransformerEncoderLayer(
            d_model, nhead, dim_feedforward=2048, dropout=0.1, batch_first=True
        )
        self.encoder = nn.TransformerEncoder(enc_layer, num_layers)
        self.head    = nn.Linear(d_model, num_classes)

    def forward(self, x):
        pos = torch.arange(x.size(1), device=x.device).unsqueeze(0)
        x   = self.emb(x) + self.pos(pos)
        return self.head(self.encoder(x)[:, 0])   # CLS token
```

</details>

---

## 11 · Transfer Learning

Fine-tuning pretrained **ResNet-50** with differential learning rates.

```python
import torchvision.models as models

# Load pretrained ResNet-50
backbone = models.resnet50(weights=models.ResNet50_Weights.DEFAULT)

# Step 1 — Freeze ALL pretrained weights
for p in backbone.parameters(): p.requires_grad = False

# Step 2 — Replace classifier head
backbone.fc = nn.Sequential(
    nn.Dropout(0.5),
    nn.Linear(backbone.fc.in_features, 256), nn.ReLU(),
    nn.Linear(256, NUM_CLASSES),
)

# Step 3 — Unfreeze top layers for fine-tuning
for p in backbone.layer4.parameters(): p.requires_grad = True

# Step 4 — Differential learning rates
optimizer = torch.optim.AdamW([
    {"params": backbone.layer3.parameters(), "lr": 1e-5},
    {"params": backbone.layer4.parameters(), "lr": 5e-5},
    {"params": backbone.fc.parameters(),     "lr": 1e-3},
], weight_decay=0.01)
```

> 🔥 **Osama's Rule:** Always use differential learning rates — lower LR for pretrained layers, higher LR for the new head. This is the #1 trick for fast fine-tuning!

---

## 12 · Save & Load Models

```python
# ── Recommended: weights only ─────────────────────────────────────
torch.save(model.state_dict(), "weights.pth")
model.load_state_dict(torch.load("weights.pth", map_location=device))

# ── Full checkpoint (for resuming training) ───────────────────────
torch.save({
    "epoch"     : epoch,
    "model"     : model.state_dict(),
    "optimizer" : optimizer.state_dict(),
    "scheduler" : scheduler.state_dict(),
    "scaler"    : scaler.state_dict(),
    "best_acc"  : best_val_acc,
}, "checkpoint.pt")

# ── Restore ────────────────────────────────────────────────────────
ckpt = torch.load("checkpoint.pt", map_location=device)
model.load_state_dict(ckpt["model"])
start_epoch = ckpt["epoch"] + 1
```

---

## 13 · GPU Acceleration

```python
# ── Device setup ──────────────────────────────────────────────────
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model  = model.to(device)

# ── Automatic Mixed Precision (AMP) — ~2× speedup ─────────────────
from torch.cuda.amp import autocast, GradScaler
scaler = GradScaler()

with autocast():
    logits = model(X)
    loss   = criterion(logits, y)

scaler.scale(loss).backward()
scaler.step(optimizer); scaler.update()

# ── Multi-GPU ─────────────────────────────────────────────────────
if torch.cuda.device_count() > 1:
    model = nn.DataParallel(model)

# ── torch.compile — PyTorch 2.x (up to 2× faster) ────────────────
model = torch.compile(model)    # ← single line, massive gains
```

---

## 14 · TorchScript & Deployment

**FastAPI Inference Server**

```python
from fastapi import FastAPI
from pydantic import BaseModel

app   = FastAPI(title="PyTorch Model API")
model = torch.jit.load("model_scripted.pt").to(device).eval()

class Request(BaseModel):
    features: list[float]

@app.post("/predict")
async def predict(data: Request):
    x = torch.tensor(data.features).unsqueeze(0).to(device)
    with torch.no_grad():
        logits = model(x)
        probs  = torch.softmax(logits, dim=1)
    return {"class": logits.argmax().item(),
            "confidence": round(probs.max().item(), 4)}

# Run: uvicorn app:app --host 0.0.0.0 --port 8000
```

---

## 15 · Best Practices & Checklist

**Paste this at the TOP of every script:**

```python
import torch, random, numpy as np

def set_seed(seed: int = 42):
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    np.random.seed(seed)
    random.seed(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark     = False

set_seed(42)
```

### ✅ Pre-Training Checklist

- [ ] `set_seed(42)` called at top of script
- [ ] `model.train()` / `model.eval()` used correctly
- [ ] `optimizer.zero_grad()` at start of every iteration
- [ ] Gradient clipping (`clip_grad_norm_`, max=1.0)
- [ ] Mixed precision (`GradScaler` + `autocast`) enabled
- [ ] `pin_memory=True` + `num_workers > 0` in DataLoader
- [ ] Checkpoint saved on best validation metric
- [ ] LR scheduler stepped at end of each epoch
- [ ] `torch.no_grad()` used during inference

---

## 📚 Resources

| Resource | Link |
|:---------|:-----|
| 📖 Official Docs | [pytorch.org/docs](https://pytorch.org/docs) |
| 🎓 Tutorials | [pytorch.org/tutorials](https://pytorch.org/tutorials) |
| 🤗 HuggingFace | [huggingface.co](https://huggingface.co) |
| 📄 Papers With Code | [paperswithcode.com](https://paperswithcode.com) |
| 💬 PyTorch Forum | [discuss.pytorch.org](https://discuss.pytorch.org) |

---

## 🤝 Contributing

1. **Fork** this repository
2. **Create** a branch: `git checkout -b feature/your-topic`
3. **Commit** your changes: `git commit -m "Add: topic"`
4. **Push**: `git push origin feature/your-topic`
5. **Open** a Pull Request

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

```
███████╗██╗██████╗ ███████╗
██╔════╝██║██╔══██╗██╔════╝
█████╗  ██║██████╔╝█████╗
██╔══╝  ██║██╔══██╗██╔══╝
██║     ██║██║  ██║███████╗
╚═╝     ╚═╝╚═╝  ╚═╝╚══════╝
```

**Made with ❤️ and 🔥 by [Osama Shabih](https://github.com/osamashabih6960)**

*Jamia Hamdard University — Deep Learning Research · MIT License © 2024*

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:EE4C2C,50:FF6B35,100:f5a623&height=100&section=footer" width="100%"/>

</div>