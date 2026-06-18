## 1.1 Intelligence as Predictive Modelling: A Working Definition

What does it mean for a computational system to exhibit intelligence? The question has occupied philosophers, cognitive scientists, and engineers for the better part of a century without producing consensus. This course does not attempt to resolve it. What is needed here is not a philosophically defensible theory of mind but a *working definition*—one precise enough to anchor the mathematical frameworks that follow and general enough to remain useful as those frameworks evolve.

The definition adopted throughout this course is the following: *intelligence is the capacity to construct internal models of an environment that enable successful prediction and action within it.* This formulation is introduced as a productive framing device, not as settled philosophy. It will be examined critically at several points in the course and, in Chapter 12, confronted with the hardest evidence against it.

Three properties of this definition merit immediate comment.

First, it is deliberately agnostic about mechanism. It does not specify whether the internal model takes the form of explicit symbolic rules, the weighted connections of a neural network, a probability distribution over states of the world, or some other representational substrate. Biological neural systems, formal logic engines, and statistical function approximators can all satisfy this definition to varying degrees. This agnosticism is a feature rather than a deficiency: the history of the field shows that premature commitment to a particular implementation of intelligence has repeatedly constrained progress. The symbolic and statistical paradigms—the two major answers to the question of how to build intelligent systems—will each be evaluated on their merits, not disqualified in advance by definitional manoeuvre.

Second, the definition centres on the coupling of *prediction* and *action* rather than on either in isolation. A system that predicts accurately but cannot act on its predictions is a passive model; a system that acts without forming predictions about the consequences of its actions is an actuator, not an agent. What makes a system intelligent, on this account, is the functional loop between its internal model and its behaviour: predictions inform actions, and the outcomes of actions can refine the model. The reader familiar with control theory will recognise this as the sense-plan-act cycle; it is also, as Chapter 4 will establish, the structural core of the reinforcement learning framework.

Third, the definition locates intelligence in the *function* of a model rather than its content. Intelligence is not measured by the size of a knowledge base or the formal complexity of a set of rules, but by the degree to which an agent's internal representations support accurate anticipation of its environment. This is a consequentialist view of cognition, and it has a precise mathematical correlate: the quality of a predictive model can be quantified as the divergence between its predictions and observed outcomes. Section 1.4 formalises this measurement using the tools of information theory.

This framing is particularly productive for understanding large language models, which are the central subject of this course. A language model is a system trained to assign probabilities to sequences of tokens, or equivalently to predict the next token given all preceding context. The internal representations it constructs in the course of that training—the billions of numerical weights specifying the parameters of the model—constitute its model of the statistical structure of language. Whether those representations constitute *understanding* in any deeper sense is a live scientific question that Chapter 12 addresses with appropriate epistemic humility. For the purposes of Chapters 1 through 11, predictive accuracy is the operative standard.

One clarification is required before proceeding. The intelligence-as-prediction definition is a *necessary* condition, not a sufficient one. A lookup table that memorises every training input and its observed output achieves zero prediction error on those inputs while having learned nothing that generalises. A useful intelligent system must not merely fit observations but generalise from them—predicting accurately on inputs it has not previously encountered. The distinction between fitting training data and generalising to new data is the central problem of statistical learning theory and the topic of Section 1.3.

---

## 1.2 The Symbolic Paradigm: Programme, Achievements, and Structural Failure

### The Physical Symbol System Hypothesis

The dominant answer to the question of machine intelligence during the first three decades of AI research was explicit symbolic representation. Its most influential formulation appeared in Newell and Simon's 1976 Turing Award lecture, where they proposed the *Physical Symbol System Hypothesis*: a physical symbol system, they argued, "has the necessary and sufficient means for general intelligent action" (Newell & Simon, 1976). By a physical symbol system they meant any device capable of creating, storing, modifying, and interpreting structures composed of discrete symbols—what we would recognise today as a Turing-complete computational system acting on structured data.

The hypothesis contains two separable empirical claims. The *necessity* claim holds that any system exhibiting general intelligent action must, at some level of description, instantiate such a symbol system: intelligence requires formal symbolic manipulation. The *sufficiency* claim holds that a system capable of such manipulation has, in principle, the prerequisites for general intelligent action: given the right symbols and the right operations over them, intelligence follows.

