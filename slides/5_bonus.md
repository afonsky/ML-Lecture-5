---
layout: center
---
# Extra Materials

---

# Key Takeaways

<v-clicks>

1. **Overfitting** is the central challenge — more features $\neq$ better model
2. **Subset Selection** reduces features but can be computationally expensive
	* Forward Stepwise is the practical go-to
3. **Regularization** shrinks coefficients — the workhorse approach in practice
	* **Ridge**: shrinks all coefficients, never zeroes out, numerically stable
	* **Lasso**: automatic feature selection, some $\beta_i$ become exactly zero
	* **ElasticNet**: combines both, handles correlated features — safe default
4. **Dimensionality Reduction** (PCA) transforms features into uncorrelated components
	* PCR = PCA + regression; closely related to Ridge
5. **Cross-validation** is your best friend for choosing hyperparameters
6. **Always scale features** before regularization

</v-clicks>

> *"With four parameters I can fit an elephant, <br> and with five I can make him wiggle his trunk."* — John von Neumann

<!--
This quote beautifully captures the essence of overfitting.
The whole lecture is about trading a little bias for a lot of variance reduction.
Emphasize: there's no single "best" method — use CV to decide.
-->

<!-- ---

# The Regularization Decision Tree
<br>
<br>

```mermaid {scale: 0.55}
flowchart LR
    A[Start: Linear Model] --> B{Many features?}
    B -- "No (p < 20)" --> C["Try OLS first"]
    C --> D{Overfitting?}
    D -- No --> E["Done! ✓"]
    D -- Yes --> F{Need feature selection?}
    B -- "Yes (p ≥ 20)" --> F
    F -- Yes --> G["Lasso or ElasticNet"]
    F -- No --> H["Ridge Regression"]
    F -- "Not sure" --> I["ElasticNet (safe default)"]
    G --> J["Cross-validate λ and α"]
    H --> J
    I --> J
    J --> K["Evaluate on test set"]
    K --> E

    A ~~~ B

    style E fill:#90EE90
    style J fill:#FFE4B5
``` -->

<!--
Andrej Karpathy's philosophy: start with the simplest thing that could possibly work.
Only add complexity when you can demonstrate it helps on validation data.
This tree is a simplification but captures the practical workflow well.
-->

---
zoom: 0.9
---

# Bias, Variance and Model Complexity

<div class="grid grid-cols-[3fr_2fr]">
<div>
<br>

<figure>
<img src="/Train_Validation_Test.svg" style="width: 430px !important">
</figure>

* **Goals**:
	* **select** a model that generalizes well
		* i.e. yields low error on unseen observations
		* Done on **validation** set
	* **measure** its performance (as an error or accuracy)
		* Done on **test** set (on **validation** as well)
* More complex models lead to overfitting<br> (high variance, and low bias)
	* Model complexity correlates with degrees of freedom (DF) and # of estimated parameters
* For a fixed model, prediction error depends on train set of observations

</div>
<div>
<br>
<br>
<br>
<br>
<br>
  <figure>
    <img src="/ESL_figure_7.1.png" style="width: 350px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;">Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=239">ESL Fig. 7.1</a>
    </figcaption>
  </figure>
</div>
</div>

---
zoom: 0.95
---

# Bias-Variance Decomposition

* Consider a model with additive noise: $Y = f(X) + \varepsilon$
* If we assume $\mathbb{E}\varepsilon = 0$, $\mathbb{V}\varepsilon = \sigma_\varepsilon^2 > 0$, we can compute EPE at $X = x_0$:<br>
$\mathrm{Err}(x_0) := \mathbb{E}\bigg[ \big(Y - \hat{f}(x_0)\big)^2\bigg] = \sigma_\varepsilon^2 +
\color{grey}\underbrace{\color{green} \bigg[\mathbb{E}\hat{f}(x_0) - f(x_0) \bigg]^2}_{\mathrm{Bias}^2(\hat{f})}
\color{#006}{+~}
\color{grey}\underbrace{\color{red} \mathbb{E}\bigg[\hat{f}(x_0) - \mathbb{E}\hat{f}(x_0) \bigg]^2}_{\mathrm{Variance,~} \mathbb{V}\hat{f}},
$<br>
where the irreducible error $\sigma_\varepsilon^2$ is out of our control
* If we assume a **kNN** model $\hat{f}_k$:
$~\mathrm{Err}(x_0) := \sigma_\varepsilon^2 +
\color{green} \big[\hat{f}(x_0) - k^{-1} \sum\limits_{\ell=1}^k f(x_{(\ell)}) \big]^2
\color{#006}{+~}
\color{red} k^{-1}\sigma_\varepsilon^2
$
	* Low $k \Longrightarrow$ high model complexity, high variance, and (usually) low bias term
<div>
<br>

* If we assume a **linear** model $\hat{f}_p := x_0^{\prime}\hat\beta_{p \times 1}$:
$~\mathrm{Err}(x_0) := \sigma_\varepsilon^2 +
\color{green} \big[\hat{f}(x_0) - x_0^{\prime}\hat\beta \big]^2
\color{#006}{+~}
\color{red} \mathbb{V}\hat{f}(x_0)
$
	* where ${\small\hat\beta := (\bm{X}^\prime\bm{X})^{-1}\bm{X}_{p \times N}^\prime \bm{y}}$ and ${\small \mathbb{V}\hat{f}(x_0) = \mathbb{V}[x_0^\prime (\bm{X}^\prime\bm{X})^{-1}\bm{X}^\prime \bm{y}] = \mathbb{V}[\bm{h}^\prime (x_0) \bm{y}] = ||\bm{h}(x_0)||^2 \sigma_\varepsilon^2}$
		* ${\small \bm{h}(x_{p \times 1})_{N \times 1} := \bm{X}(\bm{X}^\prime\bm{X})^{-1}x}$ is the vector of linear weights at observation $x$
	* Low $p \Longrightarrow$ low model complexity, low variance, high bias
</div>