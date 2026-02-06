---
background: /midjourney_lasso.jpg
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

# Regularization Methods

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
<span style="color:gray; font-size: 11px; float: right;">Image credit: Midjourney.<br> Prompt: ‘lasso regression in style of engraving'
</span>
</div>
---


# Regularization Methods: Motivation

<div>
<br>
<v-plotly style="width: 700px !important; height: 400px !important"
:data="[
{
x: [0.5, 0.9, 1.8, 2.1, 2.9, 3.0, 3.9, 4.4],
y: [1.0, 1.9, 2.6, 1.7, 2.8, 3.4, 2.6, 3.7],
type: 'scatter',
mode: 'markers',
marker: {color: 'black', size: 10, opacity: 0.5},
name: 'All observations',
showlegend: true
},
{
x: [0.5, 1.8],
y: [1.0, 2.6],
type: 'scatter',
mode: 'markers',
marker: {color: 'red', size: 10, opacity: 1.0},
name: 'Train observations',
showlegend: true,
visible: 'legendonly'
},
{
x: [0.9, 2.1, 2.9, 3.0, 3.9, 4.4],
y: [1.9, 1.7, 2.8, 3.4, 2.6, 3.7],
type: 'scatter',
mode: 'markers',
marker: {color: 'green', size: 10, opacity: 1.0},
name: 'Test observations',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [-0.8461538461538474, 6.538461538461541],
type: 'scatter',
mode: 'lines',
line: {color: 'blue', width: 3, opacity: 0.3},
name: 'OLS regression line',
showlegend: true,
visible: 'legendonly'
},
{
x: [0.9, 0.9],
y: [1.4923076923076921, 1.9],
type: 'scatter',
mode: 'lines',
line: {color: 'magenta', width: 2, dash: 'dot', opacity: 0.3},
legendgroup: 'Residuals',
name: 'Residuals',
showlegend: true,
visible: 'legendonly'
},
{
x: [2.1, 2.1],
y: [1.7, 2.9692307692307693],
type: 'scatter',
mode: 'lines',
line: {color: 'magenta', width: 2, dash: 'dot', opacity: 0.3},
legendgroup: 'Residuals',
showlegend: false
},
{
x: [2.9, 2.9],
y: [2.8, 3.9538461538461545],
type: 'scatter',
mode: 'lines',
line: {color: 'magenta', width: 2, dash: 'dot', opacity: 0.3},
legendgroup: 'Residuals',
showlegend: false
},
{
x: [3.0, 3.0],
y: [3.4, 4.0769230769230775],
type: 'scatter',
mode: 'lines',
line: {color: 'magenta', width: 2, dash: 'dot', opacity: 0.3},
legendgroup: 'Residuals',
showlegend: false
},
{
x: [3.9, 3.9],
y: [2.6, 5.184615384615386],
type: 'scatter',
mode: 'lines',
line: {color: 'magenta', width: 2, dash: 'dot', opacity: 0.3},
legendgroup: 'Residuals',
showlegend: false
},
{
x: [4.4, 4.4],
y: [3.7, 5.8],
type: 'scatter',
mode: 'lines',
line: {color: 'magenta', width: 2, dash: 'dot', opacity: 0.3},
legendgroup: 'Residuals',
showlegend: false
},
{
x: [-1, 5.0],
y: [0.7891891891891896, 3.7283783783783777],
type: 'scatter',
mode: 'lines',
line: {color: 'orange', width: 3, dash: 'dash', opacity: 0.3},
name: 'Super-Duper model fit',
showlegend: true,
visible: 'legendonly'
}
]"
:layout="{
xaxis: {title: 'x', range: [0, 5]},
yaxis: {title: 'y', range: [0, 4.5]},
margin: {l: 40, r:20, b:70, t:20, pad: 2}
}"
:config="{displayModeBar: false}"
:options="{}"/>
</div>

---

# Regularization Methods

