# **12.1 — Deterministic Sampling Stability Across Model Scales**  
### *Sub‑module of Project 12.0 — Scaling–Precision Ideation Lab*

This experiment evaluates how **model parameter scale** influences **token‑level determinism**, **entropy**, and **sequence stability** under *identical decoding conditions*. Using a unified `llama.cpp` harness and the probabilistic verification pipeline from Project 11.0, we compare models from **270M → 70B parameters** to determine whether smaller LLMs truly behave “more deterministically” or simply exhibit **collapsed probability distributions**.

---

## **Overview**

Larger LLMs have richer latent manifolds and broader candidate token sets at the same temperature. Smaller models often appear more deterministic—but is this genuine determinism or just **low‑capacity entropy collapse**?

This module provides a controlled, empirical framework to measure:

- **Output entropy**  
- **Inter‑run token variance**  
- **Greedy alignment drift**  
- **Semantic divergence**  

across multiple seeds, quantization levels, and sampling configurations.

---

## **Theoretical Motivation**

### **Determinism Fallacy in Quantized Inference**  
Smaller models are often described as “rigid” or “deterministic.” In practice, quantization and reduced parameter capacity can compress the logit distribution, creating the *illusion* of determinism. This experiment tests whether that rigidity persists under controlled sampling noise.

### **Scale vs. Latent Capacity Trade‑Off**  
Larger models maintain broader top‑k/top‑p candidate sets at identical temperatures. This raises the question:

> Does parameter scale inherently increase variance, or should temperature be scaled non‑linearly with model size?

Explore more:  
- latent capacity  
- logit entropy  
- temperature scaling

---

## **Hypotheses**

### **Primary Hypothesis**

\[
\mathcal{H}_{12.1}: \quad \text{Var}_{\text{token}}(M_{\text{small}}) < \text{Var}_{\text{token}}(M_{\text{large}}) \quad \Big|_{T>0,\;Q=\text{const}}
\]

### **Null Hypothesis**  
Token variance is **independent** of parameter count when temperature, top‑p, and quantization are fixed.

### **Alternative Hypothesis**  
Smaller models exhibit **lower variance** due to narrower pre‑softmax logit distributions.

---

## **Experimental Setup**

### **Model Matrix**

| Scale | Model |
|------|--------|
| Small | Gemma 3 270M, Gemma 3 1B |
| Medium | Llama 3.1 8B |
| Large | Llama 3.3 70B |

### **Controlled Variables**

- **Quantization:** Q4_K_M (uniform across all models)  
- **Sampling:**  
  - Temperatures: 0.0, 0.2, 0.7  
  - Top‑p: 0.9  
  - Fixed seeds: \(S_0 \dots S_n\)  
- **Prompt Domain:**  
  - Mathematical conjectures  
  - Functional equations  
  - Structured YAML/JSON transformations  

### **Pipeline**

```
Prompt Dataset
   └──► llama.cpp (quantized inference)
           ├──► Gemma 3 (270M, 1B)
           ├──► Llama 3.1 (8B)
           └──► Llama 3.3 (70B)
                 │
                 └──► Logit Extraction → Entropy Analysis → SWI‑Prolog Divergence Checks
```

---

## **Metrics**

### **Token Jaccard Similarity (Jₜ)**  
Measures inter‑run token overlap across 50 identical prompts.

### **Shannon Logit Entropy (Hₛ)**  
\[
H_s(t) = - \sum_i P(x_i) \log_2 P(x_i)
\]

Quantifies distribution spread at each generation step.

### **Greedy Drift (Δg)**  
Difference between non‑zero‑temperature outputs and the greedy baseline.

Learn more:  
- entropy metrics  
- token stability

---

## **Results Matrix (Pending)**

| Parameter Scale | Model | Mean Entropy (T=0.7) | Jₜ Stability (T=0.2) | Greedy Drift (Δg) |
|-----------------|--------|-----------------------|------------------------|--------------------|
| 270M | Gemma 3 270M | *Pending* | *Pending* | *Pending* |
| 1B | Gemma 3 1B | *Pending* | *Pending* | *Pending* |
| 8B | Llama 3.1 8B | *Pending* | *Pending* | *Pending* |
| 70B | Llama 3.3 70B | *Pending* | *Pending* | *Pending* |

---

## **Project Integration**

### **Upstream Dependencies**
Builds on the scaling‑precision framework from **Project 12.0** and the probabilistic verification mechanics from **Project 11.0**.

### **Downstream Impact**
Results inform:

- Rule‑constrained decoding in **Project 10.0**  
- Symbolic lattice generation in **Project 13.0**  

Explore related modules:  
- Project 12.0  
- Project 11.0  
- Project 13.0

---

## **Repository Structure**

```
/experiments/12.1/
    ├── prompts/
    ├── configs/
    ├── results/
    ├── analysis/
    └── prolog/
```

---

## **Status**

**Current Phase:** Data collection & entropy extraction  
**Next Steps:**  
- Expand prompt dataset  
- Run full 50‑seed matrix  
- Integrate SWI‑Prolog divergence proofs  
- Publish comparative entropy plots

---

