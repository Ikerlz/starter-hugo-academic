---
title: Using the Propensity Score in Regressions for Causal Effects
summary: Chapter 14 in "A First Course in Causal Inference".
authors: [Zhe Li]
tags: []
categories: [Causal Inference]
date: '2023-10-29T00:00:00Z'
slides:
  # Choose a theme from https://github.com/hakimel/reveal.js#theming
  theme: black
  # Choose a code highlighting style (if highlighting enabled in `params.toml`)
  #   Light style: github. Dark style: dracula (default).
  highlight_style: dracula
---


{{< slide background-image="ZIBIN.jpg" >}}

# Using the Propensity Score in Regressions for Causal Effects

$$
\begin{aligned}
\\
\end{aligned}
$$

<center> Li Zhe <center>

$$$$

<center>School of Data Science, Fudan University <center> 




$$$$

<center>November 1, 2023<center>

---

{{< slide background-image="body1.jpg" >}}
## Introduction

- This chapter discusses two simple methods to use the propensity score: 

  - the propensity score as a covariate in regressions
  - running regressions weighted by the inverse of the propensity score

- **Reasons**: 

  - they are easy to implement, which involve only standard statistical software packages for regressions;
  - their properties are comparable to many more complex methods;
  - they can be easily extended to allow for flexible statistical models including machine learning algorithms.


---

{{< slide background-image="body1.jpg" >}}


### Outline


- Regressions with the propensity score as a covariate 
  - **Theorem 14.1**
  - Proposition 14.1

$$$$

- Regressions weighted by the inverse of the propensity score
  - Average causal effect
    - Theorem 14.2
  - Average causal effect on the treated units
    - Table 14.1
    - Proposition 14.2
    - Theorem 14.3

---

{{< slide background-image="body1.jpg" >}}
### Regressions with the propensity score as a covariate

$$
\text { Theorem 11.1 If } Z \perp\\!\\!\\!\perp\\{Y(1), Y(0)\\} \mid X \text {, then } {\color{red}Z \perp\\!\\!\\!\perp\\{Y(1), Y(0)\\} \mid e(X)} \text {. }
$$

- By Theorem 11.1, if unconfoundedness holds conditioning on $X$, then it also holds conditioning on $e(X)$: $\color{red}{Z \perp\\!\\!\\!\perp\\{Y(1), Y(0)\\} \mid e(X) }.$
- Analogous to (10.5), $\tau$ is also <mark>nonparametrically</mark> identified by
$$
\tau=E[E\\{Y \mid Z=1, e(X)\\}-E\\{Y \mid Z=0, e(X)\\}],
$$
- $\Rightarrow$ The simplest regression specification is the OLS fit of $Y$ on $\\{1, Z, e(X)\\}$, with the coefficient of $Z$ as an estimator, denoted by $\tau_e$:
$$
\arg \min _{a, b, c} E\\{Y-a-b Z-c e(X)\\}^2
$$
- $\tau_e$ defined as the coefficient of $Z$. 
- It is consistent for $\tau$ if 
  - have a correct propensity score model
  - the outcome model is indeed linear in $Z$ and $e(X)$
- $\tau_e$ estimates $\tau_{\mathrm{O}}$ if we have a correct propensity score model even if the outcome model is <mark>completely misspecified</mark>

---

{{< slide background-image="body1.jpg" >}}

### Regressions with the propensity score as a covariate

<mark>Theorem 14.1</mark> If $Z \perp\\!\\!\\!\perp\\{Y(1), Y(0)\\} \mid X$, then the coefficient of $Z$ in the OLS fit of $Y$ on $\\{1, Z, e(X)\\}$ equals
$$
\tau_e=\tau_{\mathrm{O}}=\frac{E\left\\{h_{\mathrm{O}}(X) \tau(X)\right\\}}{E\left\\{h_{\mathrm{O}}(X)\right\\}},
$$
recalling that $h_{\mathrm{O}}(X)=e(X)\\{1-e(X)\\}$ and $\tau(X)=E\\{Y(1)-Y(0) \mid X\\}$.

$$
\begin{aligned}
\\
\\
\end{aligned}
$$

