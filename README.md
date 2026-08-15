# ASR Wav2Vec2 Fine-Tuning Pipeline

A robust, single-process pipeline for fine-tuning the Hugging Face `Wav2Vec2ForCTC` model on custom ASR datasets, optimized for stability on Windows and Linux environments.

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git
cd YOUR_REPOSITORY_NAME
```

### 2. Set Up a Virtual Environment

It is recommended to use a Python virtual environment.

**Windows:**

```bash
python -m venv venv
venv\Scripts\activate
```

**macOS/Linux:**

```bash
python -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

Install the required packages using pip:

```bash
pip install -r requirements.txt
```

---

## 📁 Dataset Structure

Place your training and validation CSV files inside a `dataset/` folder.

Ensure that your CSV files use a pipe (`|`) delimiter and contain the audio file paths along with their corresponding transcript text.

Example structure:

```text
YOUR_REPOSITORY_NAME/
│
├── dataset/
│   ├── train.csv
│   └── val.csv
│
├── config.toml
├── train.py
├── requirements.txt
└── ...
```

---

## ⚙️ Configuration

Hyperparameters, dataset paths, and training settings are managed through **`config.toml`**.

Key settings include:

* **`epochs`** — Total number of training epochs, e.g. `50`
* **`lr`** — Learning rate, e.g. `1e-4`
* **`batch_size`** — Batch size configured under `[train_dataset.dataloader]`
* **`use_amp`** — Enable or disable Automatic Mixed Precision

For improved stability on Windows, AMP can be disabled:

```toml
use_amp = false
```

---

## 🏋️ Training the Model

To start the training loop using your configuration file, run:

```bash
cd "c:\Users\emana\OneDrive\Desktop\FUNETUNE ENGLISH\ASR-Wav2vec-Finetune-main"

venv\Scripts\python train.py -c config.toml
```

The script will automatically:

1. Load the pre-trained `facebook/wav2vec2-base` weights.
2. Load the configured training and validation datasets.
3. Initialize the single-process training pipeline.
4. Fine-tune the `Wav2Vec2ForCTC` model.
5. Run validation cycles.
6. Calculate validation Word Error Rate (WER).
7. Save checkpoints and best-performing model shards in the `saved/` directory.

---

## 📊 Model Evaluation

The primary evaluation metric is **Word Error Rate (WER)**.

```text
WER = (Substitutions + Deletions + Insertions) / Number of Reference Words
```

A lower WER indicates better speech recognition performance.

---

## 🔬 Current Training Configuration

The current optimized configuration uses:

| Parameter                   |    Value |
| --------------------------- | -------: |
| Epochs                      |     `50` |
| Learning Rate               |   `1e-4` |
| Gradient Accumulation Steps |      `2` |
| AMP                         | Disabled |
| Training Utterances         |    `904` |
| Validation Utterances       |    `101` |

The baseline experiment used 20 epochs with a learning rate of `5e-5` and achieved a validation WER of `1.0000`.

The optimized experiment increases the training duration and learning rate to attempt to reduce the validation WER.

---

## 🛠️ Stability Improvements

### Single-Process Training

The pipeline uses a single-process training configuration to avoid multiprocessing-related memory access violations encountered during development on Windows.

### AMP Disabled

Automatic Mixed Precision is disabled in the current stable configuration because of numerical stability issues encountered during training.

```toml
use_amp = false
```

---

## 📦 Output

After training, model checkpoints and best-performing model shards are stored in:

```text
saved/
```

The best-performing checkpoint is preserved based on validation performance.

---

## 🔧 Troubleshooting

### WER Remains at `1.0000`

If validation WER remains at `1.0000`, verify:

* Audio sampling rate
* Audio/transcript pairing
* Transcript quality
* Text normalization
* Tokenizer configuration
* CTC label configuration
* Padding and label masking
* Decoder implementation
* Training/validation data distribution

It is also recommended to test the model on a small subset of approximately 10–20 utterances to verify that the training pipeline can overfit a small dataset.

### Training Crashes on Windows

Use the single-process configuration and ensure that the virtual environment is activated correctly.

### Numerical Stability Issues

Disable AMP in `config.toml`:

```toml
use_amp = false
```

---

## 📌 Project Status

* [x] Wav2Vec2 model configuration
* [x] Custom dataset loading
* [x] Training pipeline
* [x] Validation pipeline
* [x] WER evaluation
* [x] Single-process training
* [x] AMP stability workaround
* [x] Model checkpointing
* [x] 20-epoch baseline experiment
* [ ] Complete 50-epoch optimized experiment

---

## 🎯 Future Improvements

Potential improvements include:

* Hyperparameter tuning
* Learning-rate scheduling
* Warm-up steps
* Weight decay optimization
* Data augmentation
* Feature-extractor freezing/unfreezing
* Improved decoding strategies
* Larger and more diverse speech datasets
* Language-model-assisted decoding

---

## 👤 Author

**Saketh Emani**

ASR / Speech Recognition Fine-Tuning Project
