---
type: note
tags: [ai, machine-learning, deep-learning]
---


How machines learn patterns from data instead of being explicitly programmed — from classical ML through deep neural networks to the vision and language models built on them.

## 1. Machine learning fundamentals

Pipeline: **collect → preprocess → feature-engineer → select model → train → evaluate → tune → deploy → monitor** (garbage in → garbage out).

**Five learning paradigms:**

| Paradigm | Data | Goal |
|---|---|---|
| **Supervised** | labeled | learn `f(X)→Y` — classification or regression |
| **Unsupervised** | unlabeled | find structure — clustering, dimensionality reduction, anomalies |
| **Reinforcement** | reward signal | an agent learns a policy by trial and error |
| **Semi-supervised** | few labels + lots unlabeled | label-efficient learning (pseudo-labeling) |
| **Self-supervised** | labels from the data itself | masked/next-token prediction, contrastive — powers LLMs & ViTs |

**Key algorithms:** linear/logistic regression, decision trees → **random forests** (bagging) → **gradient boosting (XGBoost/LightGBM — best on tabular)**, SVM, KNN, naive Bayes (text); clustering: K-Means, DBSCAN, GMM; reduction: PCA, t-SNE/UMAP. **RL:** Q-learning → DQN (Atari) → policy gradient/PPO, actor-critic (AlphaGo, RLHF).

**Evaluation** (the part people get wrong on imbalanced data):
- *Classification:* from the confusion matrix — **precision** = TP/(TP+FP), **recall** = TP/(TP+FN), **F1** (their harmonic mean), **AUC-ROC** (0.5 random, 1.0 perfect). Accuracy alone lies when classes are imbalanced.
- *Regression:* MAE, MSE/RMSE, R².
- Validate with **k-fold cross-validation** (stratified for classification, time-split for temporal — avoids data leakage).

**Bias–variance:** *underfitting* (too simple → add capacity/features); *overfitting* (too complex → more data, **regularization** (L1/Lasso zeros weights, L2/Ridge shrinks them), dropout, early stopping). Handle class imbalance (SMOTE/weighting) and scale features (StandardScaler) for distance-based models. Tune with random/Bayesian search (Optuna). Core stack: scikit-learn, XGBoost, pandas, MLflow.

## 2. Neural networks & deep learning

A **neuron** computes `a = activation(Σwᵢxᵢ + b)`; the activation's non-linearity is what lets stacked layers learn complex functions (without it, the whole net collapses to one linear map).

| Activation | Use |
|---|---|
| **ReLU** `max(0,x)` | hidden-layer default (can "die" → Leaky ReLU) |
| Sigmoid / Tanh | binary output / RNNs |
| **Softmax** | multi-class output (probabilities sum to 1) |
| GeLU / Swish | Transformers, modern nets |

**Training loop:** forward pass → **loss** (MSE for regression; cross-entropy for classification) → **backpropagation** (chain-rule gradients) → optimizer update `θ = θ − α·∇L`. Use **mini-batch SGD**; optimizers **Adam/AdamW** (adaptive) are the default; LR schedules use warmup+decay (standard for Transformers). Watch **vanishing/exploding gradients** (fixed by ReLU, residual connections, gradient clipping). Regularize with **dropout**, weight decay, **batch/layer normalization**, augmentation, early stopping. Init with He (ReLU) or Xavier.

**Key architectures:**

| Architecture | Input | Mechanism | Use |
|---|---|---|---|
| **MLP** (dense) | tabular | fully connected | tabular, output heads |
| **CNN** | grids/images | weight-shared convolution + pooling | vision |
| **RNN / LSTM / GRU** | sequences | recurrent hidden state (gates fix vanishing gradient) | time series, pre-Transformer language |
| **Transformer** | sequences | **self-attention** (parallel, every token attends to every token) | language, vision (ViT), audio |
| **Autoencoder / VAE** | any | encode → bottleneck → decode | compression, anomaly detection, generation |
| **GAN** | noise→data | generator vs discriminator | image synthesis, deepfakes |
| **Diffusion** | noise→data | iteratively denoise | SOTA image/audio/video generation |

