# Auto-ML (AML) & Architecture Search (AS)

I have decided to run commentaries on patents regarding the classical and quantum Auto-ML and architecture search business. This stems from two factors. First, having published and reviewed extensively in this field. Second, having been embedded in an ecosystem of researchers with strong opinions on these methods.

In the upcoming posts, I will explore the curious discrepancy in how "similar" technical arguments are treated by patent offices depending on whether they are framed as Classical or Quantum.

### Cases under Review:

* **Quantum: [EP4506865 (Siemens)](/content/patents/auto_ml/quantum/2026-04-13-first-post)**    

    This patent explores Quantum Architecture Search for a **Quantum Reinforcement Learning (QRL)** task. Rather than a simple classical search, it employs a Monte-Carlo score-function and super-circuit gradient-based approach to simultaneously train and identify optimal Variational Quantum Circuit (VQC) ansatze for Q-learning. Despite this sophisticated integration of architecture search within the reinforcement learning framework, the European Search Opinion (ESO) was unexpectedly harsh, leading Siemens to withdraw.

* **Classical: [EP4396730 (Mitsubishi)](/content/patents/auto_ml/quantum/2026-04-13-first-post)**

    In contrast, this patent covers automated setup for variational neural networks (like VAEs). It utilizes Bayesian methods and pruning for architecture search and prior selection. While technically parallel in its goal of automating the design of probabilistic systems, this application was granted smoothly.

To an outsider, this schism is a peculiar phenomenon. Beyond the "quantum vs. classical" labeling, the underlying logic of automating complex architecture selection is not significantly different. 

**Explore the detailed commentaries below:**

```{tableofcontents}