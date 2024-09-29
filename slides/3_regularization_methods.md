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

# Regularization Methods: Bayesian Formulation
<v-clicks depth="2">

* Recall that a $\mathrm{RSS}$ loss in Ordinary Least Squares may be written as: <br>
$\mathcal{L}(\hat{Y}, Y) = (Y - \hat{Y})^T (Y - \hat{Y}) = - \mathrm{ln~p}_{\mathcal{N}} (Y | \hat{Y}, \hat{\sigma}^2 I) + \mathrm{Const.}$
* Which allows us to re-write the solution of $\mathrm{OLS}$ as **Maximum Likelihood Estimation** (MLE) problem: <br>
$W_{\mathrm{OLS}} = \argmax\limits_W \{\mathrm{ln~p}_{\mathcal{N}} (Y | X, W)\}$
* Let's use a Bayes theorem instead!
	* If we assume some initial distribution on weights $p(W)$, we can update it in the following way: $\mathrm{p}(W | X, Y) \propto \mathrm{p} (Y | X, W) \mathrm{p}(W)$
* This converts MLE problem into **Maximum a Posteriori** (MAP) estimation problem: <br>
$W_{\mathrm{MAP}} = \argmax\limits_W \{\mathrm{ln~p}_{\mathcal{N}} (Y | X, W) + \mathrm{ln~p}(W) \}$
</v-clicks>

---

# Regularization Methods: Bayesian Formulation

* $W_{\mathrm{MAP}} = \argmax\limits_W \{\mathrm{ln~p}_{\mathcal{N}} (Y | X, W) + \mathrm{ln~p}(W) \}$
	* If $\mathrm{p}(W)$ is a standard Normal distribution, we get **Ridge regression**
	* If $\mathrm{p}(W)$ is a standard Laplace distribution, we get **Lasso regression**
	* If we generalize this idea from using a single weight estimate to a distribution of weights and use standard Normal prior $\mathrm{p}(W)$, we get **Gaussian Process**

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

<!-- #### The constraint region increases with $\lambda$
<Arrow x1="550" y1="125" x2="553" y2="260" />
<Arrow x1="550" y1="125" x2="815" y2="263" /> -->

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