* Here we add $\mathrm{RSS}$ penalty to shrink model coefficients to zero during fitting
	* This results in fewer predictions and lower variability of coefficients <br>
	$\mathrm{RSS}(\lambda, \beta, X) := \sum(y_i - \hat{y}_i)^2 + \lambda \lVert\beta\rVert_k$, $~~\lambda \geq 0$
		* where $\lambda$ is a **tuning parameter**, $\lambda \lVert\beta\rVert_k$ is a **shrinkage penalty**, $\beta := \beta_{1:p}$

<v-clicks depth="2">

* **Ridge**: $k = 2$, i.e. $\lVert\beta\rVert_2^2 = \beta^{\prime}\beta = \sum \beta_i^2$
	* a.k.a. a 2-norm, Euclidean distance (from zero), $L_2$, $L^2$, $L2$, $\ell_2$
		* It is the length of $\beta$ vector
	* $\beta_i$ asymptotically approach zero with larger $\lambda$
* **Lasso**: $k = 1$, i.e. $\lVert\beta\rVert_1 = \bm{1}^{\prime} \beta = \sum\lvert\beta_i\rvert$
	* a.k.a. a 1-norm, Manhattan (taxi-cab) distance, $L_1$, $L^1$, $L1$, $\ell_1$
	* $\beta_i$ gradually approach and then **snap to zero** with larger $\lambda$
</v-clicks>

<!--
Josh Starmer's intuition: Ridge is like putting all features on a diet (they all get smaller).
Lasso is like eliminating features from the team entirely (some get exactly zero).
The "snap to zero" behavior of Lasso is what makes it useful for automatic feature selection.
Andrew Ng calls this "sparsity" — having many zero coefficients.
-->

---

# Regularization Methods

* $\mathrm{RSS}(\lambda, \beta, X) := \sum(y_i - \hat{y}_i)^2 + \lambda \lVert\beta\rVert_k$, $~~\lambda \geq 0$
* Both for **Ridge** and **Lasso**:
	* As $\lambda \to \infty$, optimizer focuses more on minimizing the penalty term
		* It's a **hyperparameter** we need to choose (by trying different values)
	* Note: $\beta_0$ is excluded from penalty b/c it does not relate predictors to response

---

# Regularization Methods: Ridge Example

<div class="grid grid-cols-[5fr_4fr] gap-3">
<div>
<br>
<v-plotly style="width: 550px !important; height: 350px !important"
:data="[
{
x: [0.5, 1.8],
y: [1.0, 2.6],
type: 'scatter',
mode: 'markers',
marker: {color: 'red', size: 10, opacity: 1.0},
name: 'Train',
showlegend: true,
visible: 'legendonly'
},
{
x: [0.9, 2.1, 2.9, 3.0, 3.9, 4.4],
y: [1.9, 1.7, 2.8, 3.4, 2.6, 3.7],
type: 'scatter',
mode: 'markers',
marker: {color: 'green', size: 10, opacity: 1.0},
name: 'Test',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [-0.8461538461538474, 6.538461538461541],
type: 'scatter',
mode: 'lines',
line: {color: 'blue', width: 3, opacity: 0.3},
name: 'OLS',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [-0.84615385, 6.53846154],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [-0.56613757, 6.03703704],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0.1',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [0.13754647, 4.77695167],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0.5',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [0.58807588, 3.9701897],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 1.0',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [0.84648188, 3.50746269],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 1.5',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [1.01405975, 3.20738137],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 2.0',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [1.59382204, 2.1692024],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 10.0',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [1.77782736, 1.8397045],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 100.0',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [0.7891891891891896, 3.7283783783783777],
type: 'scatter',
mode: 'lines',
line: {color: 'orange', width: 3, dash: 'dash', opacity: 0.3},
name: 'Super-Duper<br> model fit',
showlegend: true,
visible: 'legendonly'
}
]"
:layout="{
xaxis: {title: 'x', range: [0, 5]},
yaxis: {title: 'y', range: [0, 4.5]},
margin: {l: 40, r:20, b:70, t:20, pad: 2},
legend: {x:6}
}"
:config="{displayModeBar: false}"
:options="{}"/>
</div>
<div>
<br>
<br>