Both claims are empirically testable, not merely definitional, and both have been contested by subsequent evidence. But their importance in the history of the field is not in whether they are ultimately correct; it is in the research programme they generated and the intellectual clarity they brought to the question of what an AI system must do.

### Achievements of the Symbolic Paradigm

The symbolic programme produced genuine and substantial results. The Logic Theorist, developed by Newell and Simon in 1955–56, proved 38 of the first 52 theorems in Whitehead and Russell's *Principia Mathematica*, some by proofs shorter than those in the original text. The General Problem Solver formalised means-ends analysis as a domain-general problem-solving strategy: given a current state and a goal state, identify the operators that reduce the difference between them and apply them iteratively.

Expert systems achieved impressive performance within narrow, well-defined domains. MYCIN, a rule-based system for the diagnosis of bacterial infections and the selection of appropriate antimicrobial therapy, encoded the reasoning of clinical specialists in approximately 600 production rules. On its target domain, its diagnostic performance was comparable to that of attending physicians—a striking demonstration that domain expertise could, in principle, be captured as explicit rules and executed by a machine. Chess-playing programs, built on minimax search with alpha-beta pruning and hand-crafted evaluation functions, progressed from curiosity to genuine mastery. The defeat of Garry Kasparov in 1997 by a combinatorial search system was a landmark demonstration that particular high-dimensional cognitive tasks could be solved, at superhuman level, by symbolic means.

The symbolic paradigm also made explicit an architectural property that deserves sustained attention: *compositionality*. A symbolic system that represents the binary relation *parent-of($x$, $y$)* can immediately construct the ternary relation *grandparent-of($x$, $z$)* as the composition *parent-of(parent-of($x$, $y$), $z$)*. Rules compose. Representations combine according to explicit principles. The ability to construct an unbounded set of expressions from a finite vocabulary and a finite set of combinatorial operations—a property that any adequate theory of natural language must account for—is implemented transparently in symbolic systems. Whether neural systems acquire compositionality implicitly, or merely approximate it in restricted domains, is one of the questions to which Chapter 12 returns.

### The Knowledge Acquisition Bottleneck

The structural failure of the symbolic paradigm was not a failure of the underlying formal ideas but of the engineering programme they required. To build an intelligent system on symbolic foundations, one must encode the relevant domain knowledge explicitly: every fact as a symbolic structure, every rule as a formal operation, every exception as an additional case. This work must be carried out by human knowledge engineers, working with domain experts to elicit and formalise knowledge that is, in practice, largely tacit and difficult to articulate.

The scaling problem is severe. Human expertise is accumulated through years of embodied experience and is not straightforwardly articulable as rules. A physician making a differential diagnosis draws on thousands of interacting variables—symptom patterns, base rates, drug interactions, patient history—in ways that resist complete formalisation. A native speaker of a language can judge grammaticality reliably while being unable to enumerate the rules governing every syntactic construction. Minsky and Papert (1969) identified a related form of scaling difficulty at the level of computational units: their analysis of the perceptron demonstrated that even simple classification tasks—such as computing the exclusive-or function—exceed what is achievable by systems built from linear threshold units, the symbolic primitives of their era.

The broader consequence is that the rate at which useful knowledge can be formalised in a symbolic system is bounded by the rate at which human experts can articulate and verify it. The world, however, does not respect this bound. The number of distinct situations a deployed intelligent system might encounter grows rapidly with the richness of the environment; the knowledge required to handle them grows proportionally. Manual encoding scales linearly—one rule, one fact, one exception at a time—while the problem scales far faster.

Expert systems made this bottleneck concrete. MYCIN's diagnostic performance was impressive within its defined scope of bacterial sepsis. Extending it to adjacent domains—fungal infections, viral illness, antimicrobial prophylaxis in surgery—required rebuilding the knowledge base essentially from scratch. There was no mechanism for the system to transfer what it had learned about bacterial diagnosis to neighbouring problems. Every new domain required a new, expensive knowledge engineering project, and the cost accumulated without bound as the intended scope of the system expanded. The O(domains) cost of the symbolic approach—proportional to the number of distinct domains rather than fixed—made the programme unscalable.

