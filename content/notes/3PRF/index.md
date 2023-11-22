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


**Two advantages:**
- First, in practice (particularly with many predictors) one often faces unbalanced panels and missing data. 
- Second, it is useful for developing intuition behind the procedure and for understanding its relation to partial least squares.

{{< spoiler text="**Two interpretations**">}}

- Rewrite the forecast as

$$
\begin{aligned}
& \hat{\boldsymbol{y}}=\boldsymbol{\iota}\_T \bar{y}+\hat{\boldsymbol{F}} \hat{\boldsymbol{\beta}} \\\
& \hat{\boldsymbol{F}}^{\prime}=\boldsymbol{S}\_{Z Z}\left(\boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{s}\_{X Z}\right)^{-1} \boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{X}^{\prime}, \\\
& \hat{\boldsymbol{\beta}}=\boldsymbol{S}\_{Z Z} \boldsymbol{W}\_{X Z} \boldsymbol{s}\_{X Z}\left(\boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{s}\_{X X} \boldsymbol{W}\_{X Z}\right)^{-1} \boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{s}\_{X y}
\end{aligned}
$$
where $\boldsymbol{S}_{X Z} \equiv \boldsymbol{X}^{\prime} \boldsymbol{J}_T \boldsymbol{Z}$. 
  - $\hat{\boldsymbol{F}}$ is the <mark>predictive factor</mark> and $\hat{\boldsymbol{\beta}}$ is <mark>the predictive coefficient</mark> on that factor. 

---


- Also rewrite the forecast as

$$
\begin{aligned}
& \hat{\boldsymbol{y}}=\boldsymbol{\iota} \bar{y}+\boldsymbol{J}\_T \boldsymbol{X} \hat{\boldsymbol{\alpha}} \\\
& \hat{\boldsymbol{\alpha}}=\boldsymbol{W}\_{X Z}\left(\boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{s}\_{X X} \boldsymbol{W}\_{X Z}^{\prime}\right)^{-1} \boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{s}\_{X y}
\end{aligned}
$$
- $\hat{\alpha}$ is the predictive coefficient on individual predictors. 
- The regular OLS estimate of the projection coefficient $\boldsymbol{\alpha}$ is $\left(\boldsymbol{S}\_{X X}\right)^{-1} \boldsymbol{s}\_{X y}$. 
- This representation suggests that our approach can be interpreted as <mark>a constrained version of least squares</mark>.

{{< /spoiler >}}


### 2.2. Assumptions

{{< spoiler text="**Necessary assumptions**">}}

- **Assumption 1 (Factor Structure)**. The data are generated by the following:
$$
\begin{array}{l}
\boldsymbol{x}\_t=\boldsymbol{\phi}_0+\boldsymbol{\Phi} \boldsymbol{F}_t+\boldsymbol{\varepsilon}\_t \ \  y\_{t+1}=\beta_0+\boldsymbol{\beta}^{\prime} \boldsymbol{F}\_t+\eta\_{t+1} \ \  \boldsymbol{z}\_t=\boldsymbol{\lambda}_0+\boldsymbol{\Lambda} \boldsymbol{F}\_t+\boldsymbol{\omega}\_t \\\
\boldsymbol{X}=\boldsymbol{\iota} \boldsymbol{\phi}_0^{\prime}+\boldsymbol{F} \boldsymbol{\Phi}^{\prime}+\boldsymbol{\varepsilon} \ \  \boldsymbol{y}=\mathbf{\imath} \beta_0+\boldsymbol{F} \boldsymbol{\beta}+\boldsymbol{\eta} \qquad\quad \boldsymbol{Z}=\boldsymbol{\iota} \boldsymbol{\lambda}_0^{\prime}+\boldsymbol{F} \boldsymbol{\Lambda}^{\prime}+\boldsymbol{\omega}
\end{array}
$$
where $\boldsymbol{F}_t=\left(\boldsymbol{f}_t^{\prime}, \mathbf{g}_t^{\prime}\right)^{\prime}, \boldsymbol{\Phi}=\left(\boldsymbol{\Phi}_f, \boldsymbol{\Phi}_g\right), \boldsymbol{\Lambda}=\left(\boldsymbol{\Lambda}_f, \boldsymbol{\Lambda}_g\right)$, and $\boldsymbol{\beta}=$ $\left(\boldsymbol{\beta}_f^{\prime}, \mathbf{0}^{\prime}\right)^{\prime}$ with $\left|\boldsymbol{\beta}_f\right|>$ 0. $K_f>0$ is the dimension of vector $\boldsymbol{f}_t$, $K_g \geq 0$ is the dimension of vector $g_t, L$ is the dimension of vector $z_t(0<L<\min (N, T))$, and $K=K_f+K_g$.

