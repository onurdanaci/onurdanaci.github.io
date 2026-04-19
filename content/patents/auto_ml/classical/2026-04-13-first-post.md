## Classical: EP4396730 (Mitsubishi)

TL;DR: I agree with the EPO examiners that this is a patentable technical invention from the field of (classical) AutoML. It indeed is mathematically sound, and uses this math in a rather complicated setting. These mathematical gadgets they brought help Bayesian neural networks adapt to new conditions. However, the preparation of the patent, its use of language makes a stark contrast with some other rejected patents in the related fields. In my humble opinion, EPO’s two hurdle approach in this field is not an “exact science”, but rather a semantic filter and a “game” patent applicants are expected to play. Quoting from a patent lawyer’s comments I see online: EPO and BoA definitions of “technical” are an amalgamation of negative-definitions (what it is NOT) rather than a strict positive-definition (what it IS).  If the applicants of these rejected patents made the same effort “playing this game”, they wouldn’t be outwardly rejected, as their ideas are no less technical. I think even “name dropping” certain real-world tasks, data, hardware, problems and solutions would have worked in these outwardly rejected patents’ favour. Because these technical advancements are already there, but they don’t name it for some reason.

What does the paper & its related patent achieve?

The main source of ideas of the patent actually stems from a paper of the same authors, Demir et al.[*]. We can split their goals and methodology into a few steps:

Challenge & Motivation: The existing frameworks that process and handle the physiological data (e.g., EG/EMG) coming from bio-medical devices either are agnostic to the inter-personal and inter-environmental differences (aka nuisances) in the data, leading to low performance. Or, they are too specific for each of these subjects or environments. Handling these challenges all at once has an utmost importance. The technical ™️ challenge they address is the adaptability of signals and their processors in bio-medical devices using machine learning techniques. They need to handle this such that each device does not need a new, costly (both time and money) re-calibration for each subject.

How do they do it?: They devise an AutoML framework specific for Bayesian neural networks known as AutoBayes[*]. This framework uses Bayes ball algorithm (a simple yet powerful algorithm from the field of Bayesian networks, graph theory, and probability theory) to prune the causal relationships between various sources of data (raw data X, latent connections Z, labels Y, nuisances S) based on very intuitive principles. Pruning the Bayes graph leads to a conditional independence roadmap (i.e., what data sources need to be connected at all). And, this roadmap is utilized to further simplify relations between different data sources (pruning of a factor graph). This way, instead of training neural networks between each combination of different data sources, or letting an ML practitioner to intuit these connectivities by trial and error, they have an educated way to train neural networks that make such connections. Most importantly, based on different context and different data, the neural networks are also trained accordingly. For example, inter-personal differences in EEG data is a nuisance (S). If the Bayes ball algorithm recognizes which of these differences are irrelevant to the task in hand, AutoBayes can just adapt the already existing (already trained) framework to ignore these nuisances by only training a censor (adversarial) network. In addition to this, the patent adds some adaptations to make the framework developed in AutoBayes more expressive: automated selection of an assortment of prior/posterior probability distributions (not only the “vanilla” Gaussian, but also Cauchy, logistic etc.), loss functions (not only KL-divergence, but other Renyi divergences). They even pose these choices as an effective hyperparameter and architecture optimization.

What is the intuition behind the Bayes ball algorithm? How does it work? A simple intuition:

1. **The Variables (The Real-World Setup):**

   In the paper's framework, inference involves four types of variables: Task labels ($Y$), Nuisance variations ($S$), Latent representations ($Z$), and Observable data ($X$). Let's define two of each for a backyard environment:

   - Tasks ($Y$): What we want to infer. $Y_1$: Did it rain? (True/False) $Y_2$: Did the automated sprinkler run? (True/False)

   - Nuisances ($S$): Contextual factors that alter the environment but aren't what we care about inferring.$S_1$: Season (Summer vs. Autumn).$S_2$: Topography (Flat yard vs. Sloped yard).

   - Latents ($Z$): Hidden, unmeasured physical states caused by $Y$ and $S$.$Z_1$: Sub-surface soil moisture.$Z_2$: Surface water pooling (puddles).

   - Data ($X$): The raw, noisy sensor measurements we actually observe. $X_1$: Amount of mud accumulated on your shoes after walking across the yard. $X_2$: Shininess of the grass blades measured by a camera.

2. **The Fully Connected Bayes Graph ($\mathcal{B}_0$)**

   A Bayesian graph models the generative process—how reality created the data. To define the all-to-all connected base graph $\mathcal{B}_0$, you assume a topological ordering, such as $Y \rightarrow S \rightarrow Z \rightarrow X$, and connect every node to every node that comes after it. Mathematically, this represents a completely unconstrained joint probability distribution utilizing the chain rule:

   $$p(Y_1, Y_2, S_1, S_2, Z_1, Z_2, X_1, X_2) = p(Y_1) \cdot p(Y_2|Y_1) \cdot p(S_1|Y_1, Y_2) \dots p(X_2|Y_1, Y_2, S_1, S_2, Z_1, Z_2, X_1)$$

   In reality: $\mathcal{B}_0$ assumes absolute chaos. It assumes the Season ($S_1$) is caused by whether the Sprinkler ran ($Y_2$). It assumes the shininess of the grass ($X_2$) is directly dictated by the Season ($S_1$) independent of the actual puddles ($Z_2$). This is an overparameterized hypothesis.

