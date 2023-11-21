---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Notes on ''The three-pass regression filter: A new approach to forecasting using many predictors''"
subtitle: ""
summary: ""
authors: []
tags: []
categories: ["factor-model"]
date: 2023-11-20T22:16:45+08:00
lastmod: 2023-11-20T22:16:45+08:00
featured: true
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


{{< toc >}}

---

{{< figure src="/notes/3PRF/paper_intro.png" caption="">}}

---

## 1. Introduction

- How to use the vast predictive information to forecast important economic aggregates like national product or stock market value.
  - If the predictors number near or more than the number of observations, the standard OLS forecaster is known to be poorly behaved or nonexistent (Huber, 1973)
  - A economic view: data are generated from a model in which **latent factors** drive the systematic variation of <mark>both the forecast target, $\boldsymbol{y}$, and the matrix of predictors, $\boldsymbol{X}$</mark> $\Rightarrow$ <font color="red">the best prediction of $\boldsymbol{y}$ is infeasible</font> since the factors are unobserved $\Rightarrow$ require a factor estimation step
  - A benchmark method: extract factors that are significant drivers of variation in $\boldsymbol{X}$ and then uses these to forecast $\boldsymbol{y}$.


- **Motivations**
  - the factors that are relevant to $\boldsymbol{y}$ may be a strict subset of all the factors driving $\boldsymbol{X}$
  - The method, called the <font color="red">three-pass regression filter</font> (3PRF), selectively identifies only the subset of factors that <mark>influence the forecast target</mark> while discarding factors that <mark>are irrelevant for the target but that may be pervasive among predictors</mark>
  - The 3PRF has the advantage of being expressed in closed form and virtually instantaneous to compute.


- **Contributions**

  1. develop asymptotic theory for the 3PRF
  2. verify the finite sample accuracy of the asymptotic theory
  3. compare the 3PRF to other methods
  4. provide empirical support for the 3PRF's strong forecasting performance


---


## 2. The three-pass regression filter


### 2.1. The estimator


{{< spoiler text="**The environment for 3PRF**">}}

- There is <mark>a target variable</mark> which we wish to forecast. 
- There exist many predictors which may contain information useful for predicting the target variable. 
  - The number of predictors <font color="red">$N$ may be large and number near or more than the available time series observations $T$</font>, which makes OLS problematic. 
- Therefore we look to reduce the dimension of predictive information $\Rightarrow$ <font color="red">assume the data can be described by an approximate factor model.</font> 
- In order to make forecasts, the 3PRF uses **proxies**: 
  - These are variables, driven by the factors (and as we emphasize below, driven by target-relevant factors in particular), which we show are always available from 
    - the target and predictors themselves
    - the econometrician on the basis of economic theory. 
- The target is a linear function of a subset of the latent factors plus some unforecastable noise. 
- The optimal forecast therefore comes from a regression on the true underlying relevant factors. However, since these factors are unobservable, we call this the <font color="red">infeasible best forecast</font>.

{{< /spoiler >}}

$$
\boldsymbol{y} = \boldsymbol{Z} \boldsymbol{\beta} + \boldsymbol{\epsilon}
$$

- $\boldsymbol{y}\in\mathbb{R}^{T \times 1}$: the target variable time series from $2,3, \ldots, T+1$ 
- $\boldsymbol{X}\in\mathbb{R}^{T \times N}$: the predictors that have been standardized to have unit time series variance. 
  - Temporal dimension: $\boldsymbol{X}=\left(\boldsymbol{x}_1^{\prime}, \boldsymbol{x}_2^{\prime}, \ldots, \boldsymbol{x}_T^{\prime}\right)^{\prime}$
  - Cross section: $\boldsymbol{X}=\left(\mathbf{x}_1, \mathbf{x}_2, \ldots, \mathbf{x}_N\right)$
- $\boldsymbol{Z}\in\mathbb{R}^{T \times L}$: stacks period-by-period **proxy data** as $\boldsymbol{Z}=\left(\boldsymbol{z}_1^{\prime}, \boldsymbol{z}_2^{\prime}, \ldots, \boldsymbol{z}_T^{\prime}\right)^{\prime}$
- Make no assumption on the relationship between $N$ and $T$ but assume <font color="red">$L \ll \min (N, T)$</font>

