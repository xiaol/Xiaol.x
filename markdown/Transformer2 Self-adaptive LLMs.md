
# Transformer2: A Self-Adaptive LLM Framework

## Core Idea
*   **Self-Adaptive LLMs:** Models that adjust to varied tasks and dynamic contexts in real-time without external intervention
*   **Two-Pass Inference:**
    *   **Pass 1 (Dispatch):** Identifies task properties by observing test-time behavior
    *   **Pass 2 (Adaptation):** Combines task-specific "expert" vectors to modify LLM weights and generate the answer

## Key Components

### 1. Singular Value Fine-tuning (SVF)
*   **Parameter-Efficient Fine-Tuning (PEFT):** A method to modify a pre-trained model for downstream tasks while minimizing the number of trainable parameters
*   **Mechanism:** Tunes only the **singular values** within the model's weight matrices
    *   Weight matrix W is decomposed into UΣV^T, where Σ is a diagonal matrix of singular values
    *   SVF modifies Σ to Σ' = Σ ⊗ diag(z) using a learned vector z
*   **Benefits:**
    *   **Reduced Overfitting Risk**
    *   **Lower Computational Demands**
    *   **Greater Compositionality**
    *   **Fewer Parameters:** Requires significantly fewer parameters compared to LoRA
    *   **Principled Regularization**: Modifies pre-existing singular components
    *   **End-to-end optimization with RL**

### 2. "Expert" Vectors
*   **Task-Specific Components:** Each vector specializes in one type of task (e.g., math, code, reasoning)
*   **Training:** Trained using reinforcement learning (RL) to scale the singular values of weight matrices
    *   RL optimizes directly for task performance with a reward based on correctness
    *   Regularized with a KL divergence penalty to avoid drifting too far from the base model

### 3. Adaptation Strategies
*   **A) Prompt-based Adaptation:**
    *   Uses a prompt to ask the LLM to categorize the input prompt
    *   Selects an expert vector based on the category
    *   Provides an "others" category to use base weights if no expert is appropriate
*   **B) Classification Expert Adaptation:**
    *   Fine-tunes the base LLM itself using SVF for task identification
    *   Selects the appropriate expert for the input prompt
*   **C) Few-Shot Adaptation:**
    *   Uses a linear combination of expert vectors weighted by coefficients (αk)
    *   **Cross-Entropy Method (CEM):** Searches for the optimal combination of expert vectors
        *   Iteratively evaluates performance of sampled combinations on held-out prompts

## Advantages of Transformer2
*   **Dynamic Adaptation:** Adapts to new tasks in real time
*   **Efficiency:** More efficient than traditional fine-tuning and methods like LoRA
*   **Scalability:** Provides a scalable solution for enhancing LLM adaptability
*   **Compositionality:** Enables combining different capabilities for problem solving
*   **Modularity**: Supports continual learning by adding new skills without losing previous ones
*  **Overfitting Resistance**: SVF's approach mitigates the risk of overfitting
*  **Principled Regularization**: Only modifies the magnitude of pre-existing singular components

## Experimental Results
*   **SVF Outperforms LoRA:** SVF provides considerable performance gains with fewer parameters
    *   LoRA showed sporadic performance degradation
    *   SVF is more stable than LoRA when trained with RL
*   **Transformer2 Adaptation:** Improves performance across various tasks
    *   **Monotonic Performance Benefit:** Performance improves with more test-time information
    *   **Few-Shot Adaptation:** Generally achieves the highest scores
    *   **Cross-Model Transfer:** SVF experts can be transferred across different LLMs
*   **Ablation Studies**:
    *  MLP and attention updates via SVF improve performance, with MLP updates resulting in more pronounced gains
   * RL objective is superior to next-token prediction for task-specific fine-tuning
*  **Task Classification:** The classification strategies are effective at matching prompts to experts trained in similar domains
*   **Time Cost**: First pass inference time for self-adaptation is significantly less than second pass which is spent solving the problem

## Limitations and Future Work
*   **Expert Capabilities:** Tied to the latent components of the base model
    *   Model merging can help combine specialized models for more capable models
*   **CEM Scaling:** Scaling to many specialized domains may introduce computational costs
*  **Future Research:**
   * Explore other evolution algorithms for adaptation
    *   Investigate the use of past expert performance for expert selection
    *  Disentangling and recycling task-specific skills for newer/larger models

## Key Terms
*   **Self-Adaptive LLMs**
*   **Transformer2**
*   **Singular Value Fine-tuning (SVF)**
*   **Expert Vectors**
*   **Reinforcement Learning (RL)**
*   **Low-Rank Adaptation (LoRA)**
*   **Singular Value Decomposition (SVD)**
*   **Cross-Entropy Method (CEM)**
*   **Mixture of Experts (MoE)**
*   **Parameter-Efficient Fine-Tuning (PEFT)**
*    **REINFORCE algorithm**
*    **Catastrophic Forgetting**

## Cast of Characters
*   **Qi Sun:**  Prompted-based method, evaluation framework, experiments
*   **Edoardo Cetin:**  Few-shot CEM adaptation strategy, experiments, manuscript writing
*   **Yujin Tang:** Project lead, core algorithm, initial experiments, manuscript management
*   **Other Researchers:** Cited for their work on scaling laws and LoRA, and other related topics
