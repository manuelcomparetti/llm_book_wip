Good. I have everything I need. Let me now write Part 3.

## 1.6 Representation as Compression: The Conceptual Spine of the Course

The information-theoretic framework of Section 1.5 established that a language model trained by ERM is, precisely, a compression engine: it minimises the average code length for tokens in natural language by approximating the true conditional next-token distribution. That formulation raises a deeper question that the cross-entropy objective alone cannot answer: *what must a system have learned in order to compress language efficiently?*

The answer to this question is the conceptual spine of this course. It takes the form of a chain of four connected claims.

*Prediction → Compression.* A model that achieves low cross-entropy on held-out text has found a compact, generalising description of the statistical regularities of its training distribution.

*Compression → Representation.* A compact, generalising description must encode the structure of the domain—not its surface features. The internal states that carry this structure through the computation are the model's representations.

*Representation → Apparent Understanding.* Representations that accurately encode domain structure support behaviours that, observed from the outside, resemble understanding: coherent inference, correct analogical generalisation, flexible knowledge transfer.

Each link in this chain is substantive. Together they provide a principled—not merely empirical—account of why statistical learning, trained on the deceptively simple objective of next-token prediction, produces systems of remarkable cognitive range. They do not establish that such systems genuinely understand their domain; the word "apparent" carries its full weight. Whether appearance and reality coincide here is the question of Section 1.7 and the final subject of Chapter 12.

### Prediction Requires Discovering Regularities

The first link is a direct consequence of the generalisation analysis of Section 1.4.

A model that achieves low test cross-entropy on held-out text cannot have memorised its training data: it had no access to the test examples. Yet it assigns high probability to tokens that actually occur. The only explanation consistent with both observations is that the model has extracted, from the training corpus, a compact description of what makes text probable—a description that extends to new text from the same distribution.

The information-theoretic connection is exact. Shannon's source coding theorem establishes that the average code length for symbols drawn from a distribution $p$, encoded using a model $p_{\boldsymbol{\theta}}$, is $H(p, p_{\boldsymbol{\theta}})$ nats per symbol. A model with low cross-entropy is a good compressor. The Minimum Description Length (MDL) principle (Bishop, 2006, Chapter 1) makes the implication explicit: the best statistical model of a dataset is the one that minimises total description length—the model itself plus the data compressed by the model. A memorising model stores the entire training corpus verbatim; its description is very long, and it provides no compression of new data. A model that captures genuine regularities has a compact description that compresses any data from the same distribution, including data it has never seen. Generalisation and compression are, in this precise sense, the same phenomenon.

To make the point concrete: consider a corpus of text about weather. The bigram "rainy morning" occurs frequently; "rainy diamond" almost never. A model that compresses this corpus efficiently must assign high probability to "morning" following "rainy" and very low probability to "diamond." This captures a regularity—rainy weather co-occurs with mornings in human reports of weather—and it transfers to new texts about weather because the underlying regularity is stable across documents. The model has not been told about the causal structure of meteorology; it has discovered a statistical pattern that reflects that structure. The information extracted in the course of compression is a form of world knowledge, arrived at without any explicit knowledge engineering.

This is the sense in which the knowledge acquisition bottleneck of Section 1.2 is dissolved rather than solved by the statistical paradigm. The bottleneck was the cost of encoding knowledge manually, fact by fact. The statistical approach does not encode facts at all—it infers regularities from observations, storing them implicitly in the model's parameters. The cost is data and compute, not annotation.

### Compression Requires Structured Representations

The second link—efficient compression requires structured internal representations—follows from the requirement that the compression generalise across contexts.

A neural language model computes $p_{\boldsymbol{\theta}}(x_t \mid x_1, \ldots, x_{t-1})$ through a sequence of layer-wise transformations. At each layer $k$, the input sequence is mapped to a collection of intermediate vectors $\{\mathbf{z}_k^{(1)}, \ldots, \mathbf{z}_k^{(t-1)}\}$, one per position. These are the model's internal representations of the context at depth $k$. The final layer's representations are used to produce the next-token distribution.

