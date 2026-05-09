# ♠️ CardVision — Blackjack Practice Coach

**ITAI 1378: Computer Vision & AI**  
**Team:** Dylan Legault  
**Tier:** Tier 2

---

## 📌 Problem Statement

Blackjack players—especially new ones—often struggle to apply basic strategy consistently under time pressure. Correct play depends on quickly identifying the player hand, the dealer upcard, and whether the hand is hard, soft, or a pair. Traditional strategy charts are useful, but they require manual interpretation and do not automatically read the current board state.

CardVision addresses this problem as an educational practice tool. It is designed for controlled practice settings, not live casino play.

---

## 💡 Solution Overview

CardVision is a computer vision blackjack practice assistant that detects visible card ranks from a blackjack board image, assigns cards to dealer and player zones, parses the current blackjack state, and recommends a V1 basic strategy action.

**V1 supported actions:**
- Hit
- Stand
- Double
- Split

**V1 intentionally excludes:**
- Insurance
- Surrender
- Card counting
- Bet sizing
- Multi-player table tracking
- Post-split hand tracking

---

## ⚙️ Technical Approach

| Component | Choice | Justification |
|---|---|---|
| **CV Technique** | Object Detection | Detect visible card locations and ranks from a board image |
| **Model** | YOLOv8n (Ultralytics) | Lightweight, fast, and suitable for Colab-based training |
| **Framework** | PyTorch + Ultralytics | Common computer vision workflow with simple training/inference APIs |
| **Dataset Source** | Kaggle 52-card PNG deck | Provides clean individual card assets for synthetic training data |
| **Labels** | 13 rank-only classes | Blackjack decisions depend on rank, not suit |
| **Output** | Strategy recommendation table + visualization | Shows detected cards, zones, hand type, and recommended action |

### Pipeline

```text
Card PNG assets
    ↓
Synthetic blackjack board generation
    ↓
YOLOv8 rank-only training
    ↓
Card rank detection
    ↓
Dealer/player zone assignment
    ↓
Blackjack hand parser
    ↓
Basic strategy recommendation
```

---

## 🗃️ Dataset Plan

CardVision V1 uses the Kaggle 52-card PNG deck as raw card assets. The notebook programmatically generates synthetic blackjack-table images by placing one dealer card in the top zone and two player cards in the bottom zone.

Although the source dataset includes full rank+suit card images, V1 converts labels into **13 rank-only classes** because blackjack strategy decisions do not depend on suit.

| Source | Type | Labels Used | Purpose |
|---|---|---|---|
| Kaggle 52-card deck | Public PNG assets | Converted to rank-only labels | Synthetic YOLO training data |
| Synthetic board images | Generated in notebook | YOLO bounding boxes + rank class | Model training and validation |
| Future self-collected images | Real photos | Rank labels | Robustness testing |

### Rank Classes

```text
A, 2, 3, 4, 5, 6, 7, 8, 9, 10, J, Q, K
```

---

## 📊 Success Metrics

| Metric | Type | Target |
|---|---|---|
| Rank Detection Accuracy / mAP | Primary | ≥ 90% on synthetic validation images |
| Strategy Recommendation Accuracy | Primary | 100% for deterministic lookup cases |
| Inference Latency | Secondary | < 1 second per image |
| Zone Assignment Correctness | Secondary | Dealer/player cards grouped correctly in V1 layout |

---

## 📓 Notebook Progress

The current notebook draft implements an end-to-end CardVision V1 prototype:

- Downloads the Kaggle 52-card PNG dataset
- Parses card filenames into card labels
- Converts full card labels into rank-only classes
- Generates synthetic blackjack board images
- Trains a YOLOv8n rank detector
- Runs inference on validation images
- Assigns detections to dealer/player zones
- Parses the blackjack hand state
- Recommends Hit, Stand, Double, or Split

Main notebook:

```text
notebooks/CardVisionV1.ipynb
```

---

## 📅 Week-by-Week Plan

| Week | Task | Milestone |
|---|---|---|
| 10 | Finalize scope, repo, and proposal | Project plan complete |
| 11 | Build strategy engine and dataset pipeline | Logic layer working |
| 12 | Generate synthetic images and train YOLOv8 | Rank detector working |
| 13 | Integrate detection with strategy engine | End-to-end prototype |
| 14 | Test, document, and polish repo | Submission-ready materials |
| 15 | Present project | Final demo |

---

## ⚠️ Risks & Mitigation

| Risk | Probability | Mitigation |
|---|---|---|
| Synthetic images do not fully match real camera images | Medium | Add real self-collected photos later |
| Low rank detection accuracy on glare/rotation | Medium | Add augmentation and varied backgrounds |
| Confusion between similar ranks | Medium | Increase image resolution or train longer |
| Inference too slow | Low | Use YOLOv8n and reduce image size if needed |
| Complex table layouts | Medium | Keep V1 to one dealer zone and one player zone |

---

## 🖥️ Resources Needed

| Resource | Option |
|---|---|
| Compute | Google Colab with T4 GPU |
| Frameworks | Ultralytics YOLOv8, PyTorch, Pandas, Matplotlib |
| Dataset | Kaggle 52-card PNG deck |
| Estimated Cost | $0 using free tools |

---

## 📁 Repository Structure

```text
CardVision/
├── README.md
├── requirements.txt
├── notebooks/
│   └── CardVisionV1.ipynb
├── data/
│   └── README.md
├── docs/
│   └── AI_usage_log.md
├── results/
│   └── README.md
└── assets/
    └── README.md
```

---

## 📦 Requirements

Install dependencies with:

```bash
pip install -r requirements.txt
```

Core dependencies include:

```text
ultralytics
torch
torchvision
opencv-python
numpy
pandas
matplotlib
Pillow
tqdm
PyYAML
kagglehub
```

---

## 🚀 Future Work

- Test on real photos of physical playing cards
- Add more table backgrounds, lighting variation, and rotation augmentation
- Build a simple webcam demo
- Improve UI overlay for live practice mode
- Explore future wearable display integration as a stretch goal

---

## Ethics and Scope Note

CardVision is framed as a learning and practice assistant. It is not intended for cheating, casino use, card counting, or gambling automation. The V1 system demonstrates computer vision, board-state parsing, and deterministic strategy lookup in a controlled educational environment.