<mark>Corollary 14.1</mark> If $Z \perp\\!\\!\\!\perp\\{Y(1), Y(0)\\} \mid X$, then
- the coefficient of $Z-e(X)$ in the OLS fit of $Y$ on $Z-e(X)$ or $\\{1, Z-e(X)\\}$ equals $\tau_{\mathrm{O}}$;
- the coefficient of $Z$ in the OLS fit of $Y$ on $\\{1, Z, e(X), X\\}$ equals $\tau_{\mathrm{O}}$.

---

{{< slide background-image="body1.jpg" >}}

### Regressions with the propensity score as a covariate

- An unusual feature of Theorem 14.1 is that **the overlap condition** ($0 < e(x) < 1$) is not needed any more. 
- Even if some units have propensity score $e(X)$ equaling 0 or 1, their associate weight $e(X)\\{1-e(X)\\}$ is zero so that they do not contribute anything to the final parameter $\tau_O$.


---


{{< slide background-image="body1.jpg" >}}

## Frisch–Waugh–Lovell Theorem


The Frisch–Waugh–Lovell (FWL) theorem reduces multivariate OLS to univariate OLS and therefore facilitate the understanding and calculation of the OLS coefficients.


<mark>Theorem A2.2 (sample FWL)</mark> With data $\left(Y, X_1, X_2, \ldots, X_p\right)$ containing column vectors, the coefficient of $X_1$ equals the coefficient of $\tilde{X}_1$ in the OLS fit of $Y$ or $\tilde{Y}$ on $\tilde{X}_1$, where 

- $\tilde{Y}$ is the residual vector from the OLS fit of $Y$ on $\left(X_2, \ldots, X_p\right)$
- $\tilde{X}_1$ is the residual from the OLS fit of $X_1$ on $\left(X_2, \ldots, X_p\right)$.


$$$$

Based on the FWL theorem, we can obtain $\tau_e$ in two steps: 

- first, we obtain the residual $\tilde{Z}$ from the OLS fit of $Z$ on $\{1, e(X)\}$; 
- then, we obtain $\tau_e$ from the OLS fit of $Y$ on $\tilde{Z}$.


---


{{< slide background-image="body1.jpg" >}}

### Proof of Theorem 14.1

The coefficient of $e(X)$ in the OLS fit of $Z$ on $\\{1, e(X)\\}$ is
$$
\begin{aligned}
\frac{\operatorname{cov}\\{Z, e(X)\\}}{\operatorname{var}\\{e(X)\\}} & =\frac{E[\operatorname{cov}\\{Z, e(X) \mid X\\}]+\operatorname{cov}\\{E(Z \mid X), e(X)\\\}}{\operatorname{var}\\{e(X)\\}} \\\ &=\frac{0+\operatorname{var}\\{e(X)\\}}{\operatorname{var}\\{e(X)\\}}=1,
\end{aligned}
$$
- the intercept is $E(Z)-E\\{e(X)\\}=0$
- the residual is $\tilde{Z}=Z-e(X)$ (This makes sense since $Z-e(X)$ is uncorrelated with any function of $X$).

Therefore, we can obtain $\tau_e$ from the univariate OLS fit of $Y$ on $Z-e(X)$ :
$$\small{\tau_e=\frac{\operatorname{cov}\\{Z-e(X), Y\\}}{\operatorname{var}\\{Z-e(X)\\}}}$$
The denominator simplifies to
$$
\begin{aligned}
\operatorname{var}\\{Z-e(X)\\} & =E\\{Z-e(X)\\}^2 =e(X)+e(X)^2-2 e(X)^2=h_{\mathrm{O}}(X)
\end{aligned}
$$


---


{{< slide background-image="body1.jpg" >}}

### Proof of Theorem 14.1

The numerator simplifies to
$$
\begin{aligned}
& \operatorname{cov}\\{Z-e(X), Y\\} \\\
= & E[\\{Z-e(X)\\} Y] \\\
= & E[\\{Z-e(X)\\} Z Y(1)]+E[\\{Z-e(X)\\}(1-Z) Y(0)] \\\
& \quad \quad \quad{\color{red}(\text { since } Y=Z Y(1)+(1-Z) Y(0))} \\\
= & E[\\{Z-Z e(X)\\} Y(1)]-E[e(X)(1-Z) Y(0)] \\\
= & E[Z\\{1-e(X)\\} Y(1)]-E[e(X)(1-Z) Y(0)] \\\
= & E\left[e(X)\\{1-e(X)\\} \mu_1(X)\right]-E\left[e(X)\\{1-e(X)\\} \mu_0(X)\right] \\\
& \quad \quad \quad\text { {\color{red}(tower property and ignorability)} } \\\
= & E\left\\{h_{\mathrm{O}}(X) \tau(X)\right\\} .
\end{aligned}
$$


