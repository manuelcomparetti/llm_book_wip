

## 1.4 Generalisation as a Problem

ERM, established in Section 1.3, selects the hypothesis that minimises average loss on the training data:

$$\hat{h} = \arg\min_{h \in \mathcal{H}} \frac{1}{n} \sum_{i=1}^{n} \mathcal{L}(h(\mathbf{x}_i), y_i)$$

The objective being minimised is the *training error*—a quantity the learning algorithm has full access to and can, in principle, make arbitrarily small by choosing $\mathcal{H}$ to be sufficiently expressive. The quantity of actual interest is the *test error*: the expected loss of the trained hypothesis under the true distribution $P$. ERM optimises the former as a proxy for the latter. When the proxy is reliable, the system learns. When it is not, the system overfits. Understanding precisely when and why the proxy fails is the task of this section.

### Training Error, Test Error, and the Generalisation Gap

Let $\hat{h}$ be the hypothesis returned by ERM on a training dataset $\mathcal{D}$ of size $n$. Its *training error* is the empirical risk at the solution:

$$\varepsilon_{\text{train}} = \hat{\mathcal{R}}_n[\hat{h}] = \frac{1}{n} \sum_{i=1}^{n} \mathcal{L}(\hat{h}(\mathbf{x}_i), y_i)$$

Its *test error*—equivalently, its expected risk or generalisation error—is the average loss under the true data-generating distribution:

$$\varepsilon_{\text{test}} = \mathcal{R}[\hat{h}] = \mathbb{E}_{(\mathbf{x}, y) \sim P}\!\left[\mathcal{L}(\hat{h}(\mathbf{x}), y)\right]$$

The *generalisation gap* is the difference between these two quantities:

$$\Delta = \varepsilon_{\text{test}} - \varepsilon_{\text{train}} = \mathcal{R}[\hat{h}] - \hat{\mathcal{R}}_n[\hat{h}]$$

By construction, $\varepsilon_{\text{train}}$ is minimised by $\hat{h}$: no hypothesis in $\mathcal{H}$ achieves lower training error on $\mathcal{D}$. There is, however, no analogous guarantee for $\varepsilon_{\text{test}}$. The gap $\Delta$ can be large—far larger than the noise floor $\mathcal{R}^*$—when $\mathcal{H}$ is much more expressive than the available data can constrain.

The problem is not intrinsic to ERM as a principle but to the conditions under which it is applied. ERM is sound when $\hat{\mathcal{R}}_n[h]$ is a reliable estimator of $\mathcal{R}[h]$ *uniformly* across all $h \in \mathcal{H}$—that is, when the convergence $\hat{\mathcal{R}}_n[h] \to \mathcal{R}[h]$ holds simultaneously for every hypothesis in the class. When $\mathcal{H}$ is large, some hypothesis may achieve low empirical risk by chance, without corresponding low expected risk. ERM will select that hypothesis, producing a large generalisation gap.

In practice, $\varepsilon_{\text{train}}$ and $\varepsilon_{\text{test}}$ are estimated on disjoint subsets of labelled data: a training set used to fit $\hat{h}$, and a held-out test or validation set used only for evaluation. Because the test set has never influenced $\hat{h}$, it provides an approximately unbiased estimate of $\mathcal{R}[\hat{h}]$. The train/validation/test split protocol is a standard engineering practice whose justification is precisely this argument.

### Overfitting, Underfitting, and the Complexity of the Hypothesis Class

The complexity of $\mathcal{H}$ is the central parameter governing the generalisation gap. Two failure modes bracket the space of choices.

*Underfitting* occurs when $\mathcal{H}$ is too restricted to represent the true relationship between $\mathbf{x}$ and $y$ accurately. The result is high training error and high test error simultaneously: the model cannot express the correct answer regardless of the amount of data available. The remedy is to expand $\mathcal{H}$—to increase model capacity.

*Overfitting* occurs when $\mathcal{H}$ is so expressive that it fits idiosyncrasies of the particular sample $\mathcal{D}$ that are not representative of $P$. The result is low training error but high test error: the model has memorised the training data rather than learning the underlying pattern. The remedy is to constrain $\mathcal{H}$—by restricting its complexity, introducing regularisation, or collecting more data.

