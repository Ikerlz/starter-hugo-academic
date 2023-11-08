---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Notes on ''The Asymptotic Variance of Semiparametric Estimators''"
subtitle: ""
summary: ""
authors: []
tags: []
categories: ["causal-inference"]
date: 2023-11-05T22:16:45+08:00
lastmod: 2023-11-05T22:16:45+08:00
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

## 0. Background: Semiparametric Model

> **Reference**: [Lecture notes](http://www.stat.columbia.edu/~bodhi/Talks/SPThNotes.pdf) from Prof. Bodhisattva Sen (Columbia University)

A semiparametric model is a statistical model that involves both **parametric** and **nonparametric** (infinite-dimensional) components. However, we are mostly interested in estimation and inference of a <mark>finite-dimensional parameter</mark> in the model.

---

{{< spoiler text="**Example 1 (population mean)**" >}}
Suppose that $X_1, \ldots, X_n$ are i.i.d. $P$ belonging to the class $\mathcal{P}$ of distribution. Let $\psi(P) \equiv \mathbb{E}_P\left[X_1\right]$, the mean of the distribution, be the parameter of interest.

Question: 
{style="color: red"}
- Suppose that $\mathcal{P}$ is the class of all distributions that have a finite variance. 
- What is the most efficient estimator of $\psi(P)$, i.e., what is the estimator with the best asymptotic performance?

{{% callout note %}}
A model $\mathcal{P}$ is simply a collection of probability distributions for the data we observe.
{{% /callout %}}

{{< /spoiler >}}


{{< spoiler text="**Example 2 (partial linear regression model)**" >}}
Suppose that we observe i.i.d. data $\left\\{X\_i \equiv\left(Y\_i, Z\_i, V\_i\right): i=1, \ldots, n\right\\}$ from the following partial linear regression model:
$$
Y_i=Z_i^{\top} \beta+g\left(V_i\right)+\epsilon_i
$$
- $Y_i$ is the scalar response variable
- $Z_i$ and $V_i$ are vectors of predictors
- $g(\cdot)$ is the unknown (nonparametric) function
- $\epsilon_i$ is the unobserved error. 

For simplicity and to focus on the semiparametric nature of the problem: 
{style="color: red"}
- Assume that $\left(Z_i, V_i\right) \sim f(\cdot, \cdot)$, where we assume that the density $f(\cdot, \cdot)$ is known, is independent of $\epsilon_i \sim N\left(0, \sigma^2\right)$ (with $\sigma^2$ known). 
- The model, under these assumptions, has a <mark>parametric component</mark> $\beta$ and a <mark>nonparametric component</mark> $g(\cdot)$
{{< /spoiler >}}

---

> <mark>**"Separated semiparametric model"**</mark>: We say that the model $\mathcal{P}=\left\\{P_{\nu, \eta}\right\\}$ is a "separated semiparametric model", where $\nu$ is a **"Euclidean parameter"** and $\eta$ runs through a nonparametric class of distributions (or some infinite-dimensional set). This gives a semiparametric model in the strict sense, in which we <font color="red">aim at estimating $\nu$ and consider $\eta$ as a nuisance parameter.</font>

<mark>**"Frequent questions for semiparametric model"**</mark>: consider the estimation of a parameter of interest $\nu=\nu(P)$, where the data has distribution $P \in \mathcal{P}$:
- (Q1) How well can we estimate $\nu=\nu(P)$ ? What is our "gold standard"?
- (Q2) Can we compare absolute "in principle" standards for estimation of $\nu$ in a model $\mathcal{P}$ with estimation of $\nu$ in a submodel $\mathcal{P}\_0 \subset \mathcal{P}$ ? What is the effect of *not knowing* $\eta$ on estimation of $\nu$ when $\mathcal{P}=\left\\{P\_\theta: \theta \equiv(\nu, \eta) \in \Theta\right\\}$ ?
- (Q3) How do we construct **efficient** estimators of $\nu(P)$ ?

{{% callout note %}}
A model $\mathcal{P}$ is simply a collection of probability distributions for the data we observe.
{{% /callout %}}

---

## 1. Introduction

- Develop a general form for <mark>the asymptotic variance of semiparametric estimators</mark> that depend on nonparametric estimators of functions. 
- The formula is often straightforward to derive, requiring only some calculus. 
- Although the formula is not based on primitive conditions, it should be useful for semiparametric estimators, just as analogous formulae are for parametric estimators. 
- The formula gives the form of remainder terms, which facilitates specification of primitive conditions.
- It can also be used to make asymptotic efficiency comparisons and to find an efficient estimator in some class.

```markmap
- 
  - Derive the formula: **Section 2**
  - Propositions about semiparametric estimator
    - **Section 3**
    - **Section 4**
  - High-level regularity conditions: **Section 5**
  - Conditions for $\sqrt{n}$-consistency and asymptotic normality: **Section 6**
  - Primitive conditions for the examples: **Section 7**
```

## 2. The Pathwise Derivative Formula For the Asymptotic Variance


> Preliminary: [one-step estimators and pathwise derivatives]({{< relref "/post/path-dev" >}})

The formula is based on the observation that <font color="red">$\sqrt{n}$-consistent</font> **nonparametric** estimators are often efficient. 

> For example, the sample mean is known to be an efficient estimator of the population mean in a **nonparametric** model where no
restrictions, other than regularity conditions (e.g. **existence of the second moment**) are placed on the distribution of the data. 

**Idea**

- Calculate the asymptotic variance of a semiparametric estimator as the variance bound for the *functional* that it nonparametrically estimates. 
- In other words, the formula is <font color="red">the variance bound for the functional that is the limit of the estimator under general misspecification.</font>

---

Let $z_1, \ldots, z_n$ be i.i.d. data, with <font color="red">(true) distribution $F_0$</font> of $z_i$, and let $\hat{\beta}=\beta_n\left(z_1, \ldots, z_n\right)$ denote a $q \times 1$ vector of estimators. Suppose $\hat{\beta}$ can be associated with a family of distributions and a functional as
$$
\hat{\beta} \rightarrow \begin{cases}\mathscr{F}=\\{F\\} ; & \text { general family of distributions of } z \\\ \mu: \mathscr{F} \rightarrow \mathbb{R}^q ; & \text { if } z_i \text { has distribution } F \text { then } {\color{red}\operatorname{plim}(\hat{\beta})=\mu(F)}\end{cases}
$$

- The word "general" is taken to mean that $\mathscr{F}$ is unrestricted, except for regularity conditions, and allows for general misspecification. 
- This equation also specifies that <mark>$\mu(F)$ is the limit of $\hat{\beta}$ when $z_i$ has distribution $F$</mark>. 
- $\mu(F)$ traces out the limits of $\hat{\beta}$ as $F$ varies within the general family $\mathscr{F}$.
- The variance formula for $\hat{\beta}$ is the semiparametric bound for estimation of $\mu(F)$, calculated as in *Koshevnik and Levit (1976)*, *Pfanzagl and Wefelmeyer (1982)*, and others. 

---

Let <mark>$\\{F_\theta: F_\theta \in \mathscr{F}\\}$</mark> denote a one-dimensional subfamily of $\mathscr{F}$, i.e. a path in $\mathscr{F}$, that is equal to the true distribution $F_0$ when $\theta=0$. 
- Suppose that $F_\theta$ has a density $d F_\theta$ and a corresponding score 
$${\color{red}S(z)=\frac{\partial \ln \left(d F_\theta\right)}{ \partial \theta}\Big|_{\theta=0}}.$$
- Suppose that the set of scores can approximate in mean square any mean zero, finite variance function of $z$. 
- Let $E[\cdot]$ denote the expectation at the true distribution $F_0$. 

The **pathwise derivative** of $\mu(F)$ is a $q \times 1$ vector $d(z)$ with <mark>$E[d(z)]=0$ and $E\left[\|d(z)\|^2\right]<\infty$</mark> such that for every path,
$$
{\color{red}\frac{\partial \mu\left(F_\theta\right)}{\partial \theta}=E[d(z) S(z)].}\qquad(\star)
$$

- <mark>The variance bound for estimation of $\mu(F)$ is $\operatorname{Var}(d(z))$</mark>. 
- Thus, the asymptotic variance formula suggested here is the variance of the **pathwise derivative** of the functional $\mu(F)$ that is estimated under general misspecification.


{{< spoiler text="**Example**" >}}
- Consider the parameter $\beta_0=\int f_0(z)^2 d z$, where $f_0(z)$ is the density function of $z_i$. 
- One estimator is <mark>$\tilde{\beta}=\sum_{i=1}^n \hat{f}\left(z_i\right) / n$</mark>, for a nonparametric density estimator $\hat{f}(z)$ of $z_i$. 
- Suppose $z$ is symmetrically distributed around zero. Then one might hope to improve efficiency by using the antithetic estimate $\hat{f}(-z)$ of the density to form
$$\hat{\beta}=\sum_{i=1}^n\frac{[\hat{f}(z_i)+\hat{f}(-z_i)]}{2}.$$ 
- The asymptotic variance can be found by calculating the limit of $\hat{\beta}$ under <mark>general misspecification</mark>, where $z$ need not be symmetric about zero, and the pathwise derivative of this limit. 
  - Let $E_F[\cdot]$ denote the expectation at a distribution $F$ and let $E_\theta[\cdot]=E_{F_\theta}[\cdot]$ for a path $F_\theta$. 
  - By an appropriate uniform law of large numbers the limit of $\hat{\beta}$ is $\mu(F)=\int[f(z)+f(-z)] f(z) d z / 2$. 
  - Assuming that differentiation inside the integral is allowed, 
$$
\begin{aligned}
\frac{\partial \mu\left(F\_\theta\right)}{ \partial \theta} &=\int\left[\frac{\partial f\_\theta(z) }{ \partial \theta}\right] f_0(z) d z\\\
&\qquad +\frac{1}{2}\left\\{\int\left[\frac{\partial f\_\theta(-z)}{ \partial \theta}\right] f_0(z) d z\right.\\\
&\qquad\qquad\qquad\qquad\qquad +\left.\int\left[\frac{\partial f\_\theta(z) }{ \partial \theta}\right] f_0(-z) d z\right\\}\\\
&=E\left[\left\\{f_0(z)+f_0(-z)\right\\} S(z)\right]\\\
&=E[d(z) S(z)]
\end{aligned}
$$
- Thus, in this example the asymptotic variance formula is ${\color{red}\operatorname{Var}\left(2 f_0(z)\right)}$, which is the well known asymptotic variance of $\tilde{\beta}$, so <mark>no efficiency improvement results</mark>.
{{< /spoiler >}}


The **pathwise derivative** generalizes the Gateaux derivative formula for von-Mises estimators:
- The pathwise derivative formula works for estimators that are explicit functions of densities or expectations, where the domain of $\mu(F)$ may only include **continuous distributions**. 
- The Gateaux derivative formula only applies when the domain of $\mu(F)$ also includes **discrete distributions**

---

A precise justification for the asymptotic variance formula is available when $\hat\beta$ is asymptotically equivalent to a sample average: define $\hat{\beta}$ to be asymptotically linear with **influence function** $\psi(z)$ when $z_i$ has distribution $F_0$:
$$
\begin{aligned}
& \sqrt{n}\left(\hat{\beta}-\beta_0\right)=\sum_{i=1}^n \psi\left(z_i\right) / \sqrt{n}+O_p(1), \\\
& E[\psi(z)]=0, \quad \operatorname{Var}(\psi(z)) \text { finite. }
\end{aligned}
$$

> Asymptotic linearity and the central limit theorem imply $\hat{\beta}$ is asymptotically normal with variance $\operatorname{Var}(\psi(z))$.

### Regular path & regular estimator

- Define the path <font color="red">$\left\\{F_\theta: \theta \in(-\varepsilon, \varepsilon) \subset \mathbb{R}, \varepsilon>0, F_\theta \in \mathscr{F}\right\\}$</font> to be regular if <mark>each distribution is absolutely continuous w.r.t the same dominating measure and $S(z)$ satisfies the mean-square derivative condition</mark>
$$
\int\left[\theta^{-1}\left(d F_\theta^{1 / 2}-d F_0^{1 / 2}\right)-\frac{1}{2} S(z) d F_0^{1 / 2}\right]^2 d z \rightarrow 0, \text { as } \quad \theta \rightarrow 0 .
$$

- Define $\hat{\beta}$ to be a **regular estimator** of $\mu(F)$ if for any regular path and $\theta_n=$ $0(1 / \sqrt{n})$, when $z_i$ has distribution $F_{\theta_n}, \sqrt{n}\left(\hat{\beta}-\mu\left(F_{\theta_n}\right)\right)$ has a limiting distribution that <mark>does not depend on $\left\\{\theta_n\right\\}_{n=1}^{\infty}$</mark>. 

### The pathwise derivative asymptotic variance formula

<mark>**THEOREM 2.1**</mark>: Suppose that 
- (i) the set of scores for regular paths is linear; 
- (ii) for any $\varepsilon>0$ and measureable $s(z)$ with $E[s(z)]=0$ and $E\left[s(z)^2\right]<\infty$, there is a regular path with score $S(z)$ satisfying $E\left[|s(z)-S(z)|^2\right]<\varepsilon$; 
- (iii) $\hat{\beta}$ is asymptotically linear and regular.

**Then there is $d(z)$ such that equation $(\star)$ is satisfied and $\psi(z)=d(z)$**

- Condition (ii), that the scores can approximate any mean zero function, is the precise version of the "generality" property of $\mathscr{F}$.
- Regularity of $\hat{\beta}$ is the precise condition that specifies that <mark>$\hat{\beta}$ is a nonparametric estimator of $\mu(F)$</mark>.
- Innovations: calculating the bound for the functional $\mu(F)$ is nonparametrically estimated by $\hat\beta$
- Asymptotic linearity and regularity imply pathwise differentiability
- Condition (ii) implies that there is **only one influence function** and that it equals the pathwise derivative


---

- Theorem 2.1 give a justification for the pathwise derivative formula, rather than an approach to showing asymptotic normality. 
- A better approach:
  - Solve equation $(\star)$ for the pathwise derivative, as a candidate for the influence function
  - Formulate regularity conditions for the remainder $\sqrt{n}\left(\hat{\theta}-\theta_0\right)-\sum_{i=1}^n \psi\left(z_i\right) / \sqrt{n}$ to be small. 
- The formula is a very important part of this approach, because it provides the form of the remainder. 
- This approach, with formal calculation followed by regularity conditions, is similar to that used in parametric asymptotic theory (e.g. for Edgeworth expansions).

## 3. Semiparametric $M$-estimators

- Let $h$ denote a function, that can depend on the parameters $\beta$ and the data $z$. 
- Let $m(z, \beta, h)$ be a vector of functions with the same dimension as $\beta$. Here $m(z, \beta, h)$ can depend on the entire function $h$, rather than just its value at particular points, so $m(z, \beta, h)$ is a vector of functionals. 
- Suppose that $E\left[m\left(z, \beta_0, h_0\right)\right]=0$ for the true values $\beta_0$ and $h_0$. 
- Let $\hat{h}$ denote an estimator of $h$. A semiparametric $m$-estimator is one that solves a moment equation of the form
$$
{\color{red}\frac{1}{n}\sum_{i=1}^n m\left(z_i, \beta, \hat{h}\right)=0} .\qquad (\Delta)
$$
- The general idea here is that $\hat{\beta}$ is obtained by a procedure that "plugs-in" an estimated function $\hat{h}$.


<!-- {{< spoiler text="Example 3.1 Quasi-maximum Likelihood for a Conditional Mean Index">}} -->
<!-- {{< /spoiler >}} -->
{{< spoiler text="**Example 3.1 Quasi-maximum Likelihood for a Conditional Mean Index**">}}
- The conditional mean index model: $E[y \mid x]=\tau\left(v\left(x, \beta_0\right)\right)$ for a **known** function $v(x, \beta)$ and an **unknown** function $\tau(\cdot)$. 
- Let $\hat{h}(x, \beta)$ be a nonparametric estimator of $E[y \mid v(x, \beta)]$, such as a kernel estimator. 
> - An estimator of $\beta_0$ suggested by Ichimura (1993) minimizes $\sum_{i=1}^n\left[y_i-\hat{h}\left(x_i, \beta\right)\right]^2$. 
> - When $y_i$ is binary, Klein and Spady (1993) have suggested maximizing 
> $$\sum_{i=1}^n\left\\{y_i \ln [\hat{h}(x_i, \beta)]+(1-y_i) \ln [1-\hat{h}(x_i, \beta)]\right\\}$$ 
> - A generalization of these estimators is a quasi-maximum likelihood estimator (QMLE) for an exponential family. 
> - The estimator will be efficient when the true distribution has the exponential form.
- To describe the estimator, let $l(u, \nu)=\exp (A(\nu)+B(u)+C(\nu) u)$ be a linear exponential density, with mean $\nu$. 
- Consider an estimator $\hat{\beta}$ that maximizes $\sum_{i=1}^n \ln l\left(y_i, \hat{h}\left(x_i, \beta\right)\right)$.
- The first order conditions for this estimator make it a special case of equation $(\Delta)$, with
$$
m(z, \beta, h)=\left[A_\nu(h(x, \beta))+C_\nu(h(x, \beta)) y\right] \frac{\partial h(x, \beta) } {\partial \beta}
$$
{{< /spoiler >}}

 
{{< spoiler text="**Example 3.2: Inverse Density Weighted Least Squares**">}}
- Let $w(x)=r((x-$ $\zeta)^{\prime} \Omega(x-\zeta)$ ) be an elliptically symmetric density function, where $\zeta$ is a vector and $\Omega$ a positive definite matrix. 
- Let $\hat{h}\left(x_l\right)$ be an estimator of the density of $x$, such as a kernel estimator. 
- As shown by Ruud (1986), the weighted least squares estimator $\hat{\beta}=\left[\sum_{l=1}^n w\left(x_i\right) \hat{h}\left(x_l\right)^{-1} x_l x_l^{\prime}\right]^{-1} \sum_{l=1}^n w\left(x_l\right) \hat{h}\left(x_l\right)^{-1} x_i y_i$ will be consistent up to scale, for the coefficients $\gamma_0$ of an index model $E[y \mid x]=\tau\left(x^{\prime} \gamma_0\right)$. 
- This example is a special case of equation $(\Delta)$ with
$$
m(z, \beta, h)=w(x) h(x)^{-1} x\left(y-x^{\prime} \beta\right) .
$$
- This estimator will be used to illustrate the correction term for density estimates. 
- Asymptotic normality and $\sqrt{n}$-consistency for $\hat{h}(x)$ a kernel estimator, are shown in Newey and Ruud (1991).
{{< /spoiler >}}

--- 

To use the pathwise derivative formula in this derivation, it is necessary to <mark>identify the functional that is nonparametrically estimated by $\hat{\beta}$.</mark> 
- Let $h(F)$ denote the limit of $\hat{h}$ when $z$ has distribution $F$. 
- The limit $\mu(F)$ of $\hat{\beta}$ for a general $F$ should be the solution to
$$
E_F[m(z, \mu, h(F))]=0 .\qquad(\odot)
$$
- Equation $(\Delta)$ sets $\hat{\beta}$ so that sample moments are zero, and the sample moments have a limit of $E_F[m(z, \beta, h(F))]$ 
  - by the law of large numbers and $h(F)$ equal to the limit of $\hat{h}$
- <font color="red">$\Rightarrow$ $\hat{\beta}$ is consistent for the solution of $(\odot)$.</font>


**Remarks**

- the estimators depend only on the limit $h(F)$, and not on the particular form of the estimator $\hat{h}$. 
- Different nonparametric estimators of the same functions should result in <mark>the same asymptotic variance</mark>.


<mark>**Proposition 1**</mark>: <font color="blue">The asymptotic variance of semiparametric estimators depends only on the function that is nonparametrically estimated, and not on the type of estimator.</font>

---
- For a path $\left\\{F_\theta\right\\}$, let $h(\theta)=h\left(F_\theta\right)$. Here, $\mu\left(F_\theta\right)$ will satisfy the population moment equation
$$
E_\theta[m(z, \mu, h(\theta))]=0 .
$$
- Let $m(z, h)=m\left(z, \beta_0, h\right)$. Differentiation under the integral gives
$$
\frac{\partial E_\theta\left[m\left(z, h_0\right)\right]}{ \partial \theta}=\int m\left(z, h_0\right)\left[\frac{\partial d F_\theta }{ \partial \theta}\right] d z=E\left[m\left(z, h_0\right) S(z)\right] .
$$
- Using the chain rule, it follows that
$$
\frac{\partial E_\theta[m(z, h(\theta))] }{ \partial \theta}=E\left[m\left(z, h_0\right) S(z)\right]+\frac{\partial E[m(z, h(\theta))] }{ \partial \theta} .
$$
- Assuming $M \equiv \partial E\left[m\left(z, \beta, h_0\right)\right] /\left.\partial \beta\right|\_{\beta_0}$ is nonsingular, by the implicit function theorem
$$
\frac{\partial \mu\left(F_\theta\right) }{ \partial \theta}=-M^{-1}\left\\{E\left[m\left(z, h_0\right) S(z)\right]+\frac{\partial E[m(z, h(\theta))] }{ \partial \theta}\right\\} .
$$
- The first term is already in an outer product form, so that the pathwise derivative can be found by <mark>putting the second term in singular form</mark>
- Suppose there is a $\alpha(z)$ such that $E[\alpha(z)]=0$ and 
$$\frac{\partial E[m(z, h(\theta))] }{ \partial \theta}=E[\alpha(z) S(z)]\qquad(\oplus)$$.
- Move $-M^{-1}$ inside the expectation, it follows that the pathwise derivative is <font color="red">$d(z)=-M^{-1}\left\\{m\left(z, h_0\right)+\alpha(z)\right\\}$</font>
- By Theorem 2.1 the influence function of $\hat{\beta}$ is
$$
\psi(z)=-M^{-1}\left\\{m\left(z, \beta_0, h_0\right)+\alpha(z)\right\\}
$$
**Remarks**
- The leading term $-M^{-1} m\left(z, \beta_0, h_0\right)$ is the usual formula for the influence function of an $m$-estimator with moment functions $m\left(z, \beta, {\color{red}h_0}\right)$. 
- The solution to equation $(\oplus)$ is an adjustment term for the estimation of $h_0$. 
- Solving equation $(\oplus)$ is therefore the essential step in discovering <mark>how the estimation of $h$ affects the asymptotic variance</mark>. 
- This solution can be interpreted as 
  - the pathwise derivative of the functional $-M^{-1} E\left[m\left(z, \beta_0, h(F)\right)\right]$
  - the influence function of $-M^{-1} \int m\left(z, \beta_0, \hat{h}\right) d F_0(z)$.

### When will the adjustment term be zero ?

- If the adjustment term is zero, then it should not be necessary to account for the presence of $\hat{h}$, i.e. <font color="red">$\hat{h}$ can be treated as if it were equal to $h_0$</font>
- One case: equation $(\Delta)$ is <mark>the first-order condition</mark> to a maximization problem, and $\hat{h}$ has a limit that <mark>maximizes the population value of the same function</mark>.

**To be specific**, suppose that there is a function $q(z, \beta, h)$ and a set of functions $\mathscr{H}(\beta)$, possibly depending on $\beta$ but not on the distribution $F$ of $z$, such that
$$
\begin{aligned}
& m(z, \beta, h)=\partial q(z, \beta, h) / \partial \beta, \\\
& h(F)=\operatorname{argmax}_{\tilde{h} \in \mathscr{H}(\beta)} E_F[q(z, \beta, \tilde{h})] .\qquad(\otimes)
\end{aligned}
$$
- $m(z, \beta, h)$ are the first order conditions for a maximum of the function $q$
- $h(F)$ maximizes the expected value of the same function
  - i.e. $h(F)$ has been "concentrated out".

For any parametric model $F_\theta$, since $h(\theta)=h\left(F_\theta\right)$ $\Rightarrow$ $E[q(z, \beta, h(\theta))]$ is maximized at $\theta=0$ $\Rightarrow$ $\partial E[q(z, \beta, h(\theta))] / \partial \theta=0$. Differentiating again with respect to $\beta$,
$$
\begin{aligned}
0 & =\partial^2 E[q(z, \beta, h(\theta))] / \partial \theta \partial \beta\\\
&=\partial E[\partial q(z, \beta, h(\theta)) / \partial \beta] / \partial \theta \\\
& =\partial E[m(z, \beta, h(\theta))] / \partial \theta .
\end{aligned}
$$
- Evaluating this equation at $\beta_0$, it follows that $\alpha(z)=0$ will solve equation $(\oplus)$, and hence the adjustment term is zero. 


**Proposition 2**: <font color="blue">If equation $(\otimes)$ is satisfied, then the estimation of $h$ can be ignored in calculating the asymptotic variance, i.e. it is the same as if $\hat{h}=h_0$</font>


### A more direct condition

- Suppose that $m(z, h)$ depends on $h$ only through its value $h(v)$ at a subvector $v$ of $z$, i.e. $m(z, h)=m(z, h(v))$ where the last function depends on a real vector argument in $m(z, h)$. 
- Let $h(v, \theta)$ denote the limiting value of $\hat{h}\left(v, \beta_0\right)$ for a path. For $D(z)=\partial m\left(z, \beta_0, h\right) /\left.\partial h\right|_{h=h_0(v)}$, differentiation gives
$$
\frac{\partial E[m(z, h(\theta))] }{ \partial \theta}=E\left[D(z) \frac{\partial h(v, \theta) }{ \partial \theta}\right]=\frac{\partial E[D(z) h(v, \theta)] }{ \partial \theta} .
$$
- If this derivative is zero for all $h(v, \theta)$, then $\alpha(z)=0$ will solve equation $(\oplus)$, and the adjustment term is zero.
- **One simple condition** for this is that <mark>$E[D(z) \mid v]=0$</mark>. 

**More generally, the adjustment term will be zero if $h(v, \theta)$ is an element of a set to which $D(z)$ is orthogonal.**

**Proposition 3**: <font color="blue">If $E[D(z) \mid v]=0$, or more generally, for all $F, h(v, F)$ is an element of a set $\mathscr{H}$ such that $E[D(z) \tilde{h}(v)]=0$ for all $\tilde{h} \in \mathscr{H}$, then estimation of $h$ can be ignored in calculating the asymptotic variance.</font>

**Remarks**:

This condition can be checked by straightforward calculation, unlike Proposition 2, which requires finding $q(z, \beta, h)$ satisfying equation $(\otimes)$.

## 4. Functions of Mean-square Projection and Densities


## 5. Regularity Conditions


## 6. Series Estimation of Projection Functionals


## 7. Power Series Estimators For Semiparametric Individual Effects and Average Derivatives

---

## References

1. KOSHEVNIK, Y. A., AND B. Y. LEVIT (1976): "[On a Non-parametric Analogue of the Information Matrix](https://www.mathnet.ru/php/archive.phtml?wshow=paper&jrnid=tvp&paperid=3421&option_lang=eng)", Theory of Probability and Applications, 21, 738-753.
2. PFANZAGL, J., AND WEFELMEYER (1982): "[Contributions to a General Asymptotic Statistical Theory](https://www.degruyter.com/document/doi/10.1524/strm.1985.3.34.379/html)", New York: Springer-Verlag.