- **Assumption 2 (Factors, Loadings and Residuals)**. Let $M<\infty$. For any $i, s, t$
  1. $\mathbb{E}\left\|\boldsymbol{F}\_t\right\|^4<M, T^{-1} \sum\_{s=1}^T \boldsymbol{F}\_s \underset{T \rightarrow \infty}{\stackrel{p}{\longrightarrow}} \boldsymbol{\mu}$ and $T^{-1} \boldsymbol{F}^{\prime} \boldsymbol{J}\_T \boldsymbol{F} \underset{T \rightarrow \infty}{\stackrel{p}{\longrightarrow}} \boldsymbol{\Delta}\_F$
  2. $\mathbb{E}\left\|\boldsymbol{\phi}_i\right\|^4 \leq M, N^{-1} \sum\_{j=1}^N \boldsymbol{\phi}\_j \underset{T \rightarrow \infty}{\stackrel{p}{\longrightarrow}} \overline{\boldsymbol{\phi}}, N^{-1} \boldsymbol{\Phi}^{\prime} \boldsymbol{J}\_N \boldsymbol{\Phi} \underset{N \rightarrow \infty}{\stackrel{p}{\longrightarrow}} \mathcal{P}$ and $N^{-1} \boldsymbol{\Phi}^{\prime} \boldsymbol{J}\_N \boldsymbol{\phi}\_0 \underset{N \rightarrow \infty}{\stackrel{p}{\longrightarrow}} \boldsymbol{P}_1^6$
  3. $\mathbb{E}\left(\varepsilon_{i t}\right)=0, \mathbb{E}\left|\varepsilon_{i t}\right|^8 \leq M$
  4. $\mathbb{E}\left(\boldsymbol{\omega}_t\right)=\mathbf{0}, \mathbb{E}\left\|\boldsymbol{\omega}_t\right\|^4 \leq M, T^{-1 / 2} \sum\_{s=1}^T \boldsymbol{\omega}_s=\boldsymbol{o}\_p(1)$ and $T^{-1} \boldsymbol{\omega}^{\prime} \mathbf{J}_T \boldsymbol{\omega} \underset{N \rightarrow \infty}{\stackrel{p}{\longrightarrow}} \boldsymbol{\Delta}\_\omega$
  5. $\mathbb{E}\_t\left(\eta_{t+1}\right)=\mathbb{E}\left(\eta_{t+1} \mid y_t, F_t, y\_{t-1}, F\_{t-1}, \ldots\right)=0, \mathbb{E}\left(\eta\_{t+1}^4\right) \leq M$, and $\eta\_{t+1}$ is independent of $\phi_i(m)$ and $\varepsilon\_{i, t}$.

- **Assumption 3 (Dependence)**. Let $x(m)$ denote the $m$ th element of $\boldsymbol{x}$. For $M<\infty$ and any $i, j, t, s, m_1, m_2$
  1. $\mathbb{E}\left(\varepsilon\_{i t} \varepsilon\_{j s}\right)=\sigma\_{i j, t s},\left|\sigma\_{i j, t s}\right| \leq \bar{\sigma}\_{i j}$ and $\left|\sigma\_{i j, t s}\right| \leq \tau\_{t s}$, and
    - $N^{-1} \sum\_{i, j=1}^N \bar{\sigma}\_{i j} \leq M$
    - $T^{-1} \sum\_{t, s=1}^T \tau\_{t s} \leq M$
    - $N^{-1} \sum\_{i, s}\left|\sigma\_{i i, t s}\right| \leq M$
    - $N^{-1} T^{-1} \sum\_{i, j, t, s}\left|\sigma\_{i j, t s}\right| \leq M$
  2. $\mathbb{E}\left|N^{-1 / 2} T^{-1 / 2} \sum_{s=1}^T \sum_{i=1}^N\left[\varepsilon_{i s} \varepsilon_{i t}-\mathbb{E}\left(\varepsilon_{i s} \varepsilon_{i t}\right)\right]\right|^2 \leq M$
  3. $\mathbb{E}\left|T^{-1 / 2} \sum_{t=1}^T F_t\left(m_1\right) \omega_t\left(m_2\right)\right|^2 \leq M$
  4. $\mathbb{E}\left|T^{-1 / 2} \sum_{t=1}^T \omega_t\left(m_1\right) \varepsilon_{i t}\right|^2 \leq M$.