A classical illustration is polynomial regression. Suppose the true relationship is $y = f^*(\mathbf{x}) + \epsilon$ where $f^*$ is a smooth, mildly nonlinear function and $\epsilon$ is independent noise. Let $\mathcal{H}_d$ denote the class of polynomials of degree at most $d$ in the components of $\mathbf{x}$.

For $d = 1$ (linear functions), the model is too rigid to capture the curvature of $f^*$; training error and test error are both elevated. This is underfitting. For a moderate value such as $d = 3$ or $d = 4$, the model has sufficient flexibility to approximate $f^*$ well; both errors are low. For $d = n - 1$, a polynomial of degree $n-1$ can pass exactly through all $n$ training points, achieving $\varepsilon_{\text{train}} = 0$; but it will oscillate wildly between them, and $\varepsilon_{\text{test}}$ will be very large. This is severe overfitting.

This example makes precise the intuition that model complexity must be matched to the available data. Plotting training error and test error as functions of $d$ reveals the canonical picture: training error decreases monotonically with $d$; test error initially decreases, reaches a minimum at the appropriate complexity, then rises as overfitting takes hold. The gap $\Delta$ widens as $d$ increases beyond the optimal point. With larger $n$, the optimal $d$ shifts rightward: more data can support a more complex model.

### The Bias-Variance Decomposition

The bias-variance decomposition provides a formal account of the underfitting-overfitting tension. The setup is as follows. Assume squared error loss and the noise model $y = f^*(\mathbf{x}) + \epsilon$, where $f^*: \mathcal{X} \rightarrow \mathbb{R}$ is the true regression function and $\epsilon$ is zero-mean noise with variance $\sigma^2$, independent of $\mathbf{x}$ and of the training set $\mathcal{D}$.

Define the *average hypothesis* $\bar{h}(\mathbf{x}) = \mathbb{E}_{\mathcal{D}}[\hat{h}_{\mathcal{D}}(\mathbf{x})]$: the expectation, over all training sets of size $n$ drawn from $P$, of the ERM solution. For a fixed input point $\mathbf{x}$, the expected squared error of the trained hypothesis admits the following exact decomposition:

$$\mathbb{E}_{\mathcal{D}, \epsilon}\!\left[\left(y - \hat{h}_{\mathcal{D}}(\mathbf{x})\right)^2\right] = \underbrace{\left(\bar{h}(\mathbf{x}) - f^*(\mathbf{x})\right)^2}_{\text{Bias}^2} + \underbrace{\mathbb{E}_{\mathcal{D}}\!\left[\left(\hat{h}_{\mathcal{D}}(\mathbf{x}) - \bar{h}(\mathbf{x})\right)^2\right]}_{\text{Variance}} + \underbrace{\sigma^2}_{\text{Noise}}$$

The derivation follows by writing the error as the sum of three centred terms: noise $\epsilon$, a bias term $f^*(\mathbf{x}) - \bar{h}(\mathbf{x})$ (deterministic), and a variance term $\bar{h}(\mathbf{x}) - \hat{h}_{\mathcal{D}}(\mathbf{x})$ (zero-mean over $\mathcal{D}$ by definition of $\bar{h}$). Expanding the square of their sum, all three cross-products vanish: the noise-bias cross-product vanishes because $\mathbb{E}[\epsilon] = 0$; the noise-variance cross-product vanishes because $\epsilon$ is independent of $\mathcal{D}$ and $\mathbb{E}[\epsilon] = 0$; and the bias-variance cross-product vanishes because $\mathbb{E}_{\mathcal{D}}[\hat{h}_{\mathcal{D}}(\mathbf{x})] = \bar{h}(\mathbf{x})$ by definition. The three squared terms remain, giving the stated result.

The three components have precise interpretations.

**Bias²** = $(\bar{h}(\mathbf{x}) - f^*(\mathbf{x}))^2$: the squared distance between the average prediction of ERM and the true function. Bias is a property of the hypothesis class $\mathcal{H}$, not of any particular training set. It is high when $\mathcal{H}$ cannot express $f^*$, and low when $\mathcal{H}$ is expressive enough to approximate it closely. A linear hypothesis class fitting a cubic target has high bias regardless of $n$.

**Variance** = $\mathbb{E}_{\mathcal{D}}[(\hat{h}_{\mathcal{D}}(\mathbf{x}) - \bar{h}(\mathbf{x}))^2]$: the expected squared deviation of the trained hypothesis from its mean, over the randomness of the training set. Variance is high when small changes in $\mathcal{D}$ produce large changes in $\hat{h}_{\mathcal{D}}$—that is, when the model is sensitive to which particular sample was observed. High variance is the signature of overfitting.