- From the proof of Theorem 14.1, we can simply run the OLS of $Y$ on the centered treatment $\tilde{Z} = Z - e(X)$.
- Moreover, we can also include $X$ in the OLS fit which may improve efficiency in finite sample.

---


{{< slide background-image="body1.jpg" >}}

### Comments of Theorem 14.1 and Corollary 14.1

- Theorem 14.1 motivates a two-step estimator for $\tau_{\mathrm{O}}$: 
  - first, fit a propensity score model to obtain $\hat{e}\left(X_i\right)$; 
  - second, run OLS of $Y_i$ on $\left(1, X_i, \hat{e}\left(X_i\right)\right)$ to obtain the coefficient of $Z_i$. 
- Corollary 14.1 motivates another two-step estimator for $\tau_{\mathrm{O}}$: 
  - first, fit a propensity score model to obtain $\hat{e}\left(X_i\right)$; 
  - second, run OLS of $Y_i$ on $Z_i-\hat{e}\left(X_i\right)$ to obtain the coefficient of $Z_i$. 

**Remark**: OLS is convenient for obtaining point estimators, the corresponding standard errors are incorrect due to <mark>the uncertainty in the first step estimation of the propensity score</mark>. We can use the bootstrap to approximate the standard errors.

---


{{< slide background-image="body1.jpg" >}}

#### Regressions weighted by the inverse of the propensity score

We first re-examine the Hajek estimator of $\tau$ :
$$
\hat{\tau}^{\text {hajek }}=\frac{\sum_{i=1}^n \frac{Z_i Y_i}{\hat{e}\left(X_i\right)}}{\sum_{i=1}^n \frac{Z_i}{\hat{e}\left(X_i\right)}}-\frac{\sum_{i=1}^n \frac{\left(1-Z_i\right) Y_i}{1-\hat{e}\left(X_i\right)}}{\sum_{i=1}^n \frac{1-Z_i}{1-\hat{e}\left(X_i\right)}},
$$
- which equals the difference between the weighted means of the outcomes in the treatment and control groups.
- Numerically, it is identical to the coefficient of $Z_i$ in the following weighted least squares (WLS) of $Y_i$ on $\left(1, Z_i\right)$.


<mark>Proposition 14.1</mark> $\hat{\tau}^{\text {hajek }}$ equals $\hat{\beta}$ from the following $WLS$ :

$$
(\hat{\alpha}, \hat{\beta})=\arg \min_{\alpha, \beta} \sum_{i=1}^n w_i\left(Y_i-\alpha-\beta Z_i\right)^2
$$

with weights
$$
w_i=\frac{Z_i}{\hat{e}\left(X_i\right)}+\frac{1-Z_i}{1-\hat{e}\left(X_i\right)}= \begin{cases}\frac{1}{\hat{e}\left(X_i\right)} & \text { if } Z_i=1 \\\ \frac{1}{1-\hat{e}\left(X_i\right)} & \text { if } Z_i=0\end{cases}
$$

---



{{< slide background-image="body1.jpg" >}}

### Average causal effect

- By Proposition 14.1, it is convenient to obtain $\hat{\tau}^{\text {hajek }}$ based on WLS. 
- However, due to the uncertainty in the estimated propensity score, the standard error <mark>reported by WLS is incorrect</mark> for the true standard error $\Rightarrow$ **bootstrap**
{style="color: black"}
- Why does the WLS give a consistent estimator for $\tau$ ?
{style="color: red"} 
  - Recall that in the CRE with a constant propensity score, we can simply use the coefficient of $Z_i$ in the OLS fit of $Y_i$ on $(1, Z_i)$ to estimate $\tau$. 
  - In observational studies, units have different probabilities of receiving the treatment and control, respectively. 
  - If we weight the treated units by $1 / e(X_i)$ and the control units by $1 /\\{1-e(X_i)\\}$, then they can represent the whole population and we effectively have **a pseudo randomized experiment**. 
  - Consequently, the difference between the weighted means are consistent for $\tau$. 
  - The numerical equivalence of $\hat{\tau}^{\text {hajek }}$ and WLS is not only a fun numerical fact itself but also useful for motivation more complex estimator with covariate adjustment.