#### $\mathrm{RSS}_\mathrm{OLS} = \sum(y_i - \hat{y}_i)^2 = 0$
<br>
<v-click>

#### $\mathrm{RSS}_\mathrm{Ridge} = \\ \sum(y_i - \hat{y}_i)^2 + \lambda \cdot \beta^2$
</v-click>
<v-click>
<div>
<br>
<v-plotly style="width: 300px !important; height: 250px !important"
:data="[
{
x: [0., 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1. , 1.1, 1.2,
       1.3, 1.4, 1.5, 1.6, 1.7, 1.8, 1.9, 2. , 2.1, 2.2, 2.3, 2.4, 2.5,
       2.6, 2.7, 2.8, 2.9, 3. , 3.1, 3.2, 3.3, 3.4, 3.5, 3.6, 3.7, 3.8,
       3.9, 4. , 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 4.7, 4.8, 4.9, 5. , 5.1,
       5.2, 5.3, 5.4, 5.5, 5.6, 5.7, 5.8, 5.9, 6. , 6.1, 6.2, 6.3, 6.4,
       6.5, 6.6, 6.7, 6.8, 6.9, 7. , 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7,
       7.8, 7.9, 8. , 8.1, 8.2, 8.3, 8.4, 8.5, 8.6, 8.7, 8.8, 8.9, 9. ,
       9.1, 9.2, 9.3, 9.4, 9.5, 9.6, 9.7, 9.8, 9.9],
y: [14.657, 10.39 ,  7.553,  5.625,  4.295,  3.37 ,  2.726,  2.281,
        1.979,  1.78 ,  1.658,  1.592,  1.568,  1.576,  1.607,  1.655,
        1.717,  1.789,  1.867,  1.95 ,  2.036,  2.124,  2.214,  2.304,
        2.393,  2.482,  2.57 ,  2.656,  2.742,  2.825,  2.907,  2.987,
        3.065,  3.142,  3.216,  3.289,  3.36 ,  3.429,  3.497,  3.563,
        3.627,  3.689,  3.75 ,  3.809,  3.867,  3.923,  3.978,  4.032,
        4.084,  4.135,  4.184,  4.233,  4.28 ,  4.326,  4.371,  4.415,
        4.458,  4.499,  4.54 ,  4.58 ,  4.619,  4.657,  4.695,  4.731,
        4.767,  4.802,  4.836,  4.869,  4.902,  4.934,  4.965,  4.996,
        5.026,  5.055,  5.084,  5.112,  5.14 ,  5.167,  5.194,  5.22 ,
        5.245,  5.27 ,  5.295,  5.319,  5.343,  5.366,  5.389,  5.412,
        5.434,  5.455,  5.477,  5.498,  5.518,  5.538,  5.558,  5.578,
        5.597,  5.616,  5.634,  5.653],
mode: 'lines',
showlegend: false,
visible: 'true'
},
{
x: [-1, 11],
y: [1.5, 1.5],
type: 'scatter',
mode: 'lines',
line: {color: 'orange', width: 2, dash: 'dot', opacity: 0.3},
name: 'Super-Duper<br> model fit',
showlegend: false
}
]"
:layout="{
xaxis: {title: 'λ', range: [0, 10]},
yaxis: {title: 'RSS<sub>Ridge</sub>', range: [0, 14]},
margin: {l: 40, r:20, b:70, t:20, pad: 2}
}"
:config="{displayModeBar: false}"
:options="{}"/>
</div>
</v-click>
</div>
</div>

---

# Regularization Methods: Lasso Example

