---
background: /midjourney_svd.jpg
layout: cover
hideInToc: true
---
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

# Pitfalls of Linear Models

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
<span style="color:gray; font-size: 11px; float: right;">Image credit: Midjourney.<br> Prompt: ‘mathematical invertible matrix in style of engraving'
</span>
</div>

---

# The Overfitting Problem

<div class="grid grid-cols-[7fr_5fr] gap-4">
<div>

* **Key Observation**: More features $\neq$ better predictions
	* Training error always decreases with more features
	* But **test error** follows a U-shape!

<v-clicks>

* Why does this happen?
	* The model **memorizes noise** in the training data
	* Complex models have too many degrees of freedom
	* With $p \gg N$, OLS can fit training data **perfectly** — zero error!
	* But this means the model has *learned noise, not signal*

</v-clicks>
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_7.1.png" style="width: 300px; position: relative">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;">Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/">ESL Fig. 7.1</a>
    </figcaption>
  </figure>
<br>

> *"The most important problem in ML is the problem of overfitting"*<br>— Yaser Abu-Mostafa
</div>
</div>


<!--
Mention Andrew Ng's perspective: overfitting is the single most important practical problem.
Yaser Abu-Mostafa in "Learning from Data" emphasizes that fitting noise is the enemy.
Ask students: what happens if we have 10 data points and 100 features?
-->

---
zoom: 1.2
---
# Pitfalls of Linear Models


<v-clicks depth="2">

* Inference: $\underset{\textcolor{grey}{(N,~)}}{\hat{Y}} = \underset{\textcolor{grey}{(N, ~d+1)}}X \times \underset{\textcolor{grey}{(d+1,~)}}W$

* Loss function (RSS): $\mathcal{L}(\hat{Y}, Y) = (Y - \hat{Y})^T (Y - \hat{Y}) = \sum\limits_{i=1}^N (Y_i - \hat{Y}_i)^2$

* Optimal weights after "training": $\hat{W}_{OLS} = \underset{\textcolor{grey}{(d+1, ~d+1)}}{(X^T X)^{-1}} \underset{\textcolor{grey}{(d+1, ~)}}{X^T Y}$
	* Are there any restrictions for this solution?

</v-clicks>

---

# Pitfalls of Linear Models

* $\hat{W}_{OLS} = \underset{\textcolor{grey}{(d+1, ~d+1)}}{(X^T X)^{-1}} \underset{\textcolor{grey}{(d+1, ~)}}{X^T Y}$
	* Are there any restrictions for this solution?

<v-clicks>

* All parts except inversion look fine
* To understand when <span title="Gram matrix" class="icon-btn opacity-100 !border-none !hover:text-black p-0"> $X^T X$ </span> is invertible, let’s look at Singular Value Decomposition (SVD):
<div class="grid grid-cols-[9fr_5fr]">
<div>

* $X = U \Sigma V^T$,  <br> s.t. $~U^T U = \underset{\textcolor{grey}{N}}I$, $~~~V^T V = \underset{\textcolor{grey}{d+1}}I$

* $X^T X = (V \Sigma^T U^T)(U \Sigma V^T) = V \Sigma^T \Sigma V^T$
</div>
<div>
<br>
  <figure>
    <img src="/SVD.png" style="width: 290px; position: relative">
  </figure>
</div>
</div>
</v-clicks>

---

# Pitfalls of Linear Models

* $\hat{W}_{OLS} = \underset{\textcolor{grey}{(d+1, ~d+1)}}{(X^T X)^{-1}} \underset{\textcolor{grey}{(d+1, ~)}}{X^T Y}$
	* Are there any restrictions for this solution?
* We need to ensure that: <br> $X^T X = (V \Sigma^T U^T)(U \Sigma V^T) = V \Sigma^T \Sigma V^T$ is invertible

<v-clicks depth="2">

* But this is true only when $\underset{\textcolor{grey}{(d+1,~N)}}{\Sigma^T} \underset{\textcolor{grey}{(N, ~d+1)}}\Sigma$ is invertible!
	* Two cases:
		* If $d+1 \leq N$, we only require all singular values to be $> 0$
		* If $d+1 > N$, automatically some of the diagonal values are zero and the matrix is singular (non-invertible)
</v-clicks>
---

# Pitfalls of Linear Models

* $\underset{\textcolor{grey}{(d+1,~N)}}{\Sigma^T} \underset{\textcolor{grey}{(N, ~d+1)}}\Sigma$ must be invertible
* How to fix the case when $d+1 > N$?

<v-clicks depth="3">

* Possible approaches:
	* Select only a subset of features $d^\prime$ to ensure $d^\prime+1 \leq N$
		* **Best Subset Selection**, **Forward/Backward/Mixed Stepwise Subset Selection**
	* Add regularization terms
		* **Lasso**, **Ridge**, **ElasticNet**
	* Reduce dimensionality of the data
		* **Principal Component Regression**, **Partial Least Squares**
</v-clicks>
<br>
<v-click>

> **Important**: Even when $X^TX$ *is* invertible ($d+1 \leq N$), we may still <br> benefit from these methods to **prevent overfitting** and improve predictions!

</v-click>

<!--
This is a crucial point. The mathematical singularity problem is just one reason.
The deeper reason is the bias-variance tradeoff: we trade a little bias for a big reduction in variance.
Andrew Ng: "High bias = underfitting, High variance = overfitting."
-->