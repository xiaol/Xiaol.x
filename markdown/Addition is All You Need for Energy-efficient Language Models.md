
# L-Mul Algorithm for Energy-Efficient AI

## Central Idea
*   **Approximates floating-point (FP) multiplication** using integer addition

## Motivation
    *   **High energy consumption of AI models**, especially Large Language Models (LLMs)
        *   Significant electricity usage
        *   **Floating-point tensor multiplications** are the major source of energy consumption
        *   **Attention mechanisms** in transformers are computationally expensive
    *   Need for **more energy-efficient AI**
        *   Focus on arithmetic operations

## L-Mul Algorithm Details
    *   **Replaces complex FP multiplication with simpler operations**
        *   Traditional FP multiplication: involves complex exponent additions, mantissa multiplications, and rounding.
        *  **Mantissa multiplication** has a complexity of O(m²)
        *   **L-Mul:** uses a bit-shift and integer addition, reducing complexity to O(n)
    *   **Key Component: Bit-Shift Offset `2-l(m)`**
        *   **Replaces `xm * ym`** mantissa multiplication
        *   `l(m)` is defined as:
            *   `m if m <= 3`
            *   `3 if m = 4`
            *   `4 if m > 4`
        *   Approximates the effect of mantissa multiplication through a computationally cheaper offset
    *   **Implementation with Integer Addition**
        *   The `1+xm` part is handled implicitly by bit representation
        *   Mantissa carry is automatically added to exponent by the integer addition
        *   **Bit operations:**
            *   `L-mul(x, y) = x ⊕ y` (sign bit calculation)
            *   `L-mul(x, y)[1:] = x[1:] + y[1:] - offset` (exponent and mantissa calculation)
   *   **Complexity:** O(n), where n is the number of bits, compared to O(m²) for traditional FP multiplication, where m is the bits of the mantissa

## Advantages of L-Mul
    *   **Reduced energy consumption**
        *   Up to **95% reduction for element-wise multiplications**
        *   Up to **80% reduction for dot products**
        *   Integer addition much cheaper than FP multiplication
            *   fp32 multiplication is 37x more energy than int32 addition
   *   **High Precision**
        *   L-Mul with **4-bit mantissa comparable to float8 e4m3**
        *   L-Mul with **3-bit mantissa outperforms float8 e5m2**
    *   **Easy integration** into models
        *   Can replace multiplications in attention, linear transformations, etc.
    *   **Orthogonal** to other optimization efforts, like I/O optimization

## L-Mul in Practice
    *   **Attention Mechanism**
        *   Replaces standard FP matrix multiplication
        *   Minimal performance loss
    *  **Various Benchmarks**
       * MMLU, BBH, Common Sense QA, Visual QA, GSM8k, etc.
        *   L-Mul performs comparably or better than float8 and sometimes bfloat16
    *   **Fine-tuning**
        *   L-Mul fine-tuned models achieve similar performance as standard fine-tuned fp8 models
    *  **Can be used in pre-trained models directly**

## Future Directions
    *   **Hardware implementation**
    *   **Development of APIs**
    *   **Training L-Mul native AI models**