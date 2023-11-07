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
- This equation also specifies that $\mu(F)$ is the limit of $\hat{\beta}$ when $z_i$ has distribution $F$. 
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
{\color{red}\frac{\partial \mu\left(F_\theta\right)}{\partial \theta}=E[d(z) S(z)].}
$$

- The variance bound for estimation of $\mu(F)$ is $\operatorname{Var}(d(z))$. 
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

## 3. Semiparametric $M$-estimators


## 4. Functions of Mean-square Projection and Densities


## 5. Regularity Conditions


## 6. Series Estimation of Projection Functionals


## 7. Power Series Estimators For Semiparametric Individual Effects and Average Derivatives

---

## References

1. KOSHEVNIK, Y. A., AND B. Y. LEVIT (1976): "On a Non-parametric Analogue of the Information Matrix", Theory of Probability and Applications, 21, 738-753.
2. PFANZAGL, J., AND WEFELMEYER (1982): "Contributions to a General Asymptotic Statistical Theory", New York: Springer-Verlag.



