# Archigraph

> **Pythonic neural network architecture diagramming powered by a high-performance C++ backend.**  
> *As effortless as FastAPI. As fast as C++.*

---

## What is Archigraph?

**Archigraph** is a Python library designed to effortlessly create clear, beautiful, and publication-ready architecture diagrams for Machine Learning and Deep Learning models. 

**Archigraph gives you a clean, Python-native API while delegating graph optimization, layout computation, and rendering to an ultra-fast C++ engine.**

---

## Key Features

- **Intuitive Python API:** Minimal boilerplate with a clean, declarative syntax inspired by FastAPI and modern Python tools.
- **C++ Engine Under the Hood:** Instant graph generation and layout processing using low-level C++ bindings (via `pybind11`).
- **Framework Native:** Automatically extract and render diagrams directly from PyTorch or TensorFlow models, or define custom layers manually.
- **Multiple Export Formats:** Render directly to SVG, high-res PNG, PDF, or interactive HTML files.
- **Automatic Shape & Flow Inference:** Displays tensor shapes, channel counts, and operation types cleanly across layers.

---

## Installation

```bash
pip install archigraph
```

---

## Quick Start

### 1. Pure Python API (Manual Definition)

Creating custom architecture diagrams requires minimal, expressive Python code:

```python
from archigraph import Graph, Conv2D, BatchNorm, ReLU, Linear

# Initialize graph with a theme
app = Graph(title="ResNet Block Architecture", theme="modern-dark")

@app.pipeline
def resnet_block(x):
    # Declarative layer connections
    h = Conv2D(filters=64, kernel_size=3)(x)
    h = BatchNorm()(h)
    h = ReLU()(h)
    
    # Residual skip connection
    out = h + x
    return out

# Render directly to file
app.render("resnet_block.png", show_tensor_shapes=True)
```

---

### 2. Auto-Generate from PyTorch Models

Pass your PyTorch model straight into Archigraph:

```python
import torch
import torchvision.models as models
from archigraph import from_pytorch

model = models.resnet18()
dummy_input = torch.randn(1, 3, 224, 224)

# One line to parse model structure & render via C++ backend
diagram = from_pytorch(model, input_size=dummy_input)
diagram.save("resnet18.svg")
```

---

## How It Works

```
┌─────────────────────────────────────────────────────────┐
│               Python API (Developer Layer)              │
│      Declarative decorators, PyTorch module parser      │
└───────────────────────────┬─────────────────────────────┘
                            │ (pybind11 C-Extension)
┌───────────────────────────▼─────────────────────────────┐
│              C++ Core (Performance Engine)              │
│   • Graph topology parser & layout layout algorithms    │
│   • Vector graphic geometry calculation                 │
│   • Native SVG/PNG rendering renderer                   │
└─────────────────────────────────────────────────────────┘
```

---