<div class="grid grid-cols-[5fr_4fr] gap-3">
<div>
<br>
<v-plotly style="width: 550px !important; height: 350px !important"
:data="[
{
x: [0.5, 1.8],
y: [1.0, 2.6],
type: 'scatter',
mode: 'markers',
marker: {color: 'red', size: 10, opacity: 1.0},
name: 'Train',
showlegend: true,
visible: 'legendonly'
},
{
x: [0.9, 2.1, 2.9, 3.0, 3.9, 4.4],
y: [1.9, 1.7, 2.8, 3.4, 2.6, 3.7],
type: 'scatter',
mode: 'markers',
marker: {color: 'green', size: 10, opacity: 1.0},
name: 'Test',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [-0.8461538461538474, 6.538461538461541],
type: 'scatter',
mode: 'lines',
line: {color: 'blue', width: 3, opacity: 0.3},
name: 'OLS',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [-0.84615385, 6.53846154],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [-0.3372781065088758, 5.627218934911243],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0.1',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [1.698224852071006, 1.9822485207100593],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0.5',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [1.8, 1.8],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 1.0',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [1.8, 1.8],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 1.5',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [0.17159763313609455, 4.715976331360947],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0.2',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [0.6804733727810649, 3.8047337278106514],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0.3',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [1.1893491124260356, 2.8934911242603554],
type: 'scatter',
mode: 'lines',
line: {color: 'grey', width: 3, opacity: 0.3},
name: 'λ = 0.4',
showlegend: true,
visible: 'legendonly'
},
{
x: [-1, 5.0],
y: [0.7891891891891896, 3.7283783783783777],
type: 'scatter',
mode: 'lines',
line: {color: 'orange', width: 3, dash: 'dash', opacity: 0.3},
name: 'Super-Duper<br> model fit',
showlegend: true,
visible: 'legendonly'
}
]"
:layout="{
xaxis: {title: 'x', range: [0, 5]},
yaxis: {title: 'y', range: [0, 4.5]},
margin: {l: 40, r:20, b:70, t:20, pad: 2},
legend: {x:6}
}"
:config="{displayModeBar: false}"
:options="{}"/>
</div>
<div>
<br>
<br>


#### $\mathrm{RSS}_\mathrm{OLS} = \sum(y_i - \hat{y}_i)^2 = 0$
<br>
<v-click>

#### $\mathrm{RSS}_\mathrm{Lasso} = \\ \sum(y_i - \hat{y}_i)^2 + \lambda \cdot \lvert \beta \rvert$
</v-click>
<v-click>
<div>
<br>
<v-plotly style="width: 300px !important; height: 250px !important"
:data="[
{
x: [0., 0.01, 0.02, 0.03, 0.04, 0.05, 0.06, 0.07, 0.08, 0.09, 0.1 ,
   0.11, 0.12, 0.13, 0.14, 0.15, 0.16, 0.17, 0.18, 0.19, 0.2 , 0.21,
   0.22, 0.23, 0.24, 0.25, 0.26, 0.27, 0.28, 0.29, 0.3 , 0.31, 0.32,
   0.33, 0.34, 0.35, 0.36, 0.37, 0.38, 0.39, 0.4 , 0.41, 0.42, 0.43,
   0.44, 0.45, 0.46, 0.47, 0.48, 0.49, 0.5 , 0.51, 0.52, 0.53, 0.54,
   0.55, 0.56, 0.57, 0.58, 0.59, 0.6 , 0.61, 0.62, 0.63, 0.64, 0.65,
   0.66, 0.67, 0.68, 0.69, 0.7 , 0.71, 0.72, 0.73, 0.74, 0.75, 0.76,
   0.77, 0.78, 0.79, 0.8 , 0.81, 0.82, 0.83, 0.84, 0.85, 0.86, 0.87,
   0.88, 0.89, 0.9 , 0.91, 0.92, 0.93, 0.94, 0.95, 0.96, 0.97, 0.98,
   0.99, 1.  , 1.01, 1.02, 1.03, 1.04, 1.05, 1.06, 1.07, 1.08, 1.09,
   1.1 , 1.11, 1.12, 1.13, 1.14, 1.15, 1.16, 1.17, 1.18, 1.19, 1.2 ,
   1.21, 1.22, 1.23, 1.24, 1.25, 1.26, 1.27, 1.28, 1.29, 1.3 , 1.31,
   1.32, 1.33, 1.34, 1.35, 1.36, 1.37, 1.38, 1.39, 1.4 , 1.41, 1.42,
       1.43, 1.44, 1.45, 1.46, 1.47, 1.48, 1.49],
y: [145.05 , 138.792, 132.705, 126.79 , 121.046, 115.474, 110.073,
       104.844,  99.786,  94.9  ,  90.185,  85.641,  81.269,  77.069,
        73.04 ,  69.182,  65.496,  61.981,  58.638,  55.466,  52.465,
        49.636,  46.979,  44.493,  42.178,  40.035,  38.064,  36.263,
        34.635,  33.177,  31.891,  30.777,  29.834,  29.062,  28.462,
        28.034,  27.777,  27.691,  27.777,  28.034,  28.463,  29.063,
        29.834,  30.777,  31.892,  33.178,  34.635,  36.264,  38.064,
        40.036,  42.179,  44.494,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,  46.98 ,
        46.98 ,  46.98 ,  46.98],
mode: 'lines',
showlegend: false,
visible: 'true'
}
]"
:layout="{
xaxis: {title: 'λ', range: [0, 1.5]},
yaxis: {title: 'RSS<sub>Lasso</sub>', range: [0, 150]},
margin: {l: 50, r:20, b:70, t:20, pad: 2}
}"
:config="{displayModeBar: false}"
:options="{}"/>
</div>
</v-click>
</div>
</div>

