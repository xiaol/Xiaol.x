
# Large Concept Models (LCMs)

## Core Concepts
*   **Large Concept Model (LCM):** A model that operates on **high-level semantic representations** called "concepts" rather than tokens.
    *   Concepts are **language and modality-agnostic**, representing ideas or actions, typically at the sentence level.
    *   LCMs are designed for **abstract reasoning and generation**.
*   **SONAR:** A **sentence-level, multimodal, and language-agnostic embedding space** used to represent concepts in LCMs.
    *   Covers **200 languages** in text and speech.
*   **Tokens:** The basic units of text used by LLMs (words or parts of words).

## LCM Design Principles
*   **Reasoning at the conceptual level,** not at the token level.
*   Utilizing a **fixed-size sentence embedding space.**
*   Employing the **SONAR embedding space** due to its semantic capabilities.
    *   SONAR is trained with a **machine translation objective**, denoising auto-encoding, and an **MSE loss.**
    *   SONAR extends to speech through a **teacher-student approach**.
*   **Data Preparation:**
    *   **Sentence Segmentation:** Splitting long texts into sentences using **SpaCy** or **Segment Any Text (SaT)** segmenters, with "capped" extensions to handle long sentences.
        *   **SpaCy Capped:** Rule-based approach with a maximum sentence length.
        *   **SaT Capped:** Uses splitting probability estimates and a maximum length.
    *   **Evaluation with AutoBLEU score:** Measures the quality of reconstructed sentences after encoding and decoding.

## LCM Architectures

### Base-LCM
*   A baseline **decoder-only transformer model** for next-concept prediction.
*   Uses **PreNet and PostNet** linear layers to normalize embeddings.
*   Trained with **MSE regression** to predict the next concept.

### Diffusion-based LCMs
*   **Generative models** using a forward noising and reverse denoising process.
*   Use **autoregressive models** to generate concepts.
*   Two variants:
    *   **One-Tower Diffusion LCM:** A single transformer backbone predicts the clean next sentence embedding.
    *   **Two-Tower Diffusion LCM:** Separates context encoding from diffusion using cross-attention and **AdaLN** modulation
*   Different **noise schedules** are used, including Cosine, Quadratic, and Sigmoid.
*   **Classifier-free diffusion guidance** balances sample quality and diversity.
*   **Weighting strategies** like **SNR clamping** and **fragility scores** are used to improve training.

### Quantized LCMs
*   Use **Residual Vector Quantization (RVQ)** to discretize SONAR representations.
*   Two approaches:
    *   **Quant-LCM-d:** Models discrete units.
    *   **Quant-LCM-c:** Predicts continuous target SONAR vectors.

## Training and Evaluation
*   **Pre-training:** Models are trained with large datasets (up to **2.7T tokens**)
    *   Evaluated in a **teacher-forced mode** using custom metrics.
*   **Metrics:**
    *   **L2 Distance (ℓ2):** Euclidean distance between predicted and ground truth embeddings.
    *   **Round-trip L2 Distance (ℓ2-r):** Euclidean distance after encoding, decoding, and re-encoding.
    *   **Mutual Information (MI):** Measures text coherence.
    *   **AutoBLEU:** Measures similarity between encoded and decoded sentences.
     *  **Paraphrase Ratio (PAR):** Measures the similarity of generated embeddings to preceding context embeddings
    * **Contextual Anaphora (CA):** Measures how much the model paraphrases the context
    *   **Fragility Score:** Measures a sentence's reconstruction quality after being perturbed by noise.
    *   **ROUGE-L:** Measures the similarity between generated and reference text.
    *  **Coherence:** Assesses the quality of text, normalized with a sigmoid function.
*   **Instruction Tuning:**
    *  **7B parameter Two-Tower models** are instruction-tuned.
    *   Compared with models like Gemma, Mistral, and Llama on summarization and expansion tasks.

## Key Findings
*   LCMs achieve competitive **ROUGE-L scores** on summarization tasks.
*   LCMs perform well in **summary expansion tasks**.
*   LCMs exhibit strong **zero-shot generalization** to many languages.
*   **Diffusion-based models** outperform Base-LCM and Quantized LCM.
*   Different **noise schedules** impact performance and training dynamics.
    *   Weighting strategies such as clamping SNR and using fragility scores can improve the quality of generated text.

## Explicit Planning
*   **Large Planning Concept Model (LPCM):** Trained to predict both next sentence concepts and **a plan concept**.
    *   Predicts a sequence of concepts, followed by a **break concept**.
    *   Uses a simplified single-model approach, and is shown to have **improved coherence** over the baseline model.

## Future Directions
*   Focus on **cross-modal and multimodal training data.**
*   Improve **multi-level planning**.
*   Develop **explicit planning capabilities.**
*   Explore additional **concept units** beyond sentences.
*   Continue exploring **new architectures.**
