# 📌 Task (1) : Quantization Reasearch 

([`Quantization.pdf`](./Quantization.pdf))

--- 

# 📌 Task (2) : Text & Image Classifier

## 📍 Introduction

This project is a **simple sentiment analysis app** that combines an **image (or image caption)** with **additional user text** to predict whether the final combined text is **positive** or **negative**, along with a confidence score.

* **Option 1:** Upload an **image** → the app will generate a caption using a multimodal model.
* **Option 2:** Enter an **image caption manually**.
* In both cases, you then provide an **additional text input**.
* The system combines both inputs, runs them through a **sentiment classifier**, and produces a result with a confidence score.

The goal of this project is mainly **educational**:

* To practice working with different models (image-to-text + text classification).
* To experiment with deployment using **Streamlit**.

---

## 📍 System Architecture

The system is composed of **three main parts**:
### 1️⃣ Input Handling ([`image_caption.py`](./image_caption.py))

* Users can **upload an image** or **enter a caption manually**.
* If an image is uploaded, we use **BLIP (Bootstrapping Language-Image Pre-training)**, a multimodal model that converts images into text.

  * We used **BLIP v1** (lighter & faster than BLIP v2).
  * Chosen because it can be downloaded and run locally (since Hugging Face does not offer a free API for BLIP).
* If a caption is entered manually, we skip this step and use the provided text directly.

### 2️⃣ Classification ([`classifier.py`](./classifier.py))

* The caption (generated or manual) is **combined** with the additional user text.
* This combined input is sent to a **sentiment analysis model**:

  * [`cardiffnlp/twitter-roberta-base-sentiment-latest`](https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment-latest)
  * A variant of **RoBERTa** pre-trained specifically for sentiment classification.
* The output is a **label** (`positive`, `negative`, `neutral`) and a **confidence score**.
* The model is accessed via the **Hugging Face Inference API**, so no heavy local downloads are needed.

### 3️⃣ Data Storage ([`records.csv`](./records.csv))

* Every classification result is stored in a CSV file for tracking.
* Each row contains:
  * `image_caption`
  * `user_text`
  * `combined_input`
  * `label`
  * `score`

---

## 📍 User Interface (Streamlit)

The entire app is wrapped in a **Streamlit** UI for easy interaction:

* Upload images / enter captions.
* Enter additional text.
* View generated captions, sentiment predictions, and scores.
* See past records stored in CSV.

Streamlit also provides a **shareable local network link** so others can test it while your computer is running.

---

## 📍 Installation & Usage

### 1️⃣ Clone the repository

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### 2️⃣ Create and activate a virtual environment

```bash
python -m venv myenv
source myenv/bin/activate  # Mac/Linux
myenv\Scripts\activate     # Windows
```

### 3️⃣ Install dependencies

```bash
pip install -r requirements.txt
```

Key dependencies:

* `transformers`
* `torch`
* `pandas`
* `streamlit`
* `Pillow`

### 4️⃣ Add your Hugging Face API key

Create a `.env` file:

```
HF_TOKEN=your_api_key_here
```

### 5️⃣ Run the app

```bash
streamlit run classifier.py
```

---

## 📍 Lessons Learned

1. Always check **model availability** on Hugging Face:
   * Some models are available via API.
   * Some must be downloaded locally.
   * This influences design decisions.

2. First time working with **Streamlit** — found it **extremely simple and effective** for deployment.

3. Importance of **hiding warnings**:
   * Keep them visible during debugging.
   * Hide them during deployment for cleaner user experience.

4. Learned about **BLIP (Bootstrapping Language-Image Pre-training)**:
   * Converts images → text.
   * Has many real-world applications (accessibility, content generation, etc.).

5. Realized that building **real systems often requires multiple models**, not just one.
   * Here, we used BLIP (image → text) + RoBERTa (text sentiment classification).

6. Sentiment models are often **dominated by strong words**.
   * Example: *“beautiful sunrise” + “death”* → likely classified as **negative** because of word weighting.omeone visits your repo, they instantly see how the UI looks?
