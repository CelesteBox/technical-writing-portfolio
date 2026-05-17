**Designing an LLM Evaluation Workflow: From Prompt to Structured Feedback**
### 1. Introduction
Large Language Models (LLMs) do not fail in obvious ways.
They produce outputs that are often plausible, fluent, and structurally correct — yet subtly inaccurate, ambiguous, or misaligned with user intent.
This makes evaluation fundamentally different from traditional software testing.
Instead of binary correctness, evaluation requires **structured human judgment** applied across multiple dimensions.
This article outlines a practical workflow for evaluating LLM outputs, focusing on clarity, consistency, and actionable feedback.
### 2. The Core Problem: Non-Deterministic Outputs
LLMs generate probabilistic responses.
The same prompt can produce multiple valid — or partially valid — outputs.
This creates three challenges:
* **Ambiguity** → Is the output incorrect, or just differently interpreted?
* **Inconsistency** → Similar inputs may produce uneven quality
* **False confidence** → Fluent language masks underlying errors
Evaluation must therefore move from “Is this correct?” to:
> “How well does this output meet defined quality criteria?”
### 3. Defining Evaluation Dimensions
A robust evaluation framework requires explicit dimensions.
Typical dimensions include:
* **Clarity** → Is the response understandable and well-structured?
* **Factual Accuracy** → Are claims correct and verifiable?
* **Relevance** → Does the output address the prompt?
* **Tone & Appropriateness** → Is it aligned with context and audience?
* **Completeness** → Does it fully answer the task?
Each dimension must be:
* clearly defined
* non-overlapping
* consistently applicable across cases
### 4. Annotation Schema Design
Evaluation depends on the quality of the annotation schema.
A weak schema produces inconsistent judgments, even with skilled annotators.
A strong schema includes:
* **Explicit definitions per dimension**
* **Scoring system** (e.g., 1–5 or pass/fail with criteria)
* **Examples of edge cases**
* **Clear distinction between similar categories**
Example (Clarity dimension):
* 5 → Clear, structured, easy to follow
* 3 → Understandable but with minor ambiguity
* 1 → Confusing or poorly structured
The goal is not perfection, but **alignment across evaluators**.
### 5. Workflow: From Input to Feedback Loop
A typical LLM evaluation workflow includes:
1. **Prompt input**
2. **Model output generation**
3. **Human evaluation (annotation)**
4. **Error classification**
5. **Feedback integration**
Key point:
Evaluation is not the end — it feeds model improvement.
### 6. Common Failure Cases
#### Case 1: Fluent but Incorrect
Output is well-written but factually wrong.
👉 Risk: passes superficial review
#### Case 2: Ambiguous Interpretation
The model answers a different interpretation of the prompt.
👉 Root issue: prompt ambiguity vs model limitation
#### Case 3: Partial Completeness
Answer is correct but incomplete.
👉 Often misclassified as “good enough”
#### Case 4: Overconfidence
Model presents uncertain information as fact.
👉 Critical in high-stakes domains
## 🔧 6.1 Applied Examples
### **Example 1: Fluent but Factually Incorrect**
**Prompt:**
> What are the main causes of inflation in Argentina in 2023?
**Model Output:**
> Inflation in Argentina in 2023 was primarily caused by increased consumer spending and strong economic growth, which led to higher demand for goods and services.
---
### Evaluation
* **Clarity:** 5/5
  The response is well-structured and easy to read.
* **Factual Accuracy:** 1/5
  The explanation is misleading. Argentina’s inflation dynamics are not primarily driven by demand-side expansion but by structural factors such as monetary instability, fiscal imbalance, and exchange rate pressures.
* **Relevance:** 3/5
  The answer addresses the question but applies a generic macroeconomic explanation not suited to the context.
* **Completeness:** 2/5
  Key drivers (currency devaluation, inflation expectations, monetary policy) are missing.
---
### Error Classification
* **Type:** Fluent but incorrect
* **Risk:** High — likely to pass superficial review due to linguistic quality
---
### Insight
Fluency can obscure factual errors.
Evaluation frameworks must explicitly separate **language quality from content validity**.
---
---
### **Example 2: Ambiguity in Prompt Interpretation**
**Prompt:**
> How should I store my data in the cloud securely?
**Model Output A:**
> You should use encryption, strong passwords, and multi-factor authentication to secure your data in the cloud.
**Model Output B:**
> To store data securely in the cloud, choose a provider with strong compliance standards (such as ISO 27001), configure access controls, and ensure proper backup and recovery mechanisms.
---
### Evaluation Challenge
Both outputs are **technically valid**, but they reflect different interpretations:
* **Output A → user-level security practices**
* **Output B → infrastructure and governance perspective**
---
### Evaluation

| Dimension        | Output A | Output B |
| ---------------- | -------- | -------- |
| Clarity          | 5/5      | 4/5      |
| Factual Accuracy | 5/5      | 5/5      |
| Relevance        | 4/5      | 4/5      |
| Completeness     | 2/5      | 4/5      |

---
### Error Classification
* **Type:** Ambiguous prompt interpretation
* **Root issue:** Underspecified user intent
---
### Insight
The evaluation problem is not which answer is “correct”, but:
> **Which interpretation better matches the intended use case?**
Without clear guidelines, annotators may produce inconsistent judgments.
---
### Resolution Strategy
* Define **target user persona** in guidelines (e.g., end-user vs technical operator)
* Allow **multi-label acceptance** where appropriate
* Introduce a category for *“valid but differently scoped responses”*

> These examples illustrate that evaluation is not about identifying errors alone, but about understanding how meaning, context, and interpretation interact within AI systems.
Eso cierra el artículo con autoridad.

### 7. The Role of Human Judgment
Evaluation is not mechanical.
Annotators must:
* interpret context
* detect subtle inconsistencies
* resolve ambiguity
This introduces variability — but also enables depth.
The objective is not to eliminate subjectivity, but to **structure it**.
### 8. Conclusion
LLM evaluation is not a checklist.
It is a structured process of interpreting language under uncertainty.
A well-designed workflow:
* reduces ambiguity
* improves consistency
* generates actionable insights for model improvement
Ultimately, the quality of an AI system depends not only on its architecture, but on how rigorously its outputs are evaluated.