---

# Regularization Methods: Example on Credit Data

* Coefficients for important features drop to zero slower
<div class="grid grid-cols-[7fr_8fr] gap-1">
<div>

* Important features are: <br> ``income``, ``limit``, ``rating``, ``student``
* Unimportant features are in gray
* $\uparrow\lambda \Rightarrow \uparrow$ model bias, $\downarrow \mathbb{V} Y$, <br> $\downarrow$ model complexity
</div>
<div>
<figure>
  <img src="/ISLP_figure_6.4.svg" style="width: 400px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 230px;">Ridge. Image source:
    <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=249">ISLP Fig. 6.4</a>
  </figcaption>
</figure>
</div>
</div>
<br>
<div class="grid grid-cols-[7fr_8fr] gap-1">
<div>

* $r(\lambda) := \frac{\lVert\beta_{\lambda}^R\rVert_2}{\lVert\beta\rVert_2} \in [0, 1]$ is a 1-standardized penalty
	* $\uparrow r(\lambda) \Rightarrow \downarrow$ penalty
	* $\hat{\beta}_{\lambda}^R$ is a vector of best estimated ridge coefficients for each $\lambda$; $\hat{\beta}$ - LS coeffs.
</div>
<div>
<figure>
  <img src="/ISLP_figure_6.6.svg" style="width: 400px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 230px;">Lasso. Image source:
    <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=253">ISLP Fig. 6.6</a>
  </figcaption>
</figure>
</div>
</div>

---

# Credit Data Recap
<div class="grid grid-cols-[5fr_8fr] gap-10">
<div>
</div>
<div>
<figure>
  <img src="/Credit_data.png" style="width: 480px !important;">
</figure>
</div>
</div>

---

# Regularization: Bayesian Intuition

<v-clicks depth="2">

* **Key idea**: Regularization = having a **prior belief** about the weights
	* Before seeing data, we *believe* weights should be small (close to zero)
	* This belief **constrains** the model, preventing overfitting

* Formally: OLS minimizes RSS, which is equivalent to **Maximum Likelihood Estimation** (MLE)
	* MLE: *"Find weights that best explain the data"*