- **Assumption 4 (Central Limit Theorems)**. For any $i, t$
  1. $N^{-1 / 2} \sum\_{i=1}^N \phi_i \varepsilon\_{i t} \stackrel{d}{\rightarrow} \mathcal{N}\left(0, \Gamma_{\Phi_{\varepsilon}}\right)$, where $\Gamma_{\Phi_{\varepsilon}}=\operatorname{plim}\_{N \rightarrow \infty} N^{-1}$ $\sum\_{i, j=1}^N \mathbb{E}\left[\boldsymbol{\phi}\_i \boldsymbol{\phi}\_j^{\prime} \varepsilon\_{i t} \varepsilon\_{j t}\right]$
  2. $T^{-1 / 2} \sum\_{t=1}^T \boldsymbol{F}\_t \eta\_{t+1} \stackrel{d}{\rightarrow} \mathcal{N}\left(0, \Gamma\_{F \eta}\right)$, where $\boldsymbol{\Gamma}\_{F \eta}=\operatorname{plim}\_{T \rightarrow \infty} T^{-1}$ $\sum\_{t=1}^T \mathbb{E}\left[\eta\_{t+1}^2 \boldsymbol{F}_t \boldsymbol{F}_t^{\prime}\right]>0$
  3. $T^{-1 / 2} \sum\_{t=1}^T \boldsymbol{F}_t \varepsilon\_{i t} \stackrel{d}{\rightarrow} \mathcal{N}\left(0, \boldsymbol{\Gamma}\_{F \varepsilon, i}\right)$, where $\boldsymbol{\Gamma}\_{F \varepsilon, i}=\operatorname{plim}\_{T \rightarrow \infty} T^{-1}$ $\sum\_{t, s=1}^T \mathbb{E}\left[\boldsymbol{F}_t \boldsymbol{F}_s^{\prime} \varepsilon\_{i t} \varepsilon\_{i s}\right]>0$.

- **Assumption 5 (Normalization)**. $\mathcal{P}=\mathbf{I}, \boldsymbol{P}_1=\mathbf{0}$ and $\boldsymbol{\Delta}_F$ is diagonal, positive definite, and each diagonal element is unique.

- **Assumption 6 (Relevant Proxies)**. $\boldsymbol{\Lambda}=\left[\boldsymbol{\Lambda}_f, \mathbf{0}\right]$ and $\boldsymbol{\Lambda}_f$ is nonsingular.

{{< /spoiler >}}


### 2.3. Consistency

<font color="red">**Theorem 1.**</font> Let Assumptions 1-6 hold. The three-pass regression filter forecast is consistent for the infeasible best forecast, $\hat{y}_{t+1} \underset{T, N \rightarrow \infty}{\stackrel{p}{\longrightarrow}}$ $\beta_0+\boldsymbol{F}_t^{\prime} \boldsymbol{\beta}$.


<font color="red">**Theorem 2.**</font> Let $\hat{\alpha}_i$ denote the ith element of $\hat{\boldsymbol{\alpha}}$, and let Assumptions 1-6 hold. Then for any $i$,
$$
N \hat{\alpha}_i \underset{T, N \rightarrow \infty}{\stackrel{p}{\longrightarrow}}\left(\boldsymbol{\phi}_i-\overline{\boldsymbol{\phi}}\right)^{\prime} \boldsymbol{\beta} .
$$


<font color="blue">**Corollary 1.**</font> Let Assumptions 1–5 hold with the exception of Assumptions 2.4, 3.3 and 3.4. Additionally, assume that there is only one relevant factor. Then the target-proxy three-pass regression filter forecaster is consistent for the infeasible best forecast.