3. **Pruning to the Target Bayes Graph ($\mathcal{B}$)**

   We prune $\mathcal{B}_0$ by cutting edges based on logical independence, creating a sparser graph $\mathcal{B}$. This represents a specific hypothesis about how the backyard actually works (analogous to Models A through K in the paper).

   **Example Pruning Logic**: Cut $Y \rightarrow S$: The Season ($S_1$) and Topography ($S_2$) dictate their own states; they are not caused by Rain ($Y_1$) or Sprinklers ($Y_2$). Cut direct $Y \rightarrow X$ and $S \rightarrow X$: The mud on your shoes ($X_1$) and grass shininess ($X_2$) are purely physical results of moisture ($Z_1$) and puddles ($Z_2$). If you know $Z$, you don't need to know the Season or the Sprinkler schedule to determine the sensor readings. The pruned graph $\mathcal{B}$ now defines a much simpler factorization, similar to equation (3) in the paper:

   $$p(Y, S, Z, X) = p(Y)p(S)p(Z|S, Y)p(X|Z)$$

4. **Bayes-Ball and the Independence List ($\mathcal{I}$):**

   The Bayes-Ball algorithm is a mechanical set of rules applied to $\mathcal{B}$ to test if information can "flow" between nodes under certain conditions. If a path is blocked, the variables are conditionally independent. By running Bayes-Ball on our pruned backyard graph $\mathcal{B}$, we generate the independence list $\mathcal{I}$.

   Examples of what $\mathcal{I}$ would contain:

   $\{X_1 \perp Y_1 \mid Z_1, Z_2\}$: Mud ($X_1$) is independent of Rain ($Y_1$) given Moisture and Puddles ($Z$). If you already perfectly know the state of the puddles and soil moisture, knowing that it rained adds zero new information about how muddy your shoes will get.

   $\{Y_1 \perp Y_2 \mid \emptyset\}$: Rain ($Y_1$) and Sprinklers ($Y_2$) are marginally independent. Crucial Step: If Bayes-Ball reveals that a specific latent feature (e.g., $Z_2$) is marginally independent of a nuisance variable (e.g., $S_1$), the AutoBayes algorithm flags this as a structural justification to attach an adversarial network during training to enforce that disentanglement.

5. **From Full Factor Graph ($\mathcal{F}_0$) to Pruned Factor Graph ($\mathcal{F}$):**

   While the Bayes graph models the generative physics (Reality $\rightarrow$ Data), the Factor graph models the inference network (Data $\rightarrow$ Guessing reality). The full-chain inference factor graph $\mathcal{F}_0$ attempts to infer everything from the data $X$. An unpruned "Z-first" strategy expands to:

   $$p(Y, S, Z \mid X) = p(Z \mid X) \cdot p(S \mid Z, X) \cdot p(Y \mid S, Z, X)$$

   To get the final inference graph $\mathcal{F}$, we plug the independencies from $\mathcal{I}$ directly into $\mathcal{F}_0$ to cancel out redundant conditional dependencies.

   Plugging $\mathcal{I}$ into $\mathcal{F}_0$:

   Because $\mathcal{I}$ tells us that $S$ is independent of $X$ given $Z$ ($S \perp X \mid Z$), the term $p(S \mid Z, X)$ simplifies to $p(S \mid Z)$. Because $\mathcal{I}$ tells us $Y$ is independent of $X$ given $Z$ and $S$, the term $p(Y \mid S, Z, X)$ simplifies to $p(Y \mid S, Z)$.

   The Final Result ($\mathcal{F}$):
   $$p(Y, S, Z \mid X) = p(Z \mid X) \cdot p(S \mid Z) \cdot p(Y \mid S, Z)$$

6. **The Translation to Neural Networks:**

   This final equation $\mathcal{F}$ is not just math; it is the exact blueprint for wiring the neural network:

   $p(Z \mid X)$ becomes the Encoder Network: It takes sensor data (Mud, Shininess) and outputs a latent vector (estimating Moisture, Puddles).

   $p(S \mid Z)$ becomes the Nuisance Estimator: It takes the latent vector and attempts to predict the Nuisances (Season, Topography).

   $p(Y \mid S, Z)$ becomes the Classifier Network: It takes the latents and the nuisance estimates to output the final Task prediction (Rain, Sprinklers).

Disclaimer: Obviously this is a rather trivial, intuitive example. In real life bio-medical data these nuisances you want to censor via adversarial networks are in fact biological markers that depend on the person.

What is the missed opportunity for the rejected applicants?

I want to specifically refer to the Bosch Reinforcement Learning case (went to European Board of Appeals T 1952/21) that I referred to in my blog, and the quantum differential architecture search for the quantum reinforcement learning case (Siemens EP4506865) that I dedicated a whole post to in my blog. Deep down, both the Bosch RL and the Siemens QRL patent applications have a technical effect: the former improves the RL exploration that can be very well used in a real life setting such as a robotic arm. Or, the latter can decrease redundant gates, hardware noise sources on a physical quantum hardware while the said hardware runs an RL algorithm tackling a real life control task (e.g., nuclear fusion plasma control). And, as I noticed pretty late that EPO already granted a patent (EP4006788B1) to stuff pretty relevant to Siemens QRL application. So the EPO’s two hurdle approach of: i) Any hardware?, ii) Any technical effect? is not an impossible ask. Just remember that the patent application is different from a NeurIPS or ICLR application, different from a Kaggle competition. Invoking toy data sets such as MNIST for neural networks, or toy environments such as Cart Pole or ATARI games for RL alone won’t cut it for a patent application. Rather, do what this Mitsubishi patent does: show your data has physical sources, collected by a hardware, its inter-contextual variations are pruned and handled by your method, and all of these have real life consequences. Even me, a mere passenger, learned a ton from reading these and contrasting their approaches.

[*]https://arxiv.org/abs/2007.01255




