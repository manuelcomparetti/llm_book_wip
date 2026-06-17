# LLMs and Agentic Workflows: A Technical Crash Course
## Merged and Revised Syllabus — 12 Lessons

**Audience:** Software engineers and computer scientists with graduate-level knowledge of computer architecture and prior exposure to basic intelligent systems and neural networks at an introductory level. Mathematical maturity is assumed: comfort with linear algebra, multivariate calculus, and probability is required. No prior deep learning specialisation is assumed.

**Mathematical treatment (global policy):** Hybrid. Key results and formulas are stated precisely with full notation. The most foundational derivations (backpropagation, scaled dot-product attention, the policy gradient theorem) are carried out in full. Secondary results are stated with proofs sketched or referenced. Intuitive framings accompany all formal statements. Code is excluded; worked conceptual and numerical examples are used throughout.

**Reference policy:** Each lesson lists Primary References (fully specified: author, year, title, venue, identifier where applicable) and Further Reading (curated, annotated). Agents writing chapters must integrate primary references naturally into the text and must not invent or modify citations.

---

## Lesson 1 — From Symbolic AI to Statistical Intelligence: Foundations of Machine Learning

### Summary

This chapter establishes the conceptual and historical ground on which the entire course rests. It answers a question that is rarely posed explicitly but is prerequisite to understanding everything that follows: why does the field of artificial intelligence look the way it does today, and what intellectual commitments does the dominant paradigm carry?

The chapter opens with the question of what intelligence is, computationally. A working definition is proposed: intelligence is the capacity to construct internal models that enable successful prediction and action in an environment. This definition is deliberately agnostic about mechanism — it accommodates biological neural systems, symbolic programs, and statistical models equally — and is introduced not as settled philosophy but as a productive framing device.

The first major historical arc covers symbolic AI: the programme, dominant from the 1950s through the 1980s, that sought to represent knowledge explicitly as symbols and to implement intelligence as rule-governed manipulation of those symbols. The founding works of Newell and Simon are the anchor. Its structural weakness is then exposed: the knowledge acquisition bottleneck. Encoding human expertise as rules requires human effort per fact; the world scales exponentially while annotation scales linearly.

The second arc introduces the statistical turn: instead of encoding knowledge, we infer it from observations. This reframes AI as an optimisation problem over a parameterised function class, driven by data. The chapter presents this shift as arguably the most consequential idea in the history of artificial intelligence.

Learning is formalised as function approximation: given a dataset of input-output pairs drawn from an unknown joint distribution P(X,Y), find a function f: X → Y from a hypothesis class H that generalises well to unseen data. Generalisation is treated rigorously as a problem. The distinction between training error and test error is introduced, along with Empirical Risk Minimisation (ERM): minimising the average loss on training data as a proxy for minimising expected loss on the true distribution. Bias and variance are introduced as the two competing sources of the generalisation gap.

The chapter then introduces entropy and information theory as the natural mathematical language for machine learning. Shannon entropy H(X) = −Σ p(x) log p(x) is defined, interpreted as average uncertainty, and connected to the cross-entropy loss used in classification and language modelling. The KL divergence is introduced as a measure of distributional discrepancy. A key conceptual anchor: an LLM is, at its core, an entropy-reduction machine — it takes a context and reduces uncertainty about what comes next.

The chapter closes with two ideas that recur throughout the course. First: a system that predicts efficiently must discover regularities; discovering regularities means compressing information; compression produces internal representations; representations can produce behaviour resembling understanding. This chain — prediction → compression → representation → apparent understanding — is the conceptual spine of the course. Second: whether reasoning is a qualitatively distinct capacity or an emergent consequence of sufficiently powerful prediction is one of the most important open questions in contemporary AI, and the course will return to it without pretending to resolve it.

### Primary References

- Newell, A. & Simon, H.A. (1976). "Computer Science as Empirical Inquiry: Symbols and Search." *Communications of the ACM*, 19(3), 113–126.
- Shannon, C.E. (1948). "A Mathematical Theory of Communication." *Bell System Technical Journal*, 27(3–4), 379–423 and 623–656.
- Vapnik, V.N. (1998). *Statistical Learning Theory*. Wiley. [Chapters 1–3: ERM, VC dimension, structural risk minimisation]
- Bishop, C.M. (2006). *Pattern Recognition and Machine Learning*. Springer. [Chapter 1: probability, decision theory, information theory fundamentals]
- Mitchell, T. (1997). *Machine Learning*. McGraw-Hill. [Chapter 1: the learning problem as function approximation]
- Minsky, M. & Papert, S. (1969). *Perceptrons: An Introduction to Computational Geometry*. MIT Press. [Introduction and Chapter 1: limits of linear threshold units; symbolic AI critique]

### Further Reading