### 2.4. Asymptotic distributions


<font color="red">**Theorem 3.**</font> Under Assumptions 1-6, as $N, T \rightarrow \infty$ we have
$$
\frac{\sqrt{T} N\left(\hat{\alpha}\_i-\tilde{\alpha}\_i\right)}{A\_i} \stackrel{d}{\rightarrow} \mathcal{N}(0,1)
$$
{{< spoiler text="where">}}
$A_i^2$ is the ith diagonal element of $\widehat{\operatorname{Avar}}(\hat{\boldsymbol{\alpha}})=\boldsymbol{\Omega}\_\alpha\left(\frac{1}{T} \sum\_t \hat{\eta}\_{t+1}^2\left(\boldsymbol{X}\_t\right.\right.$ $\left.-\overline{\boldsymbol{X}})\left(\boldsymbol{X}\_t-\overline{\boldsymbol{X}}\right)^{\prime}\right) \boldsymbol{\Omega}\_\alpha^{\prime}, \hat{\eta}\_{t+1}$ is the estimated 3PRF forecast error, $\tilde{\alpha}\_i \equiv$ $\boldsymbol{S}\_i \boldsymbol{G}\_\alpha \boldsymbol{\beta}$, where $\boldsymbol{S}\_i$ is selects the ith element of vector $\boldsymbol{G}\_\alpha \boldsymbol{\beta}$ and
$$
\begin{aligned}
\boldsymbol{G}\_\alpha&=\boldsymbol{J}\_N\left(T^{-1} \boldsymbol{X}^{\prime} \boldsymbol{J}\_T \boldsymbol{Z}\right)\left(T^{-3} N^{-2} \boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{S}_{X X} \boldsymbol{W}\_{X Z}\right)^{-1}\times\\\
&~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\left(N^{-1} T^{-2} \boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{X}^{\prime} \boldsymbol{J}\_T \boldsymbol{F}\right),
\end{aligned}
$$
and
$$
\boldsymbol{\Omega}\_\alpha=\boldsymbol{J}\_N\left(\frac{1}{T} \boldsymbol{S}\_{X Z}\right)\left(\frac{1}{T^3 N^2} \boldsymbol{W}\_{X Z}^{\prime} \boldsymbol{S}\_{X X} \boldsymbol{W}\_{X Z}\right)^{-1}\left(\frac{1}{T N} \boldsymbol{W}\_{X Z}^{\prime}\right)
$$
{{< /spoiler >}}


<font color="red">**Theorem 4.**</font> Under Assumptions $1-6$, as $N, T \rightarrow \infty$ we have
$$
\frac{\sqrt{T}\left(\hat{y}\_{t+1}-\mathbb{E}_t y\_{t+1}\right)}{Q_t} \stackrel{d}{\rightarrow} \mathcal{N}(0,1)
$$
where $\mathbb{E}_t y\_{t+1}=\beta_0+\boldsymbol{\beta}^{\prime} \boldsymbol{F}_t$ and $Q_t^2$ is the th diagonal element of $\frac{1}{N^2} J_T \boldsymbol{X} \widehat{\operatorname{Avar}}(\hat{\boldsymbol{\alpha}}) \boldsymbol{X}^{\prime} \boldsymbol{J}_T$


<font color="red">**Theorem 5.**</font> Under Assumptions $1-6$, as $N, T \rightarrow \infty$ we have
$$
\sqrt{T}\left(\hat{\boldsymbol{\beta}}-\boldsymbol{G}\_\beta \boldsymbol{\beta}\right) \stackrel{d}{\rightarrow} \mathcal{N}\left(\mathbf{0}, \boldsymbol{\Sigma}\_\beta\right)
$$
where $\boldsymbol{\Sigma}\_\beta=\boldsymbol{\Sigma}\_z^{-1} \boldsymbol{\Gamma}\_{F \eta} \boldsymbol{\Sigma}\_z^{-1}$ and $\boldsymbol{\Sigma}\_z=\mathbf{\Lambda} \boldsymbol{\Delta}\_F \boldsymbol{\Lambda}^{\prime}+\boldsymbol{\Delta}\_\omega$. Furthermore,
$$
\begin{aligned}
\widehat{\operatorname{Avar}}(\hat{\boldsymbol{\beta}})= & \left(T^{-1} \hat{\boldsymbol{F}}^{\prime} \boldsymbol{J}\_T \hat{\boldsymbol{F}}\right)^{-1} T^{-1} \sum\_t \hat{\eta}\_{t+1}^2\left(\hat{\boldsymbol{F}}\_t-\hat{\boldsymbol{\mu}}\right)\left(\hat{\boldsymbol{F}}\_t-\hat{\boldsymbol{\mu}}\right)^{\prime} \\
& \times\left(T^{-1} \hat{\boldsymbol{F}}^{\prime} \boldsymbol{J}\_T \hat{\boldsymbol{F}}\right)^{-1}
\end{aligned}
$$
is a consistent estimator of $\boldsymbol{\Sigma}\_\beta$. $\boldsymbol{G}\_\beta$ is defined in the Appendix.