* Adding a prior on weights gives us **Maximum a Posteriori** (MAP) estimation:
	* MAP: *"Find weights that best explain the data AND are consistent with our beliefs"*
	* $\underbrace{\mathrm{p}(W | \mathrm{data})}_{\text{posterior}} \propto \underbrace{\mathrm{p}(\mathrm{data} | W)}_{\text{likelihood}} \cdot \underbrace{\mathrm{p}(W)}_{\text{prior belief}}$

</v-clicks>

<!--
Yaser Abu-Mostafa: "Regularization is the price we pay for simplicity."
The Bayesian view makes regularization feel natural — we're just encoding our preference for simpler models.
This connects to Occam's Razor: prefer the simplest explanation consistent with the data.
-->

---

# Regularization: Bayesian Intuition

* **The choice of prior determines the type of regularization:**

| Prior on weights $\mathrm{p}(W)$ | Shape | Regularization | Effect |
|:---:|:---:|:---:|:---:|
| Normal (Gaussian) | Bell curve | **Ridge** ($L_2$) | Shrinks all $\beta$ toward zero |
| Laplace (Double-exponential) | Peaked at zero | **Lasso** ($L_1$) | Some $\beta$ exactly zero |

<v-clicks>

* **Why Lasso produces exact zeros**: The Laplace prior has a *sharp peak* at zero
	* This encourages the optimizer to snap coefficients to exactly zero
	* The Normal prior has a *smooth peak* — coefficients get small but never reach zero

