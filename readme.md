# 🔥 The Journey of PyTorch — An In-Depth Overview

![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)
![CUDA](https://img.shields.io/badge/CUDA-11.8%2B-76B900?style=for-the-badge&logo=nvidia&logoColor=white)

> **An end-to-end guide to mastering PyTorch — from Tensors to Production Deployment**

---

### 👤 Author

| Field | Details |
|---|---|
| **Name** | Osama Shabih |
| **University** | Jamia Hamdard University |
| **Domain** | Deep Learning & AI Research |
| **Framework** | PyTorch 2.x |

---

## 📋 Table of Contents

1. [Introduction](#1-introduction)
2. [Installation & Setup](#2-installation--setup)
3. [Tensors — The Core](#3-tensors--the-core)
4. [Autograd & Computational Graphs](#4-autograd--computational-graphs)
5. [Building Neural Networks](#5-building-neural-networks)
6. [Loss Functions](#6-loss-functions)
7. [Optimizers](#7-optimizers)
8. [The Complete Training Loop](#8-the-complete-training-loop)
9. [Datasets & DataLoaders](#9-datasets--dataloaders)
10. [CNN, RNN & Transformers](#10-cnn-rnn--transformers)
11. [Transfer Learning](#11-transfer-learning)
12. [Save & Load Models](#12-save--load-models)
13. [GPU Acceleration](#13-gpu-acceleration)
14. [TorchScript & Deployment](#14-torchscript--deployment)
15. [Best Practices & Checklist](#15-best-practices--checklist)

---

## 1. Introduction

PyTorch is an open-source deep learning framework originally developed by **Meta AI (FAIR)**. It offers:

- **Dynamic computation graphs** — define-by-run, fully Pythonic
- **Strong GPU acceleration** — via CUDA, ROCm, and Apple MPS
- **Huge ecosystem** — torchvision, torchaudio, torchtext, HuggingFace
- **Production ready** — TorchScript, ONNX, torch.compile (2.x)

### Why PyTorch over TensorFlow?

| Feature | PyTorch | TensorFlow |
|---|---|---|
| Graph type | Dynamic (eager) | Static + Eager |
| Debugging | Pythonic, easy | More complex |
| Research adoption | ⭐ Dominant | Used in production |
| Mobile/Edge | TorchMobile | TF Lite |
| Community | Growing fast | Established |

### PyTorch 2.x — What's New?

```python
# torch.compile() — up to 2x speedup via kernel fusion
model = torch.compile(model)   # single line, massive gains
```

---

## 2. Installation & Setup

### via pip

```bash
# CPU only
pip install torch torchvision torchaudio

# CUDA 11.8
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# CUDA 12.1
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

### via conda

```bash
# CPU
conda install pytorch torchvision torchaudio cpuonly -c pytorch

# CUDA 12.1
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

### Verify Installation

```python
import torch
print(torch.__version__)           # e.g. 2.3.0+cu121
print(torch.cuda.is_available())   # True if GPU present
print(torch.cuda.get_device_name(0))
```

---

## 3. Tensors — The Core

Tensors are the fundamental data structure — multi-dimensional arrays that live on CPU or GPU.

```python
import torch

# ── Creation ──────────────────────────────────────────────────────
x  = torch.tensor([[1.0, 2.0], [3.0, 4.0]])   # from Python list
z  = torch.zeros(3, 4)                          # all zeros
o  = torch.ones(3, 4)                           # all ones
r  = torch.rand(3, 4)                           # uniform [0, 1)
rn = torch.randn(3, 4)                          # standard normal N(0,1)
ar = torch.arange(0, 10, 2)                     # [0, 2, 4, 6, 8]
ls = torch.linspace(0, 1, 5)                    # [0.0, 0.25, 0.5, 0.75, 1.0]
eye= torch.eye(3)                               # 3x3 identity matrix

# ── Properties ────────────────────────────────────────────────────
print(x.shape)    # torch.Size([2, 2])
print(x.dtype)    # torch.float32
print(x.device)   # cpu
print(x.ndim)     # 2

# ── Type Casting ──────────────────────────────────────────────────
x_int   = x.to(torch.int32)
x_half  = x.to(torch.float16)   # for mixed precision
x_bool  = x.bool()

# ── Arithmetic Operations ─────────────────────────────────────────
a = torch.randn(2, 3)
b = torch.randn(3, 4)

c = a @ b                         # matrix multiply → (2, 4)
d = a + 1                         # broadcast scalar add
e = torch.cat([a, a], dim=0)      # concat along rows → (4, 3)
f = torch.stack([a, a], dim=0)    # new dim → (2, 2, 3)

# Element-wise ops
g = torch.mul(a, a)               # same as a * a
h = torch.exp(a)
i = torch.sqrt(torch.abs(a))

# Reduction ops
print(a.sum())          # scalar
print(a.mean(dim=1))    # mean across cols
print(a.max())
print(a.argmax(dim=1))  # index of max per row

# ── Reshape & View ────────────────────────────────────────────────
t  = torch.arange(12)
t2 = t.view(3, 4)          # same memory
t3 = t.reshape(2, 6)       # may copy
t4 = t2.permute(1, 0)      # transpose → (4, 3)
t5 = t2.unsqueeze(0)       # add batch dim → (1, 3, 4)
t6 = t5.squeeze(0)         # remove dim of size 1

# ── Numpy Bridge ─────────────────────────────────────────────────
import numpy as np
arr = np.array([1.0, 2.0, 3.0])
t   = torch.from_numpy(arr)        # shares memory!
arr2= t.numpy()                    # back to numpy (CPU only)
```

> 💡 **Tip (Osama):** Always check `.shape` after every operation. Most PyTorch bugs are shape mismatches — especially the batch dimension!

---

## 4. Autograd & Computational Graphs

PyTorch builds a dynamic computation graph and computes gradients automatically via **reverse-mode autodiff**.

```python
import torch

# requires_grad=True → track all operations on this tensor
x = torch.tensor([2.0], requires_grad=True)
y = x ** 3 + 2 * x          # y = x³ + 2x

y.backward()                 # compute dy/dx
print(x.grad)                # tensor([14.]) — dy/dx = 3x² + 2 = 14

# ── Higher-order gradients ────────────────────────────────────────
x  = torch.tensor([3.0], requires_grad=True)
y  = x ** 4
dy = torch.autograd.grad(y, x, create_graph=True)[0]   # 4x³
d2y= torch.autograd.grad(dy, x)[0]                     # 12x²
print(d2y)   # tensor([108.])

# ── No-grad context (inference / eval mode) ───────────────────────
with torch.no_grad():
    y_eval = x ** 2    # graph NOT built → faster + less memory

# ── Detach from graph ─────────────────────────────────────────────
y_detached = y.detach()          # new tensor, no grad
y_np       = y.detach().numpy()  # safe numpy conversion

# ── Zero gradients (CRITICAL — do this every iteration!) ─────────
optimizer.zero_grad()    # without this, grads accumulate across batches!
```

---

## 5. Building Neural Networks

### Method 1 — `nn.Sequential` (for simple stacks)

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

### Method 2 — `nn.Module` (recommended, full flexibility)

```python
import torch
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

# Instantiate
model = DeepNet(784, 256, 10)
print(model)

# Count parameters
total   = sum(p.numel() for p in model.parameters())
trainable = sum(p.numel() for p in model.parameters() if p.requires_grad)
print(f"Total params:     {total:,}")
print(f"Trainable params: {trainable:,}")
```

### Common Layers Reference

| Layer | Usage |
|---|---|
| `nn.Linear(in, out)` | Fully connected |
| `nn.Conv2d(in, out, k)` | 2D Convolution |
| `nn.BatchNorm1d/2d` | Batch normalisation |
| `nn.LayerNorm(dim)` | Layer normalisation |
| `nn.Dropout(p)` | Regularisation |
| `nn.Embedding(vocab, dim)` | Token embeddings |
| `nn.LSTM(in, hidden)` | Recurrent layer |
| `nn.MultiheadAttention` | Self-attention |
| `nn.TransformerEncoderLayer` | Full transformer block |

---

## 6. Loss Functions

```python
import torch.nn as nn

# Multi-class classification — raw logits input, no softmax needed
criterion = nn.CrossEntropyLoss(label_smoothing=0.1)

# Binary classification — sigmoid applied internally
criterion = nn.BCEWithLogitsLoss(pos_weight=torch.tensor([2.0]))

# Regression
criterion = nn.MSELoss()          # Mean Squared Error
criterion = nn.L1Loss()           # Mean Absolute Error
criterion = nn.SmoothL1Loss()     # Huber loss (robust to outliers)

# Metric learning
criterion = nn.TripletMarginLoss(margin=1.0)

# Usage
logits = model(x_batch)               # shape (B, num_classes)
loss   = criterion(logits, y_batch)   # y_batch: class indices (Long)
print(loss.item())                     # scalar float
```

---

## 7. Optimizers

```python
import torch.optim as optim

# SGD with momentum — best for CNNs with careful tuning
opt_sgd   = optim.SGD(model.parameters(), lr=0.01,
                      momentum=0.9, weight_decay=1e-4, nesterov=True)

# Adam — robust default for most tasks
opt_adam  = optim.Adam(model.parameters(), lr=1e-3,
                       betas=(0.9, 0.999), eps=1e-8)

# AdamW — decoupled weight decay (recommended for Transformers)
opt_adamw = optim.AdamW(model.parameters(), lr=1e-3, weight_decay=0.01)

# ── Learning Rate Schedulers ──────────────────────────────────────

# Step decay: multiply LR by gamma every step_size epochs
scheduler = optim.lr_scheduler.StepLR(opt_adamw, step_size=10, gamma=0.5)

# Cosine annealing — smooth decay to zero then restart
scheduler = optim.lr_scheduler.CosineAnnealingLR(opt_adamw, T_max=50)

# OneCycle — warmup + decay in one cycle (great for fast training)
scheduler = optim.lr_scheduler.OneCycleLR(
    opt_adamw, max_lr=1e-2,
    steps_per_epoch=len(train_loader), epochs=30
)

# Reduce on plateau — reduce LR when val loss stops improving
scheduler = optim.lr_scheduler.ReduceLROnPlateau(
    opt_adamw, mode='min', factor=0.5, patience=5
)
```

---

## 8. The Complete Training Loop

> ⚠️ Always call `model.train()` before training and `model.eval()` before validation — this switches BatchNorm and Dropout behaviour.

```python
import torch

def train_one_epoch(model, loader, criterion, optimizer, device, scaler=None):
    model.train()
    total_loss, correct, total = 0.0, 0, 0

    for X, y in loader:
        X, y = X.to(device), y.to(device)

        optimizer.zero_grad()                            # 1. Clear gradients

        if scaler:                                       # Mixed precision path
            from torch.cuda.amp import autocast
            with autocast():
                logits = model(X)                        # 2. Forward pass
                loss   = criterion(logits, y)            # 3. Compute loss
            scaler.scale(loss).backward()                # 4. Backprop (scaled)
            scaler.unscale_(optimizer)
            torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
            scaler.step(optimizer)                       # 5. Update weights
            scaler.update()
        else:
            logits = model(X)
            loss   = criterion(logits, y)
            loss.backward()
            torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
            optimizer.step()

        total_loss += loss.item() * X.size(0)
        preds   = logits.argmax(dim=1)
        correct += (preds == y).sum().item()
        total   += X.size(0)

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
            correct    += (logits.argmax(dim=1) == y).sum().item()
            total      += X.size(0)

    return total_loss / total, correct / total


# ── Main Loop ──────────────────────────────────────────────────────
device    = "cuda" if torch.cuda.is_available() else "cpu"
model     = model.to(device)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.AdamW(model.parameters(), lr=1e-3)
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=50)
scaler    = torch.cuda.amp.GradScaler()    # for mixed precision

best_val_acc = 0.0

for epoch in range(1, 51):
    tr_loss, tr_acc = train_one_epoch(model, train_loader, criterion,
                                      optimizer, device, scaler)
    va_loss, va_acc = evaluate(model, val_loader, criterion, device)
    scheduler.step()

    print(f"Epoch {epoch:02d} | "
          f"Train Loss: {tr_loss:.4f}  Acc: {tr_acc:.3f} | "
          f"Val Loss:   {va_loss:.4f}  Acc: {va_acc:.3f}")

    if va_acc > best_val_acc:
        best_val_acc = va_acc
        torch.save(model.state_dict(), "best_model.pth")
        print(f"  ✅ Saved best model (val_acc={best_val_acc:.4f})")
```

---

## 9. Datasets & DataLoaders

### Custom Dataset

```python
from torch.utils.data import Dataset, DataLoader, random_split

class TabularDataset(Dataset):
    def __init__(self, X, y):
        self.X = torch.tensor(X, dtype=torch.float32)
        self.y = torch.tensor(y, dtype=torch.long)

    def __len__(self):
        return len(self.X)

    def __getitem__(self, idx):
        return self.X[idx], self.y[idx]
```

### Transforms (Image Pipeline)

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

### DataLoader

```python
# Load standard dataset
cifar10 = datasets.CIFAR10(root="data", train=True,
                           download=True, transform=train_tfm)

# Train / val split
n_val  = int(0.2 * len(cifar10))
n_train= len(cifar10) - n_val
train_ds, val_ds = random_split(cifar10, [n_train, n_val])

# DataLoader — batching, shuffling, multi-worker loading
train_loader = DataLoader(train_ds, batch_size=64, shuffle=True,
                          num_workers=4, pin_memory=True,
                          persistent_workers=True)
val_loader   = DataLoader(val_ds,   batch_size=128, shuffle=False,
                          num_workers=4, pin_memory=True)
```

---

## 10. CNN, RNN & Transformers

### Convolutional Neural Network (CNN)

```python
import torch.nn as nn

class ConvNet(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.features = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=3, padding=1),
            nn.BatchNorm2d(32),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2),                               # /2

            nn.Conv2d(32, 64, kernel_size=3, padding=1),
            nn.BatchNorm2d(64),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2),                               # /2

            nn.Conv2d(64, 128, kernel_size=3, padding=1),
            nn.BatchNorm2d(128),
            nn.ReLU(inplace=True),
            nn.AdaptiveAvgPool2d((4, 4)),                  # any input size
        )
        self.classifier = nn.Sequential(
            nn.Flatten(),
            nn.Linear(128 * 4 * 4, 512),
            nn.ReLU(inplace=True),
            nn.Dropout(0.5),
            nn.Linear(512, num_classes),
        )

    def forward(self, x):
        return self.classifier(self.features(x))
```

### LSTM for Sequences

```python
class LSTMClassifier(nn.Module):
    def __init__(self, vocab_size, embed_dim, hidden, n_layers, n_class):
        super().__init__()
        self.emb  = nn.Embedding(vocab_size, embed_dim, padding_idx=0)
        self.lstm = nn.LSTM(embed_dim, hidden, n_layers,
                            batch_first=True, dropout=0.3,
                            bidirectional=True)
        self.fc   = nn.Linear(hidden * 2, n_class)   # *2 for bidirectional

    def forward(self, x):
        emb, _ = self.lstm(self.emb(x))
        # Mean pool over sequence
        out = emb.mean(dim=1)
        return self.fc(out)
```

### Transformer Encoder

```python
class TransformerClassifier(nn.Module):
    def __init__(self, vocab_size, d_model=512, nhead=8,
                 num_layers=6, num_classes=10, max_len=512):
        super().__init__()
        self.emb = nn.Embedding(vocab_size, d_model)
        self.pos = nn.Embedding(max_len, d_model)

        encoder_layer = nn.TransformerEncoderLayer(
            d_model=d_model, nhead=nhead,
            dim_feedforward=2048, dropout=0.1,
            batch_first=True
        )
        self.encoder  = nn.TransformerEncoder(encoder_layer, num_layers)
        self.head     = nn.Linear(d_model, num_classes)

    def forward(self, x):
        pos = torch.arange(x.size(1), device=x.device).unsqueeze(0)
        x   = self.emb(x) + self.pos(pos)
        x   = self.encoder(x)
        return self.head(x[:, 0])    # CLS token
```

---

## 11. Transfer Learning

```python
import torchvision.models as models

# ── Load pretrained backbone ──────────────────────────────────────
backbone = models.resnet50(weights=models.ResNet50_Weights.DEFAULT)

# ── Freeze ALL layers ─────────────────────────────────────────────
for p in backbone.parameters():
    p.requires_grad = False

# ── Replace classifier head ───────────────────────────────────────
in_features  = backbone.fc.in_features       # 2048
backbone.fc  = nn.Sequential(
    nn.Dropout(0.5),
    nn.Linear(in_features, 256),
    nn.ReLU(),
    nn.Linear(256, NUM_CLASSES),
)

# ── Fine-tune: unfreeze last 2 residual blocks ────────────────────
for p in backbone.layer4.parameters():
    p.requires_grad = True
for p in backbone.layer3.parameters():
    p.requires_grad = True

# ── Differential learning rates (lower LR for pretrained layers) ──
optimizer = torch.optim.AdamW([
    {"params": backbone.layer3.parameters(), "lr": 1e-5},
    {"params": backbone.layer4.parameters(), "lr": 5e-5},
    {"params": backbone.fc.parameters(),     "lr": 1e-3},
], weight_decay=0.01)
```

> ✅ **Osama's Rule:** Always use a lower LR (1e-5 to 1e-4) for pretrained layers vs. a higher LR (1e-3) for the new head. Use param groups as shown above!

---

## 12. Save & Load Models

```python
# ── Recommended: save state_dict only ────────────────────────────
torch.save(model.state_dict(), "model_weights.pth")

model = DeepNet(784, 256, 10)
model.load_state_dict(torch.load("model_weights.pth", map_location=device))
model.eval()

# ── Full checkpoint (resume training from exact state) ────────────
torch.save({
    "epoch"      : epoch,
    "model"      : model.state_dict(),
    "optimizer"  : optimizer.state_dict(),
    "scheduler"  : scheduler.state_dict(),
    "scaler"     : scaler.state_dict(),
    "best_acc"   : best_val_acc,
    "config"     : {"lr": 1e-3, "batch_size": 64},
}, "checkpoint.pt")

# ── Restore from checkpoint ───────────────────────────────────────
ckpt = torch.load("checkpoint.pt", map_location=device)
model.load_state_dict(ckpt["model"])
optimizer.load_state_dict(ckpt["optimizer"])
scheduler.load_state_dict(ckpt["scheduler"])
start_epoch = ckpt["epoch"] + 1
print(f"Resumed from epoch {start_epoch}, best_acc={ckpt['best_acc']:.4f}")
```

---

## 13. GPU Acceleration

```python
import torch

# ── Device setup ──────────────────────────────────────────────────
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(f"Using : {device}")
print(f"CUDA  : {torch.version.cuda}")
print(f"GPU   : {torch.cuda.get_device_name(0)}")
print(f"Memory: {torch.cuda.get_device_properties(0).total_memory / 1e9:.1f} GB")

model = model.to(device)
x     = x.to(device)      # tensors must be on same device as model!

# ── Automatic Mixed Precision (AMP) — ~2x speedup ─────────────────
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

# ── Multi-GPU — DataParallel (simple) ─────────────────────────────
if torch.cuda.device_count() > 1:
    model = nn.DataParallel(model)
    print(f"Using {torch.cuda.device_count()} GPUs")

# ── torch.compile — PyTorch 2.x kernel fusion ─────────────────────
model = torch.compile(model)    # up to 2x faster, one line change!

# ── Memory management ─────────────────────────────────────────────
torch.cuda.empty_cache()            # free unused cached memory
print(f"Allocated: {torch.cuda.memory_allocated()/1e9:.2f} GB")
print(f"Reserved:  {torch.cuda.memory_reserved()/1e9:.2f} GB")
```

---

## 14. TorchScript & Deployment

### TorchScript (serialize for C++ / mobile)

```python
# Method 1: torch.jit.script (handles control flow)
scripted = torch.jit.script(model)
scripted.save("model_scripted.pt")

# Method 2: torch.jit.trace (for fixed input shapes)
example_input = torch.randn(1, 3, 224, 224).to(device)
traced = torch.jit.trace(model, example_input)
traced.save("model_traced.pt")

# Load in Python or C++
loaded = torch.jit.load("model_scripted.pt")
output = loaded(example_input)
```

### ONNX Export

```python
dummy_input = torch.randn(1, 3, 224, 224).to(device)

torch.onnx.export(
    model,
    dummy_input,
    "model.onnx",
    input_names=["input"],
    output_names=["output"],
    dynamic_axes={"input": {0: "batch_size"}},
    opset_version=17,
    export_params=True,
)

# Verify with onnxruntime
import onnxruntime as ort
sess = ort.InferenceSession("model.onnx")
out  = sess.run(None, {"input": dummy_input.cpu().numpy()})
```

### FastAPI Inference Server

```python
from fastapi import FastAPI
from pydantic import BaseModel
import torch

app    = FastAPI(title="PyTorch Model Server")
model  = torch.jit.load("model_scripted.pt").to(device)
model.eval()

class PredictRequest(BaseModel):
    features: list[float]

@app.post("/predict")
async def predict(data: PredictRequest):
    x = torch.tensor(data.features, dtype=torch.float32).unsqueeze(0).to(device)
    with torch.no_grad():
        logits = model(x)
        probs  = torch.softmax(logits, dim=1)
    return {
        "prediction": logits.argmax().item(),
        "confidence": probs.max().item(),
        "probabilities": probs.squeeze().tolist(),
    }

# Run: uvicorn app:app --host 0.0.0.0 --port 8000
```

---

## 15. Best Practices & Checklist

### Reproducibility — Set This at the TOP of Every Script

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

### Training Checklist

- [ ] `set_seed()` called at script start
- [ ] `model.train()` before training loop, `model.eval()` before eval
- [ ] `optimizer.zero_grad()` at start of every iteration
- [ ] Gradient clipping (`clip_grad_norm_`) to prevent exploding gradients
- [ ] Mixed precision (`GradScaler` + `autocast`) on GPU
- [ ] `pin_memory=True` and `num_workers>0` in DataLoader
- [ ] Checkpoint saved on best validation metric
- [ ] LR scheduler stepped at end of each epoch
- [ ] `torch.no_grad()` during inference

### Performance Tips

```python
# 1. Use channels_last memory format for CNNs
model = model.to(memory_format=torch.channels_last)
x     = x.to(memory_format=torch.channels_last)

# 2. Compile model (PyTorch 2.x)
model = torch.compile(model, mode="reduce-overhead")

# 3. Increase DataLoader workers
loader = DataLoader(ds, num_workers=min(8, os.cpu_count()),
                    persistent_workers=True, pin_memory=True)

# 4. Use cudnn benchmark for fixed input sizes
torch.backends.cudnn.benchmark = True   # auto-tunes convolution algorithms

# 5. Profile to find bottlenecks
with torch.profiler.profile(activities=[
    torch.profiler.ProfilerActivity.CPU,
    torch.profiler.ProfilerActivity.CUDA,
]) as prof:
    output = model(x)
print(prof.key_averages().table(sort_by="cuda_time_total", row_limit=10))
```

---

## 📚 References & Resources

| Resource | Link |
|---|---|
| Official PyTorch Docs | https://pytorch.org/docs |
| PyTorch Tutorials | https://pytorch.org/tutorials |
| HuggingFace | https://huggingface.co |
| Papers With Code | https://paperswithcode.com |
| PyTorch Forum | https://discuss.pytorch.org |

---

## 📄 License

```
MIT License — Copyright (c) 2024 Osama Shabih
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files, to deal in the Software
without restriction.
```

---

<div align="center">

**Made with ❤️ by Osama Shabih · Jamia Hamdard University**

*If this helped you, please ⭐ star the repository!*

</div>