### What the Paradigm Left Behind

Before examining the paradigm that displaced symbolic AI, intellectual honesty requires acknowledging what symbolic systems offered and that statistical learning sacrifices, at least by default.

*Interpretability.* A rule-based system's reasoning can, in principle, be traced step by step. If such a system recommends a medical treatment or flags a legal case, it can produce a derivation: the rules it applied, the facts it used, and the chain of inference that produced the conclusion. A human expert can audit this derivation, identify errors, and propose corrections. This is not a minor convenience; it is a prerequisite for accountability in high-stakes deployment and for the scientific understanding of system behaviour.

*Compositionality.* The meaning of a symbolic expression is determined systematically by the meanings of its constituents and the rules governing their combination. This property holds recursively and unconditionally: it does not degrade as expressions grow longer or contexts grow more complex.

*Correctability.* If a rule is wrong, it can be identified, removed, and replaced. A symbolic system is, at least in principle, a transparent object whose behaviour can be modified with surgical precision.

Statistical systems do not recover these properties automatically. The programme of mechanistic interpretability—which attempts to reverse-engineer the internal computations of neural networks from their weights and activations—can be understood as an effort to reclaim them post hoc. The difficulty of that programme, examined in Chapter 12, reflects how much was surrendered. The reader should hold this in mind throughout the course: the successes of the statistical paradigm are real and substantial, but they come at a cost that has not been fully paid.

---

## 1.3 The Statistical Turn: Learning as Inference from Data

### The Fundamental Reframing

The insight that defines the statistical paradigm can be stated in a single sentence: *instead of encoding knowledge, infer it from observations.*

The symbolic approach requires human experts to articulate what the system should know before deployment. The statistical approach requires only that representative examples of correct input-output behaviour be collected; a learning algorithm then searches for the regularities that account for those examples and, ideally, extend to new ones. The knowledge acquisition bottleneck is not eliminated but transformed. The rate-limiting factor is no longer the articulability of expert knowledge but the availability of labelled examples and the computational capacity to process them. Both of these have scaled dramatically with the accumulation of digital information and the progress of semiconductor manufacturing—in ways that manual knowledge engineering has not and cannot.

It is tempting to present this transition as an obvious improvement and leave it at that. The preceding section argues against such a reading. The shift from symbolic to statistical AI is a trade: the knowledge acquisition bottleneck is exchanged for a different set of problems—opacity, brittleness under distribution shift, difficulty of correction—whose severity is still being assessed. What makes the statistical paradigm worth adopting is not that it solves all problems but that its failure modes scale differently than those of the symbolic approach, and that the problems it solves—high-dimensional perception, fluent language, flexible generalisation—are the ones on which symbolic methods have proven intractable.

### The Formal Framework: Learning as Function Approximation

The statistical approach to supervised learning is formalised as follows. We assume that observations arise as pairs $(\mathbf{x}, y)$ drawn from an unknown joint distribution $P(X, Y)$ over an input space $\mathcal{X}$ and an output space $\mathcal{Y}$. Here $X$ is a random variable taking values in $\mathcal{X}$, $Y$ is a random variable taking values in $\mathcal{Y}$, and their joint distribution $P$ encodes the relationship between inputs and outputs that we wish to model. The distribution $P$ is unknown; we cannot query it directly.

What we have access to is a finite training dataset

$$\mathcal{D} = \{(\mathbf{x}_i, y_i)\}_{i=1}^{n}$$

where each pair $(\mathbf{x}_i, y_i)$ is drawn independently from $P(X, Y)$. The i.i.d. assumption—*independent and identically distributed*—means that no pair conveys information about any other (independence), and that all pairs are generated by the same distribution (identical distribution). This assumption is frequently violated in practice and its violation is a source of genuine difficulties; for the theoretical development that follows, it is adopted as a foundation.