For the prediction to be simultaneously accurate and general, the representations must be *selectively informative*: they must retain the aspects of the context that are predictive of the next token and discard the aspects that are not. The context preceding any given position contains far more information than is relevant to predicting what follows. Byte-level encoding choices, exact punctuation, the precise phrasing of clauses far back in the sequence—these carry negligible signal about the conditional next-token distribution in most contexts. What carries substantial signal is semantic content, syntactic structure, referential relationships, discourse topic, genre expectations, and world-knowledge implications of the text so far.

A representation that selectively retains these features has not merely compressed the input; it has organised the input according to what matters linguistically and semantically. That organisation is not trivially equivalent to understanding, but it is structurally isomorphic to a significant part of it: the model has learned which aspects of prior context govern which aspects of subsequent text. Layer by layer, successive transformations extract increasingly abstract features of the input—a point documented empirically for convolutional networks by Zeiler and Fergus (2014), as Chapter 3 details, and analogously observed for the hidden layers of large language models. Lower layers encode surface features; higher layers encode more abstract linguistic and semantic properties.

Chapter 3 formalises the intuition that useful representations are compressed representations through the Information Bottleneck framework (Tishby & Schwartz-Ziv, 2017), which characterises optimal intermediate representations as those that maximise information about the prediction target while minimising information retained from the raw input. That framework's empirical applicability to deep networks is contested, and Chapter 3 presents it accordingly. The underlying principle—that representations which generalise must be selective compressions of their inputs—is, however, robust to those debates and is the appropriate lens for understanding what a well-trained language model has learned.

### Structured Representations and Apparent Understanding

The third link—structured representations produce behaviour that resembles understanding—is the most empirically immediate and the most philosophically contested.

Consider the representations of words that emerge from training a language model on a large corpus. Semantically related words acquire geometrically proximate representations in the model's embedding space: the vector for "physician" is closer to that of "surgeon" than to that of "locomotive." This proximity is not programmed; it emerges because words that occur in statistically similar contexts—similar surrounding words, similar syntactic roles, similar discourse functions—are forced by the prediction objective to acquire similar representations. Words that occur in similar contexts carry similar information about what comes next, so their representations converge.

More strikingly, relational structure in the world is often encoded as consistent geometric offsets. The vector difference $\mathbf{v}(\texttt{king}) - \mathbf{v}(\texttt{man})$ approximates $\mathbf{v}(\texttt{queen}) - \mathbf{v}(\texttt{woman})$; $\mathbf{v}(\texttt{Paris}) - \mathbf{v}(\texttt{France})$ approximates $\mathbf{v}(\texttt{Berlin}) - \mathbf{v}(\texttt{Germany})$. These relationships are not engineered; they arise because the statistical co-occurrence structure of language reflects the relational structure of the world, and a model that compresses that co-occurrence structure efficiently acquires representations in which those relations are geometrically instantiated. LeCun, Bengio, and Hinton (2015) noted the general phenomenon: deep networks trained on rich data acquire representations that are increasingly abstract and invariant, encoding not just surface features but categories, relations, and structures.

Operationally, this encoding supports behaviours that look like understanding. A representation that encodes the royalty-gender relationship geometrically can support correct analogical completion on novel instances—not because the model was told about the relationship, but because its compression of the linguistic distribution has made that relationship explicit in the geometry of the representation space. A representation that encodes syntactic structure can support correct agreement resolution across long-distance dependencies—not because the model was given a grammar, but because syntactic structure is a regularity of language that efficient compression forces it to represent.

Whether this constitutes genuine understanding—whether a model that has learned to compress the statistical patterns of human language thereby understands language—is precisely the question that Section 1.7 declines to answer. What the chain establishes without philosophical ambiguity is a *causal account* of why these behaviours emerge. They are the predictable consequence of finding compact generalisable descriptions of the training distribution. Prediction requires compression; compression requires discovering structure; discovered structure supports behaviour. The chain is the deepest explanation available for why the statistical paradigm works as well as it does.

---

## 1.7 The Reasoning Question: Honestly Unresolved

