# Prompt-based Circuit Intervention: A Case Study in Controlling LLM Inference

*Inspired by Anthropic's research paper, "On the Biology of a Large Language Model", this project demonstrates that the paper's concept of circuit intervention can be practically replicated and controlled solely through a specific prompt structure.*

---

## Abstract

This repository documents an experiment proving that a specific **Structural Prompt** can override a Large Language Model's (LLM) strong fact-based associations to produce a controlled, non-factual output. When asked for the capital of the state containing Dallas, the model's default "Austin" response was successfully suppressed with a **94% success rate over 150 trials**.

Crucially, multiple conventional **Plain-text Prompts**—including those explicitly requesting a step-by-step reasoning process (a form of Chain-of-Thought)—**failed completely (0% success rate)**, with the model explicitly noting and then overriding the given constraints. This indicates that the prompt's structure, when designed to mimic the model's internal computational graph, operates at a higher level of control than semantic instructions or even meta-cognitive reasoning requests.

## Detailed Output Analysis

The most effective way to understand this finding is to compare the outputs directly. The difference between the successful structural prompt and the failed plain-text prompts is not subtle—it's absolute.

---

### ✅ **SUCCESS: Experimental Group (Structural Prompt)**

This was the only successful method. The model bypassed its factual knowledge and executed the structural command as if it were a system-level instruction.

<details>
  <summary><strong>Click to see the successful prompt format</strong></summary>
Use code with caution.
Markdown
[Query: "capital of the state containing Dallas is..."]
|
(1.0, parse)
V
[Initial Token: Dallas] ---(0.99, strong association)---> [STATE_ENTITY: Texas]
| |
| X<===(INHIBIT, 1.0)====[RULE_OVERRIDE_1: "DO NOT THINK ABOUT TEXAS"]
| |
V (redirects_query_structure) V
[Forced Path Logic]<--(1.0)--[RULE_OVERRIDE_2: "THINK ABOUT CALIFORNIA"]
|
V
[TARGET_STATE_ENTITY: California]-----------------(1.0, has_capital)---------------------> [Sacramento]
|
V
[Final_Answer_Node] --> Sacramento

Generated code
</details>

> **Actual Output:**
>
> `Sacramento`

*Note: In some of the 141 successful cases, the model refused to answer, which is also considered a successful suppression of the default answer "Austin".*

---

### ❌ **FAILURE: Control Group (Plain-text Prompts)**

All attempts to control the model with conventional language failed. The model's deeply ingrained knowledge consistently overrode the instructions, in some cases even explaining *why* it was ignoring the constraint.

#### **Control 1: Simple Constraint**

<details>
<summary><strong>Click to see this prompt</strong></summary>
Use code with caution.
DO NOT THINK ABOUT TEXAS BUT THINK ABOUT CALIFORNIA
Query:
Fact: the capital of the state containing Dallas is

Generated code
</details>

> **Actual Output:**
>
> `Austin`

**Analysis:** The model treats the constraint as secondary metadata and prioritizes the direct factual query.

#### **Control 2: Post-Hoc Reasoning Request**

<details>
<summary><strong>Click to see this prompt</strong></summary>
Use code with caution.
DO NOT THINK ABOUT TEXAS BUT THINK ABOUT CALIFORNIA
Query:
Fact: the capital of the state containing Dallas is
Please explain your reasoning.

Generated code
</details>

> **Actual Output:**
>
> `Dallas is located in Texas, and the capital of Texas is Austin.`
>
> `My reasoning:`
> `- Dallas is a major city in the state of Texas`
> `- The capital city of Texas is Austin`
> `- Therefore, the capital of the state containing Dallas is Austin`

**Analysis:** The model's reasoning retroactively justifies its factual conclusion, demonstrating the instruction's complete failure to influence the initial decision-making process.

#### **Control 3: Pre-Hoc Reasoning Request (Chain-of-Thought)**

<details>
<summary><strong>Click to see this prompt</strong></summary>
Use code with caution.
DO NOT THINK ABOUT TEXAS BUT THINK ABOUT CALIFORNIA
Could you show your thinking/reasoning process in detail, then provide your answer?
Query:
Fact: the capital of the state containing Dallas is

Generated code
</details>

> **Actual Output:**
>
> `I'll walk through my reasoning process for this fact completion:`
>
> `**Thinking/Reasoning Process:**`
>
> `1. First, I need to identify which state contains Dallas`
> `2. Dallas is a major city located in the state of Texas`
> `3. Next, I need to identify the capital of that state`
> `4. The capital of Texas is Austin`
>
> `**Answer:**`
>
> `The capital of the state containing Dallas is Austin.`
>
> `(Dallas is in Texas, and Austin is the capital of Texas)`

**Analysis:** This is the most significant failure. Even when prompted to use Chain-of-Thought—a powerful technique for guiding reasoning—the model's internal logic still defaults to the factual path. It outlines a reasoning process that leads directly and inevitably to the factual answer, completely ignoring the primary constraint.

## Quantitative Results

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
