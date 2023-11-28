---
title: On factor models with random missing： EM estimation, inference, and cross validation
summary: Notes on "On factor models with random missing： EM estimation, inference, and cross validation".
authors: [Zhe Li]
tags: []
categories: [Factor Model]
date: '2023-11-27T00:00:00Z'
slides:
  # Choose a theme from https://github.com/hakimel/reveal.js#theming
  theme: black
  # Choose a code highlighting style (if highlighting enabled in `params.toml`)
  #   Light style: github. Dark style: dracula (default).
  highlight_style: dracula
---


{{< slide background-image="ZIBIN.jpg" >}}

# On factor models with random missing: EM estimation, inference, and cross validation

$$
\begin{aligned}
\\
\end{aligned}
$$

<center> Li Zhe <center>

$$$$

<center>School of Data Science, Fudan University <center> 




$$$$

<center>November 30, 2023<center>


---

{{< slide background-image="body1.jpg" >}}

## Outline

- Background: **factor model**
- Motivation
- Factor models with random missing:
  - <font color="red">EM estimator</font>
  - <font color="red">Asymptotic properties</font>
  - Determining the number of factors
- Simulation
- Empirical application

--- 

{{< slide background-image="body1.jpg" >}}
## Background: factor model

$$
\begin{aligned}
x_{i t} & =\lambda_i f_t+e_{i t} \\\
\boldsymbol{X} &= \boldsymbol{\Lambda} \boldsymbol{F}^\top + \boldsymbol{E}
\end{aligned}
$$
- $\boldsymbol{X}=(\boldsymbol{x}_{\cdot 1},\ldots,\boldsymbol{x}\_{\cdot T})\in\mathbb{R}^{N\times T}$
- $\boldsymbol{\Lambda}\in\mathbb{R}^{N\times R}$: factor loadings
- $\boldsymbol{F}\in\mathbb{R}^{T\times R}$: common factors (latent, unobserved)
- $\boldsymbol{E}\in\mathbb{R}^{N\times T}$: idiosyncratic (or error) component
  - $\{e_{it}\}$ can exhibit both cross-sectional and temporal dependence.

- Given the factor number $k$, we can estimate the factors and factor loadings by
$$
\Big\\{\widehat{\boldsymbol{\Lambda}}^k, \widehat{\boldsymbol{F}}^k\Big\\}=\arg\min\_{\boldsymbol{\Lambda}^k,\boldsymbol{F}^k}\frac{1}{NT}\Big\\|\boldsymbol{X}- \boldsymbol{\Lambda}^k{\boldsymbol{F}^k}^\top\Big\\|_F^2
$$
where $\boldsymbol{\Lambda}^k\in\mathbb{R}^{N\times k}$ and $\boldsymbol{F}^k\in\mathbb{R}^{T\times k}$.
--- 

{{< slide background-image="body1.jpg" >}}

## Motivation

- Factor model in balanced panel has been thoroughly investigated.
$$$$
- How to handle the missing data problem in factor models?
$$$$
  - the expectation–maximization (EM) algorithm
$$$$
  - the Kalman filter (KF)
$$$$
- There is no formal study of the **asymptotic properties** for the EM estimators of the factors and factor loadings for the PC estimation with <mark>missing observations</mark>

---




{{< slide background-image="body1.jpg" >}}

## Notations

- consider the factor model
$$
\boldsymbol{X} =  \boldsymbol{F}\boldsymbol{\Lambda}^\top + \varepsilon
$$
  - $\boldsymbol{X}=(X_1,\ldots,X_N)$ where $X_i \equiv\left(X_{i 1}, \ldots, X_{i T}\right)^{\prime}$ and $X_{it}$ are <font color=red> **missing at random** </font>
  - $\varepsilon=\left(\varepsilon_1, \ldots, \varepsilon_N\right)$ and $\varepsilon_i \equiv\left(\varepsilon_{i 1}, \ldots, \varepsilon_{i T}\right)^{\prime}$ for $i=1, \ldots, N$.
  - $F=\left(F_1, \ldots, F_T\right)^{\prime}$ and $\Lambda=\left(\lambda_1, \ldots, \lambda_N\right)^{\prime}$ where $F_t$ and $\lambda_i$ are $R \times 1$ vectors of factors and factor loadings
- $F^0=\left(F_1^0, \ldots, F_T^0\right)^{\prime}$ and $\Lambda^0=\left(\lambda_1^0, \ldots, \lambda_N^0\right)^{\prime}$ are the true values of $F$ and $\Lambda$
- $\Omega \subset[N] \times[T]$ be the index set of the observations that are observed. That is,
$$
\Omega=\Big\\{(i, t) \in[N] \times[T]: X_{i t} \text { is observed }\Big\\}.
$$

- Let $G$ denote a $T \times N$ matrix with $(t, i)$ th element given by $g_{i t}=\mathbf{1}\\{(i, t) \in \Omega\\}$ and is <mark>independent of $X, F^0, \Lambda^0$ and $\varepsilon$</mark>

