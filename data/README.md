# Data

CardVision V1 does not store the full dataset in this repository.

The notebook downloads the public Kaggle 52-card PNG dataset at runtime using `kagglehub`:

```python
kagglehub.dataset_download("avinashtare/complete-52-card-deck-dataset-for-card-game")
```

The source images are individual playing-card PNG assets. CardVision converts those full card assets into 13 rank-only labels because blackjack basic strategy depends on card rank, not suit.

## Generated Data

The notebook programmatically creates a synthetic YOLOv8 dataset:

```text
/content/cardvision_yolo_rank/
├── images/
│   ├── train/
│   └── val/
├── labels/
│   ├── train/
│   └── val/
└── data.yaml
```

Each generated image contains:

- one dealer card in the top zone
- two player cards in the bottom zone
- YOLO-format bounding box labels
- rank-only class labels

These generated files are not committed to GitHub because they can be recreated by running the notebook.