<font color="red">**Theorem 6.**</font> Under Assumptions 1-6, as $N, T \rightarrow \infty$ we have for every $t$
- if $\sqrt{N} / T \rightarrow 0$, then
$$
\sqrt{N}\left[\hat{\boldsymbol{F}}\_t-\left(\boldsymbol{H}\_0+\boldsymbol{H} \boldsymbol{F}\_t\right)\right] \stackrel{d}{\rightarrow} \mathcal{N}\left(\mathbf{0}, \boldsymbol{\Sigma}\_F\right)
$$
- if $\liminf \sqrt{N} / T \geq \tau \geq 0$, then
$$
T\left[\hat{\boldsymbol{F}}\_t-\left(\boldsymbol{H}\_0+\boldsymbol{H F}\_t\right)\right]=\boldsymbol{O}\_p(1)
$$
where $\boldsymbol{\Sigma}\_F=\left(\boldsymbol{\Lambda} \boldsymbol{\Delta}\_F \boldsymbol{\Lambda}^{\prime}+\boldsymbol{\Delta}_\omega\right)\left(\boldsymbol{\Lambda} \boldsymbol{\Delta}\_F^2 \boldsymbol{\Lambda}^{\prime}\right)^{-1} \boldsymbol{\Lambda} \boldsymbol{\Delta}\_F \boldsymbol{\Gamma}\_{\Phi \varepsilon} \boldsymbol{\Delta}\_F \boldsymbol{\Lambda}^{\prime}\left(\boldsymbol{\Lambda} \boldsymbol{\Delta}\_F^2\right.$ $\left.\boldsymbol{\Lambda}^{\prime}\right)^{-1}\left(\boldsymbol{\Lambda} \boldsymbol{\Delta}\_F \boldsymbol{\Lambda}^{\prime}+\boldsymbol{\Delta}\_\omega\right) . \boldsymbol{H}\_0$ and $\boldsymbol{H}$ are defined in the Appendix.


### 2.5. Proxy selection


**How to select the proxies that depend only on relevant factors ?**


#### 2.5.1. Automatic proxies

- Initialize $\boldsymbol{r}_0=\boldsymbol{y}$.
- For $k=1, \ldots, L$ :
  - Define the $k$ th automatic proxy to be $\boldsymbol{r}_{k-1}$. Stop if $k=L$; otherwise proceed.
  - Compute the 3PRF for target $\boldsymbol{y}$ using cross section $\boldsymbol{X}$ using statistical proxies 1 through $k$. Denote the resulting forecast $\hat{\boldsymbol{y}}_k$. 
  - Calculate $\boldsymbol{r}_k=\boldsymbol{y}-\hat{\boldsymbol{y}}_k$, advance $k$, and go to step 1.


<font color="red">**Theorem 7.**</font> Let Assumptions 1-5 hold with the exception of Assumptions 2.4, 3.3 and 3.4. Then the L-automatic-proxy three pass regression filter forecaster of $\boldsymbol{y}$ automatically satisfies Assumptions 2.4, 3.3, 3.4 and 6 when $L=K_f$. As a result, <mark>the $L$ automatic-proxy is consistent and asymptotically normal according to Theorems 1 and 4.</mark>


#### 2.5.2. Theory proxies

