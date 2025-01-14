
# Cosmos World Foundation Model Platform

## Central Idea
*   **Building customized world models for Physical AI**
    *   Uses **digital twins** for both the AI (policy model) and the world (world model)
    *   Aims to train models digitally first

## Key Components
    *  **Video Curation Pipeline**
        *   **Shot Detection**: Splits videos into clips
             *   Discards clips < 2s, splits clips > 60s
             *   Uses ShotBench
        *  **Filtering**:  Locates high-quality, dynamic, information-rich subsets
        *  **Annotation**: Uses a VLM to label videos and generate captions
        *   **Deduplication**: Semantic de-duplication for a diverse, compact dataset
        *   **Sharding**: For data distribution
    *   **Video Tokenizers**
        *   **Purpose**: Transforms videos into compact tokens
        *   **Types**:
            *   **Continuous Tokens**: Represented as vectors
                *   Modeled with a vanilla autoencoder (AE)
            *  **Discrete Tokens**: Represented as integers
                 *   Use Finite-Scalar-Quantization (FSQ)
         *  **Cosmos Tokenizer Suite**: 
              *   Handles high-resolution images and videos
               *  Aspect-ratio-agnostic
              *  Temporally causal design
               *  **Architecture:** Uses wavelet transform, residual blocks, spatio-temporal 3D convolutions, causal self-attention
              *  **Performance:** High visual reconstruction quality, fast inference
    *   **Pre-trained World Foundation Models (WFMs)**
        *   Trained using large-scale video datasets
        *   Two approaches:
            *   **Diffusion Models**: Based on denoising
                 *    Latent diffusion models using continuous tokens
                 *   Training follows EDM approach
                 *   Uses denoising score matching loss
            *   **Autoregressive Models**: Based on next-token prediction
                *  Llama3-style GPT models for video prediction
                *  Uses discrete tokens
                 *  T5 embeddings for text prompts
        *   **Examples:**
            *   **Text2World Models**: Maps text prompts to videos
                *   *Cosmos-1.0-Diffusion-7B-Text2World*, *Cosmos-1.0-Diffusion-14B-Text2World*
            *   **Video2World Models**: Predicts future video from current video and text prompt
                 *  *Cosmos-1.0-Diffusion-7B-Video2World*, *Cosmos-1.0-Diffusion-14B-Video2World*
                  * *Cosmos-1.0-Autoregressive-5B-Video2World*
            *   **Autoregressive models**: *Cosmos-1.0-Autoregressive-4B*
    *   **Post-training WFMs**
        *   Fine-tuned pre-trained WFMs for specific applications
        *   Examples:
            *   **Camera Control**: For 3D navigable worlds
            *   **Robotic Manipulation**: Instruction and action based
            *   **Autonomous Driving**: With multi-view support
                *   *Cosmos-1.0-Diffusion-7B-Text2World-Sample-MultiView*
                *    *Cosmos-1.0-Diffusion-7B-Text2World-Sample-MultiView-TrajectoryCond*
                *   *Cosmos-1.0-Diffusion-7B-Video2World-Sample-MultiView*
     *   **Guardrail System**
          *   **Pre-Guard**: Blocks harmful inputs
               *   **Keyword Blocking**: Uses a hard-coded blocklist
               *   **Aegis Guardrail**: Uses an LLM based content safety system
           *   **Post-Guard**: Blocks harmful outputs
                *   Video content safety classifier
                *   Face blur filter

## Key Technical Details
*   **Temporal Causality**:
    *   Processes video frames sequentially using past and current frames, not future
    *   Achieved through causal temporal convolutions and causal attention
*   **Tokenizer Training**:
    *   Joint training of images and videos
    *   Supervised only on the final decoder output
    *   Avoids auxiliary losses like commitment loss and KL prior loss
*   **Diffusion Model Details**:
    *   Uses denoising score matching loss
    *   Parameterizes the denoiser using EDM
*   **Autoregressive Model Details**:
    *  Formulated as next-token prediction
    *   Uses cross-attention for text conditioning
    *   Employs Medusa heads for faster inference
*  **Memory Optimization**:
     *   **Fully Sharded Data Parallelism (FSDP)**: Shards model parameters, gradients and optimizer states across devices
     *   **Context Parallelism (CP)**: Distributes activation memory across GPUs
*  **Cosmos Diffusion Decoder**:
     *  Fine-tunes a diffusion model to enhance the output of the autoregressive WFM

## Evaluation Metrics
*   **3D Consistency**: Measured using novel view synthesis and Sampson error
*   **Physics Alignment**:  Compares predicted dynamics to ground truth with pixel-level, feature-level, object level metrics
*   **Instruction Following**: Evaluates video generation based on instruction accuracy and object permanence
*   **Multi-View Consistency**: Assessed on the ability to generate coherent surround-view videos

## Applications
*   **Verification**: Assessing complex scenes
*   **Planning and Model Predictive Control**: Simulating and optimizing actions
*   **Model-Based Reinforcement Learning**: Learning from simulated environments
*   **Autonomous Driving**: Realistic driving simulators
    *   Generates driving videos from diverse inputs

## Key Takeaways
*   **Comprehensive platform** for building custom world models
*   **Advanced tokenization** with high compression and fidelity
*   **Scalable training** using FSDP and CP
*   **Versatile models** that can be fine-tuned for diverse Physical AI applications
*  **Open-source and open-weight** for broader use and research