**Residual (skip) connections** `out = F(x)+x` let gradients flow through, enabling 100+ layer nets (ResNet). **Self-attention** `softmax(QKᵀ/√d)·V` is the core of Transformers (deep dive in [[LLMs & Prompting]]). Frameworks: **PyTorch** (research), TensorFlow/Keras, JAX; deploy via ONNX/TensorRT.

> *Why deep learning took off (~2012, AlexNet):* big labeled datasets + GPUs + better training tricks (ReLU, batchnorm, dropout).

## 3. Computer vision

Teaching machines to extract meaning from pixels (a 224×224 image = 150K values). Tasks: **classification** (one label), **object detection** (boxes — YOLO real-time, Faster R-CNN, DETR; metric **mAP**/**IoU**), **segmentation** (per-pixel — U-Net, Mask R-CNN), pose estimation, **facial recognition** (embeddings + cosine similarity — FaceNet/ArcFace), OCR, and **image generation** (GAN/diffusion).

**How CNNs see:** convolutional filters slide over the image; layers learn a **feature hierarchy** (edges → textures → object parts → concepts); pooling downsamples for translation invariance and grows the receptive field. Architecture lineage: LeNet → **AlexNet (2012)** → VGG → ResNet (skip connections) → EfficientNet (compound scaling) → **Vision Transformer** (image as patches + attention; needs more data but excels pretrained). **Transfer learning** dominates: take an ImageNet-pretrained backbone and either freeze it (feature extraction, small data) or fine-tune (medium data). Tools: OpenCV, torchvision, **Ultralytics YOLOv8**, SAM, CLIP (zero-shot).

## 4. Natural language processing

Evolution: rule-based → statistical (n-grams) → neural (word2vec, 2013) → **Transformer (2017)**.

**Tasks:** classification (sentiment, spam), **NER** (extract people/orgs/locations — and CVEs/IPs/hashes for security), translation (BLEU), summarization (extractive vs abstractive), QA, generation, information extraction.

**Representing text as numbers:** Bag-of-Words → **TF-IDF** (weight by distinctiveness) → **word embeddings** (Word2Vec/GloVe/FastText — `king − man + woman ≈ queen`) → **contextual embeddings** (BERT — same word, different vector by context). **Tokenization** uses subwords: **BPE** (GPT), WordPiece (BERT), SentencePiece (language-agnostic).

**Model families:** **BERT** = Transformer *encoder*, bidirectional, masked-LM pretraining → fine-tune for classification/NER/QA (not generation). **GPT** = Transformer *decoder*, autoregressive next-token → generation (see [[LLMs & Prompting]]). **T5** casts every task as text-to-text. **Seq2Seq** (encoder-decoder + attention) for translation/summarization. Tools: spaCy (industrial), Hugging Face Transformers, sentence-transformers (semantic search), LangChain.

## 5. Security applications (cross-cutting)

ML/DL is woven through a SOC: **IDS** (classify traffic), **malware classification** (byte n-grams or binary-as-image CNNs), **UEBA** (baseline behavior, flag deviations), **phishing/spam** (BERT >99%), **fraud** (imbalanced classification), **threat-intel NER** (pull CVEs/IPs from reports), **deepfake detection** (CNN on frames — but detection lags generation). The flip side is **adversarial ML**: crafting inputs that evade classifiers, and attackers using generative models for spear-phishing and deepfake fraud. See [[Internet & Application Security|AI & ML Security]].

Related: [[AI Foundations]] · [[LLMs & Prompting]] · [[14 AI|domain overview]] · [[Languages & Applied Programming|data analytics]] · [[CPU & Processing Units#3.1 GPU (Graphics Processing Unit)|GPUs]]
