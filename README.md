# ♠️ CardVision — Blackjack Card Detection & Strategy Advisor

**ITAI 1378: Computer Vision & AI**
**Team:** Dylan
**Tier:** Tier 2

---

## 📌 Problem Statement

Blackjack players—especially new ones—routinely make suboptimal decisions due to lack of strategy knowledge. Phones are banned at most tables, printed cheat sheets are frowned upon, and memorizing basic strategy is unreliable under pressure. There is no real-time, discreet tool to guide players at the table.

---

## 💡 Solution Overview

CardVision is a real-time computer vision system that detects playing cards on a blackjack table using a camera feed, identifies each card's rank and suit, and instantly recommends the optimal move (Hit, Stand, Double Down, Split, or Surrender) based on standard basic strategy.

The MVP runs as a Python script with a webcam input and visual HUD overlay. The long-term vision is integration with Meta Ray-Ban smart glasses for a wearable, heads-up display experience.

---

## ⚙️ Technical Approach

| Component | Choice | Justification |
|---|---|---|
| **CV Technique** | Object Detection + Classification | Identify card position, rank, and suit from a live feed |
| **Model** | YOLOv8n (Ultralytics) | Best-in-class speed/accuracy tradeoff; <30ms inference |
| **Framework** | PyTorch + Ultralytics | Industry standard; easy fine-tuning on custom data |
| **Input** | Webcam / RTSP stream | Flexible for dev; maps to Ray-Ban camera long-term |
| **Output** | OpenCV HUD overlay | Visual recommendation rendered directly on video feed |

**Pipeline:**
```
[Camera Frame] → [YOLOv8 Card Detection] → [Rank+Suit Classification] → [Strategy Engine] → [HUD Overlay]
```

---

## 🗃️ Dataset Plan

| Source | Type | Size | Labels |
|---|---|---|---|
| [Roboflow Universe](https://universe.roboflow.com) | Public, pre-labeled | ~5,000–15,000 images | 52 classes (rank + suit) |
| [Kaggle Playing Cards](https://www.kaggle.com/) | Public | ~3,000 images | Rank + suit |
| Self-collected | Manual photos | ~500 images | Custom — felt table surface |

- **Total target:** 10,000+ images after augmentation
- **Label format:** YOLO `.txt` bounding boxes
- **Classes:** 52 (13 ranks × 4 suits)
- **Split:** 80% train / 20% validation, stratified by class
- **Augmentation:** Rotation ±15°, brightness shifts, blur, horizontal flip

---

## 📊 Success Metrics

| Metric | Type | Target |
|---|---|---|
| Card Detection Accuracy (mAP) | Primary | ≥ 92% |
| Rank + Suit Classification Accuracy | Primary | ≥ 95% |
| Strategy Recommendation Accuracy | Primary | 100% (deterministic lookup) |
| Inference Latency | Secondary | < 100ms per frame |
| False Positive Rate | Secondary | < 5% |

---

## 📅 Week-by-Week Plan

| Week | Date | Task | Milestone |
|---|---|---|---|
| 10 | Oct 30 | Finalize dataset, set up repo & environment | Dataset ready + repo live |
| 11 | Nov 6 | Train YOLOv8 baseline on Roboflow dataset | Model detects all 52 cards |
| 12 | Nov 13 | Integrate strategy engine, test on video | End-to-end pipeline working |
| 13 | Nov 20 | Tune accuracy, add augmentation, UI polish | ≥92% detection accuracy |
| 14 | Nov 27 | Final testing, README, GitHub cleanup | Submission-ready |
| 15 | Dec 4 | In-class presentation 🎉 | Live demo |

---

## ⚠️ Risks & Mitigation

| Risk | Probability | Mitigation |
|---|---|---|
| Low detection accuracy on glare/worn cards | Medium | Add augmentation; switch to YOLOv8s (small) for better accuracy |
| Insufficient labeled training data | Low | Fall back to Roboflow's public playing card universe (~15K images, fully labeled) |
| Inference too slow for real-time | Medium | Use YOLOv8 nano; reduce input resolution to 416×416 |
| Partial card occlusion | Low | Train on synthetically occluded images; fall back to rank-only detection |

---

## 🖥️ Resources Needed

| Resource | Option |
|---|---|
| Compute | Google Colab (T4 GPU), Kaggle Notebooks (P100) |
| Frameworks | Ultralytics YOLOv8, PyTorch, OpenCV |
| Datasets | Roboflow Universe, Kaggle (both free) |
| Estimated Cost | **$0** — all free tiers and open-source tools |

---

## 📁 Repository Structure

```
CardVision/
├── README.md                  ← You are here
├── requirements.txt           ← Python dependencies
├── notebooks/
│   └── 01_exploration.ipynb   ← Initial dataset exploration
├── data/
│   └── README.md              ← Dataset sources and download instructions
└── docs/
    └── proposal.pdf           ← Presentation slides (PDF)
```

---

## 📦 Requirements

```
ultralytics>=8.0.0
torch>=2.0.0
torchvision>=0.15.0
opencv-python>=4.8.0
numpy>=1.24.0
Pillow>=9.5.0
```

---

## 🚀 Future Work

- Integration with **Meta Ray-Ban Smart Glasses API** for wearable HUD
- Support for multi-deck shoe tracking (card counting assistance)
- Voice output for hands-free recommendations
- Mobile app wrapper for casual use
