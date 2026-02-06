---
background: /midjourney_curse_of_dimensionality.jpg
layout: cover
hideInToc: true
---
<br>
<br>
<br>

# Dimensionality Reduction

<div>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<span style="color:gray; font-size: 11px; float: right;">Image credit: Midjourney.<br> Prompt: ‘curse of dimensionality'
</span>
</div>

---

# Dimensionality Reduction

* Original predictors $X_{p \times 1}$ are transformed into **new predictors** $Z_{M \times 1}$, where $M < p$
* **Linear transformation matrix** $\phi_{p \times M}$ produces $M$ linear transformations from $X_{1:p}$ <br>
$Z := X^{\prime} \phi = [X_1, ..., X_p] {\scriptsize \begin{bmatrix}\phi_{11} & \ldots & \phi_{1M} \\ \vdots & \ddots & \vdots \\ \phi_{p1} & \ldots & \phi_{pM}\end{bmatrix}}$ <br>
$Z_m := \sum\limits_{j=1}^p \phi_{jm} X_j = X^{\prime} \phi_{\cdot, m}$, where $\phi_{\cdot, m} := [\phi_{11}, ..., \phi_{p1}]^{\prime}$
* So, we can achieve $M < n < p$ to be able to estimate least squares coefficients in <br> $\hat{y}_i := \hat{\theta}_0 + \hat{\theta}_1 z_1 + ... + \hat{\theta}_M z_M$
* Estimation of $\beta_{1:p}$ is equivalent to the estimation of $\theta_{1:M}$ for the given $\phi$ matrix:
$$\sum\limits_{m=1}^M \theta_m z_{im} = \sum\limits_{m=1}^M \theta_m \sum\limits_{j=1}^p \phi_{im} x_{ij} = \sum\limits_{m=1}^M \sum\limits_{j=1}^p \theta_m \phi_{jm} x_{ij} = \sum\limits_{j=1}^p \sum\limits_{m=1}^M \theta_m \phi_{jm} x_{ij} = \sum\limits_{j=1}^p \beta{j} x_{ij}$$

---

# Dimensionality Reduction: PCA on Advertising