- The filter may instead be employed using alternative disciplining variables (factor proxies) which may <mark>be distinct from the target and chosen on the basis of economic theory or by statistical arguments</mark>. 
- Consider a situation in which $K_f$ is one, so that the target and proxy are given by $y_{t+1}=\beta_0+\beta f_t+\eta_{t+1}$ and $z_t=\lambda_0+\Lambda f_t+\omega_t$. 
- Also suppose that the population $R^2$ of the proxy equation is substantially higher than the population $R^2$ of the target equation.
  - The forecasts from using either $z_t$ or the target as proxy are asymptotically identical. 
  - However, in finite samples, forecasts can be improved by proxying with $z_t$ due to its higher signal-to-noise ratio.

#### 2.5.2 Information criteria


''Trace of the Krylov Representation'' method of Kramer and Sugiyama (2011).


$$
\begin{aligned}
& \widehat{\operatorname{DoF}}(m)=1+\sum_{j=1}^m c_j \operatorname{trace}\left(\boldsymbol{K}^j\right)-\sum_{l, j=1}^m \boldsymbol{t}_l^{\prime} \mathbf{K}^j \boldsymbol{t}\_l \\\
& ~~~~~~~~~~~~~~~~~~~~~~+\left(\boldsymbol{y}-\hat{\boldsymbol{y}}\_m\right)^{\prime} \sum\_{j=1}^m \boldsymbol{K}^j \boldsymbol{v}_j+m
\end{aligned}
$$
where $\boldsymbol{K}=\boldsymbol{X} \boldsymbol{X}^{\prime}, c_j$ are elements of the vector $\boldsymbol{c}=\boldsymbol{B}^{-1} \boldsymbol{T} \boldsymbol{y}, \boldsymbol{B}$ is a Krylov basis decomposition, $\boldsymbol{T}$ is the matrix of PLS factor estimate vectors $\boldsymbol{t}_j$, and $\boldsymbol{v}_j$ are columns of the matrix $\boldsymbol{T}\left(\boldsymbol{B}^{-1}\right)^{\prime}$. The BIC is then calculated as 
$$\sum\_t\left(y_t-\hat{y}\_{m, t}\right)^2 / T+\log (T) \hat{\sigma}^2 \widehat{D o F}(m) / T$$ 
where $\hat{\sigma}=\sqrt{\sum_t\left(y_t-\hat{y}\_{m, t}\right)^2 /(T-D o F(m))}$.

---



## 3. Related procedures




### 3.1. Constrained least squares


<font color="red">**Theorem 8.**</font> The three-pass regression filter's implied $N$-dimensional predictive coefficient, $\hat{\alpha}$, is the solution to
$$
\begin{aligned}
& \arg \min _{\alpha_0, \boldsymbol{\alpha}}\left\\|\boldsymbol{y}-\alpha_0-\boldsymbol{X} \boldsymbol{\alpha}\right\\| \\\
& \text { subject to } \quad\left(\boldsymbol{I}-\boldsymbol{W}\_{X Z}\left(\boldsymbol{S}\_{X Z}^{\prime} \boldsymbol{W}\_{X Z}\right)^{-1} \boldsymbol{W}\_{X Z}\right) \boldsymbol{\alpha}=\mathbf{0} .\quad (5)
\end{aligned}
$$

- The 3PRF's answer is to impose the constraint in Eq. (5), which exploits the proxies and has an intuitive interpretation. 
- Premultiplying both sides of the equation by $\boldsymbol{J}\_T \boldsymbol{X}$, we can rewrite the constraint as $\left(\boldsymbol{J}\_T \boldsymbol{X}-\boldsymbol{J}\_T \hat{\boldsymbol{F}} \hat{\boldsymbol{\Phi}}^{\prime}\right) \boldsymbol{\alpha}=$ 0. For large $N$ and $T$,
$$
\boldsymbol{J}\_T \boldsymbol{X}-\boldsymbol{J}\_T \hat{\boldsymbol{F}} \hat{\boldsymbol{\Phi}}^{\prime} \approx \boldsymbol{\varepsilon}+(\boldsymbol{F}-\boldsymbol{\mu})\left(\boldsymbol{I}-\boldsymbol{S}\_{K\_f}\right) \boldsymbol{\Phi}^{\prime}
$$
- Because the covariance between $\boldsymbol{\alpha}$ and $\varepsilon$ is zero by the assumptions of the model, the constraint simply imposes that <mark>the product of $\alpha$ and the target-irrelevant common component of $\boldsymbol{X}$ is equal to zero</mark>. 
- This is because the matrix $\boldsymbol{I}-\boldsymbol{S}\_{K\_f}$ selects only the terms in the total common component $\boldsymbol{F} \boldsymbol{\Phi}^{\prime}$ that <mark>are associated with irrelevant factors</mark>. 
- This constraint is important because it ensures that <mark>factors irrelevant to $\boldsymbol{y}$ drop out of the 3PRF forecast</mark>. It also ensures that $\hat{\boldsymbol{\alpha}}$ is consistent for the factor model's population projection coefficient of $y_{t+1}$ on $\boldsymbol{x}_t$.

