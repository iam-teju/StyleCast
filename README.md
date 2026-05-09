---
title: StyleCast
emoji: 🎨
colorFrom: yellow
colorTo: yellow
sdk: docker
app_port: 7860
pinned: false
---

# 🎨 StyleCast — Neural Style Transfer with AdaIN

Real-time artistic style transfer web app powered by **Adaptive Instance Normalization (AdaIN)**. Upload any content image and a style painting — the neural network transfers the artistic style instantly.

🔗 **Live Demo:** [tejas-m-stylecast.hf.space](https://tejas-m-stylecast.hf.space)

---

## How It Works

StyleCast uses a three-step pipeline based on the AdaIN algorithm ([Huang & Belongie, ICCV 2017](https://arxiv.org/abs/1703.06868)):

1. **Encode** — Both content and style images pass through a pre-trained VGG-19 encoder, extracting deep feature representations.
2. **AdaIN Transfer** — Content features are normalized to match the mean and variance of the style features, aligning their statistical distributions in feature space.
3. **Decode** — A trained decoder network converts the transformed features back to pixel space, producing the final stylized image.

The **alpha slider** (0–1) controls how much style is applied — 0 preserves the original content, 1 applies full style transfer.

---

## Results

| Content | Style | Output |
|---------|-------|--------|
| <img src="content_data/brad_pitt.jpg" width="200"> | <img src="style_data/woman_in_peasant_dress.jpg" width="200"> | <img src="examples/stylized_brad_pitt.jpg" width="200"> |

---

## Training Details

The decoder was trained from scratch in two phases:

| Phase | Epochs | Resolution | Style Weight | Learning Rate |
|-------|--------|------------|-------------|---------------|
| Phase 1 | 160 | 256×256 | 5.0 | 1e-4 |
| Phase 2 | 100 | 512×512 | 10.0 | 5e-5 |

- **Content dataset:** ~40,000 images from MS-COCO
- **Style dataset:** ~8,000 paintings from WikiArt
- **Encoder:** Pre-trained VGG-19 (frozen weights)
- **Decoder:** Trained from scratch
- **Loss:** Content loss (MSE on relu4_1 features) + Style loss (MSE on mean/std across relu1_1 to relu4_1)
- **Optimizer:** Adam
- **Hardware:** NVIDIA RTX 5050

---

## Tech Stack

- **PyTorch** — Model training and inference
- **Flask** — Web backend
- **VGG-19** — Pre-trained encoder
- **Gunicorn** — Production server
- **Docker** — Containerized deployment
- **HuggingFace Spaces** — Hosting

---

## Project Structure

```
StyleCast/
├── app.py                  # Flask web app
├── train.py                # Training script
├── decoder_final.pth       # Trained decoder weights
├── vgg_normalised.pth      # Pre-trained VGG-19 encoder
├── Dockerfile              # Docker config for deployment
├── Procfile                # Process config
├── requirements.txt        # Python dependencies
├── templates/
│   └── index.html          # Frontend
├── examples/               # Example images
├── static/uploads/         # User uploads
├── content_data/           # Sample content images
├── style_data/             # Sample style images
└── utils/
    ├── models.py           # VGG Encoder & Decoder architectures
    └── utils.py            # AdaIN, dataset, transforms
```

---

## Run Locally

```bash
# Clone the repo
git clone https://github.com/iam-teju/StyleCast.git
cd StyleCast

# Install dependencies
pip install -r requirements.txt

# Run the app
python app.py
```

Open `http://localhost:5000` in your browser.

---

## References

- Huang, X., & Belongie, S. (2017). [Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization](https://arxiv.org/abs/1703.06868). ICCV 2017.

---

Built by **Tejas M** · [LinkedIn](https://www.linkedin.com/in/iamteju/) · [GitHub](https://github.com/iam-teju)