---



{{< slide background-image="body1.jpg" >}}


### An extension to a more complex estimator

- In the CRE, we can use the coefficient of $Z_i$ in the OLS fit of $Y_i$ on $(1, Z_i, X_i, Z_i X_i)$ to estimate $\tau$, where the covariates are centered with $\bar{X}=0$. 
- This is Lin (2013)'s estimator which uses covariates to improve efficiency. 
- A natural extension to observational studies is to estimate $\tau$ using the coefficient of $Z_i$ in the WLS fit of $Y_i$ on $\left(1, Z_i, X_i, Z_i X_i\right)$ with weights defined in (14.1). 
- If the linear models
$$
E(Y \mid Z=1, X)=\beta_{10}+\beta_{1 x}^{\top} X, \quad E(Y \mid Z=0, X)=\beta_{00}+\beta_{0 x}^{\top} X,
$$
are correctly specified, then <mark>both OLS and WLS give consistent estimators for the coefficients</mark> and the estimators of the coefficient of $Z$ is consistent for $\tau$. 
- More interestingly, the estimator of the coefficient of $Z$ based on WLS is also consistent for $\tau$ if **the propensity score model is correct and the outcome model is incorrect**. $\Rightarrow$ the estimator based on WLS is **doubly robust**.

---


{{< slide background-image="body1.jpg" >}}

### A doubly robust estimator

- Let $\hat{e}(X_i)$ be the fitted propensity score and $(\mu_1(X_i, \hat{\beta}_1), \mu_0(X_i, \hat{\beta}_0))$ be the fitted values of the outcome means based on the WLS. 
- The outcome regression estimator is

$$
\hat{\tau}\_{\mathrm{wls}}^{\mathrm{reg}}= \frac{1}{n}\sum\_{i=1}^n\mu_1\left(X\_i, \hat{\beta}\_1\right)-\frac{1}{n} \sum\_{i=1}^n \mu_0\left(X_i, \hat{\beta}_0\right)
$$


- The doubly robust estimator for $\tau$ is

$$
\hat{\tau}_{\mathrm{wls}}^{\mathrm{dr}}=\hat{\tau}\_{\mathrm{wls}}^{\mathrm{reg}}+\frac{1}{n} \sum\_{i=1}^n \frac{Z_i\left\\{Y_i-\mu_1\left(X_i, \hat{\beta}_1\right)\right\\}}{\hat{e}\left(X_i\right)}-\frac{1}{n} \sum\_{i=1}^n \frac{\left(1-Z_i\right)\left\\{Y_i-\mu_0\left(X_i, \hat{\beta}_0\right)\right\\}}{1-\hat{e}\left(X_i\right)} .
$$

- An interesting result is that this doubly robust estimator equals the outcome regression estimator, which reduces to the coefficient of $Z_i$ in the WLS fit of $Y_i$ on $\left(1, Z_i, X_i, Z_i X_i\right)$ if we use weights (14.1).


---



{{< slide background-image="body1.jpg" >}}

### Theorem 14.2

If $\bar{X}=0$ and $(\mu_1(X_i, \hat{\beta}\_1), \mu_0(X_i, \hat{\beta}\_0))=(\hat{\beta}_{10}+\hat{\beta}\_{1 x}^{\top} X_i, \hat{\beta}\_{00}+\hat{\beta}\_{0 x}^{\top} X_i)$ based on the WLS fit of $Y_i$ on $\left(1, Z_i, X_i, Z_i X_i\right)$ with weights (14.1), then
{style="color: blue"}
$$
\hat{\tau}\_{\mathrm{wls}}^{\mathrm{dr}}=\hat{\tau}\_{\mathrm{wls}}^{\mathrm{reg}}=\hat{\beta}\_{10}-\hat{\beta}\_{00},
$$
{style="color: blue"}
which is the coefficient of $Z_i$ in the WLS fit.
{style="color: blue"}