---

**Construct the 3PRF**


| Pass | Description |
| :---: | :---: |
| 1. | Run <mark>time series regression</mark> of $\mathbf{x}_i$ on $\boldsymbol{Z}$ for $i=1, \ldots, N$, |
|  | $x_{i, t}=\phi\_{0, i}+\boldsymbol{z}^{\prime} \boldsymbol{\phi}\_i+\epsilon\_{i t}$, retain slope estimate $\hat{\boldsymbol{\phi}}\_i$. |
| 2. | Run <mark>cross section regression</mark> of $\boldsymbol{x}_t$ on $\hat{\boldsymbol{\phi}}_i$ for $t=1, \ldots, T$, |
|  | $x_{i, t}=\phi\_{0, t}+\hat{\boldsymbol{\phi}}^{\prime} \boldsymbol{F}\_t+\varepsilon\_{i t}$, retain slope estimate $\hat{\boldsymbol{F}}\_t$. |
| 3. | Run <mark>time series regression</mark> of $y_{t+1}$ on predictive factors $\hat{\boldsymbol{F}}_t$, |
|  | $y_{t+1}=\beta\_0+\hat{\boldsymbol{F}}^{\prime} \boldsymbol{\beta}+\eta\_{t+1}$, delivers forecast $\hat{y}\_{t+1}$. |


- **Pass 1** : the estimated coefficients describe the sensitivity of the predictor to factors represented by the proxies.
- **Pass 2** : first-stage coefficient estimates map the cross-sectional distribution of predictors to the latent factors. Second-stage cross section regressions use this map to back out estimates of the factors at each point in time.
- **Pass 3** : This is a single time series forecasting regression of the target variable $y_{t +1}$ on the second-pass estimated predictive factors $\hat{\boldsymbol{F}}_t$.  The third-pass fitted value $\beta\_0+\hat{\boldsymbol{F}_t}^{\prime} \boldsymbol{\beta}$ is the 3PRF time $t$ forecast

{{% callout note %}}
**An one-step closed form:**

$$
{\color{red}{\hat{\boldsymbol{y}}=\boldsymbol{\iota}\_T \bar{y}+\boldsymbol{J}\_T \boldsymbol{X} \boldsymbol{W}\_{X Z}\left(\boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{S}\_{X X} \boldsymbol{W}\_{X Z}\right)^{-1} \boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{s}\_{X y}}}
$$
- $\boldsymbol{J}_T \equiv \boldsymbol{I}_T-\frac{1}{T} \boldsymbol{\iota}_T \boldsymbol{\iota}_T^{\prime}$
  - $\boldsymbol{I}_T$: the $T$-dimensional identity matrix
  - $\boldsymbol{\iota}_T$: the $T$-vector of ones $\left(\boldsymbol{J}_N\right.$ is analogous)
- $\bar{y}=\boldsymbol{\iota}\_T^{\prime} \boldsymbol{y} / T, \boldsymbol{W}\_{X Z} \equiv$ $\boldsymbol{J}\_{\boldsymbol{N}} \boldsymbol{X}^{\prime} \boldsymbol{J}\_T \boldsymbol{Z}, \boldsymbol{S}\_{X X} \equiv \boldsymbol{X}^{\prime} \boldsymbol{J}\_T \boldsymbol{X}$ and $\boldsymbol{s}\_{X \boldsymbol{y}} \equiv \boldsymbol{X}^{\prime} \boldsymbol{J}\_T \boldsymbol{y}$.

{{% /callout %}}


### 2.2. Assumptions




### 2.3. Consistency


### 2.4. Asymptotic distributions


### 2.5. Proxy selection


#### 2.5.1. Automatic proxies


#### 2.5.2. Theory proxies


---



## 3. Related procedures


### 3.1. Constrained least squares


### 3.2. Partial least squares


---



## 4. Simulation evidence


### 4.1. Comparison against alternatives


### 4.2. Information criteria


---

## 5. Empirical evidence


### 5.1. Forecasting macroeconomic aggregates


### 5.2. Forecasting market returns


### 5.3. Examples of theory proxies for macroeconomic forecasts