**Noise** = $\sigma^2$: the Bayes error $\mathcal{R}^*$—the irreducible stochasticity of $P(Y \mid X)$, which no hypothesis can eliminate.

The practical content is the trade-off. Increasing the complexity of $\mathcal{H}$ typically reduces bias (the class can express more functions) and increases variance (more complex functions are more sensitive to the particular training sample). Increasing $n$ reduces variance—more data better constrains the estimate—without affecting bias. The optimal hypothesis class minimises the sum of bias² and variance for a given $n$; this optimum shifts toward greater complexity as $n$ grows. The goal of architecture search, regularisation design, and data collection strategy is, in each case, to achieve a favourable position in this trade-off.

### Capacity, Generalisation Bounds, and the Deep Learning Puzzle

The bias-variance decomposition is an intuitive framework for understanding overfitting; the *VC dimension* (Vapnik, 1998) is its formal counterpart for binary classification. The VC dimension $\text{VC}(\mathcal{H})$ is the cardinality of the largest set of points that $\mathcal{H}$ can *shatter*: classify correctly under every possible binary labelling. Intuitively, it measures how many degrees of freedom the hypothesis class possesses.

The *fundamental theorem of statistical learning* establishes that for a hypothesis class with finite VC dimension $d$, with probability at least $1 - \delta$ over the draw of a training set of size $n$ (Vapnik, 1998; Shalev-Shwartz & Ben-David, 2014, Chapters 3–5):

$$\mathcal{R}[\hat{h}] \leq \hat{\mathcal{R}}_n[\hat{h}] + O\!\left(\sqrt{\frac{d \log(n/d) + \log(1/\delta)}{n}}\right)$$

The generalisation gap is bounded by a term that grows with $d$ and shrinks as $n^{-1/2}$. This formalises the bias-variance trade-off: richer hypothesis classes require proportionally more data to close the gap. The bound is tight up to logarithmic factors for the worst-case distribution.

For modern deep neural networks, however, this classical picture encounters a significant puzzle. Large language models have parameter counts in the tens to hundreds of billions; their effective VC dimension is of comparable magnitude. Yet they are trained on datasets of comparable scale—tens to hundreds of billions of tokens—and they generalise to new text far better than the classical bound predicts. A model that, in the classical analysis, should overfit catastrophically instead achieves low test cross-entropy on previously unseen documents.

The resolution of this puzzle is an active area of research. It appears to involve an implicit bias of stochastic gradient descent toward solutions with particular geometric properties—solutions that, while not unique ERM minimisers, happen to generalise well—as well as aspects of loss surface geometry and the structure of overparameterised function classes that the classical VC-dimension framework does not capture. Chapter 2 examines the geometry of loss surfaces in detail. For now, it suffices to note that the classical theory is necessary but not sufficient to explain deep learning generalisation: it provides the right conceptual vocabulary while failing to predict the empirical facts at scale.

---

## 1.5 Entropy and Information Theory

Section 1.1 characterised the quality of a predictive model as the divergence between its predictions and observed outcomes, and flagged that this measurement would require information-theoretic tools. Three quantities—Shannon entropy, cross-entropy, and the Kullback-Leibler divergence—constitute the required vocabulary. Their relationships also clarify a central claim that this section makes explicit: a large language model is, at its core, an entropy-reduction machine.

### Shannon Entropy

Let $X$ be a discrete random variable taking values in a finite alphabet $\mathcal{X}$ with probability mass function $p(x) = P(X = x)$. The *Shannon entropy* of $X$ is

$$H(X) = -\sum_{x \in \mathcal{X}} p(x) \log p(x)$$

with the convention that $0 \log 0 = 0$, justified by the limit $\lim_{p \to 0^+} p \log p = 0$. When the natural logarithm is used, $H(X)$ is measured in *nats*; when log base 2 is used, it is measured in *bits*. The formulas and relationships are identical in either case; the difference is purely one of units. Machine learning practice standardly uses the natural logarithm, since it connects directly to log-likelihood. The coin example below uses natural units, noting the bit equivalent.