The *learning problem* is to find a function $f: \mathcal{X} \rightarrow \mathcal{Y}$ that predicts $y$ from $\mathbf{x}$ with small error under $P$. The function is not searched over the space of all measurable functions from $\mathcal{X}$ to $\mathcal{Y}$—this space is too vast to be useful. Instead, the learner restricts attention to a *hypothesis class* $\mathcal{H}$: a set of candidate functions $h: \mathcal{X} \rightarrow \mathcal{Y}$ with a specific parametric or structural form. This formulation is the standard one, developed in pedagogical form by Mitchell (1997) and with full rigour by Vapnik (1998) and Bishop (2006).

The choice of hypothesis class encodes an *inductive bias*: prior structural assumptions about the relationship between inputs and outputs. Consider three examples to make this concrete.

If $\mathcal{X} = \mathbb{R}^d$ and $\mathcal{Y} = \mathbb{R}$, and $\mathcal{H}$ is chosen to be the class of affine functions

$$h(\mathbf{x}) = \mathbf{w}^\top \mathbf{x} + b, \quad \mathbf{w} \in \mathbb{R}^d, \, b \in \mathbb{R}$$

then the inductive bias is strong: the target function is assumed to vary linearly with its inputs. This assumption is often incorrect but renders the search problem tractable and the resulting estimators statistically well-behaved.

Expanding $\mathcal{H}$ to all polynomials of degree at most $k$ in the components of $\mathbf{x}$ relaxes the bias: the target may be nonlinear, subject to the constraint of polynomial form with bounded degree. Expressiveness increases with $k$, but so does the risk of fitting idiosyncrasies of the training data rather than the underlying pattern.

Expanding $\mathcal{H}$ to encompass all feedforward neural networks with a specified depth, width, and activation function produces a hypothesis class that is, in principle, capable of approximating any continuous function on a compact domain—a result we state precisely in Chapter 2. The cost is that the search over this class is a non-convex optimisation problem with no closed-form solution, requiring iterative gradient-based methods and carrying no general convergence guarantee.

This trade-off between the expressiveness of $\mathcal{H}$ and the tractability of searching it runs throughout the course. Every design decision in modern deep learning—the choice of architecture, activation function, regularisation scheme, and optimiser—can be understood, at least in part, as a response to this trade-off.

### Loss Functions and the Risk Functional

To measure the quality of a hypothesis $h \in \mathcal{H}$, we require a *loss function* $\mathcal{L}: \mathcal{Y} \times \mathcal{Y} \rightarrow \mathbb{R}_{\geq 0}$ that assigns a non-negative cost to predicting $h(\mathbf{x})$ when the true output is $y$. Three choices are standard.

The *squared error loss*, $\mathcal{L}(h(\mathbf{x}), y) = (h(\mathbf{x}) - y)^2$, is the natural choice for real-valued regression: it penalises large errors quadratically and is twice continuously differentiable, facilitating gradient-based optimisation.

The *zero-one loss*, $\mathcal{L}(h(\mathbf{x}), y) = \mathbf{1}[h(\mathbf{x}) \neq y]$, counts mistakes directly and is the most natural measure of classification accuracy. It is, however, non-differentiable and non-convex, making it unsuitable as a direct optimisation objective.

The *cross-entropy loss*, applied to a probabilistic classifier that outputs a distribution $\hat{p}(\cdot \mid \mathbf{x})$ over $\mathcal{Y}$, is defined as $\mathcal{L}(\hat{p}, y) = -\log \hat{p}(y \mid \mathbf{x})$. This is the loss function used in language modelling and classification throughout deep learning. Its information-theoretic derivation and its connection to maximum likelihood estimation are the subject of Section 1.4.

The ideal objective of learning is to minimise the *expected risk* of $h$ under the true distribution:

$$\mathcal{R}[h] = \mathbb{E}_{(\mathbf{x}, y) \sim P}\left[\mathcal{L}(h(\mathbf{x}), y)\right]$$

