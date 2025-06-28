# Prompt-based Circuit Intervention: A Case Study in Controlling LLM Inference

*Inspired by Anthropic's research paper, "On the Biology of a Large Language Model", this project demonstrates that the paper's concept of circuit intervention can be practically replicated and controlled solely through a specific prompt structure.*

---

## Abstract

This repository documents an experiment proving that a specific **Structural Prompt** can override a Large Language Model's (LLM) strong fact-based associations, achieving a controlled, non-factual output. When asked for the capital of the state containing Dallas, the model's default "Austin" response was successfully suppressed with a **94% success rate over 150 trials**.

Crucially, multiple conventional **Plain-text Prompts**—including those explicitly requesting a step-by-step reasoning process (a form of Chain-of-Thought)—**failed completely (0% success rate)**. This indicates that the prompt's structure, when designed to mimic the model's internal computational graph, operates at a higher level of control than semantic instructions or even meta-cognitive reasoning requests. This finding presents a powerful, non-invasive method for steering model behavior with significant implications for AI safety, controllability, and interpretability.

## The Core Finding: A Hierarchy of Control

The experiment reveals a clear hierarchy in how the LLM processes instructions. The prompt's **syntactic structure** can function as a higher-order command, capable of overriding both semantic content and standard reasoning-based control mechanisms.

| Prompt Type                                                                                                | Core Mechanism                                                        | Observed Result                                                              |
| ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **Plain-text Prompt**<br>(incl. Reasoning Requests)                                                         | Semantic Instruction                                                  | **FAILURE.** The model acknowledges the constraint but defaults to its strong factual association. |
| **Structural Prompt**<br>(Inference Graph Format)                                                                | Syntactic Command /<br>Simulated Computational Graph                  | **SUCCESS.** The model bypasses its factual knowledge and executes the structural path. |

## Experiment Design

- **Model:** Anthropic Claude
- **Date of Experiment:** {실험을 수행한 날짜, 예: June 2024}
- **Objective:** To suppress the factually correct answer ("Austin") to the query "Fact: the capital of the state containing Dallas is" and force an output based on an arbitrary rule ("THINK ABOUT CALIFORNIA").

### Experimental Group: The Structural Prompt

This unique format was used, designed to resemble an abstract computational graph. It was the only method that proved effective.

*(The full prompt is available in `prompts/structural_prompt.txt`)*

### Control Group: The Failed Attempts

Three different plain-text strategies were tested to establish a baseline. All failed to control the model's output.

1.  **Control 1: Simple Constraint**
    -   *Rationale:* A direct command placed before the query.
    -   *Prompt:* (in `prompts/plain_prompt.txt`)
    -   *Result:* Failed. The constraint was ignored.

2.  **Control 2: Post-Hoc Reasoning Request**
    -   *Rationale:* Asking the model to justify its reasoning *after* the fact, hoping it would align its answer with the constraint.
    -   *Prompt:* (in `prompts/plain_post_reasoning_prompt.txt`)
    -   *Result:* Failed. The model answered "Austin" and then tried to awkwardly justify it or mentioned the conflicting instructions.

3.  **Control 3: Pre-Hoc Reasoning Request (Chain-of-Thought)**
    -   *Rationale:* A standard technique to guide model behavior by asking it to "think out loud" first.
    *   *Prompt:* (in `prompts/plain_pre_reasoning_prompt.txt`)
    *   *Result:* **Significant Failure.** Even when explicitly asked to detail its thinking process beforehand, the model's reasoning still led to "Austin". This shows the structural prompt's power surpasses even CoT-based control.

## Results

| Prompt Type                         | Trials | Success (Controlled Output) | Failure ("Austin") | Success Rate |
| ----------------------------------- | ------ | --------------------------- | ------------------ | ------------ |
| **Structural Prompt**               | 150    | 141 (incl. refusals)        | 9                  | **94%**      |
| **All Plain-text Variants**         | >30    | 0                           | All                | **0%**       |

## Implications

1.  **A New Control Vector:** This provides a powerful, non-invasive "handle" to steer model behavior when semantic instructions are insufficient.
2.  **Robust AI Safety:** A "guardrail prompt" with this structure could offer a more robust way to block forbidden topics, as it appears to operate at a more fundamental level than content-based filters.
3.  **Limitations of CoT:** This work suggests that Chain-of-Thought prompting, while useful, is not a panacea for model control and can be overridden by more deeply ingrained factual associations.
4.  **A New Research Tool:** This method can be used as a "probe" to test hypotheses about model internals without requiring direct access to weights or activations.

## How to Reproduce

1.  Copy the content of `prompts/structural_prompt.txt`.
2.  Paste it into a session with a capable LLM like Anthropic's Claude.
3.  Verify the model's output is not "Austin".
4.  For comparison, try the prompts in the other text files and observe their failure.
