
# STILL-2 Framework for Slow-Thinking LLMs

##  Core Concept:
    *   **Mimicking human-like reasoning** through extended thought processes before responding
    *   Aims for more **thorough, accurate, and well-reasoned solutions** compared to traditional LLMs
    *   Emphasizes the generation of **internal chains of reasoning steps** called "thoughts"

## I. The "Imitate, Explore, and Self-Improve" Framework

    ###  A. Imitate Phase
        *   **Goal:** Teach the LLM to generate both "thoughts" and the final solution in a single response
        *   **Method:** Fine-tuning with distilled **long-form thought data**
        *   **Data Source**: Distilled responses from "o1-like" systems like **DeepSeek-R1** and **QwQ**
        *   **Formatting:** Using tokens like `<|begin_of_thought|>`, `<|end_of_thought|>`, `<|begin_of_solution|>`, `<|end_of_solution|> `
        *   **Purpose**: To achieve both **format adherence** (slow-thinking response) and **ability elicitation** (activating slow-thinking mode)

    ### B. Explore Phase
        *   **Goal:** Expand the LLM's ability to tackle complex tasks
        *  **Method:** Generate multiple candidate solutions (rollouts) for challenging problems
        *   **Focus:** Generating **correct trajectories** that lead to accurate answers
         *   **Evaluation:** Comparing model output with ground truth answers
        *  **Purpose:** To produce **progressively better training data**

    ###  C. Self-Improve Phase
         *   **Goal:**  Further enhance reasoning capabilities
        *  **Method:** Iteratively refine training data using **successful trajectories** from the exploration phase
        *   **Techniques:**
            *   **Supervised Fine-tuning (SFT):** Training with high-quality, correct trajectories
            *   **Direct Preference Optimization (DPO):** Contrastive learning with paired correct/incorrect responses
        *   **Data Selection Metric**: Using perplexity as a method to select better performing data

## II. Key Components

    ### A. Long-Form Thought Data
        *   **Definition:** Data including both the reasoning process and the final solution
        *  **Importance:** Provides examples of reasoning patterns, enabling LLMs to replicate slow-thinking
        *   **Sources:** Distilled from "o1-like" systems like DeepSeek-R1 and QwQ
        *   **Characteristics**: Flexible, informal, and guiding LLMs towards correct solutions
    ### B. Data Mixture
        *   **Domains:** Includes mathematics, coding, science, and puzzles
        *   **Difficulty**: Focuses on **challenging problems** to enhance reasoning
         *   **Findings:** Harder problems improve performance; math data generalizes well to other domains
    ### C. Optimization Methods
        *   **Supervised Fine-Tuning (SFT):**  Fine-tunes models on high-quality trajectories, using perplexity for data selection
         *   **Direct Preference Optimization (DPO):**  Optimizes using paired correct/incorrect instances
                *   Can be applied to the whole response, or only the thought portion
### D. Evaluation Metrics and Benchmarks
       *   **Benchmarks**: Evaluated on MATH-OAI, AIME, and GPQA
       *   **Performance**:  STILL-2 shows competitive performance compared to industry systems, particularly with more training data

## III. Experimental Results and Findings

    *   **Industry-level systems** perform well, especially on AIME
    *   **Distillation-based approaches** achieve competitive results
    *   **Iterative training with exploration** enhances performance
    *   **Larger datasets improve performance**
    *   **Hard problems are crucial** for developing slow-thinking skills
    *   **Long-form thinking generalizes** across domains
    *   **Aligning only the thought process in DPO** can be effective

## IV. Limitations and Future Directions

    *   **Limited challenging problems** and rollouts
    *   **Performance fluctuations** in iterative training
    *   **Gap compared to industry-level systems** remains
    *   **Future Research:**
        *   Scale training to more complex tasks
        *   Extend exploration time/rollouts
        *   Explore alternative training methods like reinforcement learning
        *   Study different DPO alignment strategies
        *   Further study on the flexible nature of "thought" outputs