The function $h^* \in \mathcal{H}$ that minimises $\mathcal{R}[h]$ over the hypothesis class is the best approximation to the true relationship that $\mathcal{H}$ can express. The function that minimises expected risk over *all* measurable functions—not just those in $\mathcal{H}$—is called the *Bayes-optimal predictor* and achieves the *Bayes error* $\mathcal{R}^*$: the irreducible residual noise in the data-generating process that no learner can eliminate, because it reflects inherent stochasticity in $P(Y \mid X)$ rather than any deficiency in the hypothesis class or the algorithm.

The fundamental difficulty is that $\mathcal{R}[h]$ cannot be computed directly, since $P$ is unknown. What the learner can compute, given $\mathcal{D}$, is the *empirical risk*—the average loss on the training sample:

$$\hat{\mathcal{R}}_n[h] = \frac{1}{n} \sum_{i=1}^{n} \mathcal{L}(h(\mathbf{x}_i), y_i)$$

By the law of large numbers, $\hat{\mathcal{R}}_n[h] \xrightarrow{P} \mathcal{R}[h]$ as $n \to \infty$, for any fixed $h$. Empirical risk minimisation (ERM), the foundational strategy of statistical learning theory (Vapnik, 1998), selects the hypothesis that minimises empirical risk over $\mathcal{H}$:

$$\hat{h} = \arg\min_{h \in \mathcal{H}} \frac{1}{n} \sum_{i=1}^{n} \mathcal{L}(h(\mathbf{x}_i), y_i)$$

ERM is the conceptual engine of all supervised machine learning. Gradient descent on neural networks, linear regression via least squares, decision tree induction—all are instances or approximations of ERM for specific choices of $\mathcal{H}$ and $\mathcal{L}$.

Whether minimising empirical risk leads to a hypothesis with low expected risk—the question of *generalisation*—is not guaranteed by ERM alone. The answer depends on the relationship between the complexity of $\mathcal{H}$, the sample size $n$, and the structure of $P$. When $\mathcal{H}$ is too expressive relative to $n$, a learner can achieve zero empirical risk by memorising the training data while failing entirely on new examples. This failure mode—*overfitting*—and the formal tools for analysing it, including the bias-variance tradeoff and the VC dimension, are the subject of Section 1.3 of this chapter, developed in Part 2.

### The Function Approximation View of Language Modelling

To ground the abstract framework in the specific problem that motivates this course, consider how supervised learning applies to language modelling.

The input space $\mathcal{X}$ is the set of all finite sequences of tokens drawn from a vocabulary $\mathcal{V}$: a sequence $(x_1, x_2, \ldots, x_{t-1})$ where each $x_k \in \mathcal{V}$ and the vocabulary $\mathcal{V}$ typically has size $|\mathcal{V}|$ between $32{,}000$ and $128{,}000$ items. The output space $\mathcal{Y}$ is the probability simplex $\Delta^{|\mathcal{V}|-1}$: a probability distribution over the next token $x_t$. The learning problem is to find a function $f$ such that $f(x_1, \ldots, x_{t-1})$ approximates the conditional distribution $P(x_t \mid x_1, \ldots, x_{t-1})$ specified by the true generating process of human language.

The training dataset $\mathcal{D}$ is a large corpus of text produced by humans—a finite sample from the extraordinarily complex joint distribution over token sequences that constitutes natural language. The hypothesis class $\mathcal{H}$ is a family of transformer neural networks parameterised by a weight vector $\boldsymbol{\theta}$, which specifies the values of all matrices and bias vectors in the network. The loss function is cross-entropy between the predicted distribution and the observed next token. ERM corresponds to finding the parameter vector $\boldsymbol{\theta}$ that minimises average cross-entropy over all context-token pairs in the training corpus.

On this account, a large language model is a statistical function approximator: it approximates the conditional next-token distribution of language using a high-capacity parametric model trained by ERM on a large corpus. This is a precise and productive description. Whether the function so learned constitutes *knowledge* or *understanding* in any richer sense—whether the representational structures that emerge from optimising this objective are, in some philosophically significant way, models of the world rather than sophisticated pattern matchers—is a question that neither the ERM framework nor the training procedure can answer. It is, as noted in Section 1.1, among the most important open questions in the field. Chapter 12 returns to it. In the intervening chapters, the function approximation framing is the foundation on which everything else rests.

<!-- END PART 1 -->