### 3.2. Partial least squares

{{< spoiler text="PLS">}}
- partial least squares (PLS) constructs forecasting indices as linear combinations of the underlying predictors. 
- These predictive indices are referred to as "directions" in the language of PLS. 
- The PLS forecast based on the first $K$ PLS directions, $\hat{\boldsymbol{y}}^{(k)}$, is constructed according to the following algorithm (as stated in Hastie et al. (2009)):
  1. Standardize each $\mathbf{x}\_i$ to have mean zero and variance one by setting $\tilde{\mathbf{x}}\_i=\frac{\mathbf{x}\_i-\hat{\mathbb{E}}\left[\mathrm{x}\_{i t}\right]}{\hat{\sigma}\left(\mathrm{x}\_{i t}\right)}, i=1, \ldots, N$
  2. Set $\hat{\boldsymbol{y}}^{(0)}=\bar{y}$, and $\mathbf{x}_i^{(0)}=\tilde{\mathbf{x}}_i, i=1, \ldots, N$
  3. For $k=1,2, \ldots, K$
    - $\boldsymbol{u}\_k=\sum\_{i=1}^N \hat{\phi}\_{k i} \mathbf{x}\_i^{(k-1)}$, where $\hat{\phi}\_{k i}=\widehat{\operatorname{Cov}}\left(\mathbf{x}\_i^{(k-1)}, \boldsymbol{y}\right)$
    - $\hat{\beta}_k=\widehat{\operatorname{Cov}}\left(\boldsymbol{u}_k, \boldsymbol{y}\right) / \widehat{\operatorname{Var}}\left(\boldsymbol{u}_k\right)$
    - $\hat{\boldsymbol{y}}^{(k)}=\hat{\boldsymbol{y}}^{(k-1)}+\hat{\beta}_k \boldsymbol{u}_k$
    - Orthogonalize each $\mathbf{x}_i^{(k-1)}$ with respect to $\boldsymbol{u}_k$ :
  $$
  \begin{aligned}
  \mathbf{x}_i^{(k)} & =\mathbf{x}_i^{(k-1)}-\left(\widehat{\operatorname{Cov}}\left(\boldsymbol{u}_k, \mathbf{x}_i^{(k-1)}\right) / \widehat{\operatorname{Var}}\left(\boldsymbol{u}_k\right)\right) \boldsymbol{u}_k, \\\
  i & =1,2, \ldots, N .
  \end{aligned}
  $$
{{< /spoiler >}}


- partial least squares forecasts are identical to those from the 3PRF when 
  1. the predictors are demeaned and variance-standardized in a preliminary step
  2. the first two regression passes are run without constant terms
  3. proxies are automatically selected. 

Consider the case where a single predictive index is constructed from the partial least squares algorithm. 
- Assume, for the time being, that each predictor has been previously standardized to have mean zero and variance one. Following the construction of the PLS forecast given above, we have
1. Set $\hat{\phi}_i=x_i^{\prime} y$, and $\hat{\boldsymbol{\Phi}}=\left(\hat{\phi}_1, \ldots, \hat{\phi}_N\right)^{\prime}$.
2. Set $\hat{u}_t=\boldsymbol{x}_t^{\prime} \hat{\Phi}$, and $\hat{\boldsymbol{u}}=\left(\hat{u}_1, \ldots, \hat{u}_T\right)^{\prime}$.
3. Run a predictive regression of $\boldsymbol{y}$ on $\hat{\boldsymbol{u}}$.

