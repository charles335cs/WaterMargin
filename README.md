# Anchor-QIM Watermark

> **This is the code for a training-free image watermarking system based on DCT-QIM, chrominance anchors, and margin-guided reinforcement.**

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-supported-5C3EE8?logo=opencv&logoColor=white)
![Pillow](https://img.shields.io/badge/Pillow-supported-3776AB)

## ✨ Code Structure

```text
final_anchor_qim_20260930/
├── final_api.py                         # Public encoding and decoding API
├── attacks.py                           # Evaluation-only attack module
├── requirements.txt                     # Python dependencies
├── methods/
│   ├── deep_sync_qim/
│   │   └── natural_invariant_qim.py     # Anchor detection and alignment
│   ├── qim_watermark/
│   │   └── qim_core.py                  # DCT-QIM and carrier allocation
│   └── margin_watermark/
│       └── ecc.py                       # CRC and convolutional coding
└── scripts/
    ├── evaluate_robustness_parallel.py # Robustness evaluation
    ├── verify_blind_allocation.py      # Carrier-layout verification
    └── verify_single_image_decode.py   # Single-image decoding test
```

## 🚀 Inference

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. Encode a watermark

```python
import numpy as np
from PIL import Image
from final_api import encode_one

message = np.random.default_rng(0).integers(
    0, 2, 256, dtype=np.uint8
)

image = Image.open("input.png").convert("RGB")
watermarked, info = encode_one(image, message)
watermarked.save("watermarked.png")

print(info)
```

### 3. Decode a watermark

```python
from PIL import Image
from final_api import decode_one

recovered, confidence, crc_valid = decode_one(
    Image.open("watermarked.png").convert("RGB")
)

print("Recovered bits:", len(recovered))
print("Confidence:", confidence)
print("CRC valid:", crc_valid)
```

The decoder takes only the received image as input. The fixed protocol and key schedule are stored in the implementation; no per-image carrier list or reinforcement mask is required.

### ⚙️ Default Configuration

```text
Message length     256 bits
Coded length       417 bits
DCT block          16 x 16
QIM step           40
Carrier region     Central 80%
Margin threshold   tau = 1.98
Synchronization    Four chrominance anchors
```