$H(X)$ is always non-negative. It equals $0$ when $X$ is deterministic—one outcome has probability 1—and achieves its maximum of $\log |\mathcal{X}|$ under the uniform distribution. Entropy measures *average uncertainty*: the expected amount of information gained upon observing the outcome of $X$. Shannon (1948) established that this is also, asymptotically, the minimum number of bits (or nats) required to encode the outcome of $X$ with a uniquely decodable code.

**Worked example: a fair coin versus a biased coin.**

Let $X \in \{0, 1\}$ represent the outcome of a coin flip. For a fair coin with $p(1) = p(0) = 0.5$:

$$H_{\text{fair}} = -\!\left(0.5 \log 0.5 + 0.5 \log 0.5\right) = -\log 0.5 = \log 2 \approx 0.693 \text{ nats} = 1 \text{ bit}$$

For a biased coin with $p(1) = 0.9$ and $p(0) = 0.1$:

$$H_{\text{biased}} = -\!\left(0.9 \log 0.9 + 0.1 \log 0.1\right) = -\!\left(0.9 \times (-0.105) + 0.1 \times (-2.303)\right) = 0.325 \text{ nats} \approx 0.469 \text{ bits}$$

The fair coin maximises entropy over all binary distributions: its outcome is maximally uncertain. The biased coin, whose outcome can be predicted with 90% accuracy without any observation, has substantially lower entropy. Observing the outcome of the biased coin provides less information, on average, because the outcome is less surprising. The relationship between entropy and compressibility is exact: under an optimal code, sequences of fair coin flips require 1 bit per flip, while sequences from the biased coin can be compressed to approximately 0.469 bits per flip.

### Cross-Entropy

Entropy characterises the irreducible uncertainty of a source. *Cross-entropy* characterises how well a candidate probability model $p_{\boldsymbol{\theta}}$ approximates the true distribution $p$.

For two distributions $p$ and $p_{\boldsymbol{\theta}}$ over the same alphabet $\mathcal{X}$, the *cross-entropy* is

$$H(p, p_{\boldsymbol{\theta}}) = -\sum_{x \in \mathcal{X}} p(x) \log p_{\boldsymbol{\theta}}(x)$$

Cross-entropy is the *expected negative log-probability* assigned to outcomes of $p$ by the model $p_{\boldsymbol{\theta}}$: it measures the average surprise experienced by a model whose distribution is $p_{\boldsymbol{\theta}}$ when it observes data drawn from $p$. If $p_{\boldsymbol{\theta}} = p$, then $H(p, p_{\boldsymbol{\theta}}) = H(p)$: the model is correct, and average surprise equals entropy. If $p_{\boldsymbol{\theta}} \neq p$, then $H(p, p_{\boldsymbol{\theta}}) > H(p)$: the model is wrong, and it is more surprised by the data than necessary.

In the supervised learning setting of Section 1.3, $p(\cdot \mid \mathbf{x})$ is the true conditional distribution of $Y$ given $\mathbf{x}$, and $p_{\boldsymbol{\theta}}(\cdot \mid \mathbf{x})$ is the model's predicted distribution. The cross-entropy loss at a single training example $(\mathbf{x}_i, y_i)$ is

$$\mathcal{L}(p_{\boldsymbol{\theta}}, (\mathbf{x}_i, y_i)) = -\log p_{\boldsymbol{\theta}}(y_i \mid \mathbf{x}_i)$$

This is a single-sample Monte Carlo estimate of $H(p(\cdot \mid \mathbf{x}_i), p_{\boldsymbol{\theta}}(\cdot \mid \mathbf{x}_i))$: using the one observed label $y_i$ as the sole representative of the true conditional distribution. Averaging over the training set recovers the empirical risk $\hat{\mathcal{R}}_n[p_{\boldsymbol{\theta}}]$.

### The Kullback-Leibler Divergence

The *Kullback-Leibler (KL) divergence* from $p$ to $p_{\boldsymbol{\theta}}$ is defined as

$$D_{\text{KL}}(p \| p_{\boldsymbol{\theta}}) = \sum_{x \in \mathcal{X}} p(x) \log \frac{p(x)}{p_{\boldsymbol{\theta}}(x)} = \mathbb{E}_{x \sim p}\!\left[\log \frac{p(x)}{p_{\boldsymbol{\theta}}(x)}\right]$$