This chapter has proposed a working definition of intelligence, traced the history of two competing paradigms, established the mathematical and information-theoretic foundations of statistical learning, and developed the prediction-compression-representation chain that links the training objective to the cognitive behaviours observed in large-scale systems. The chapter closes by posing, without resolving, the question that these developments leave open.

Is reasoning—the capacity to construct multi-step arguments, apply rules to novel instances, and draw valid inferences across systematically diverse situations—a qualitatively distinct cognitive capacity? Or is it an emergent consequence of sufficiently powerful and well-trained predictive systems?

This is a live scientific dispute. Serious researchers hold well-reasoned positions on both sides; substantive empirical evidence bears on the question without yet adjudicating it. The course returns to it at multiple points—Chapter 7 surveys the empirical record at scale; Chapter 10 examines structured prompting techniques that elicit apparent multi-step reasoning; Chapter 12 confronts the full evidence base including the most challenging failures. None of these chapters settle the question. This section states the dispute with the precision it deserves.

### The Prediction-Sufficiency View

The prediction-sufficiency view holds that reasoning, as it appears in capable AI systems, is an emergent property of large-scale prediction over rich data.

Human reasoning—mathematical inference, causal analysis, commonsense deduction, analogical generalisation—is extensively documented in the text that constitutes the training distribution of large language models. A model that compresses this distribution efficiently has, by the argument of Section 1.6, internalised the statistical patterns of reasoning as it appears in language. At sufficient scale, these patterns may be rich enough, and sufficiently well-generalised, to support multi-step inference on novel problems—not because the model has been given a reasoning engine, but because the patterns of reasoning are regularities of human language that efficient compression forces the model to represent.

The empirical record provides substantial support. Language models at scale solve novel mathematical problems, construct valid logical arguments in context, complete analogical reasoning tasks at levels competitive with graduate-level human performance on some benchmarks, and perform in-context learning—adapting their outputs to task specifications provided at inference time without any modification to model weights. Performance on structured reasoning benchmarks scales continuously with model size, training data volume, and compute budget, a pattern consistent with reasoning capacity as a quantitative property that improves with predictive power rather than appearing discontinuously at some architectural threshold.

The prediction-sufficiency view does not claim that current language models reason as humans do or that the mechanisms are identical. Its claim is more restrained: the apparent categorical distinction between "genuine reasoning" and "very powerful prediction" may not be as fundamental as it intuitively appears. What we call reasoning may be, at the computational level, structured prediction over a learned model of the inferential patterns that characterise competent human thought.

### The Pattern-Matching Critique

The pattern-matching critique holds that, despite impressive performance on many benchmarks, current language models exhibit systematic failure modes that are inconsistent with genuine reasoning and consistent with sophisticated pattern completion from a very large training distribution.

The empirical basis of the critique is precise. Language models fail on tasks requiring *compositional generalisation*: correctly applying a known rule to a novel combination of familiar primitives that was not represented in the training distribution. They are sensitive to *superficial rephrasing*: restating a logical problem in an unfamiliar syntactic form, or altering the order of premises, can dramatically change the model's response without altering the logical content—a pattern no genuine inference engine would exhibit. Performance on *precise counting and arithmetic* over numbers encoded in unfamiliar token-level representations degrades sharply, suggesting that numerical behaviour is tied to surface form rather than an internal model of arithmetic. Performance on *systematic spatial reasoning* tasks—problems that would be trivial for a system with an accurate three-dimensional spatial model—fails in ways consistent with guessing from learned visual-language correlations rather than deriving answers from a geometric model of the world.

These failures have a common structure: the model succeeds where the surface form of the problem—the specific words, syntax, and context—is a reliable guide to the answer, and fails where the problem requires a novel compositional inference that goes beyond the training distribution's statistical coverage. This is precisely the failure profile of a very high-quality pattern-completer, and it is inconsistent with the profile of a system that reasons over an internal model of the domain.

The pattern-matching critique does not argue that language models are unimpressive or that their capabilities are unimportant. It argues that the nature of those capabilities differs from what the prediction-sufficiency view implies. What looks like reasoning from the outside may be, under the hood, very high-quality interpolation in a learned representation space—sufficient for most practical purposes, insufficient for tasks that require genuinely systematic generalisation.

