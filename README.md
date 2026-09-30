# Watermark

> **This is the code for the paper “What Makes a Watermark Survive? Understanding Robust Image Watermarking through Representation Margins”.**

Training-free image watermarking based on DCT-QIM, chrominance anchors, and margin-guided reinforcement.

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![OpenCV](https://img.shields.io/badge/OpenCV-supported-5C3EE8?logo=opencv&logoColor=white)](https://opencv.org/)
[![Pillow](https://img.shields.io/badge/Pillow-supported-3776AB)](https://python-pillow.org/)

## 📁 Code Structure

```text
watermark/
├── requirements.txt
├── embed.py                         # Watermark embedding
├── extract.py                       # Watermark extraction
├── attack_test.py                   # Attack generation and evaluation
└── methods/
    ├── deep_sync_qim/
    │   └── natural_invariant_qim.py # Anchor synchronization and decoding
    ├── qim_watermark/
    │   └── qim_core.py              # DCT-QIM and carrier operations
    └── margin_watermark/
        └── ecc.py                   # CRC and convolutional coding
```

## ⚙️ Installation

```bash
pip install -r requirements.txt
```

## 🔍 Inference

```python
import numpy as np
from PIL import Image

from embed import embed_watermark
from extract import extract_watermark

image = Image.open("input.png").convert("RGB")
message = np.random.default_rng(0).integers(0, 2, 256, dtype=np.uint8)

watermarked, info = embed_watermark(image, message)
watermarked.save("watermarked.png")

recovered, confidence, crc_valid = extract_watermark(watermarked)
print("Message accuracy:", np.mean(recovered == message))
print("CRC valid:", crc_valid)
```

The decoder requires only the received image. No per-image carrier list or additional side information is needed.
