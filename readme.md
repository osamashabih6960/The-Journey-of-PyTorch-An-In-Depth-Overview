<div align="center">

<img src="https://raw.githubusercontent.com/pytorch/pytorch/main/docs/source/_static/img/pytorch-logo-dark.png" width="320" alt="PyTorch Logo"/>

<br/>

# 🔥 The Journey of PyTorch
### An In-Depth Overview — From Tensors to Production

<br/>

[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)](https://pytorch.org)
[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![CUDA](https://img.shields.io/badge/CUDA-11.8%2B-76B900?style=for-the-badge&logo=nvidia&logoColor=white)](https://developer.nvidia.com/cuda-toolkit)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-f97316?style=for-the-badge)]()
[![Stars](https://img.shields.io/github/stars/osamashabih6960?style=for-the-badge&color=fbbf24&logo=github)](https://github.com/osamashabih6960)

<br/>

> *"The best way to understand deep learning is to build it — one tensor at a time."*

<br/>

---

### 👨‍💻 Author

<img src="https://avatars.githubusercontent.com/u/osamashabih6960" width="80" style="border-radius:50%" alt="Osama Shabih"/>

**Osama Shabih**
<br/>
🎓 **Jamia Hamdard University** &nbsp;|&nbsp; 🔬 Deep Learning Researcher &nbsp;|&nbsp; 🔥 PyTorch Enthusiast

[![GitHub](https://img.shields.io/badge/GitHub-osamashabih6960-181717?style=flat-square&logo=github)](https://github.com/osamashabih6960)
[![University](https://img.shields.io/badge/Jamia%20Hamdard-University-006400?style=flat-square)](https://jamiahamdard.edu.in)

---

</div>

## 📋 Table of Contents

<details open>
<summary><b>Click to expand / collapse</b></summary>

| # | Section | Description |
|:-:|---------|-------------|
| 01 | [🌟 Introduction](#01-introduction) | What is PyTorch & why use it |
| 02 | [⚙️ Installation & Setup](#02-installation--setup) | pip, conda, Colab |
| 03 | [🧱 Tensors — The Core](#03-tensors--the-core) | Creation, ops, reshaping |
| 04 | [🔁 Autograd & Graphs](#04-autograd--computational-graphs) | Backprop & gradients |
| 05 | [🏗️ Neural Networks](#05-building-neural-networks) | `nn.Module`, layers |
| 06 | [📉 Loss Functions](#06-loss-functions) | Cross-entropy, MSE & more |
| 07 | [⚡ Optimizers](#07-optimizers) | SGD, Adam, AdamW, schedulers |
| 08 | [🔄 Training Loop](#08-the-complete-training-loop) | Full train + eval loop |
| 09 | [📦 Datasets & DataLoaders](#09-datasets--dataloaders) | Custom datasets, transforms |
| 10 | [🧠 CNN · RNN · Transformer](#10-cnn-rnn--transformers) | All 3 architectures |
| 11 | [🔀 Transfer Learning](#11-transfer-learning) | Fine-tuning ResNet50 |
| 12 | [💾 Save & Load](#12-save--load-models) | Checkpoints & state dicts |
| 13 | [🚀 GPU Acceleration](#13-gpu-acceleration) | CUDA, AMP, multi-GPU |
| 14 | [📦 Deployment](#14-torchscript--deployment) | TorchScript, ONNX, FastAPI |
| 15 | [✅ Best Practices](#15-best-practices--checklist) | Checklist & pro tips |

</details>

---

## 01. Introduction

<div align="center">

```
Torch (C, 2002) ──► Torch7 (Lua, 2011) ──► PyTorch (Python, 2016) ──► PyTorch 2.x (2023)
```

</div>

PyTorch is an open-source deep learning framework by **Meta AI (FAIR)**. It powers everything from cutting-edge research papers to production AI at massive scale.

<table>
<tr>
<td width="50%">

**✅ Why PyTorch?**
- Dynamic computation graphs (define-by-run)
- Fully Pythonic — debug with `print()` and `pdb`
- Dominant in academic research (~75% of papers)
- Production-ready: TorchScript + ONNX + `torch.compile`

</td>
<td width="50%">

**⚡ PyTorch 2.x Highlights**
- `torch.compile()` → up to **2× speedup**
- Kernel fusion via TorchInductor
- FlexAttention for custom attention patterns
- Backward-compatible with all 1.x code

</td>
</tr>
</table>

| Feature | PyTorch | TensorFlow |
|---------|:-------:|:----------:|
| Graph type | Dynamic (eager) | Static + Eager |
| Debugging | ✅ Pythonic | ⚠️ Complex |
| Research adoption | ⭐ Dominant | Popular |
| Mobile/Edge | TorchMobile | TFLite |
| torch.compile | ✅ 2.x | ✅ XLA |

---

## 02. Installation & Setup

<details>
<summary>📦 <b>pip (recommended)</b></summary>

```bash
# CPU only
pip install torch torchvision torchaudio

# CUDA 11.8
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# CUDA 12.1
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```
</details>

<details>
<summary>🐍 <b>conda</b></summary>

```bash
# CPU
conda install pytorch torchvision torchaudio cpuonly -c pytorch

# CUDA 12.1
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```
</details>

<details>
<summary>☁️ <b>Google Colab</b></summary>

```python
# PyTorch is pre-installed — just verify:
import torch
print(torch.__version__)           # e.g. 2.3.0+cu121
print(torch.cuda.is_available())   # True if GPU runtime
```
</details>

---

## 03. Tensors — The Core

> 🧱 Tensors are multi-dimensional arrays that can live on CPU or GPU. Everything in PyTorch is a tensor.

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

c = a @ b                         # matmul → (2, 4)
d = a + 1                         # broadcast scalar
e = torch.cat([a, a], dim=0)      # concat rows → (4, 3)
f = torch.stack([a, a], dim=0)    # new dim    → (2, 2, 3)

# Reduction
print(a.sum())            # scalar
print(a.mean(dim=1))      # mean per row
print(a.argmax(dim=1))    # index of max per row

# ── Reshape & View ────────────────────────────────────────────────
t  = torch.arange(12)
t2 = t.view(3, 4)           # same memory, new shape
t3 = t.reshape(2, 6)        # may copy if non-contiguous
t4 = t2.permute(1, 0)       # transpose → (4, 3)
t5 = t2.unsqueeze(0)        # add batch dim → (1, 3, 4)
t6 = t5.squeeze(0)          # remove size-1 dim

# ── NumPy Bridge ─────────────────────────────────────────────────
import numpy as np
arr = np.array([1.0, 2.0, 3.0])
t   = torch.from_numpy(arr)         # shares memory!
np2 = t.numpy()                     # back to numpy (CPU only)
```

> 💡 **Tip by Osama:** Always check `.shape` after every operation. 90% of PyTorch bugs are shape mismatches — especially the batch dimension!

---

## 04. Autograd & Computational Graphs

> 🔁 PyTorch builds a **dynamic computation graph** on every forward pass and computes gradients via reverse-mode autodiff.

```python
import torch

# ── Basic gradient ────────────────────────────────────────────────
x = torch.tensor([2.0], requires_grad=True)
y = x ** 3 + 2 * x          # y = x³ + 2x

y.backward()                 # compute dy/dx
print(x.grad)                # tensor([14.])  →  dy/dx = 3x² + 2 = 14

# ── No-grad context (inference / eval) ───────────────────────────
with torch.no_grad():
    y_eval = x ** 2          # graph NOT built → faster + less VRAM

# ── Detach from graph ─────────────────────────────────────────────
y_np = y.detach().numpy()    # safe NumPy conversion

# ── CRITICAL — zero grads every iteration ─────────────────────────
optimizer.zero_grad()        # without this, gradients ACCUMULATE!
```

---

## 05. Building Neural Networks

### 🔷 Method 1 — `nn.Sequential` (simple stacks)

```python
import torch.nn as nn

model = nn.Sequential(
    nn.Linear(784, 256),
    nn.ReLU(),
    nn.Dropout(0.3),
    nn.Linear(256, 128),
    nn.ReLU(),
    nn.Dropout(0.3),
    nn.Linear(128, 10),
)
```

### 🔷 Method 2 — `nn.Module` *(recommended)*

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

# Count parameters
total     = sum(p.numel() for p in model.parameters())
trainable = sum(p.numel() for p in model.parameters() if p.requires_grad)
print(f"Total:     {total:,}")
print(f"Trainable: {trainable:,}")
```

### 📌 Common Layers Reference

| Layer | Purpose |
|-------|---------|
| `nn.Linear(in, out)` | Fully connected / dense |
| `nn.Conv2d(in, out, k)` | 2D convolution |
| `nn.BatchNorm1d/2d` | Batch normalisation |
| `nn.LayerNorm(dim)` | Layer normalisation |
| `nn.Dropout(p)` | Regularisation |
| `nn.Embedding(vocab, dim)` | Token embeddings |
| `nn.LSTM(in, hidden)` | Recurrent layer |
| `nn.MultiheadAttention` | Self-attention |
| `nn.TransformerEncoderLayer` | Full transformer block |

---

## 06. Loss Functions

```python
import torch.nn as nn

# Multi-class classification  (input = raw logits, NOT softmax)
criterion = nn.CrossEntropyLoss(label_smoothing=0.1)

# Binary classification  (sigmoid applied internally — numerically stable)
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

## 07. Optimizers

```python
import torch.optim as optim

# SGD + Nesterov momentum — great for CNNs with careful tuning
opt = optim.SGD(model.parameters(), lr=0.01,
                momentum=0.9, weight_decay=1e-4, nesterov=True)

# Adam — robust default for most tasks
opt = optim.Adam(model.parameters(), lr=1e-3, betas=(0.9, 0.999))

# AdamW — decoupled weight decay (recommended for Transformers)  ⭐
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

## 08. The Complete Training Loop

> ⚠️ **Critical:** Call `model.train()` before training and `model.eval()` before validation — this controls BatchNorm and Dropout behaviour.

```python
def train_one_epoch(model, loader, criterion, optimizer, device, scaler=None):
    model.train()
    total_loss, correct, total = 0.0, 0, 0

    for X, y in loader:
        X, y = X.to(device), y.to(device)

        optimizer.zero_grad()                              # 1️⃣  Clear gradients

        if scaler:
            from torch.cuda.amp import autocast
            with autocast():
                logits = model(X)                          # 2️⃣  Forward pass
                loss   = criterion(logits, y)              # 3️⃣  Compute loss
            scaler.scale(loss).backward()                  # 4️⃣  Backprop (scaled)
            scaler.unscale_(optimizer)
            torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
            scaler.step(optimizer)                         # 5️⃣  Update weights
            scaler.update()
        else:
            logits = model(X)
            loss   = criterion(logits, y)
            loss.backward()
            torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
            optimizer.step()

        total_loss += loss.item() * X.size(0)
        correct    += (logits.argmax(1) == y).sum().item()
        total      += X.size(0)

    return total_loss / total, correct / total


def evaluate(model, loader, criterion, device):
    model.eval()
    total_loss, correct, total = 0.0, 0, 0

    with torch.no_grad():
        for X, y in loader:
            X, y   = X.to(device), y.to(device)
            logits = model(X)
            loss   = criterion(logits, y)
            total_loss += loss.item() * X.size(0)
            correct    += (logits.argmax(1) == y).sum().item()
            total      += X.size(0)

    return total_loss / total, correct / total


# ── Main Loop ─────────────────────────────────────────────────────
device    = "cuda" if torch.cuda.is_available() else "cpu"
model     = model.to(device)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.AdamW(model.parameters(), lr=1e-3)
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=50)
scaler    = torch.cuda.amp.GradScaler()

best_val_acc = 0.0

for epoch in range(1, 51):
    tr_loss, tr_acc = train_one_epoch(model, train_loader, criterion,
                                      optimizer, device, scaler)
    va_loss, va_acc = evaluate(model, val_loader, criterion, device)
    scheduler.step()

    print(f"Epoch [{epoch:02d}/50]  "
          f"Train → loss: {tr_loss:.4f}  acc: {tr_acc:.3f}  |  "
          f"Val   → loss: {va_loss:.4f}  acc: {va_acc:.3f}")

    if va_acc > best_val_acc:
        best_val_acc = va_acc
        torch.save(model.state_dict(), "best_model.pth")
        print(f"  ✅ New best saved  (val_acc = {best_val_acc:.4f})")
```

---

## 09. Datasets & DataLoaders

### Custom Dataset

```python
from torch.utils.data import Dataset, DataLoader, random_split

class TabularDataset(Dataset):
    def __init__(self, X, y):
        self.X = torch.tensor(X, dtype=torch.float32)
        self.y = torch.tensor(y, dtype=torch.long)

    def __len__(self):            return len(self.X)
    def __getitem__(self, idx):   return self.X[idx], self.y[idx]
```

### Image Transforms

```python
from torchvision import transforms, datasets

train_tfm = transforms.Compose([
    transforms.RandomHorizontalFlip(p=0.5),
    transforms.RandomCrop(32, padding=4),
    transforms.ColorJitter(brightness=0.2, contrast=0.2),
    transforms.ToTensor(),
    transforms.Normalize(mean=(0.485, 0.456, 0.406),
                         std=(0.229, 0.224, 0.225)),
])

val_tfm = transforms.Compose([
    transforms.Resize(256),
    transforms.CenterCrop(224),
    transforms.ToTensor(),
    transforms.Normalize(mean=(0.485, 0.456, 0.406),
                         std=(0.229, 0.224, 0.225)),
])
```

### DataLoader (Optimal Settings)

```python
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

## 10. CNN, RNN & Transformers

<details open>
<summary>🖼️ <b>CNN — Image Classification</b></summary>

```python
class ConvNet(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.features = nn.Sequential(
            nn.Conv2d(3, 32,  3, padding=1), nn.BatchNorm2d(32),  nn.ReLU(True), nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 3, padding=1), nn.BatchNorm2d(64),  nn.ReLU(True), nn.MaxPool2d(2),
            nn.Conv2d(64, 128,3, padding=1), nn.BatchNorm2d(128), nn.ReLU(True),
            nn.AdaptiveAvgPool2d((4, 4)),
        )
        self.classifier = nn.Sequential(
            nn.Flatten(),
            nn.Linear(128*4*4, 512), nn.ReLU(True), nn.Dropout(0.5),
            nn.Linear(512, num_classes),
        )
    def forward(self, x): return self.classifier(self.features(x))
```
</details>

<details>
<summary>📝 <b>LSTM — Sequence Classification</b></summary>

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
        return self.fc(out.mean(dim=1))   # mean pool over sequence
```
</details>

<details>
<summary>🤖 <b>Transformer — Text Classification</b></summary>

```python
class TransformerClassifier(nn.Module):
    def __init__(self, vocab_size, d_model=512, nhead=8,
                 num_layers=6, num_classes=10, max_len=512):
        super().__init__()
        self.emb     = nn.Embedding(vocab_size, d_model)
        self.pos     = nn.Embedding(max_len, d_model)
        enc_layer    = nn.TransformerEncoderLayer(
            d_model, nhead, dim_feedforward=2048,
            dropout=0.1, batch_first=True
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

## 11. Transfer Learning

```python
import torchvision.models as models

# Load pretrained ResNet-50
backbone = models.resnet50(weights=models.ResNet50_Weights.DEFAULT)

# Step 1 — Freeze ALL pretrained weights
for p in backbone.parameters():
    p.requires_grad = False

# Step 2 — Replace classifier head with your task
backbone.fc = nn.Sequential(
    nn.Dropout(0.5),
    nn.Linear(backbone.fc.in_features, 256),
    nn.ReLU(),
    nn.Linear(256, NUM_CLASSES),
)

# Step 3 — Unfreeze top layers for fine-tuning
for p in backbone.layer4.parameters(): p.requires_grad = True
for p in backbone.layer3.parameters(): p.requires_grad = True

# Step 4 — Differential LRs (lower for pretrained, higher for head)
optimizer = torch.optim.AdamW([
    {"params": backbone.layer3.parameters(), "lr": 1e-5},
    {"params": backbone.layer4.parameters(), "lr": 5e-5},
    {"params": backbone.fc.parameters(),     "lr": 1e-3},
], weight_decay=0.01)
```

> ✅ **Osama's Rule:** Always use differential learning rates — lower LR for pretrained layers, higher LR for the new head. This is the #1 trick for fast fine-tuning!

---

## 12. Save & Load Models

```python
# ── Recommended: weights only ─────────────────────────────────────
torch.save(model.state_dict(), "weights.pth")

model = MyModel()
model.load_state_dict(torch.load("weights.pth", map_location=device))
model.eval()

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
optimizer.load_state_dict(ckpt["optimizer"])
start_epoch = ckpt["epoch"] + 1
print(f"Resumed from epoch {start_epoch}  |  best_acc = {ckpt['best_acc']:.4f}")
```

---

## 13. GPU Acceleration

```python
# ── Device setup ──────────────────────────────────────────────────
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(f"Device : {device}")
print(f"GPU    : {torch.cuda.get_device_name(0)}")
print(f"VRAM   : {torch.cuda.get_device_properties(0).total_memory/1e9:.1f} GB")

model = model.to(device)
x     = x.to(device)       # tensors must match model device!

# ── Automatic Mixed Precision (AMP) — ~2× speedup ─────────────────
from torch.cuda.amp import autocast, GradScaler
scaler = GradScaler()

with autocast():
    logits = model(X)
    loss   = criterion(logits, y)

scaler.scale(loss).backward()
scaler.unscale_(optimizer)
torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
scaler.step(optimizer)
scaler.update()

# ── Multi-GPU ─────────────────────────────────────────────────────
if torch.cuda.device_count() > 1:
    model = nn.DataParallel(model)

# ── torch.compile — PyTorch 2.x (up to 2× faster) ────────────────
model = torch.compile(model)    # ← single line, massive gains
```

---

## 14. TorchScript & Deployment

```python
# ── TorchScript (serialize for C++ / mobile) ──────────────────────
scripted = torch.jit.script(model)
scripted.save("model_scripted.pt")

# Load anywhere — Python or C++
loaded = torch.jit.load("model_scripted.pt")
out    = loaded(example_input)

# ── ONNX Export (run on any runtime) ─────────────────────────────
dummy = torch.randn(1, 3, 224, 224).to(device)
torch.onnx.export(model, dummy, "model.onnx",
                  input_names=["input"], output_names=["output"],
                  dynamic_axes={"input": {0: "batch_size"}},
                  opset_version=17)

# ── FastAPI Inference Server ──────────────────────────────────────
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

## 15. Best Practices & Checklist

### 🔒 Reproducibility — Paste at the TOP of Every Script

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

### ⚡ Performance Tips

```python
# Channels-last format for CNNs (memory layout optimisation)
model = model.to(memory_format=torch.channels_last)

# torch.compile for kernel fusion (PyTorch 2.x)
model = torch.compile(model, mode="reduce-overhead")

# cudnn autotuner for fixed input shapes
torch.backends.cudnn.benchmark = True

# Profile to find bottlenecks
with torch.profiler.profile(
    activities=[torch.profiler.ProfilerActivity.CPU,
                torch.profiler.ProfilerActivity.CUDA]
) as prof:
    output = model(x)
print(prof.key_averages().table(sort_by="cuda_time_total", row_limit=10))
```

---

## 📚 Resources

<div align="center">

| Resource | Link |
|----------|------|
| 📖 Official Docs | [pytorch.org/docs](https://pytorch.org/docs) |
| 🎓 Tutorials | [pytorch.org/tutorials](https://pytorch.org/tutorials) |
| 🤗 HuggingFace | [huggingface.co](https://huggingface.co) |
| 📄 Papers With Code | [paperswithcode.com](https://paperswithcode.com) |
| 💬 PyTorch Forum | [discuss.pytorch.org](https://discuss.pytorch.org) |

</div>

---

## 📄 License

```
MIT License — Copyright (c) 2024 Osama Shabih, Jamia Hamdard University
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and using it without restriction, including without limitation
the rights to use, copy, modify, merge, publish, distribute, and sublicense.
```

---

<div align="center">

<br/>

```
███████╗██╗██████╗ ███████╗
██╔════╝██║██╔══██╗██╔════╝
█████╗  ██║██████╔╝█████╗
██╔══╝  ██║██╔══██╗██╔══╝
██║     ██║██║  ██║███████╗
╚═╝     ╚═╝╚═╝  ╚═╝╚══════╝
```

**Made with ❤️ and 🔥 by [Osama Shabih](https://github.com/osamashabih6960)**
<br/>
**Jamia Hamdard University — Deep Learning Research**

<br/>

*If this helped you, please give it a ⭐ — it means a lot!*

<br/>

![visitors](https://visitor-badge.liteflabs.io/badge?page_id=osamashabih6960.pytorch-journey)

</div>