### The State of the Question

The two positions make different predictions about the trajectory of progress. The prediction-sufficiency view predicts that compositional generalisation failures will diminish as scale and training data quality improve. The pattern-matching critique predicts that qualitative failures will persist because they reflect a structural limitation of the learning objective, not merely insufficient capacity.

The current empirical record is consistent with both predictions: frontier models improve continuously on aggregate benchmarks while continuing to exhibit systematic failures of the kind the critique identifies. The evidence does not yet distinguish the hypotheses with sufficient resolution to adjudicate the dispute.

The position this course maintains is that we do not know the answer. It is scientifically possible that further scaling, combined with improved training procedures, will produce systems for which the prediction-sufficiency description is fully adequate. It is also scientifically possible that a different approach—one that explicitly incorporates structured symbolic computation, grounded representation, or some mechanism not currently deployed at scale—is required for robust systematic reasoning. Both possibilities are consistent with the available evidence; both are being actively pursued.

The question demands intellectual honesty of the reader: the ability to hold a genuine empirical uncertainty rather than resolving it prematurely in either direction. Chapter 12 returns to it with the best evidence currently available and with the same honest posture.

---

## 1.8 Roadmap: The Structure of This Course

The eleven chapters that follow develop the technical architecture, training procedures, deployment patterns, and open problems of modern LLM-based systems in a strictly sequential order. Each chapter depends on the concepts established by its predecessors.

**Chapter 2: Neural Networks: Architecture, Computation, and Optimisation.** The mathematical machinery of neural networks from the perceptron through the multilayer perceptron. Backpropagation is derived in full as reverse-mode automatic differentiation. The Adam optimiser is derived from its moment estimates and bias-correction procedure. The dynamical systems perspective on training—loss surface geometry, saddle points, attractors, and basins of attraction—is introduced and will recur throughout the course.

**Chapter 3: Deep Learning and the Geometry of Representations.** Why depth confers exponentially more representational power than shallow networks of comparable parameter count. Convolutional networks and the empirically documented hierarchy of learned features. Recurrent networks, the vanishing gradient pathology, and the LSTM as its resolution. The manifold hypothesis and the Information Bottleneck framework as formal tools for understanding representation learning—developed as a conceptual framework with honest acknowledgement of contested empirical status.

**Chapter 4: Reinforcement Learning and Adaptive Agents.** The Markov Decision Process as the foundational formalism for sequential decision-making under uncertainty. The Bellman equations derived from the definition of expected discounted return. Policy gradient methods derived via the log-derivative trick. Proximal Policy Optimisation. The chapter maps the language model post-training problem onto the MDP formalism, building the formal preparation for Chapter 8.

**Chapter 5: The Attention Mechanism: From Alignment to Self-Attention.** The sequence-to-sequence bottleneck that motivated the development of attention. Bahdanau additive attention derived step by step from the encoder failure mode it addresses. Luong dot-product attention and the natural emergence of the query-key-value abstraction. Self-attention, its parallelism advantage over recurrent architectures, and the $1/\sqrt{d}$ scaling factor derived from the variance argument.

**Chapter 6: The Transformer Architecture: A Complete Technical Specification.** Multi-head attention with full index notation. The feedforward sublayer as key-value memory. Sinusoidal positional encoding and its linearity property. The complete transformer block in pre-LN and post-LN variants. Parameter count analysis. Encoder-only, decoder-only, and encoder-decoder architectures compared systematically. FlashAttention and the IO-aware resolution of the quadratic memory cost of attention.

**Chapter 7: Large Language Models: Pretraining, Scaling Laws, and Emergent Capabilities.** Byte Pair Encoding tokenisation from first principles. The causal language modelling objective and its connection to the entropy minimisation framework developed in this chapter. The Kaplan et al. power laws for performance as a function of model size, training data, and compute budget. The Chinchilla reanalysis and its practical consequences. The empirical debate over emergent capabilities: the Wei et al. evidence and the Schaeffer et al. counter-argument, presented with equal rigour.