- Constructing the forecast in this manner may be represented as a one-step estimator
$$
\hat{\boldsymbol{y}}^{\mathrm{PLS}}=\boldsymbol{X} \boldsymbol{X}^{\prime} \boldsymbol{y}\left(\boldsymbol{y}^{\prime} \boldsymbol{X} \boldsymbol{X}^{\prime} \boldsymbol{X} \boldsymbol{X}^{\prime} \boldsymbol{y}\right)^{-1} \boldsymbol{y}^{\prime} \boldsymbol{X} \boldsymbol{X}^{\prime} \boldsymbol{y}
$$
- <font color="red">which upon inspection is identical to the 1-automatic-proxy 3PRF forecast when constants are omitted from the first and second passes. </font>


---

## 4. Empirical evidence


### 4.1. Forecasting macroeconomic aggregates

- Quarterly data from Stock and Watson (2012) for the sample 1959:I-2009:IV
- Take as our predictors a set of 108 macroeconomic variables compiled by Stock and Watson (2012)
- Out-of-sample $R^2$ of one quarter ahead forecasts, in percentage.

|  | 3PRF1 | PCR1 | PCLAR | PCLAS | 10LAR | FA1 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| GDP | 30.12 | $35.18^{\mathrm{a}}$ | 29.70 | 29.51 | 26.38 | 20.11 |
| Consumption | $23.20^{\mathrm{a},{ }^*}$ | 7.06 | 9.12 | 7.32 | -14.85 | 2.72 |
| Investment | $38.88^{\mathrm{a}}$ | 37.37 | 36.81 | 36.30 | 24.01 | 34.35 |
| Exports | $16.75^{\mathrm{a}}$ | 13.25 | -11.58 | -9.42 | -61.36 | 16.44 |
| Imports | $37.18^a$ | 36.50 | 18.46 | 16.93 | 22.77 | 36.58 |
| Industrial Production | $16.56^{\mathrm{a},{ }^*}$ | 8.92 | 5.67 | 5.71 | 11.04 | 12.04 |
| Capacity Utilization | 54.32 | 54.79 | 53.77 | 54.85 | $64.69^{a,{}^*}$ | 55.58 |
| Total Hours | $53.81^{\mathrm{a}}$ | 50.47 | 48.58 | 47.39 | 39.56 | 42.53 |
| Total Employment | $48.84^a$ | 47.27 | 38.14 | 37.16 | 18.91 | 41.73 |
| Average Hours | $20.12^{\mathrm{a}}$ | 10.12 | 18.52 | 13.89 | 17.55 | 15.84 |
| Housing Starts | 26.97 | -0.14 | 31.54 | 29.66 | $46.89^a$ | 0.13 |
| GDP Inflation | 0.64 | 2.05 | -0.94 | 1.38 | -5.89 | $2.80^{\mathrm{a}}$ |
| PCE Inflation | -1.29 | -3.73 | 10.60 | 9.82 | $12.22^{\mathrm{a}}$ | -2.50 |


### 4.2. Forecasting market returns

- We estimate the extent of market return predictability using 25 log price-dividend ratios of portfolios sorted by market equity and book-to-market ratio. 
- The data is annual over the post-war period 1945-2010 (following Fama and French (1992)). 
- We assume that the predictors take 
  - the form $p d_{i, t}=\phi\_{i, 0}+\boldsymbol{\phi}_i^{\prime} \boldsymbol{F}_t+\varepsilon\_{i, t}$
  - the target takes the form $r\_{t+1}=\beta_0^r+\boldsymbol{F}_t^{\prime} \boldsymbol{\beta}^r+\eta\_{t+1}^r$.

|  | 3PRF1 | 3PRF2 | 3PRF-IC | PC1 | 
| :---: | :---: | :---: | :---: | :---: |
| Return $R^2$ <br> # of factors | 27.63 | $36.34^{\mathrm{a}}$ | 31.15 <br> 1.36 | -10.45 | 
| | **PC2** | **PC-IC** | **10LAR** | **FA1** | **FA2** | **FA-IC** |
|Return $R^2$ <br> # of factors|-8.89 | 27.00 <br> 4.58 | 13.28 | -10.08 | -11.76 | 21.68 <br> 4.58 |