The KL divergence is always non-negative: $D_{\text{KL}}(p \| p_{\boldsymbol{\theta}}) \geq 0$, with equality if and only if $p = p_{\boldsymbol{\theta}}$ almost everywhere. This follows directly from Jensen's inequality applied to the strictly convex function $-\log$. It is *not* symmetric—$D_{\text{KL}}(p \| p_{\boldsymbol{\theta}}) \neq D_{\text{KL}}(p_{\boldsymbol{\theta}} \| p)$ in general—and it does not satisfy the triangle inequality. For these reasons it is called a divergence, not a metric.

Cross-entropy and KL divergence are linked by a fundamental identity. Expanding $D_{\text{KL}}$ by splitting the logarithm:

$$D_{\text{KL}}(p \| p_{\boldsymbol{\theta}}) = \sum_x p(x) \log p(x) - \sum_x p(x) \log p_{\boldsymbol{\theta}}(x) = -H(p) + H(p, p_{\boldsymbol{\theta}})$$

Rearranging:

$$H(p, p_{\boldsymbol{\theta}}) = H(p) + D_{\text{KL}}(p \| p_{\boldsymbol{\theta}})$$

Cross-entropy decomposes additively into two terms: the entropy of the true source $H(p)$ (irreducible, independent of $\boldsymbol{\theta}$), and the KL divergence $D_{\text{KL}}(p \| p_{\boldsymbol{\theta}})$ (the additional cost due to model error, reducible by training). Minimising cross-entropy with respect to $\boldsymbol{\theta}$ is therefore equivalent to minimising KL divergence: the two objectives differ by a constant.

### Cross-Entropy as Maximum Likelihood Estimation

The connection between cross-entropy loss and maximum likelihood estimation is not a coincidence but an algebraic equivalence; understanding it is prerequisite to understanding why training objectives in deep learning are what they are.

The *likelihood* of the training data $\mathcal{D} = \{(\mathbf{x}_i, y_i)\}_{i=1}^n$ under the parametric model $p_{\boldsymbol{\theta}}$ is

$$\mathcal{L}(\boldsymbol{\theta}; \mathcal{D}) = \prod_{i=1}^{n} p_{\boldsymbol{\theta}}(y_i \mid \mathbf{x}_i)$$

This is the joint probability that the model assigns to the observed training labels, under the i.i.d. assumption. *Maximum likelihood estimation* (MLE) selects the parameter vector maximising this probability:

$$\hat{\boldsymbol{\theta}}_{\text{MLE}} = \arg\max_{\boldsymbol{\theta}} \prod_{i=1}^{n} p_{\boldsymbol{\theta}}(y_i \mid \mathbf{x}_i)$$

Taking logarithms converts the product to a sum—a monotone transformation that preserves the argmax:

$$\hat{\boldsymbol{\theta}}_{\text{MLE}} = \arg\max_{\boldsymbol{\theta}} \sum_{i=1}^{n} \log p_{\boldsymbol{\theta}}(y_i \mid \mathbf{x}_i)$$

Equivalently, negating and dividing by $n$:

$$\hat{\boldsymbol{\theta}}_{\text{MLE}} = \arg\min_{\boldsymbol{\theta}} \left(-\frac{1}{n} \sum_{i=1}^{n} \log p_{\boldsymbol{\theta}}(y_i \mid \mathbf{x}_i)\right)$$

This expression is the cross-entropy loss averaged over the training set. To see this precisely, define the *empirical distribution* $\hat{p}_n$ as the uniform distribution over the training pairs: $\hat{p}_n(\mathbf{x}, y) = \frac{1}{n}\sum_{i=1}^n \mathbf{1}[(\mathbf{x}_i, y_i) = (\mathbf{x}, y)]$. Then:

$$-\frac{1}{n} \sum_{i=1}^{n} \log p_{\boldsymbol{\theta}}(y_i \mid \mathbf{x}_i) = -\mathbb{E}_{(\mathbf{x}, y) \sim \hat{p}_n}\!\left[\log p_{\boldsymbol{\theta}}(y \mid \mathbf{x})\right] = H\!\left(\hat{p}_n,\, p_{\boldsymbol{\theta}}\right)$$

The negative average log-likelihood is the cross-entropy between the empirical distribution and the model. Applying the decomposition $H(\hat{p}_n, p_{\boldsymbol{\theta}}) = H(\hat{p}_n) + D_{\text{KL}}(