**Chapter 8: Post-Training: Alignment, RLHF, and Frontier Architectures.** The alignment gap between pretraining and deployment objectives. Supervised fine-tuning. RLHF via the Bradley-Terry preference model and the PPO-based optimisation framework from Chapter 4. Direct Preference Optimisation derived from the RLHF problem. Constitutional AI and RLAIF. LoRA for parameter-efficient adaptation. Mixture of Experts architectures. Benchmark evaluation and its systematic limitations.

**Chapter 9: Knowledge, Context, and the Architecture of Memory.** The four-way distinction between memorisation, retrieval, inference, and reasoning in LLMs—a conceptual framework that refines the question posed in this chapter. The KV cache and its memory cost formula. Rotary position embeddings for long-context generalisation. Retrieval-Augmented Generation from document chunking through dense and sparse retrieval to re-ranking. A memory taxonomy for LLM system design.

**Chapter 10: Agentic Programming: Reasoning, Tools, and Structured Execution.** The conditions under which a language model component constitutes an agent. Chain-of-Thought and Tree of Thoughts as techniques for externalising intermediate computation into the autoregressive generation sequence. ReAct and the Thought/Action/Observation execution cycle. Tool use, function calling schemas, and grammar-constrained decoding. Reflexion. Honest evaluation on HumanEval and SWE-bench.

**Chapter 11: Agentic Workflows: Multi-Agent Systems, Orchestration, and Production.** When multi-agent architectures are preferable to single-agent systems. Canonical orchestration patterns. The Model Context Protocol as a standardisation approach for tool connectivity. Shared state, concurrency, and consistency. Reliability failure modes specific to multi-agent systems, prompt injection and privilege escalation as security problems, and their countermeasures. The complexity dynamics of feedback-coupled agent networks.

**Chapter 12: Open Frontiers: Interpretability, Continual Learning, and the Limits of Current Paradigms.** Mechanistic interpretability and the transformer circuits framework. Catastrophic forgetting and Elastic Weight Consolidation. The reasoning question confronted with the full evidence base—including the most challenging failure modes—and left unresolved as an honest intellectual matter. Scalable oversight at frontier capability. The book closes by equipping the reader with the most important skill a practitioner in this field can possess: reliably distinguishing what current systems actually do from what they appear to do.

---

## References

### Primary References

Newell, A. & Simon, H.A. (1976). "Computer Science as Empirical Inquiry: Symbols and Search." *Communications of the ACM*, 19(3), 113–126.

Shannon, C.E. (1948). "A Mathematical Theory of Communication." *Bell System Technical Journal*, 27(3–4), 379–423 and 623–656.

Vapnik, V.N. (1998). *Statistical Learning Theory*. Wiley. [Chapters 1–3: ERM, VC dimension, structural risk minimisation]

Bishop, C.M. (2006). *Pattern Recognition and Machine Learning*. Springer. [Chapter 1: probability, decision theory, information theory fundamentals]

Mitchell, T. (1997). *Machine Learning*. McGraw-Hill. [Chapter 1: the learning problem as function approximation]

Minsky, M. & Papert, S. (1969). *Perceptrons: An Introduction to Computational Geometry*. MIT Press. [Introduction and Chapter 1: limits of linear threshold units; symbolic AI critique]

### Further Reading

Domingos, P. (2012). "A Few Useful Things to Know About Machine Learning." *Communications of the ACM*, 55(10), 78–87. [Accessible synthesis of core ML principles; recommended supplementary reading]

Shalev-Shwartz, S. & Ben-David, S. (2014). *Understanding Machine Learning: From Theory to Algorithms*. Cambridge University Press. [Chapters 1–5: rigorous treatment of PAC learning, ERM, and generalisation bounds]

LeCun, Y., Bengio, Y. & Hinton, G.E. (2015). "Deep Learning." *Nature*, 521, 436–444. [Authoritative overview by the field's principal architects; useful historical and forward-looking context]

<!-- END PART 3 — CHAPTER 1 COMPLETE -->