- Domingos, P. (2012). "A Few Useful Things to Know About Machine Learning." *Communications of the ACM*, 55(10), 78–87. [Accessible synthesis of core ML principles; recommended supplementary reading]
- Shalev-Shwartz, S. & Ben-David, S. (2014). *Understanding Machine Learning: From Theory to Algorithms*. Cambridge University Press. [Chapters 1–5: rigorous treatment of PAC learning, ERM, and generalisation bounds]
- LeCun, Y., Bengio, Y. & Hinton, G.E. (2015). "Deep Learning." *Nature*, 521, 436–444. [Authoritative overview by the field's principal architects; useful historical and forward-looking context]

### Agent Writing Brief

Write Chapter 1 of a 12-chapter graduate-level technical textbook on LLMs and agentic systems. Title: "From Symbolic AI to Statistical Intelligence: Foundations of Machine Learning." Target length: approximately 10,000 words. The reader is a software engineer or computer scientist with graduate-level knowledge of computer architecture and some prior exposure to basic AI; they can follow formal mathematical notation but have not specialised in machine learning.

The chapter must cover in sequence: (1) a working definition of intelligence as predictive modelling, framed as a tool not a philosophical commitment; (2) symbolic AI — programme, achievements, structural failure due to the knowledge acquisition bottleneck, citing Newell & Simon (1976); (3) the statistical turn — learning as inference from data, formally as function approximation over a hypothesis class H; (4) generalisation as a problem: training error, test error, overfitting, bias-variance tradeoff, ERM with the notation min_{h∈H} (1/n)Σᵢ L(h(xᵢ), yᵢ); (5) entropy and information theory: Shannon entropy, cross-entropy, KL divergence, with the explicit statement that an LLM is an entropy-reduction machine; (6) representation as compression: the prediction → compression → representation → apparent understanding chain; (7) the open question of whether reasoning is emergent from prediction, stated honestly as unresolved.

Mathematical treatment: state entropy and cross-entropy formulas precisely. Derive the connection between cross-entropy loss and maximum likelihood estimation. Use worked examples: fair coin versus biased coin for entropy; next-word prediction for cross-entropy. Do not include code. Tone: rigorous, expository, intellectually honest. Do not present the statistical turn as obviously correct; acknowledge what was lost with symbolic AI (transparency, compositionality). End with an explicit roadmap of the remaining 11 chapters.

---

## Lesson 2 — Neural Networks: Architecture, Computation, and Optimisation

### Summary

This chapter develops the mathematical and computational machinery of neural networks from first principles, treating the subject with rigour appropriate for an audience that will later need to understand why these systems behave as they do at large scale. It also introduces the dynamical systems framing of training, providing a vocabulary — attractors, saddle points, loss surface geometry — that will recur throughout the course.

The chapter begins with the perceptron: a single linear threshold unit, formalised as ŷ = σ(wᵀx + b). Rosenblatt's convergence theorem is stated. The XOR failure is proved by geometric argument. The multilayer perceptron (MLP) resolves this by stacking linear transformations interleaved with non-linearities, creating decision boundaries of arbitrary shape. The Universal Approximation Theorem (Cybenko, 1989) is stated precisely: a feedforward network with a single hidden layer of sufficient width can approximate any continuous function on a compact domain to arbitrary precision, given a non-polynomial activation. The theorem is an existence result; it provides no guidance on architecture, width, or training procedure.

Activation functions are covered with attention to their gradient properties. The sigmoid saturates for large |x|, producing near-zero gradients. ReLU, f(x) = max(0, x), has gradient exactly 1 for positive inputs, eliminating saturation — but introduces the dying ReLU problem. GELU, used in most modern LLMs, is presented as a smooth probabilistic approximation to ReLU.

The computational graph is introduced as the formal substrate of automatic differentiation. The backward pass computes ∂L/∂wᵢ for every parameter by applying the chain rule in reverse topological order, reusing intermediate values. The full derivation of backpropagation is carried out for a two-layer network and generalised. The chapter is explicit that backpropagation is reverse-mode automatic differentiation, not a distinct algorithm.

Gradient descent and its pathologies are then addressed. Mini-batch SGD is established as the practical standard. Adam (Kingma & Ba, 2014) is derived: it maintains running estimates of the first moment (mean gradient) m_t and second moment (uncentred variance) v_t, applies bias correction, and scales each parameter's update by its historical gradient magnitude. Initialisation is treated carefully: Xavier/Glorot (for sigmoid/tanh) and He (for ReLU) are derived from the requirement that activation variance remain stable across layers.

The dynamical systems perspective is then introduced. Training is a discrete-time trajectory w(t+1) = w(t) − η ∇L(w(t)) in high-dimensional parameter space. The loss surface is the landscape through which this trajectory evolves. Dauphin et al. (2014) demonstrate that in high dimensions, saddle points are far more prevalent than local minima. The chapter introduces attractors and basins of attraction, noting that large networks appear to have many equivalent minima with similar loss values.

Batch normalisation and layer normalisation are covered as stabilisation mechanisms. The key practical distinction: batch normalisation normalises across the batch dimension (sensitive to batch size, unsuitable for variable-length sequences); layer normalisation normalises across the feature dimension and is the standard in transformers.

### Primary References

- Rosenblatt, F. (1958). "The Perceptron: A Probabilistic Model for Information Storage and Organization in the Brain." *Psychological Review*, 65(6), 386–408.
- Rumelhart, D.E., Hinton, G.E. & Williams, R.J. (1986). "Learning Representations by Back-propagating Errors." *Nature*, 323, 533–536.
- Cybenko, G. (1989). "Approximation by Superpositions of a Sigmoidal Function." *Mathematics of Control, Signals and Systems*, 2(4), 303–314.
- Kingma, D.P. & Ba, J. (2014). "Adam: A Method for Stochastic Optimization." arXiv:1412.6980. [Published at ICLR 2015]
- He, K., Zhang, X., Ren, S. & Sun, J. (2015). "Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification." *Proceedings of ICCV 2015*, 1026–1034.
- Ioffe, S. & Szegedy, C. (2015). "Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift." *Proceedings of ICML 2015*, 448–456.
- Ba, J.L., Kiros, J.R. & Hinton, G.E. (2016). "Layer Normalization." arXiv:1607.06450.
- Goodfellow, I., Bengio, Y. & Courville, A. (2016). *Deep Learning*. MIT Press. [Chapters 6–8: MLPs, regularisation, optimisation]

### Further Reading

- Saxe, A.M., McClelland, J.L. & Ganguli, S. (2014). "Exact Solutions to the Nonlinear Dynamics of Learning in Deep Linear Networks." *ICLR 2014*. [Analytical treatment of training dynamics; foundational for the dynamical systems framing]
- Dauphin, Y., Pascanu, R., Gulcehre, C., Cho, K., Ganguli, S. & Bengio, Y. (2014). "Identifying and Attacking the Saddle Point Problem in High-dimensional Non-convex Optimization." *NeurIPS 2014*.
- Choromanska, A., Henaff, M., Mathieu, M., Arous, G.B. & LeCun, Y. (2015). "The Loss Surfaces of Multilayer Networks." *AISTATS 2015*.

### Agent Writing Brief

Write Chapter 2 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Neural Networks: Architecture, Computation, and Optimisation." Target length: approximately 10,000 words. The reader has completed Chapter 1 and understands supervised learning as function approximation, ERM, entropy, and the statistical paradigm.

Cover in sequence: (1) the perceptron — formal definition, convergence theorem stated, XOR failure proved geometrically; (2) MLP — stacked affine maps plus non-linearities, activation functions (sigmoid, tanh, ReLU, GELU) with gradient properties, Universal Approximation Theorem stated precisely with its limitations; (3) computational graph and backpropagation — derive the backward pass for a two-layer network step by step, then state the general reverse-mode autodiff result; (4) gradient descent and Adam — derive Adam from its moment estimates with bias correction, write all update equations explicitly; (5) initialisation — derive Xavier and He from variance-preservation requirements; (6) dynamical systems framing — training as trajectory in parameter space, loss surface geometry, saddle points versus local minima, attractors; (7) batch normalisation and layer normalisation.

Mathematical treatment: carry out the full backpropagation derivation. Include the Adam update equations with all components named and indexed. Derive the He initialisation formula. Use a concrete example: trace a forward and backward pass through a two-layer network with specific scalar values. Do not include code. Cite Saxe et al. (2014) and Dauphin et al. (2014) when making claims about loss surface geometry; do not overstate what is known about convergence of non-linear networks.

---

## Lesson 3 — Deep Learning and the Geometry of Representations

### Summary

This chapter addresses three questions that Chapter 2 leaves open: why does depth matter beyond what the Universal Approximation Theorem already guarantees? What is actually learned inside a deep network? And why do useful representations emerge from optimising a prediction objective?

On depth: Montufar et al. (2014) prove that deep ReLU networks produce exponentially more linear regions in input space than shallow networks of comparable parameter count. ResNets (He et al., 2016) confirm this empirically: skip connections h = F(x) + x allow 100+ layer networks to train reliably, changing the optimisation landscape by ensuring gradient flow.

Convolutional Neural Networks are introduced as the canonical architecture for spatially structured data. Convolution implements weight sharing across spatial positions — built-in translational equivariance. The AlexNet result (Krizhevsky et al., 2012) initiated the current era. Zeiler and Fergus (2014) provide the key interpretive insight: lower layers learn edge detectors; intermediate layers learn textures and parts; higher layers encode semantic concepts. This hierarchy of abstraction is the first empirical evidence that deep networks learn structured representations, not arbitrary pattern matchers.

Recurrent Neural Networks process sequences by maintaining a hidden state: hₜ = σ(Wₕ hₜ₋₁ + Wₓ xₜ + b). The repeated multiplication by Wₕ causes gradients to vanish or explode exponentially in sequence length. The LSTM (Hochreiter & Schmidhuber, 1997) resolves this with gated cell state dynamics. The full LSTM equations are presented with each gate defined precisely.

The chapter's theoretical core is an information-theoretic account of representation. The manifold hypothesis states that high-dimensional real-world data concentrates near low-dimensional manifolds. The Information Bottleneck principle (Tishby & Schwartz-Ziv, 2017) formalises what an optimal representation achieves: maximise I(Z;Y) while minimising I(Z;X). Deep networks appear to implement an approximate version of this compression through successive layers. This is presented as a conceptual framework rather than an established theorem — the empirical evidence is contested — but it provides the correct intuition for why representations emerge from prediction objectives.

The chapter closes by stating the RNN limitations that motivate Chapters 5–6: sequential computation prevents parallelisation; the fixed-dimension hidden state limits long-range dependency modelling; training on long sequences is slow. These are not engineering inconveniences but structural constraints.

### Primary References

- Krizhevsky, A., Sutskever, I. & Hinton, G.E. (2012). "ImageNet Classification with Deep Convolutional Neural Networks." *NeurIPS 2012*, 1097–1105.
- He, K., Zhang, X., Ren, S. & Sun, J. (2016). "Deep Residual Learning for Image Recognition." *Proceedings of CVPR 2016*, 770–778.
- Hochreiter, S. & Schmidhuber, J. (1997). "Long Short-Term Memory." *Neural Computation*, 9(8), 1735–1780.
- Zeiler, M.D. & Fergus, R. (2014). "Visualizing and Understanding Convolutional Networks." *ECCV 2014*, Lecture Notes in Computer Science 8689, 818–833.
- Bengio, Y., Courville, A. & Vincent, P. (2013). "Representation Learning: A Review and New Perspectives." *IEEE Transactions on Pattern Analysis and Machine Intelligence*, 35(8), 1798–1828.
- Tishby, N. & Schwartz-Ziv, R. (2017). "Opening the Black Box of Deep Neural Networks via Information." arXiv:1703.00810.
- Montufar, G., Pascanu, R., Cho, K. & Bengio, Y. (2014). "On the Number of Linear Regions of Deep Neural Networks." *NeurIPS 2014*, 2924–2932.

### Further Reading

- LeCun, Y., Bottou, L., Bengio, Y. & Haffner, P. (1998). "Gradient-Based Learning Applied to Document Recognition." *Proceedings of the IEEE*, 86(11), 2278–2324. [Original LeNet paper]
- Cho, K., van Merrienboer, B., Gulcehre, C., Bahdanau, D., Bougares, F., Schwenk, H. & Bengio, Y. (2014). "Learning Phrase Representations using RNN Encoder-Decoder for Statistical Machine Translation." *EMNLP 2014*, 1724–1734. [GRU]
- Olshausen, B.A. & Field, D.J. (1997). "Sparse Coding with an Overcomplete Basis Set: A Strategy Employed by V1?" *Vision Research*, 37(23), 3311–3325.

### Agent Writing Brief

Write Chapter 3 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Deep Learning and the Geometry of Representations." Target length: approximately 10,000 words. The reader has completed Chapters 1–2 and understands perceptrons, MLPs, backpropagation, Adam, and the dynamical systems framing of training.

Cover in sequence: (1) why depth matters — cite Montufar et al. (2014) on exponential linear regions; (2) skip connections and ResNets — present h = F(x) + x and its effect on gradient flow; (3) CNNs — convolution as weight sharing, hierarchy of learned features from Zeiler & Fergus (2014), AlexNet; (4) RNNs and LSTMs — derive vanishing gradient from repeated matrix product, present full LSTM equations with all gate definitions; (5) manifold hypothesis and Information Bottleneck — I(Z;Y) maximisation and I(Z;X) minimisation as compression objective, note the contested empirical status; (6) why RNN limitations motivate the transition to attention.

Mathematical treatment: include the full LSTM equations (input gate, forget gate, output gate, cell state update). Include the residual block formula. State the Montufar et al. linear regions result qualitatively. Present the Information Bottleneck with formal mutual information notation but without full derivation. Use examples: trace a single LSTM timestep with symbolic values; describe Zeiler & Fergus layer-by-layer findings concretely. Do not extend into the attention mechanism or transformers — these are Chapters 5 and 6.

---

## Lesson 4 — Reinforcement Learning and Adaptive Agents

### Summary

The previous three chapters addressed supervised learning: a fixed dataset, a fixed loss, a static optimisation problem. This chapter introduces reinforcement learning (RL) as a first-class intellectual framework for sequential decision-making under uncertainty. RL is treated independently, not merely as scaffolding for RLHF in Chapter 8. Its concepts — policies, value functions, the Bellman equations, the exploration-exploitation tradeoff — are necessary for understanding post-training procedures, agentic systems, and the general problem of goal-directed behaviour.

The Markov Decision Process (MDP) is the foundational formalism: a tuple (S, A, P, R, γ) where S is the state space, A the action space, P(s'|s,a) the transition function, R(s,a) the reward signal, and γ ∈ [0,1) the discount factor. The agent's goal is a policy π(a|s) that maximises expected discounted return Gₜ = Σ_{k=0}^∞ γᵏ R_{t+k+1}.

The value function V^π(s) = E_π[Gₜ | Sₜ = s] and action-value function Q^π(s,a) = E_π[Gₜ | Sₜ = s, Aₜ = a] are defined. The Bellman equations are derived as consistency conditions: V^π(s) = Σ_a π(a|s) Σ_{s'} P(s'|s,a)[R(s,a) + γV^π(s')]. Dynamic programming (value iteration, policy iteration) solves these exactly when P and R are known.

For the model-free case, Q-learning (Watkins, 1989) is the foundational tabular algorithm: Q(s,a) ← Q(s,a) + α[R + γ max_{a'} Q(s',a') − Q(s,a)]. Deep Q-Networks (DQN; Mnih et al., 2015) approximate Q(s,a;θ) with a neural network, introducing experience replay and target networks to stabilise training.

Policy gradient methods directly optimise the policy. The REINFORCE algorithm (Williams, 1992) uses the policy gradient theorem: ∇_θ J(θ) = E_π[∇_θ log π(a|s;θ) · Gₜ]. The log-derivative trick that produces this result is derived in full. Variance reduction via baseline subtraction and the advantage function A^π(s,a) = Q^π(s,a) − V^π(s) leads to actor-critic methods. Proximal Policy Optimisation (PPO; Schulman et al., 2017) constrains policy update magnitude via a clipped surrogate objective, which is presented in full and motivated.

A brief connection to control theory is drawn: the RL problem is structurally isomorphic to optimal control. The Bellman equations are the discrete-time analogue of the Hamilton-Jacobi-Bellman equation. The chapter closes by mapping the language model post-training problem onto the MDP formalism — states are prompt-response prefixes, actions are next tokens, rewards come from a reward model — so that RLHF in Chapter 8 reads as applied RL, not an opaque recipe.

### Primary References

- Sutton, R.S. & Barto, A.G. (2018). *Reinforcement Learning: An Introduction* (2nd ed.). MIT Press. [Chapters 1–4: MDPs, dynamic programming; Chapter 13: policy gradients. Freely available at incompleteideas.net]
- Mnih, V., Kavukcuoglu, K., Silver, D. et al. (2015). "Human-Level Control through Deep Reinforcement Learning." *Nature*, 518(7540), 529–533.
- Williams, R.J. (1992). "Simple Statistical Gradient-Following Algorithms for Connectionist Reinforcement Learning." *Machine Learning*, 8(3–4), 229–256.
- Schulman, J., Wolski, F., Dhariwal, P., Radford, A. & Klimov, O. (2017). "Proximal Policy Optimization Algorithms." arXiv:1707.06347.
- Mnih, V., Badia, A.P., Mirza, M. et al. (2016). "Asynchronous Methods for Deep Reinforcement Learning." *Proceedings of ICML 2016*, 1928–1937. [A3C; introduces advantage estimation]

### Further Reading

- Bellman, R. (1957). *Dynamic Programming*. Princeton University Press. [Original derivation of the principle of optimality]
- Silver, D., Huang, A., Maddison, C.J. et al. (2016). "Mastering the Game of Go with Deep Neural Networks and Tree Search." *Nature*, 529(7587), 484–489.
- Bertsekas, D.P. (2019). *Reinforcement Learning and Optimal Control*. Athena Scientific. [Graduate-level treatment connecting RL to control theory]

### Agent Writing Brief

Write Chapter 4 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Reinforcement Learning and Adaptive Agents." Target length: approximately 10,000 words. The reader has completed Chapters 1–3 and understands supervised learning, neural networks, and backpropagation. The reader has not studied RL or control theory.

Cover in sequence: (1) the MDP formalism — define the full tuple (S, A, P, R, γ) precisely, with a concrete example (a grid world or a text generation episode); (2) value function and Q-function — derive the Bellman equations step by step from the definition of expected discounted return; (3) dynamic programming — value iteration and policy iteration, noting the requirement for known P and R; (4) Q-learning — update rule, off-policy nature; (5) DQN — neural function approximation, experience replay, target networks; (6) policy gradients — derive REINFORCE from the policy gradient theorem, carrying out the log-derivative trick explicitly; (7) variance reduction — baseline subtraction, advantage function, actor-critic; (8) PPO — the clipped surrogate objective L^CLIP written in full, motivation for clipping; (9) connection to optimal control — HJB equation analogy; (10) preview of RLHF — map the language model post-training problem onto the MDP.

Mathematical treatment: fully derive the Bellman equations. Derive the policy gradient theorem (the log-derivative trick is the key step — carry it out explicitly). Present the PPO objective with all terms defined. Use a concrete two-state MDP to illustrate value iteration convergence. Treat RL as a first-class intellectual framework; do not rush toward the LLM application.


---

## Lesson 5 — The Attention Mechanism: From Alignment to Self-Attention

### Summary

This chapter traces the origin of the attention mechanism through a sequence of motivated design choices, from the failure of encoder-decoder RNNs on long sequences to the formulation of scaled dot-product attention that underlies every modern transformer. Students arrive at the attention formula not by memorising it but by understanding why it is the natural resolution of a specific technical problem.

The starting point is the sequence-to-sequence architecture (Sutskever et al., 2014): an encoder RNN reads a source sequence and compresses it into a single fixed-length context vector c (the final hidden state); a decoder RNN generates the target conditioned on c. The pathology is structural: regardless of source length, all information must fit in a vector of fixed dimensionality. This is the bottleneck, and it is not a training failure.

Bahdanau, Cho, and Bengio (2015) proposed additive attention as the solution. At each decoder step t, a separate context vector cₜ = Σᵢ αₜᵢ hᵢ is computed as a weighted sum over all encoder hidden states {h₁,...,hT}. The weights αₜᵢ are produced by a small learned alignment network: eₜᵢ = vᵀ tanh(Ws sₜ₋₁ + Wh hᵢ), then softmax-normalised. This derivation is worked through fully. The attention weights are interpretable: high αₜᵢ means the decoder is attending strongly to source position i when generating output token t.

Luong, Pham, and Manning (2015) replaced the feedforward alignment network with a dot product: eₜᵢ = sₜ₋₁ᵀ hᵢ. This exposes the structural logic: the dot product measures similarity between the decoder's current state (a query for needed information) and each encoder state (a key containing information about the source). Attention selects the most relevant keys and returns a weighted combination of values. The query-key-value decomposition emerges naturally from this analysis.

Self-attention follows as the case where queries, keys, and values all come from the same sequence. Every position can attend to every other position, capturing long-range dependencies without sequential computation. The attention output at position i is: yᵢ = Σⱼ αᵢⱼ vⱼ, where αᵢⱼ = softmax(qᵢᵀ kⱼ / √d)ⱼ. All positions process in parallel via the matrix form: Attention(Q, K, V) = softmax(QKᵀ/√d)V. The scaling factor 1/√d is derived from the observation that without it, dot products between d-dimensional vectors grow as O(√d), pushing softmax into saturation.

The chapter closes with an information-theoretic framing: attention dynamically routes information through the network. Each attention pattern selectively amplifies or suppresses parts of the input based on content, implementing a learned context-dependent information filter. This framing — attention as dynamic information routing — connects to Chapter 1's entropy perspective and is more productive than the "matrix multiplication" framing for building intuition about large networks.

### Primary References

- Sutskever, I., Vinyals, O. & Le, Q.V. (2014). "Sequence to Sequence Learning with Neural Networks." *NeurIPS 2014*, 3104–3112.
- Bahdanau, D., Cho, K. & Bengio, Y. (2015). "Neural Machine Translation by Jointly Learning to Align and Translate." *ICLR 2015*. arXiv:1409.0473.
- Luong, M.T., Pham, H. & Manning, C.D. (2015). "Effective Approaches to Attention-based Neural Machine Translation." *EMNLP 2015*, 1412–1421.
- Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A.N., Kaiser, Ł. & Polosukhin, I. (2017). "Attention Is All You Need." *NeurIPS 2017*, 5998–6008. [Introduced for motivation; fully developed in Chapter 6]

### Further Reading

- Xu, K., Ba, J., Kiros, R., Cho, K., Courville, A., Salakhutdinov, R., Zemel, R. & Bengio, Y. (2015). "Show, Attend and Tell: Neural Image Caption Generation with Visual Attention." *ICML 2015*, 2048–2057.
- Graves, A., Wayne, G. & Danihelka, I. (2014). "Neural Turing Machines." arXiv:1410.5401. [Concurrent work on content-based addressing; alternative perspective on soft retrieval]

### Agent Writing Brief

Write Chapter 5 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "The Attention Mechanism: From Alignment to Self-Attention." Target length: approximately 9,000–10,000 words. The reader has completed Chapters 1–4 and understands MLPs, backpropagation, RNNs, LSTMs, vanishing gradients, and the basics of sequence modelling.

The chapter must follow the historical derivation precisely: (1) encoder-decoder RNNs and the bottleneck problem — include quantitative discussion of performance degradation as sequence length increases; (2) Bahdanau additive attention — write out the full alignment score formula, softmax normalisation, context vector computation; explain what the alignment weights represent with reference to linguistic alignment; (3) Luong dot-product attention — derive from Bahdanau's formulation, show how it exposes the query-key-value structure; (4) the Q/K/V abstraction — present this as a soft differentiable dictionary lookup; (5) self-attention — derive it as the case Q = K = V = input, show the parallelisation; (6) scaling by 1/√d — derive the variance argument; (7) the information-routing perspective — connect to entropy framing of Chapter 1.

This chapter must read as a detective story: each design choice is motivated by the failure of the previous one. Do not present self-attention as an obvious idea. Include a worked illustrative example tracing attention weights through a short (3–4 token) source sequence. The transformer architecture is introduced only in the final paragraph as the subject of Chapter 6; do not jump ahead.

---

## Lesson 6 — The Transformer Architecture: A Complete Technical Specification

### Summary

This chapter provides the full technical specification of the transformer. Every component is derived or motivated, every design choice is explained, and the distinctions between encoder-only, decoder-only, and encoder-decoder variants are established precisely.

Scaled dot-product attention from Chapter 5 is extended to multi-head attention (MHA): h parallel attention operations on linearly projected subspaces, outputs concatenated and projected back. MHA(X) = Concat(head₁,...,headₕ)W^O, where headᵢ = Attention(X Wᵢ^Q, X Wᵢ^K, X Wᵢ^V). With h heads and d_k = d_v = d/h, the total parameter count matches single-head attention. Clark et al. (2019) demonstrate empirically that different heads specialise in different relationship types. Geva et al. (2021) establish that the feedforward sublayer behaves as a key-value memory.

Positional encoding is necessary because self-attention is permutation-equivariant. The sinusoidal scheme: PE(pos, 2i) = sin(pos/10000^{2i/d}), PE(pos, 2i+1) = cos(pos/10000^{2i/d}), is derived and its key property established: PE(pos+k) is a linear function of PE(pos), enabling relative position representation. Limitations for long-context generalisation are noted, motivating Chapter 9's RoPE and ALiBi.

A full transformer block (encoder-style, pre-LN) applies two sublayers with residual connections and layer normalisation: (1) MHA; (2) position-wise FFN(x) = max(0, xW₁ + b₁)W₂ + b₂ with inner dimension 4d. The chapter derives the parameter count formula: approximately 12Ld² for a standard transformer with L layers and model dimension d.

The decoder block adds a third sublayer: masked self-attention (causal mask: eᵢⱼ = −∞ for j > i) followed by cross-attention over encoder states. Decoder-only models (GPT, LLaMA, Mistral) eliminate the encoder entirely. Encoder-only models (BERT) use bidirectional self-attention and are optimised for representation learning. The pre-LN versus post-LN distinction is analysed using Xiong et al. (2020).

Computational complexity: self-attention is O(n²d); the feedforward sublayer is O(nd²) per token. For n ≪ d, feedforward dominates; for large n, attention dominates. This is the quantitative basis for the context window scalability problem. FlashAttention (Dao et al., 2022) reduces attention memory cost from O(n²) to O(n) through IO-aware tiling — not an approximation but an exact reimplementation. It makes long-context training practically feasible and is standard in modern LLM training.

### Primary References

- Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A.N., Kaiser, Ł. & Polosukhin, I. (2017). "Attention Is All You Need." *NeurIPS 2017*, 5998–6008.
- Geva, M., Schuster, R., Berant, J. & Levy, O. (2021). "Transformer Feed-Forward Layers Are Key-Value Memories." *EMNLP 2021*, 5484–5495.
- Clark, K., Khandelwal, U., Levy, O. & Manning, C.D. (2019). "What Does BERT Look at? An Analysis of BERT's Attention." *ACL BlackboxNLP Workshop 2019*, 276–286.
- Ba, J.L., Kiros, J.R. & Hinton, G.E. (2016). "Layer Normalization." arXiv:1607.06450.
- Dao, T., Fu, D.Y., Ermon, S., Rudra, A. & Ré, C. (2022). "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness." *NeurIPS 2022*.
- Xiong, R., Yang, Y., He, D. et al. (2020). "On Layer Normalization in the Transformer Architecture." *ICML 2020*, 10524–10533.

### Further Reading

- Devlin, J., Chang, M.W., Lee, K. & Toutanova, K. (2019). "BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding." *NAACL 2019*, 4171–4186.
- Raffel, C., Shazeer, N., Roberts, A. et al. (2020). "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer." *Journal of Machine Learning Research*, 21(140), 1–67.
- Press, O. & Wolf, L. (2017). "Using the Output Embedding to Improve Language Models." *EACL 2017*, 157–163. [Weight tying between input embeddings and output projection]

### Agent Writing Brief

Write Chapter 6 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "The Transformer Architecture: A Complete Technical Specification." Target length: approximately 10,000–12,000 words. The reader has completed Chapters 1–5 and holds the full Q/K/V formulation from Chapter 5.

Cover in sequence: (1) MHA — full equations with all projection matrices, parameter count analysis, head specialisation evidence from Clark et al. (2019); (2) the feedforward sublayer — equations, inner dimension 4d, key-value memory interpretation from Geva et al. (2021); (3) positional encoding — derive the sinusoidal scheme, prove the linearity property for relative positions; (4) the full transformer block — pre-LN variant with residual connections; (5) parameter count — derive the ~12Ld² formula; (6) encoder-only vs. decoder-only vs. encoder-decoder — explain the causal mask, cross-attention, a comparison table across BERT, GPT, and T5 covering pretraining objective, attention mask, generation capability, typical use case; (7) computational complexity — O(n²d) vs. O(nd²), when each dominates; (8) FlashAttention — explain the IO-aware rewrite at the level of GPU memory hierarchy without requiring hardware expertise.

This chapter is the most mathematically dense in the book. Use subsection headers for navigability. This is the reference chapter the reader returns to throughout later chapters. Write out the complete MHA equations with all indices. The pre-LN vs. post-LN stability difference must be explained with reference to Xiong et al. (2020).


---

## Lesson 7 — Large Language Models: Pretraining, Scaling Laws, and Emergent Capabilities

### Summary

With the transformer architecture established, this chapter addresses what makes a language model large and what emerges at scale. It covers tokenisation, the pretraining objective, and — centrally — the empirical laws governing how performance scales with compute, data, and model size.

Tokenisation is the first step and the first source of non-obvious complexity. LLMs process tokens — sub-word units produced by Byte Pair Encoding (BPE; Sennrich et al., 2016). BPE initialises with a character vocabulary and iteratively merges the most frequent adjacent pair until reaching a target vocabulary size (typically 32K–128K tokens). The algorithm is presented explicitly with a worked example. Practical consequences are discussed: English averages ~0.75 words per token; arithmetic on numbers that span token boundaries is systematically harder; code and multilingual text tokenise very differently from prose.

The pretraining objective for decoder-only LLMs is causal language modelling: given tokens (x₁,...,xₜ₋₁), predict xₜ. The loss is cross-entropy averaged over all positions: L = −(1/N) Σₙ Σₜ log P(xₜ^(n) | x_{<t}^(n); θ). This is entirely self-supervised. Connecting to Chapter 1: this is entropy minimisation over the conditional next-token distribution. The model that minimises this loss most efficiently has learned the best statistical model of its training distribution.

Scaling laws (Kaplan et al., 2020) establish that test loss decreases as a smooth power law with model size N, dataset size D, and compute C: L(N) ∝ N^{−0.076}, L(D) ∝ D^{−0.095}, L(C) ∝ C^{−0.050}. These laws hold across many orders of magnitude, providing a quantitative basis for investment in scale. The Chinchilla analysis (Hoffmann et al., 2022) refines this: the Kaplan et al. models were undertrained. For a given compute budget C, the optimal allocation approximately satisfies N ∝ C^{0.5} and D ∝ C^{0.5}, implying roughly equal FLOPs on model size and data. Practical implication: GPT-3 (175B parameters, ~300B tokens) should have been trained on ~3.4T tokens. LLaMA 2 (Touvron et al., 2023) at 70B parameters and 2T tokens outperforms GPT-3 on most benchmarks as a direct consequence.

In-context learning (Brown et al., 2020, GPT-3): providing task examples in the prompt, without any weight update, allows the model to adapt its behaviour. This is the model conditioning its next-token predictions on the structure of provided examples — a form of implicit meta-learning at inference time. The mechanism is not fully understood theoretically.

Emergent capabilities (Wei et al., 2022) appear abruptly above certain scale thresholds and are absent below them. The chapter presents this evidence alongside the counter-argument of Schaeffer et al. (2023): emergence may be partly an artefact of non-linear evaluation metrics; in smooth metrics, capabilities scale continuously. This debate is presented with equal rigour on both sides without resolution — it reflects a genuinely open empirical question.

### Primary References

- Brown, T., Mann, B., Ryder, N. et al. (2020). "Language Models are Few-Shot Learners." *NeurIPS 2020*, 1877–1901. [GPT-3]
- Kaplan, J., McCandlish, S., Henighan, T., Brown, T.B. et al. (2020). "Scaling Laws for Neural Language Models." arXiv:2001.08361.
- Hoffmann, J., Borgeaud, S., Mensch, A., Buchatskaya, E. et al. (2022). "Training Compute-Optimal Large Language Models." arXiv:2203.15556. [Chinchilla]
- Wei, J., Tay, Y., Bommasani, R., Raffel, C. et al. (2022). "Emergent Abilities of Large Language Models." *Transactions on Machine Learning Research* (TMLR), 2022.
- Sennrich, R., Haddow, B. & Birch, A. (2016). "Neural Machine Translation of Rare Words with Subword Units." *ACL 2016*, 1715–1725.
- Touvron, H., Martin, L., Stone, K. et al. (2023). "LLaMA 2: Open Foundation and Fine-Tuned Chat Models." arXiv:2307.09288.

### Further Reading

- Schaeffer, R., Miranda, B. & Koyejo, S. (2023). "Are Emergent Abilities of Large Language Models a Mirage?" *NeurIPS 2023*. [Critical analysis of emergence; essential counterpoint to Wei et al.]
- Biderman, S., Schoelkopf, H., Anthony, Q.G. et al. (2023). "Pythia: A Suite for Analyzing Large Language Models Across Training and Scaling." *ICML 2023*. [Open models with released training checkpoints; useful for empirical study of scaling]
- Radford, A., Wu, J., Child, R., Luan, D., Amodei, D. & Sutskever, I. (2019). "Language Models are Unsupervised Multitask Learners." OpenAI Blog. [GPT-2; demonstrates zero-shot task performance]

### Agent Writing Brief

Write Chapter 7 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Large Language Models: Pretraining, Scaling Laws, and Emergent Capabilities." Target length: approximately 10,000 words. The reader has completed Chapters 1–6 and understands transformer architecture, the decoder-only variant, and cross-entropy loss.

Cover in sequence: (1) tokenisation — BPE algorithm step by step with a worked merging example on a small character vocabulary; vocabulary size trade-offs; practical consequences for arithmetic, multilingual text, code; (2) the causal language modelling objective — write the loss in full summation notation, connect explicitly to the entropy minimisation framing of Chapter 1; (3) pretraining data — describe Common Crawl and curated corpora briefly as context; data quality filtering as a significant practical factor; (4) in-context learning — GPT-3's few-shot results, the meta-learning interpretation, what is and is not understood about the mechanism; (5) scaling laws — present Kaplan et al. power law form with empirical exponents for N, D, and C; (6) Chinchilla — the reanalysis, the equal-scaling implication N ∝ C^0.5 and D ∝ C^0.5, the practical consequences for LLaMA; (7) emergent abilities — Wei et al. (2022) evidence, Schaeffer et al. (2023) counter-argument; present both positions rigorously.

Mathematical treatment: write the cross-entropy loss with full notation. Present the power laws with empirical exponents. Include a comparison table: GPT-3 vs. Chinchilla vs. LLaMA 2 on parameters, training tokens, and representative benchmark performance. On emergence: do not flatten the debate into a settled answer; the chapter should convey genuine scientific uncertainty.

---

## Lesson 8 — Post-Training: Alignment, RLHF, and Frontier Architectures

### Summary

A pretrained language model minimises next-token prediction loss. It will complete a helpful medical query or generate misinformation with equal facility, depending on what the training distribution predicts as likely. Post-training is the collection of techniques that align outputs with the goals of deployers and users.

The alignment gap is stated precisely: the pretraining objective rewards plausibility, not correctness, helpfulness, or safety. A model that predicts human text has learned something about human values as statistical patterns, not as explicit constraints. The gap between "predicts human text well" and "behaves as a helpful, honest, and harmless assistant" is the alignment problem in its practical form.

Supervised Fine-Tuning (SFT): the pretrained model is fine-tuned on curated (prompt, ideal response) pairs produced by human contractors. SFT is necessary but insufficient: it teaches what good responses look like in the annotated distribution but provides no mechanism for generalising the underlying values to unanticipated situations.

RLHF (Christiano et al., 2017; Ouyang et al., 2022) has three phases. First, human raters rank multiple model responses to the same prompt by preference. Second, a reward model (RM) is trained on pairwise comparisons via the Bradley-Terry preference model: P(response A ≻ response B) = σ(r(A) − r(B)). Third, the SFT model is fine-tuned with PPO to maximise RM score subject to a KL-divergence penalty: J(θ) = E[r(x,y)] − β · KL[π_θ || π_SFT]. The KL term prevents reward hacking — the model learning to produce degenerate outputs that fool the reward model.

DPO (Rafailov et al., 2023) eliminates the separate reward model. Starting from the same Bradley-Terry model, DPO derives a closed-form expression for the optimal policy, yielding a contrastive loss: L_DPO(θ) = −E[log σ(β log(π_θ(y_w|x)/π_ref(y_w|x)) − β log(π_θ(y_l|x)/π_ref(y_l|x)))], where y_w is the preferred response and y_l the rejected one. This derivation is the centrepiece of the section.

Constitutional AI (Bai et al., 2022) replaces human preference labels with AI-generated feedback. The model critiques and revises its own outputs according to a written constitution, generating preference data at scale — RLAIF.

LoRA (Hu et al., 2021) makes fine-tuning tractable at scale. The weight update is approximated as ΔW = BA where B ∈ ℝ^{d×r} and A ∈ ℝ^{r×k} with r ≪ min(d,k). Pretrained weights are frozen; only A and B are trained, reducing trainable parameters by orders of magnitude.

Mixture of Experts (MoE; Shazeer et al., 2017) replaces the dense feedforward sublayer with E expert networks and a learned routing function selecting the top-k experts per token. Only active experts compute, making effective parameter count much larger than compute cost implies. GPT-4 and Mixtral use this design.

Multimodal systems (CLIP; Radford et al., 2021) align image and text representations via contrastive learning on image-caption pairs, learning a joint embedding space. Multimodal LLMs typically project non-text inputs through modality-specific encoders into the text token embedding space.

The chapter closes with rigorous analysis of benchmark evaluation limitations: contamination (test sets in training data), distribution shift (benchmarks differ from production queries), saturation (top models near ceiling), and metric invalidity (multiple-choice accuracy does not measure generation quality). HELM (Liang et al., 2022) is presented as the most systematic attempt to address these.

### Primary References

- Ouyang, L., Wu, J., Jiang, X., Almeida, D. et al. (2022). "Training Language Models to Follow Instructions with Human Feedback." *NeurIPS 2022*. [InstructGPT]
- Christiano, P., Leike, J., Brown, T.B., Martic, M., Legg, S. & Amodei, D. (2017). "Deep Reinforcement Learning from Human Preferences." *NeurIPS 2017*, 4299–4307.
- Rafailov, R., Sharma, A., Mitchell, E., Ermon, S., Manning, C.D. & Finn, C. (2023). "Direct Preference Optimization: Your Language Model is Secretly a Reward Model." *NeurIPS 2023*.
- Bai, Y., Jones, A., Ndousse, K., Askell, A. et al. (2022). "Constitutional AI: Harmlessness from AI Feedback." arXiv:2212.08073.
- Hu, E.J., Shen, Y., Wallis, P. et al. (2021). "LoRA: Low-Rank Adaptation of Large Language Models." *ICLR 2022*. arXiv:2106.09685.
- Shazeer, N., Mirhoseini, A., Maziarz, K. et al. (2017). "Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer." *ICLR 2017*.
- Radford, A., Kim, J.W., Hallacy, C. et al. (2021). "Learning Transferable Visual Models from Natural Language Supervision." *ICML 2021*, 8748–8763. [CLIP]
- Liang, P., Bommasani, R., Lee, T., Tsipras, D. et al. (2022). "Holistic Evaluation of Language Models." arXiv:2211.09110.

### Further Reading

- Dettmers, T., Pagnoni, A., Holtzman, A. & Zettlemoyer, L. (2023). "QLoRA: Efficient Finetuning of Quantized LLMs." *NeurIPS 2023*.
- Jiang, A.Q., Sablayrolles, A., Roux, A., Mensch, A. et al. (2024). "Mixtral of Experts." arXiv:2401.04088.
- Ziegler, D.M., Stiennon, N., Wu, J. et al. (2019). "Fine-Tuning Language Models from Human Preferences." arXiv:1909.08593. [Precursor to InstructGPT]

### Agent Writing Brief

Write Chapter 8 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Post-Training: Alignment, RLHF, and Frontier Architectures." Target length: approximately 12,000 words. The reader has completed Chapters 1–7 and understands transformers, pretraining, and PPO from Chapter 4.

Cover in sequence: (1) the alignment gap — state precisely why next-token prediction does not imply alignment; (2) SFT — procedure, what it achieves, why insufficient; (3) RLHF — the three phases; write the Bradley-Terry pairwise loss explicitly; write J(θ) = E[r(x,y)] − β · KL[π_θ || π_SFT] in full; explain reward hacking and why the KL term is necessary; (4) DPO — derive the DPO loss from the RLHF optimisation problem step by step; this is the centrepiece of the section and must not be abbreviated; (5) Constitutional AI — describe the critique-revision loop and RLAIF; (6) LoRA — derive ΔW = BA, analyse parameter count reduction as a function of r; (7) MoE — routing mechanism, top-k selection, how sparsity enables capacity scaling; (8) multimodal systems — CLIP contrastive objective, projection into LLM token space; (9) benchmark evaluation — explain each failure mode with examples; cite Liang et al. (2022).

Mathematical treatment: write the Bradley-Terry loss explicitly. Write J(θ) in full. Derive the DPO loss — the most technically demanding part, must not be skipped. Write the LoRA parameter count formula. For MoE, write the gating function. On benchmarks: be analytically rigorous, not merely dismissive.

---

## Lesson 9 — Knowledge, Context, and the Architecture of Memory

### Summary

An LLM at inference time is a stateless function: it maps a token sequence to a next-token distribution. Everything the model can use must be present in the context window; nothing persists across calls. This architectural fact is the central engineering constraint of all LLM-based systems.

The chapter opens with a conceptual distinction that determines the design of every serious LLM application. There are four distinct phenomena that practitioners conflate under "the model knows": (1) Memorisation — the model reproduces content from training data. (2) Retrieval — the model locates information encoded in its weights in response to a query; Geva et al. (2021) established that feedforward layers implement this via key-value lookup. (3) Inference — the model combines retrieved information with statistical or logical rules to produce conclusions not explicit in training data. (4) Reasoning — the model constructs multi-step arguments where intermediate conclusions constrain subsequent steps. These are not the same process; they are not equally reliable; and they fail differently under distribution shift. Current LLMs do (1) and (2) reliably, (3) with significant variance, and (4) with deep structural limitations not yet resolved.

The KV cache is the central inference optimisation. Key and value projections of all previous tokens are computed once and cached; each new token attends over the cached K, V. Cache size grows as O(n · L · 2 · d_h · h), where n is sequence length, L layers, d_h head dimension, h heads. For a 70B-parameter model at 100K tokens, the KV cache requires tens of gigabytes of GPU memory.

Rotary Position Embedding (RoPE; Su et al., 2021) encodes position by rotating the query and key vectors such that the relative position between positions m and n appears in the attention score only as a function of m − n. ALiBi (Press et al., 2022) adds a linear penalty to attention logits as a function of key-query distance. YaRN (Peng et al., 2023) extends RoPE to contexts longer than those seen in training by adjusting rotation frequencies with interpolation.

Retrieval-Augmented Generation (RAG; Lewis et al., 2020) decouples parametric knowledge (weights) from non-parametric knowledge (retrieval index). The full RAG stack is covered: chunking strategies (fixed-size, sentence-level, semantic), dense passage retrieval (Karpukhin et al., 2020), BM25 sparse retrieval, hybrid approaches, and re-ranking. The architectural trade-offs between retrieval precision, latency, and context window consumption are discussed.

Where factual knowledge is stored is analysed mechanistically using Meng et al. (2022), who localise specific factual associations to MLP layers in GPT-2 and demonstrate causal editing by modifying individual weights.

The chapter closes with a memory taxonomy for LLM systems: in-context (active context window), external retrieval (RAG corpus), parametric (training-time knowledge in weights), and external state (key-value stores updated by tool calls). Understanding this taxonomy is prerequisite to Chapter 10.

### Primary References

- Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V. et al. (2020). "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks." *NeurIPS 2020*, 9459–9474.
- Su, J., Lu, Y., Pan, S., Murtadha, A., Wen, B. & Liu, Y. (2021). "RoFormer: Enhanced Transformer with Rotary Position Embedding." arXiv:2104.09864.
- Press, O., Smith, N.A. & Lewis, M. (2022). "Train Short, Test Long: Attention with Linear Biases Enables Input Length Extrapolation." *ICLR 2022*.
- Karpukhin, V., Oguz, B., Min, S. et al. (2020). "Dense Passage Retrieval for Open-Domain Question Answering." *EMNLP 2020*, 6769–6781.
- Meng, K., Bau, D., Andonian, A. & Belinkov, Y. (2022). "Locating and Editing Factual Associations in GPT." *NeurIPS 2022*. [ROME]
- Mallen, A., Asai, A., Zhong, V., Das, R., Hajishirzi, H. & Weld, D.S. (2023). "When Not to Trust Language Models: Investigating Effectiveness of Parametric and Non-Parametric Memories." *ACL 2023*, 9802–9822.

### Further Reading

- Peng, B., Quesnelle, J., Fan, H. & Shippole, E. (2023). "YaRN: Efficient Context Window Extension of Large Language Models." arXiv:2309.00071.
- Izacard, G., Lewis, P., Lomeli, M. et al. (2023). "Atlas: Few-shot Learning with Retrieval Augmented Language Models." *Journal of Machine Learning Research*, 24(251), 1–43.
- Geva, M., Schuster, R., Berant, J. & Levy, O. (2021). "Transformer Feed-Forward Layers Are Key-Value Memories." *EMNLP 2021*, 5484–5495. [Revisited in this chapter for its knowledge localisation implications]

### Agent Writing Brief

Write Chapter 9 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Knowledge, Context, and the Architecture of Memory." Target length: approximately 10,000 words. The reader has completed Chapters 1–8 and understands the stateless inference paradigm, transformer architecture, and pretraining.

Open with a section titled "What Does 'The Model Knows' Actually Mean?" — develop the four-way distinction between memorisation, retrieval, inference, and reasoning with precision and with concrete examples showing where each operates and where each fails. This is the conceptual centrepiece. Then cover: (2) KV cache — full memory cost formula with all components named and a concrete worked example for a 70B model; (3) positional encodings for long context — derive RoPE in mathematical detail (rotation matrix formulation), present ALiBi conceptually, mention YaRN; (4) RAG — full stack from chunking to retrieval to re-ranking to generation; explain BM25, dense retrieval, hybrid approaches; (5) mechanistic knowledge localisation — cite Meng et al. (2022) and Geva et al. (2021); (6) memory taxonomy.

Mathematical treatment: write the RoPE rotation matrix explicitly. Write the KV cache memory cost as a complete formula with all variables defined. On the four-way distinction: no differential equations required, but precise conceptual definitions backed by examples and citations are mandatory. Cite Mallen et al. (2023) for empirical evidence on reliability of parametric vs. non-parametric memory. The tone should challenge naive framings of LLM knowledge — this is an analytically critical chapter, not an exposition of settled facts.


---

## Lesson 10 — Agentic Programming: Reasoning, Tools, and Structured Execution

### Summary

A language model responding to a single prompt is not an agent. An agent perceives an environment, selects actions, receives feedback, and iterates toward a goal. This chapter covers the techniques that transform a language model into a component of an agentic system: the reasoning patterns, tool-use mechanisms, and evaluation methodologies that define the current engineering frontier.

The chapter opens with the definitional question: when does a model become an agent? The minimal criterion is that the model's output influences the environment and the environmental response influences the model's subsequent inputs. The more demanding criterion — the model maintains a goal across multiple steps and adapts its plan in response to feedback — is the target. Both are connected to Chapter 4's RL framework: the agent is a policy operating in an MDP.

Chain-of-Thought prompting (Wei et al., 2022) is the foundational technique for eliciting multi-step reasoning. Instructing the model to produce intermediate steps before its final answer improves performance dramatically on arithmetic, symbolic manipulation, and commonsense tasks. The likely mechanism: the transformer's per-forward-pass computational budget is fixed; CoT extends this budget by externalising intermediate computation into the autoregressive generation sequence. Zero-shot CoT (Kojima et al., 2022) demonstrates that the single phrase "Let's think step by step" produces significant gains without examples.

Tree of Thoughts (Yao et al., 2023) generalises CoT from a linear chain to a search tree: multiple reasoning branches are generated, evaluated (by the model or an external heuristic), and the most promising expanded. This is best-first search over a space of partial reasoning states — a rediscovery of classical AI planning within the language model paradigm.

ReAct (Yao et al., 2022) interleaves reasoning with action execution in a single generation context. The pattern is Thought → Action → Observation, iterated until task completion. The model reasons about what to do, specifies a structured action, receives the result as an observation, and revises its reasoning. This is structurally isomorphic to the sense-plan-act loop of classical robotics.

Tool use / function calling: the model is given a schema describing available tools in JSON Schema format. When it emits a tool call, the runtime parses the structured output and executes the corresponding function; the result is injected into the context. Schema design is a first-class engineering concern: ambiguous descriptions, missing parameters, and overlapping tools produce systematic failures.

Structured outputs constrain generation to valid schema instances via grammar-constrained decoding: at each step, logits for tokens that would produce an invalid prefix are set to −∞. For production agentic systems, structured output is a hard requirement.

Reflexion (Shinn et al., 2023) introduces verbal feedback: the model evaluates its own output and produces a natural-language critique prepended to the next attempt, implementing trial-and-error without weight updates. Limitations are noted: the context window bounds the number of attempts, and self-evaluation is unreliable.

Evaluation: HumanEval (Chen et al., 2021) measures pass@k on programming tasks. SWE-bench (Jimenez et al., 2024) requires resolving real GitHub issues in actual codebases — far harder and more realistic. State-of-the-art models achieve 20–50% on SWE-bench depending on configuration. The gap between benchmark and deployment performance is the primary unsolved engineering problem.

### Primary References

- Wei, J., Wang, X., Schuurmans, D., Bosma, M. et al. (2022). "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models." *NeurIPS 2022*, 24824–24837.
- Kojima, T., Gu, S.S., Reid, M., Matsuo, Y. & Iwasawa, Y. (2022). "Large Language Models are Zero-Shot Reasoners." *NeurIPS 2022*, 22199–22213.
- Yao, S., Zhao, J., Yu, D., Du, N., Shafran, I., Narasimhan, K. & Cao, Y. (2022). "ReAct: Synergizing Reasoning and Acting in Language Models." *ICLR 2023*. arXiv:2210.03629.
- Yao, S., Yu, D., Zhao, J., Shafran, I., Griffiths, T.L., Cao, Y. & Narasimhan, K. (2023). "Tree of Thoughts: Deliberate Problem Solving with Large Language Models." *NeurIPS 2023*.
- Shinn, N., Cassano, F., Labash, B., Gopinath, A., Narasimhan, K. & Yao, S. (2023). "Reflexion: Language Agents with Verbal Reinforcement Learning." *NeurIPS 2023*.
- Chen, M., Tworek, J., Jun, H. et al. (2021). "Evaluating Large Language Models Trained on Code." arXiv:2107.03374. [HumanEval benchmark]
- Jimenez, C.E., Yang, J., Wettig, A., Yao, S., Pei, K., Press, O. & Narasimhan, K. (2024). "SWE-bench: Can Language Models Resolve Real-World GitHub Issues?" *ICLR 2024*.

### Further Reading

- Wang, X., Wei, J., Schuurmans, D., Le, Q., Chi, E., Narang, S., Chowdhery, A. & Zhou, D. (2023). "Self-Consistency Improves Chain of Thought Reasoning in Language Models." *ICLR 2023*.
- Schick, T., Dwivedi-Yu, J., Dessì, R. et al. (2023). "Toolformer: Language Models Can Teach Themselves to Use Tools." *NeurIPS 2023*.

### Agent Writing Brief

Write Chapter 10 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Agentic Programming: Reasoning, Tools, and Structured Execution." Target length: approximately 10,000 words. The reader has completed Chapters 1–9 and understands transformer inference, in-context learning, the memory taxonomy, and the RL framework from Chapter 4.

Cover in sequence: (1) agency defined — minimal and demanding criteria, connection to Chapter 4's MDP sense-plan-act loop; (2) Chain-of-Thought — empirical results from Wei et al. (2022), the computational mechanism hypothesis, zero-shot CoT; (3) Tree of Thoughts — formalise as search over reasoning states, connect to classical AI planning; (4) ReAct — the Thought/Action/Observation cycle; include a fully worked trace through a realistic example (e.g., answering a compound question requiring a retrieval step followed by arithmetic); (5) tool use and function calling — schema design, execution flow, failure modes from ambiguous schemas; (6) structured outputs and grammar-constrained decoding; (7) Reflexion — verbal feedback loop, limitations; (8) evaluation — HumanEval, SWE-bench, honest assessment of the benchmark-to-deployment gap; state the 20–50% SWE-bench figure with caveats about scaffolding.

Mathematical treatment: formalise Tree of Thoughts as a search problem with states, transitions, and heuristic function. Define agent, observation, action, and trace formally. Use worked examples heavily: include a realistic tool-use trace showing schema definition, model call, tool result, and next reasoning step. Do not oversell current capabilities. The chapter should leave the reader with genuine respect for the difficulty of reliable agentic execution.

---

## Lesson 11 — Agentic Workflows: Multi-Agent Systems, Orchestration, and Production

### Summary

Single-agent systems have hard limits: a single context window bounds scope, a single model may not optimally serve all subtasks, and sequential execution creates latency bottlenecks. This chapter addresses multi-agent architectures — where multiple model instances cooperate under coordinated orchestration — and the production concerns that determine whether such systems are viable in practice.

When is a multi-agent system preferable? Three dimensions: scope (tasks exceeding one context window, or requiring parallel reasoning threads); specialisation (different tasks suited to differently prompted or fine-tuned models); and verification (one agent checking another's output, analogous to code review).

Canonical architectural patterns: orchestrator–subagent (central model decomposes, dispatches, synthesises); pipeline (sequential chain); parallel fan-out (concurrent independent subtasks with aggregation); critic/verifier (second model evaluating primary output). These compose. LangGraph (LangChain) formalises them as directed graphs with explicit shared state and conditional transitions.

The Model Context Protocol (MCP; Anthropic, 2024) is the current industry attempt at standardisation. MCP defines a client-server protocol for tool connectivity: an MCP server exposes tools; an MCP client (the model host) discovers and invokes them via a standard interface. The analogy to the Language Server Protocol in software development is exact. Without such a protocol, each LLM system requires bespoke integration per tool — an O(models × tools) engineering problem. MCP reduces this to O(models + tools).

Memory architecture in multi-agent systems: each agent has its own context window; shared state must be externalised. Concurrency introduces consistency problems: two agents updating the same external state without coordination can produce contradictions. Engineering options — shared document stores, message queues with explicit ordering, optimistic locking — are surveyed.

Reliability failure modes specific to multi-agent systems: error propagation (a subagent error corrupts the orchestrator's context); deadlock (agents waiting on each other's outputs); attribution difficulty (determining which agent caused a downstream failure). Human-in-the-loop checkpoints — gates requiring human approval before consequential or irreversible actions — are the current standard mitigation.

Safety introduces novel attack surfaces: prompt injection (malicious content in tool outputs or retrieved documents hijacking instruction following); privilege escalation (an agent manipulated into actions beyond its intended scope); uncontrolled side effects (irreversible real-world actions taken without confirmation). These are explicitly analogised to classical software security vulnerabilities — SQL injection, OS privilege escalation — and the security engineering community's standard countermeasures (input validation, least privilege, audit logging) are applied.

Observability is a first-class production requirement: all model calls, tool invocations, intermediate states, token counts, and latencies must be captured for post-hoc debugging and cost attribution.

The chapter closes with a complexity dynamics perspective. A multi-agent system is a feedback-coupled network; each agent's outputs are inputs to other agents. Feedback loops can produce emergent behaviour unpredictable from individual agent behaviour. The stability vocabulary from Chapter 2 — attractors, stability, divergence — applies: whether the system converges to useful states or diverges into unproductive loops depends on the agent graph structure and orchestration logic.

### Primary References

- Wu, Q., Bansal, G., Zhang, J., Wu, Y., Li, B. et al. (2023). "AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation Framework." arXiv:2308.08155.
- Anthropic (2024). "Model Context Protocol: Specification and Documentation." modelcontextprotocol.io. [Technical specification of the MCP client-server protocol]
- Wang, L., Ma, C., Feng, X., Zhang, Z., Yang, H. et al. (2024). "A Survey on Large Language Model based Autonomous Agents." *Frontiers of Computer Science*, 18(6), 186345.
- Perez, E., Huang, S., Song, F., Cai, T., Ring, R. et al. (2022). "Red Teaming Language Models with Language Models." arXiv:2202.03286.

### Further Reading

- Hong, S., Zhuge, M., Chen, J. et al. (2023). "MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework." arXiv:2308.00352.
- Park, J.S., O'Brien, J.C., Cai, C.J., Morris, M.R., Liang, P. & Bernstein, M.S. (2023). "Generative Agents: Interactive Simulacra of Human Behavior." *UIST 2023*, 1–22.
- Weng, L. (2023). "LLM-Powered Autonomous Agents." *Lil'Log*, June 2023. lilianweng.github.io/posts/2023-06-23-agent/.

### Agent Writing Brief

Write Chapter 11 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Agentic Workflows: Multi-Agent Systems, Orchestration, and Production." Target length: approximately 10,000–12,000 words. The reader has completed Chapters 1–10 and understands single-agent systems, tool use, ReAct, memory taxonomy, and RL from Chapter 4.

Cover in sequence: (1) when multi-agent systems are preferable — scope, specialisation, verification; (2) architectural patterns — orchestrator-subagent, pipeline, parallel fan-out, critic/verifier — describe each with a diagram expressed in prose; (3) MCP — client-server protocol, the LSP analogy, the O(models + tools) reduction argument; (4) shared state and memory — concurrency, consistency problems, engineering options; (5) reliability and error propagation — trace through a failure scenario step by step; (6) human-in-the-loop — when and how to gate on human approval; (7) safety — prompt injection, privilege escalation, uncontrolled side effects — draw explicit analogies to SQL injection, OS privilege escalation; describe countermeasures; (8) observability — what must be traced and why; (9) complexity dynamics — feedback loops, emergent behaviour, connection to Chapter 2's dynamical systems vocabulary.

The complexity dynamics section should be qualitatively rigorous using Chapter 2's vocabulary (attractors, stability, divergence) without formal dynamical systems mathematics. Tone: engineering-focused and realistic. Do not undersell reliability challenges. The reader should finish this chapter with genuine respect for the difficulty of production multi-agent systems and a clear taxonomy of failure modes.

---

## Lesson 12 — Open Frontiers: Interpretability, Continual Learning, and the Limits of Current Paradigms

### Summary

The final chapter maps what is not known. It addresses the most important open problems in the field with the seriousness they deserve and resists projecting current progress linearly into the future. This chapter is deliberately less settled in its conclusions than its predecessors.

Mechanistic interpretability is the programme of reverse-engineering the internal computations of neural networks — understanding not just what a model produces but how its weights implement the computation. Elhage et al. (2021) provide the foundational mathematical framework, decomposing transformers into circuits: compositions of attention heads and MLP layers implementing identifiable algorithms. Induction heads — pairs of attention heads that implement in-context copying — are presented as the clearest example. Meng et al. (2022, ROME) localise factual associations to specific MLP layers in GPT-2 and demonstrate causal editing: modifying individual weights to change factual outputs without disturbing unrelated behaviour. The chapter presents these as genuine scientific progress and is equally explicit that mechanistic interpretability at 100B+ parameter scale — where features are superposed in high-dimensional spaces and circuits interact non-linearly — remains an unsolved problem.

Catastrophic forgetting (McCloskey & Cohen, 1989): neural networks, when trained on new data, lose previously acquired knowledge. For LLMs this manifests whenever fine-tuning on a new domain degrades performance on previously acquired tasks. Kirkpatrick et al. (2017) proposed Elastic Weight Consolidation (EWC): penalise changes to weights important for previous tasks, using the Fisher information matrix as the importance measure. The EWC objective is stated formally: L(θ) = L_B(θ) + (λ/2) Σᵢ Fᵢ(θᵢ − θ*_{A,i})². Its limitations at LLM scale are discussed honestly.

The reasoning versus prediction question is returned to explicitly. Chapter 1 introduced it; this chapter closes the loop by making clear it has not been resolved. The central claim of the current paradigm: sufficiently large, data-rich predictive models develop representations supporting generalisation beyond pattern matching. The counter-claim (Marcus & Davis, 2019): current models lack grounded, structured representations of causality, space, time, and number; apparent reasoning is brittle pattern matching that fails under systematic distribution shift. Both positions are presented rigorously. The empirical record shows that LLMs fail systematically on compositional generalisation, systematic spatial reasoning, and precise counting in ways consistent with the pattern-matching critique. Whether this is fundamental or an engineering problem is genuinely unknown.

Scalable oversight: as models become more capable, the consequences of misalignment grow, and tools for detecting misalignment become less reliable when the model exceeds human evaluator expertise. This is the core challenge of alignment at frontier capability. Debate (Irving et al., 2018) and recursive reward modelling are proposed solutions; none is solved.

Autonomous science: AI systems that formulate hypotheses, design experiments, and revise theories. AlphaFold (Jumper et al., 2021) for protein structure prediction represents what is achievable. General creative hypothesis generation is unsolved.

The chapter closes with a calibrated discussion of AGI: definitions (task-based, cognitive, economic) are contested; timelines are subject to wide expert disagreement; current outstanding problems (reasoning, alignment, continual learning, interpretability) are precisely those that would need resolution for systems approaching general capability. The chapter does not predict timelines and is explicit about why.

### Primary References

- Elhage, N., Nanda, N., Olsson, C., Henighan, T. et al. (2021). "A Mathematical Framework for Transformer Circuits." *Transformer Circuits Thread*, Anthropic. transformer-circuits.pub/2021/framework/index.html.
- Meng, K., Bau, D., Andonian, A. & Belinkov, Y. (2022). "Locating and Editing Factual Associations in GPT." *NeurIPS 2022*.
- McCloskey, M. & Cohen, N.J. (1989). "Catastrophic Interference in Connectionist Networks: The Sequential Learning Problem." *Psychology of Learning and Motivation*, 24, 109–165.
- Kirkpatrick, J., Pascanu, R., Rabinowitz, N., Veness, J. et al. (2017). "Overcoming Catastrophic Forgetting in Neural Networks." *Proceedings of the National Academy of Sciences*, 114(13), 3521–3526.
- Bubeck, S., Chandrasekaran, V., Eldan, R., Gehrke, J. et al. (2023). "Sparks of Artificial General Intelligence: Early Experiments with GPT-4." arXiv:2303.12528.
- Marcus, G. & Davis, E. (2019). *Rebooting AI: Building Artificial Intelligence We Can Trust*. Pantheon Books.

### Further Reading

- Bommasani, R., Hudson, D.A., Aditi, E. et al. (2021). "On the Opportunities and Risks of Foundation Models." arXiv:2108.07258.
- Bengio, Y., Hinton, G., Yao, A., Song, D. et al. (2023). "Managing AI Risks in an Era of Rapid Progress." arXiv:2310.17688.
- Russell, S. (2019). *Human Compatible: Artificial Intelligence and the Problem of Control*. Viking.

### Agent Writing Brief

Write Chapter 12 of a 12-chapter graduate technical textbook on LLMs and agentic systems. Title: "Open Frontiers: Interpretability, Continual Learning, and the Limits of Current Paradigms." Target length: approximately 10,000 words. The reader has completed all previous chapters. This is the most difficult chapter to write correctly because it requires maintaining high technical standards while engaging with genuinely unsettled questions.

Cover in sequence: (1) mechanistic interpretability — the transformer circuits framework (Elhage et al., 2021), induction heads as the clearest example, ROME (Meng et al., 2022); be precise about what has been understood and what has not, especially at large scale; (2) catastrophic forgetting — McCloskey & Cohen (1989), EWC (Kirkpatrick et al., 2017) — write the EWC objective L(θ) = L_B(θ) + (λ/2) Σᵢ Fᵢ(θᵢ − θ*_{A,i})² with all terms defined, explain the Fisher information matrix at a conceptual level; discuss why continual learning is structurally hard in neural networks; (3) reasoning versus prediction — return to Chapter 1's open question; present both the emergence-from-prediction view and the Marcus & Davis (2019) critique; document specific failure modes (compositional generalisation, spatial reasoning, counting) with examples; do not resolve the debate; (4) scalable oversight — state why human evaluation fails at frontier capability; describe debate and recursive reward modelling as proposed, unsolved approaches; (5) autonomous science — AlphaFold as achieved benchmark, creative hypothesis generation as unsolved; (6) AGI — definitional ambiguity, expert disagreement on timelines, what current trajectories do and do not imply; do not predict.

Tone: intellectual honesty is the primary requirement. Do not resolve open questions. Do not project linear extrapolation. The chapter should model the epistemic humility that good scientists bring to frontier problems. End the book — and the chapter — by noting that the most valuable skill a practitioner can develop is the ability to distinguish what current systems actually do from what they appear to do.

---

## Appendix: Production Notes for the Writing Team

**Chapter dependencies:** strictly sequential. Each chapter explicitly states which preceding concepts it assumes. Agents must not introduce concepts from later chapters without a forward reference.

**Notation consistency throughout all chapters:**
- Bold uppercase for matrices: **W**, **Q**, **K**, **V**, **H**
- Bold lowercase for vectors: **x**, **h**, **w**, **b**
- Italic for scalars: n, d, L, γ, η, r, α
- Greek letters for parameters and distributions: θ, π, σ, α, β, γ
- Subscript t for timestep, i/j for position indices

**Citation format:** author-year in body text (e.g., Vaswani et al., 2017); full reference list at end of each chapter in the format shown in the Primary References sections above. Agents must not invent, modify, or summarise citations differently from how they appear in the syllabus.

**Length policy:** target ~10,000 words per chapter; expand to ~12,000 for Chapters 6 and 8 which carry the highest technical density. Do not pad; do not compress foundational derivations for the sake of brevity.

**Examples policy:** each chapter must include at least two concrete worked examples. Examples use symbolic or small numerical values; they do not require code.

**Mathematical policy:** all formulas are typeset in standard LaTeX-compatible notation. Derivations that are designated as "full" in the writing briefs must be carried through completely; they may not be replaced by "it can be shown that."