$$$$

- Freedman and Berk (2008) showed that when the outcome model is correct, the WLS estimator is worse than the OLS estimator 
- When the errors have variance proportional to the inverse of the propensity scores, the WLS estimator will be more effcient than the OLS estimator. 
- The estimated standard error based on the WLS fit is not consistent for the true standard error because it **ignores the uncertainty in the estimated propensity score**. 
- This can be easily fixed by using the bootstrap to approximate the variance of the WLS estimator. 
- Nevertheless, they found that "weighting may help under some circumstances" because when the outcome model is incorrect, the
WLS estimator is still consistent if the propensity score model is correct.


---




{{< slide background-image="body1.jpg" >}}


### Average causal effect on the treated units


<mark>Proposition 14.2</mark> $\hat{\tau}\_{\mathrm{T}}^{\text {hajek }}$ is numerically identical to $\hat{\beta}$ in the following WLS:
$$
(\hat{\alpha}, \hat{\beta})=\arg \min\_{\alpha, \beta} \sum\_{i=1}^n w_{\mathrm{T} i}\left(Y_i-\alpha-\beta Z_i\right)^2
$$
with weights
$$
w_{\mathrm{T} i}=Z_i+\left(1-Z_i\right) \hat{o}\left(X_i\right)= \begin{cases}1 & \text { if } Z_i=1 \\\ \hat{o}\left(X_i\right) & \text { if } Z_i=0\end{cases}
$$
where $\hat{o}\left(X_i\right)=\hat{e}\left(X_i\right) /\\{1-\hat{e}\left(X_i\right)\\}$


---

{{< slide background-image="body1.jpg" >}}

### Regression estimators

|  | CRE | unconfounded observational studies |
| :---: | :---: | :---: |
| without $X$ | $Y_i \sim Z_i$ | $Y_i \sim Z_i$ with weights $w_i$ |
| with $X$ | $Y_i \sim\left(Z_i, X_i, Z_i X_i\right)$ | $Y_i \sim\left(Z_i, X_i, Z_i X_i\right)$ with weights $w_i$ |


---

{{< slide background-image="body1.jpg" >}}

### Average causal effect on the treated units

- If we center covariates with $\hat{X}(1)=0$, then we can estimate $\tau\_{\mathrm{T}}$ using the coefficient of $Z_i$ in the WLS fit of $Y_i$ on $\left(1, Z_i, X_i, Z_i X_i\right)$ with weights defined in (14.2). Similarly, this estimator equals the regression estimator

$$
\hat{\tau}\_{\mathrm{T}, \mathrm{wls}}^{\mathrm{reg}}=\hat{\bar{Y}}(1)-\frac{1}{n_1} \sum\_{i=1}^n Z_i \mu_0\left(X_i, \hat{\beta}_0\right)
$$

which also equals the doubly robust estimator

$$
\hat{\tau}\_{\mathrm{T}, \mathrm{wls}}^{\mathrm{dr}}=\hat{\tau}\_{\mathrm{T}, \mathrm{wls}}^{\mathrm{reg}}-\frac{1}{n_1} \sum\_{i=1}^n \hat{o}\left(X_i\right)\left(1-Z_i\right)\left\\{Y_i-\mu_0\left(X_i, \hat{\beta}_0\right)\right\\}.
$$

<mark>Theorem 14.3</mark> If $\hat{\bar{X}}(1)=0$ and $\mu_0(X_i, \hat{\beta}_0)=\hat{\beta}\_{00}+\hat{\beta}\_{0x}^{\top} X_i$ based on the WLS fit of $Y_i$ on $(1, Z_i, X_i, Z_i X_i)$ with weights (14.2), then

$$
\hat{\tau}\_{\mathrm{T}, \mathrm{wls}}^{\mathrm{dr}}=\hat{\tau}\_{\mathrm{T}, \mathrm{wls}}^{\mathrm{reg}}=\hat{\beta}\_{10}-\hat{\beta}\_{00},
$$

which is the coefficient of $Z_i$ in the $W L S$ fit.