* For $100$ cities we have the following predictors: Ad Spending ``ad`` & Population ``pop``
* We wish to **decorrelate** the predictors via PCA: <br>
$Z_1 :=
\color{grey}\underbrace{\color{#006}{0.839}}_{\phi_{11}}
\color{#006}{~\cdot~}
\color{grey}\underbrace{\color{#006}{(\mathrm{pop} - \overline{\mathrm{pop}})}}_{\mathrm{residual}}
\color{#006}{~+~}
\color{grey}\underbrace{\color{#006}{0.544}}_{\phi_{21}}
\color{#006}{~\cdot~}
\color{grey}\underbrace{\color{#006}{(\mathrm{ad} - \overline{\mathrm{ad}})}}_{\mathrm{residual}}
\color{#006}{~=~}
\color{grey}\underbrace{\color{#006}{[0.839, 0.544]}}_{\mathrm{loadings}}
\color{#006}{~\cdot~} [r_{\mathrm{pop}}, r_{\mathrm{ad}}]$
	* We apply loadings to every observation to compute their first PC, $[z_{i1}]_{1:n}$
	* Loadings help us "move" points to the new position (or rotate coordinate system)
	* $\mathbb{V}Z_1$ is maximal
		* We note that ``pop`` is slightly more important than ``ad`` ($0.839 > 0.544$)
* If $\mathbb{V}Z_1 >> \mathbb{V}Z_2$, then we drop $Z_2$ and build <br> $Y | Z_1 = \theta_0 + \theta_1 Z_1 + \epsilon$
* PC variances are estimated by **eigenvalues**

---

# Dimensionality Reduction: PCA
### Principal Components Analysis (PCA)

<br>
<figure>
  <img src="/ISLP_figure_6.14.svg" style="width: 530px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 400px;">Image source:
    <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=262">ISLP Fig. 6.14</a>
  </figcaption>
</figure>
<br>

#### Don't forget to standardize variables before PCA!

<!--
Sebastian Raschka: "PCA is sensitive to the scale of features. Always standardize first."
The principal components are computed from the covariance (or correlation) matrix.
If one feature has range 0-1000 and another 0-1, the first will dominate the PCs.
-->

---
layout: iframe

# PCA demo
url: https://setosa.io/ev/principal-component-analysis/
---

---
layout: iframe

# PCA more demo
url: https://projector.tensorflow.org/
---

---

# (If you see this from PDFs)

* PCA demos:
	* https://setosa.io/ev/principal-component-analysis/
	* https://projector.tensorflow.org/

---

# Dimensionality Reduction: Choosing # of PCs

* We prefer the number of PCs that capture **most** variability of all of the predictors $X_i$
	* The uncaptured variability is assumed to be rising from noise
* Scree or Elbow plot are often used
	* Look for the biggest drop in proportion of variance explained
		* Or for biggest rise in cumulative proportion of variance explained
* This compression works best for linearly related predictors

<div class="grid grid-cols-[7fr_8fr] gap-1">
<div>

* You can "compress" thousands of predictors to a dozen with little "information loss"
* For non-linearly related predictors, the scree plot may not have an "elbow"
</div>
<div>
<figure>
  <img src="/ISLP_figure_12.3.svg" style="width: 430px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 230px;">Image source:
    <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=518">ISLP Fig. 12.3</a>
  </figcaption>
</figure>
</div>
</div>

---

# Dimensionality Reduction: Partial Least Squares

* In PCA, an unsupervised method, the response is not used
	* So, we can't guarantee that the PCs will perform better in predicting response
* PLS finds new features that best explain the response, $y$
	* Predictors most correlated to response get greatest weight
* Standardize $X_i$
* PLS direction $Z_1$ is determined by the coefficient $\beta_1$ from regresion $Y \sim X_1$
* PLS direction $Z_2$
	* Regress each remaining variable on $Z_1$ and consider just the residuals, $r_2$
		* They contain most of the remaining information
	* Then again, determine the direction of $Z_2$ from regression $r_2 \sim X_1$

<!--
PLS is supervised unlike PCA. It uses the response to guide the dimensionality reduction.
In practice, PLS is less commonly used than PCA + regression or Ridge/Lasso.
But PLS is popular in chemometrics and bioinformatics where p >> n.
-->

---
zoom: 0.9
---

# Dimensionality Reduction: PCR in Practice

* **Principal Component Regression (PCR)** = PCA + OLS on top PCs
	1. Standardize features
	2. Compute PCs from $X$ (ignoring $Y$)
	3. Keep top $M$ PCs that explain most variance
	4. Fit linear regression: $Y = \theta_0 + \theta_1 Z_1 + ... + \theta_M Z_M + \epsilon$

<div class="grid grid-cols-[4fr_3fr] gap-2">
<div>
<v-click at="1">

* **Key insight**: PCR is closely related to Ridge regression
	* Both shrink less-important directions more than important ones
	* Ridge shrinks **gradually**; PCR **drops** low-variance components entirely
</v-click>
<v-click at="2">

* **Limitation**: PCA doesn't use $Y$<br> — the top PCs may not be the most predictive!
	* That's why PLS exists
		* but in practice, Ridge/Lasso are often better
</v-click>
</div>
<div>
<br>
<br>
<v-click at="3">

* scikit-learn:
```python
from sklearn.decomposition import PCA
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import Pipeline

pcr = Pipeline([('pca', PCA(n_components=5)),
                ('reg', LinearRegression())])
```
</v-click>
</div>
</div>

<!--
Andrew Ng: PCR is a useful technique but Ridge/Lasso are usually preferred in practice.
The connection between PCR and Ridge is elegant: both shrink along the principal component directions,
but Ridge does it smoothly while PCR does hard thresholding.
-->

---
zoom: 0.9
---

# Beyond Linear Dimensionality Reduction

#### PCA assumes **linear** relationships between features
#### For non-linear structure, consider:

<div class="grid grid-cols-[5fr_3fr] gap-2">
<div>
<v-clicks depth="3">

* **t-SNE** (t-distributed Stochastic Neighbor Embedding)
	* Great for **visualization** (2D/3D) of high-dimensional data
	* Preserves local neighborhood structure
	* Not suitable for feature extraction in predictive models

* **UMAP** (Uniform Manifold Approximation and Projection)
	* Faster than t-SNE, preserves more global structure
	* Increasingly popular for exploratory data analysis

* **Kernel PCA**
	* Applies PCA in a higher-dimensional space via the kernel trick
	* Can capture non-linear relationships

</v-clicks>
</div>
<div>
<br>
<br>
<br>
<br>
<v-click at="11">

> These are **visualization/exploration** tools.<br> For prediction, regularization (Ridge/Lasso/ElasticNet) usually wins!
</v-click>
</div>
</div>

<!--
Yann LeCun and Yoshua Bengio have emphasized that learning good representations (features)
is at the heart of modern ML. t-SNE/UMAP are key tools for understanding what your model learns.
Andrej Karpathy uses t-SNE extensively to visualize embedding spaces in neural networks.
-->