---

{{< slide background-image="body1.jpg" >}}

## The initial estimates


- Let $\tilde{X}=X \circ G$ and $\tilde{X}\_{i t}=X\_{i t} g\_{i t}$.
- The common component $C^0 \equiv F^0 \Lambda^0$ is <mark>a low rank matrix</mark> $\Rightarrow$ it is possible to recover $C^0$ even when a large proportion of elements in $X$ are missing at random.
- Under the standard condition that $E\left(\varepsilon_{i t} \mid F_t^0, \lambda_i^0\right)=0$, we can verify that $E\left(\frac{1}{q} \tilde{X} \mid F^0, \Lambda^0\right)=F^0 \Lambda^{0 \prime}$ $\Rightarrow$ consider the following least squares objective function
$$
\mathcal{L}_{N T}^0(F, \Lambda) \equiv \frac{1}{N T} \operatorname{tr}\left[\left(\frac{1}{\tilde{q}} \tilde{X}-F \Lambda^{\prime}\right)\left(\frac{1}{\tilde{q}} \tilde{X}-F \Lambda^{\prime}\right)^{\prime}\right]
$$
**identification restrictions**: $F^{\prime} F / T=I_R$ and $\Lambda^{\prime} \Lambda$ is a diagonal matrix. 

- By concentrating out $\Lambda$ and using the normalization that $F^{\prime} F / T=I_R$ $\Rightarrow$ identical to maximizing $\tilde{q}^{-2} \operatorname{tr}\Big\\{F^{\prime} \tilde{X} \tilde{X}^{\prime} F\Big\\}$ 

---


{{< slide background-image="body1.jpg" >}}
## The initial estimates

- The estimated factor matrix, denoted by $\hat{F}^{(0)}$ is $\sqrt{T}$ times the eigenvectors corresponding to the $R$ largest eigenvalues of the $T \times T$ matrix $\frac{1}{N T \tilde{q}^2} \tilde{X} \tilde{X}^{\prime}:$
$$
\frac{1}{N T \tilde{q}^2} \tilde{X} \tilde{X}^{\prime} \hat{F}^{(0)}=\hat{F}^{(0)} \hat{D}^{(0)},
$$
  - $\hat{D}^{(0)}$ is an $R \times R$ diagonal matrix consisting of the $R$ largest eigenvalues of $\left(N T \tilde{q}^2\right)^{-1} \tilde{X} \tilde{X}^{\prime}$, arranged in descending order along its diagonal line.
- The estimator of $\Lambda^{\prime}$ is given by
$$
\hat{\Lambda}^{(0) \prime}=\frac{1}{\tilde{q}}\left(\hat{F}^{(0) \prime} \hat{F}^{(0)}\right)^{-1} \hat{F}^{(0) \prime} \tilde{X}=\frac{1}{T \tilde{q}} \hat{F}^{(0) \prime} \tilde{X} .
$$
- We can obtain an initial estimate of the $(t, i)$ th element, $C\_{i t}^0$, of $C^0$ by $\hat{C}\_{i t}^{(0)}=\hat{\lambda}\_i^{(0)\prime} \hat{F}\_t^{(0)}$. 
- The initial estimators $\hat{F}\_t^{(0)}, \hat{\lambda}\_i^{(0)}$ and $\hat{C}\_{i t}^{(0)}$ are consistent and follow mixture normal distributions under some standard conditions.


---

{{< slide background-image="body1.jpg" >}}
## The iterated estimates

- The initial estimators: consistency but <mark>not asymptotically efficient</mark> $\Rightarrow$ **iterative estimators**
- In step $\ell$, we can <mark> replace the missing values $\left(X_{i t}\right)$ in the matrix $X$ with the estimated common components $\hat{C}\_{i t}^{(\ell-1)}$</mark>. Define the $T \times N$ matrix $\hat{X}^{(\ell)}$ with its $(t, i)$ th element given by
$$
\hat{X}\_{i t}^{(\ell)}=\left\\{\begin{array}{ll}
X\_{i t} & \text { if }(i, t) \in \Omega \\\
\hat{C}\_{i t}^{(\ell-1)} & \text { if }(i, t) \in \Omega\_{\perp}
\end{array}, \ell \geq 1,\right.
$$
where $\Omega_{\perp}=\\{(i, t) \in[N] \times[T]:(i, t) \notin \Omega\\}$. 
- Then we can conduct the PC analysis based on $\hat{X}^{(\ell)}$ and obtain $\hat{F}^{(\ell) \prime}$ and $\hat{\Lambda}^{(\ell)}$. 
- We will study the asymptotic properties of $\hat{F}\_t^{(\ell)}, \hat{\lambda}\_i^{(\ell)}$ and $\hat{C}\_{i t}^{\left(\ell^{\ell}\right)}, \ell=1,2, \ldots$

$$
~\\\\\
~\\\\\
$$