* In summary: **Regularization = Occam's Razor** (prefer simpler explanations)
	* > *"Everything should be made as simple as possible, but not simpler"*<br> — Albert Einstein ([from here](https://en.wikiquote.org/wiki/Albert_Einstein))

</v-clicks>

<!--
This Bayesian interpretation is beautiful because it unifies different regularization methods under one framework.
Yann LeCun often discusses how priors/constraints shape learning.
For advanced students: if we don't just take the MAP estimate but integrate over the full posterior, we get Bayesian regression / Gaussian Processes.
-->

---

# Regularization Methods: Lagrangian Formulation

* In optimization theory, the following are equivalent for $\beta = \beta_{1:p}$, $k = 1, 2$: <br>
$\hat{\beta}_{0:p} = \argmin\limits_{\forall \beta} \{\mathrm{RSS}_{\beta_0, \beta} + \lambda \lVert\beta\rVert_k\} = \argmin\limits_{\forall \beta} \{\mathrm{RSS}_{\beta_0, \beta} ~|~ \lVert \beta \rVert_k \leq s\}$
	* where: $\mathrm{RSS}_{\beta_0, \beta}$ is **objective function**, $\lVert \beta \rVert_k \leq s$ is **constraint (budget)**
	* For any $\lambda$, we can find some $s$ that yields the same minimum
	* Large budget wields unconstrained OLS
* We can also express the best subset selection (BSS) as: <br>
$\hat{\beta}_{0:p} = \argmin\limits_{\forall \beta} \{ \mathrm{RSS}_{\beta_0, \beta} ~|~ \lVert\beta\rVert_{\textcolor{red}{0}} \leq s\}$
	* where $\lVert\beta\rVert_0 = \sum I \{\beta_i \neq 0 \}$, i.e. just a count of coefficients is constrained

---

# Regularization Methods: Lasso vs. Ridge

* We can use Lagrangian formulation to compare Lasso and Ridge models

<br>
<div>
<figure>
  <img src="/ridge_lasso_explained.png" style="width: 690px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: -50px; left: 650px;">Based on:
    <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=255">ISLP Fig. 6.7</a>
  </figcaption>
</figure>
</div>

<!--
This is the most important geometric intuition!
The diamond shape (L1) has corners on the axes → the elliptical contours of RSS are more likely
to touch the constraint region at a corner where one coefficient is exactly zero.
The circle shape (L2) has no corners → contours touch at non-zero values.
Yaser Abu-Mostafa uses this exact diagram to explain why Lasso gives sparse solutions.
Andrew Ng: "This geometric view is the best way to understand why L1 gives sparsity."
-->

---

# Regularization Methods: Lasso vs. Ridge

<div class="grid grid-cols-[8fr_4fr] gap-10">
<div>
<figure>
  <img src="/ISLP_figure_6.8.png" style="width: 540px !important;">
</figure>
<br>
<figure>
  <img src="/ISLP_figure_6.9.png" style="width: 540px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: -50px; left: 650px;">Based on:
    <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=257">ISLP Figs. 6.8 & 6.9</a>
  </figcaption>
</figure>
</div>
<div>
<br>
<br>

* All predictors are informative
<br>
<br>
<br>
<br>
<br>
<br>
<br>

* Only 2 predictors are related to the response
</div>
</div>

<!--
Key insight (Josh Starmer / StatQuest): When most predictors matter, Ridge performs better.
When only a few predictors matter, Lasso wins because it zeroes out the irrelevant ones.
In real life, we usually don't know which case we're in — so try both (or use ElasticNet).
-->

---

# ElasticNet: Best of Both Worlds

* Combines Ridge ($L_2$) and Lasso ($L_1$) penalties:

$$\mathrm{RSS}_{\mathrm{ElasticNet}} = \sum(y_i - \hat{y}_i)^2 + \lambda \Big[ \alpha \cdot \lVert\beta\rVert_1 + (1-\alpha) \cdot \lVert\beta\rVert_2^2 \Big]$$

<v-clicks depth="2">

* Mixing parameter $\alpha \in [0, 1]$ controls the blend:
	* $\alpha = 1$: pure Lasso &nbsp;&nbsp;|&nbsp;&nbsp;  $\alpha = 0$: pure Ridge

* **When to use ElasticNet?**
	* Correlated predictors (groups of related features)
		* Lasso may **arbitrarily** pick one from a group; ElasticNet keeps the group
	* When you're unsure whether Ridge or Lasso is better — **safe default**
	* Often the go-to regularization in practice

* scikit-learn: ``ElasticNet(alpha=1.0, l1_ratio=0.5)``
	* ``l1_ratio`` = $\alpha$ (mixing parameter)

</v-clicks>

<!--
Yoshua Bengio: "In practice, ElasticNet often outperforms both Ridge and Lasso."
Mention Zou & Hastie (2005) who introduced ElasticNet specifically to handle correlated features.
-->

---
zoom: 0.9
---

# Feature Scaling: Critical for Regularization!

* **Problem**: Regularization penalizes large coefficients
	* But coefficient size depends on the **scale** of the feature!
	* Feature in meters vs. kilometers $\to$ different coefficient $\beta$, same prediction


* **Solution**: Always **standardize** features before regularization
$$\tilde{X}_j = \frac{X_j - \bar{X}_j}{s_j}$$

<div class="grid grid-cols-[8fr_4fr] gap-10">
<div>
<v-click at="1">

* Why? After standardization, a unit change in any feature means the same thing
	* The penalty treats all coefficients **fairly**
	* The intercept $\beta_0$ is **NOT** regularized<br> (it's just $\bar{Y}$ after centering)
</v-click>
<v-click at="2">

* **Practical tip** (from Sebastian Raschka):
  * Use ``Pipeline`` to avoid data leakage
</v-click>
</div>
<div>
<br>
<br>
<v-click at="2">

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import Ridge

pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('model', Ridge(alpha=1.0))
])
pipe.fit(X_train, y_train)
```
</v-click>
</div>
</div>

<!--
Andrew Ng emphasizes this in every regularization lecture.
Without scaling, a small-range feature (0–1) gets almost no penalty while a large-range feature (0–10000) gets heavily penalized. This makes regularization unfair.
Always use Pipeline to prevent fitting the scaler on test data.
-->

---
zoom: 0.9
---

# Choosing $\lambda$: Cross-Validation

* $\lambda$ is a **hyperparameter** — we choose it, not learn it from data

<v-clicks depth="2">

* Standard approach: **k-fold Cross-Validation**
	1. Define a grid of $\lambda$ values, e.g. $\lambda \in \{10^{-4}, 10^{-3}, ..., 10^{2}, 10^{3}\}$
	2. For each $\lambda$, compute average validation error across $k$ folds
	3. Pick the $\lambda$ with lowest CV error (**min rule**)

* **One Standard Error rule** (Hastie & Tibshirani):
	* Pick the simplest model (largest $\lambda$) whose CV error is within 1 SE of the minimum
	* Leads to more regularized, **simpler** models

* scikit-learn makes this easy:
	* ``RidgeCV``, ``LassoCV``, ``ElasticNetCV`` — efficient built-in CV
	* ``LassoCV`` uses coordinate descent along the **regularization path**
		* Much faster than fitting independent models for each $\lambda$

</v-clicks>

<!--
Mention that scikit-learn's CV estimators use efficient algorithms.
LassoCV uses warm starts along the regularization path (LARS algorithm).
Andrew Ng: always use validation/CV for hyperparameter tuning, never test set.
-->

---
zoom: 0.9
---

# Regularization: Practical Tips

> *"Start simple, add complexity only as needed"*<br>— Andrew Ng

<v-clicks>

1. **Always scale** your features before applying regularization
2. **Start with Ridge** as a baseline (it's numerically stable and rarely hurts)
3. **Use Lasso** if you need automatic feature selection
4. **Use ElasticNet** as a safe default when unsure
5. **Cross-validate** to select $\lambda$ (and $\alpha$ for ElasticNet)
6. **Never** use training error to select hyperparameters

</v-clicks>

<v-click>
<br>
<div class="bg-orange-100 p-3 rounded">

#### Common Mistakes to Avoid
* Regularizing the intercept $\beta_0$
* Forgetting to standardize features
* Using training metrics to select $\lambda$
* Applying regularization without understanding bias-variance tradeoff

</div>
</v-click>

<!--
Karpathy's recipe for training neural nets (adapted for linear models):
1. First, verify your model can overfit a small batch (catches bugs).
2. Then regularize to improve generalization.
Sebastian Raschka: regularization should be thought of as part of model selection, not model fitting.
-->

---

# Regularization: Ex. for Classification

<div class="grid grid-cols-[4fr_9fr] gap-2">

<div>
  <figure>
  <img src="/iris_setosa.png" style="width: 135px !important;">
  <img src="/iris_versicolor.png" style="width: 135px !important;">
  <img src="/iris_virginica.png" style="width: 135px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 9px;">Images source:<br>
    <a href="https://en.wikipedia.org/wiki/Iris_flower_data_set">https://en.wikipedia.org/wiki/Iris_flower_data_set</a>
  </figcaption>
</figure>
</div>
<div>
  <figure>
  <img src="/coefficient_path_plot_original.svg" style="width: 700px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 9px;">Image source:
    <a href="https://scikit-learn.org/stable/auto_examples/linear_model/plot_logistic_path.html">https://scikit-learn.org/stable/auto_examples/linear_model/plot_logistic_path.html</a> (reproduced)
  </figcaption>
  </figure>
</div>
</div>

---

# Regularization: Ex. for Classification

<div class="grid grid-cols-[3fr_3fr] gap-2">

<div>
  <figure>
  <img src="/coefficient_path_plot.svg" style="width: 325px !important;">
  </figure>
</div>
<div>
  <figure>
  <img src="/decision_boundary_C_0.0308.svg" style="width: 290px !important;">
  </figure>
</div>
</div>
<br>

<div class="grid grid-cols-[3fr_3fr] gap-2">

<div>
  <figure>
  <img src="/decision_boundary_C_0.0664.svg" style="width: 290px !important;">
  </figure>
</div>
<div>
  <figure>
  <img src="/decision_boundary_C_0.1430.svg" style="width: 290px !important;">
  </figure>
</div>
</div>