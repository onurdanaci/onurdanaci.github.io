
# Quantum Patent Review: Siemens and EP4506865

**TL;DR:** I agree with the EPO’s examiners that this patent application is weak in terms of clarity and novelty. However, I strongly disagree with the assertion that this invention is purely mathematical and lacks a technical nature. Furthermore, the examiners' insistence that the Siemens team name specific qubit hardware—despite the invention being hardware-agnostic—highlights a fundamental disconnect in how quantum algorithms are currently being assessed.

**Very important note coming after many months**: Tencent already had a patent grant by EPO on the QDARTS business! EP4006788B1. I didn't know about this. This fact damages the novelty of the Siemens application even further, and demonstrate that it is in fact pretty doable to show that QDARTS has a technical effect.

---

## The "Clarity" Issue and the Moving Parts

I want to discuss this particular patent, **EP4506865**, because it has many moving parts. This complexity led the European Patent Office (EPO) examiner to hoist the red flag of “clarity” in the formal **European Search Opinion (ESO)**. 

To understand the critique, we have to look at what the invention actually accomplishes. What is the problem they are claiming to solve? 

**Point 0: What is the task?** The task is designing “good” quantum circuits and optimizing their parameters classically. These circuits implement a quantum version of Reinforcement Learning (RL). It is critical to note it is not the other way around: the task is *not* to use classical RL to design circuits for a quantum task. This distinction is not entirely clear from reading the patent alone, which understandably led the examiner to raise a Clarity issue under Art. 84 EPC.

### i) Motivation and Purpose
The goal is to implement an RL algorithm to solve a particular optimization or control problem on a quantum chip. The problems can range from the rudimentary **Cart-Pole** problem to sophisticated tasks like the **control of plasma in nuclear fusion**. These are tasks where an agent does trial-and-error runs to chase a signal (a reward or "Q-function").

In classical deep RL, the input-output mapping is handled by a neural network. In Quantum RL, this mapping is handled by a **parameterized quantum circuit**. This warrants two distinct optimizations:
1.  **Parameter Optimization:** Optimizing the angles $\theta$ of the circuit to maximize long-term reward.
2.  **Structural Optimization:** Choosing the array of input-output maps based on a structural parameter $\alpha$, which determines the "expressivity" of the ansatz.

The purpose is to choose both better ansatze (by optimizing $\alpha$) and better parameters for those ansatze (by optimizing $\theta$).

---

## ii) Technical Implementation: How is it actually done?

We must distinguish between the optimization of circuit parameters $\theta$ (the "weights") and the structural parameters $\alpha$ (the "architecture"). While $\theta$ optimization is standard in hybrid classical-quantum algorithms (using solvers like COBYLA or SPSA), the joint optimization of $\alpha$ and $\theta$ is the difficult bit.

### The DARTS Framework
**Differentiable ARchiTecture Search (DARTS)** is a framework for "Auto-ML." Since a combination of gates is not naturally differentiable, we use two main tricks:
1.  Continuous relaxation of categorical variables.
2.  The **Monte-Carlo score-function trick** (aka REINFORCE).

In the quantum case, continuous relaxation often fails, leaving us with the score-function gradient. This is critical because on real quantum hardware, we cannot use pathwise gradients; we need unitary gates. We cannot offload this to classical simulation as the Hilbert space grows exponentially. 

### Q-DARTS and the "Super-Circuit"
The patent utilizes the **Q-DARTS** framework, where training is sequential. At each iteration $j$, $K$ circuits are sampled based on the current architecture parametrization $\alpha_j$. The patent calls this set of $K$ circuits a **super-circuit**. 

The average loss function is evaluated at the optimized $\theta$:
$f(\theta;\alpha) = \frac{1}{K}\sum_k \mathcal{L}_k(\theta_k)$

This yields a score-function evaluation used to compute the gradient signal:
$\nabla_\alpha = \frac{1}{K}\sum_k \mathcal{L}_k(\theta_k)\nabla_\alpha \log( p(\theta_k;\alpha) )$

This gradient $\nabla_\alpha$ updates $\alpha$ so that the next set of structures is sampled. The implementation proposes using this for **Quantum Q-learning**, incorporating an adaptive pruning mechanism as the search progresses. 

---

## My Disagreement with the Search Opinion

While the examiner's argument about the lack of **Novelty** is likely justified (the adaptive pruning is a weak original contribution), I strongly disagree with the claim that the invention lacks **technical character**.

### 1. The Double Standard in Auto-ML
I have looked into similar patents in classical Auto-ML and architecture search. The EPO has granted patents for classical algorithms implementing nearly identical logic! It appears the examiner teams for classical and quantum are disjointed. It makes no sense that an Auto-ML algorithm on a classical chip is "technical," while on a quantum chip it is suddenly "non-technical" or "purely mathematical."

### 2. The Hardware "Game"
The examiner criticized Siemens for not naming specific qubit hardware. The lawyers could have easily name-dropped: **transmons, fluxoniums, neutral Rydberg atoms, ion traps, or quantum dots**. But they didn't. 

These tricks can be implemented on *any* of these hardwares. The list of hardware types is growing super-polynomially. Forcing the inventors to specify hardware for a general architecture search algorithm is like forcing a compiler architect to name every specific transistor model the code might run on. 

The Siemens team could have argued that their architecture set was limited by a specific hardware's **native gate-set** or **qubit-to-qubit connectivity**. Perhaps they chose not to "play the game." Evidently, they grew so frustrated with the process that they totally rescinded their application.

---

### References
* [Monte Carlo Gradient Estimation in Machine Learning (NeurIPS 2019)](https://arxiv.org/abs/1906.10652) - Shakir Mohamed, Mihaela Rosca, Michael Figurnov, Andriy Mnih (DeepMind).

```{tableofcontents}