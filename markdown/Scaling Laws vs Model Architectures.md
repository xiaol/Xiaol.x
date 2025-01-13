
# Scaling Laws and Model Architectures

## Main Research Question
*   Do different model architectures scale differently?
*   How do inductive biases affect scaling behavior?
    *   Impacting both pretraining and downstream transfer performance
    *   Improvements at a specific scale do not always transfer to other scales

## Key Findings
*   Architecture is crucial for scaling
    *   The best performing model fluctuates at different scales
    *   No single "best" architecture across all scales
*   Upstream vs. Downstream Discrepancy
    *   Models with good pretraining perplexity may not transfer well to downstream tasks
*   Systematic Evaluation is needed
    *   Evaluate performance across various scales
    *   Move beyond pointwise comparisons
*  Inductive bias significantly affects scaling
    * Different architectures have unique inductive biases
     * Some inductive biases are more amenable to scaling
*   Scaling Law Variations: Each model has a different scaling law, captured by the  α  value
    *   α represents how well a model scales relatively across all scales
*   Different Scaling Protocols Model architectures react differently to varying scaling of depth and width

## Model Architectures Studied
*   Transformer Variants
    *   Transformers (vanilla)
    *  Evolved Transformers
    *   Universal Transformers (UT)
    *   Switch Transformers
*   Efficient Transformer Variants
    *   Performer
    *  Funnel Transformer
    *  ALBERT
*  General Improvements
     *   Mixture of Softmaxes (MoS)
    *  Gated Linear Units (GLU)
*   Non-Transformer Architectures
    *   Lightweight Convolutions
    *   Dynamic Convolutions
    *   MLP-Mixers

## Experimental Setup
*   Pre-training:  English C4 corpus, 219 steps
    *   Inverse square root learning rate with Adafactor
    *   SentencePiece tokenizer with 32K subwords
*   Fine-tuning: GLUE, SuperGLUE, and SQuAD datasets
*   Hardware: 16 TPU-v3 chips with Mesh TensorFlow
*   Model Sizes: Tiny, Small, Base, Large, XL

## Evaluation Metrics
*   Upstream (Pre-training): Negative log-perplexity
    *   Lower values indicate better performance in language modeling
*   Downstream (Transfer): Accuracy on GLUE, SuperGLUE, SQuAD
    *   Evaluates how well representations adapt to specific NLP tasks
 *  Downstream SQuAD accuracy

## Implications
*   Careful Model Selection: Practitioners should be cautious about developing architectures based on upstream performance alone, as it may not translate to good downstream results.
*   Model evaluation should consider performance across various scales
*  Inductive biases should be carefully considered when developing new models
*   A 'one size fits all' approach to scaling may not work
*   Novel inductive biases can be risky when scaling large models
*   More testing on downstream accuracy is needed

## Key Terms
*   Inductive Bias: Assumptions a learning algorithm makes to generalize from training data to unseen data
*   Scaling Laws: Relationships between model performance and factors like size, data, or compute
*  Parameter Sharing:  Reusing the same weights across layers or operations, reduces trainable parameters
    *   Used by Universal Transformers and ALBERT
*   FLOPs: Floating Point Operations per Second, measure of computational cost
*   Negative Log-Perplexity: Measure of how well a language model predicts a sequence of words
*  Throughput:  Number of operations or data points a system can process per unit of time
    *   The inverse of processing time
*   C4 Corpus: Large-scale, cleaned text dataset from Common Crawl
*  SentencePiece:  Subword tokenization algorithm
*  Adafactor: An adaptive learning rate optimization method
