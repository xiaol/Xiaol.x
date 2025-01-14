
# SNOOPI Framework

## Core Goal
*   **Enhance one-step diffusion models**
    *   Improve training stability
    *   Enable negative prompt guidance

## Problems Addressed
### Training Instability
*   **Variational Score Distillation (VSD)** framework is unstable
    *   **Fixed guidance scale** leads to instability
    *   Inconsistent performance across different architectures
*   Existing solutions add computational burden or require image supervision

### Lack of Negative Prompt Guidance
*   One-step models lack negative prompting ability
*   Limits control and flexibility
*   Direct application of Classifier-Free Guidance (CFG) fails

## SNOOPI Components
### **Proper Guidance - SwiftBrush (PG-SB)**
*   **Enhances training stability**
*   **Dynamic guidance scale**
    *   Varies the guidance scale of teacher models during training
    *   Randomly samples scale from a uniform distribution
*   **Broadens teacher model output distributions**
    *   Makes teacher models more adaptable
    *   Applies varied guidance to both frozen and LoRA teachers
*   **More robust VSD loss**
*   **Improves adaptability** across different backbones
*   **No additional computational cost**

### **Negative-Away Steer Attention (NASA)**
*   **Enables negative prompt guidance** in one-step models
*   **Manipulates cross-attention layers**
    *   Filters features by subtracting attention responses to negative prompts from positive prompts
*   **Filters in intermediate feature space**
    *   Removes undesirable elements before image generation
    *   Avoids blending artifacts
    *  Calculates attention outputs for positive (Z+) and negative (Z-) text prompts
    *  Combines to create final feature set: **ZNASA = Z+ - α * Z-**
*   **Training-free method**
*   Applies to both **one-step and few-step** models

## Key Methodologies
### Variational Score Distillation (VSD)
*   Aligns output distributions of teacher and student models
*   Enables rapid, high-quality text-to-image generation
*   Uses a two-teacher approach
    *   Fixed pre-trained diffusion model
    *   Adaptive LoRA-based teacher
*   PG-SB enhances VSD's stability

### Classifier-Free Guidance (CFG)
*   Enhances image quality
    *   Blends conditional and unconditional model predictions
*   PG-SB improves upon CFG by dynamically varying guidance scale

### Cross-Attention Layers
*   Capture semantic connections between image and text
*   NASA manipulates these layers to filter unwanted features

## Results and Evaluation
### Metrics
*   **Human Preference Score v2 (HPSv2)**: primary metric
    *   Correlates better with human perception of image quality
*   CLIP scores: for text alignment
*   Precision/Recall: for image quality and diversity
*  Fréchet Inception Distance (FID): reported for completeness

### Performance
*   **PG-SB**: Consistently achieves superior HPSv2 scores
*   **NASA**: Effectively excludes unwanted features via negative prompts
    *   Preserves key semantic features while filtering out unwanted attributes
*   **State-of-the-art HPSv2 score of 31.08** achieved on PixArt-α backbone when combining both PG-SB and NASA
*   **Restores image-free status** of SwiftBrushv2, by removing the need for image supervision

## Limitations
*   **PG-SB**: Lacks support for few-step models
*  **NASA**:
    *  Requires appropriate scale factor for negative feature removal
    *   Primarily designed for cross-attention architectures

## Conclusion
*   **SNOOPI improves stability and control in one-step diffusion models**
*   **PG-SB enhances stability** via dynamic guidance scaling
*   **NASA enables negative prompting** via cross-attention adjustments
*   Outperforms strong baselines
*   Offers more stable training and better image quality
