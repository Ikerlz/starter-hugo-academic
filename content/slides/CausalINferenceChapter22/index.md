---
title: Disentangle Mixture Distributions and Instrumental Variable Inequalities
summary: Chapter 22 in "A First Course in Causal Inference".
authors: [Zhe Li]
tags: []
categories: [Causal Inference]
date: '2024-01-01T00:00:00Z'
slides:
  # Choose a theme from https://github.com/hakimel/reveal.js#theming
  theme: black
  # Choose a code highlighting style (if highlighting enabled in `params.toml`)
  #   Light style: github. Dark style: dracula (default).
  highlight_style: dracula
---


{{< slide background-image="ZIBIN.jpg" >}}

# Disentangle Mixture Distributions and Instrumental Variable Inequalities

$$
\begin{aligned}
\\
\end{aligned}
$$

<center> Li Zhe <center>

$$$$

<center>School of Data Science, Fudan University <center> 




$$$$

<center>January 3, 2024<center>

---

{{< slide background-image="body1.jpg" >}}
## Introduction

- The IV model in last chapter imposes three assumptions

  - (<mark>randomization</mark>) $Z \perp\\!\\!\\!\perp\\{D(1), D(0), Y(1), Y(0)\\}$
  - (<mark>monotonicity</mark>) $\operatorname{pr}(U=\mathrm{d})=0$ or $D_i(1)\geq D_i(0)$
  - (<mark>exclusionrestriction</mark>) $Y(1)=Y(0) \text { for } U=\mathrm{a} \text { or } \mathrm{n}$

$$
{\tiny
U_i= \begin{cases}
a, & \text { if } D_i(1)=1 \text { and } D_i(0)=1 \\\ 
c, & \text { if } D_i(1)=1 \text { and } D_i(0)=0 \\\ 
d, & \text { if } D_i(1)=0 \text { and } D_i(0)=1 \\\ 
n, & \text { if } D_i(1)=0 \text { and } D_i(0)=0
\end{cases}
}
$$

- Observed groups and latent groups under the assumptions

$$
{\small
\begin{array}{cccl}
Z=1 & D=1 & D(1)=1 & U=\mathrm{c} \text { or a } \\\
Z=1 & D=0 & D(1)=0 & U=\mathrm{n} \\\
Z=0 & D=1 & D(0)=1 & U=\mathrm{a} \\\
Z=0 & D=0 & D(0)=0 & U=\mathrm{c} \text { or } \mathrm{n}
\end{array}
}
$$

- Interestingly, the assumptions have some <mark>**testable implications**</mark>. Balke and Pearl (1997) called them the <mark>**instrumental variable inequalities**</mark>.

---

{{< slide background-image="body1.jpg" >}}


### Outline


- Disentangle Mixture Distributions and Instrumental Variable Inequalities

- Testable implications

- Examples

---

{{< slide background-image="body1.jpg" >}}
### Disentangle Mixture Distributions and IV Inequalities

- Recall $\pi_u$ as the proportion of type $U=u$, and define

$$
\mu_{z u}=E\\{Y(z) \mid U=u\\}, \quad(z=0,1 ; u=\mathrm{a}, \mathrm{n}, \mathrm{c}) .
$$

<mark>**Theorem 22.1**</mark> Under the three assumptions, we can identify the proportions of the latent types by
$$
\begin{aligned}
& \pi_{\mathrm{n}}=\operatorname{pr}(D=0 \mid Z=1), \\\
& \pi_{\mathrm{a}}=\operatorname{pr}(D=1 \mid Z=0), \\\
& \pi_{\mathrm{c}}=E(D \mid Z=1)-E(D \mid Z=0),
\end{aligned}
$$
and the type-specific means of the potential outcomes by
$$
\begin{aligned}
\mu_{1 \mathrm{n}}=\mu_{0 \mathrm{n}} \equiv \mu_{\mathrm{n}} & =E(Y \mid Z=1, D=0) \\\
\mu_{1 \mathrm{a}}=\mu_{0 \mathrm{a}} \equiv \mu_{\mathrm{a}} & =E(Y \mid Z=0, D=1) \\\
\mu_{1 \mathrm{c}} & =\pi_{\mathrm{c}}^{-1}\\{E(D Y \mid Z=1)-E(D Y \mid Z=0)\\} \\\
\mu_{0 \mathrm{c}} & =\pi_{\mathrm{c}}^{-1}[E\\{(1-D) Y \mid Z=0\\}-E\\{(1-D) Y \mid Z=1\\}]
\end{aligned}
$$


---

{{< slide background-image="body1.jpg" >}}

### Proof of Theorem 22.1 (Part I)

- We can identify the proportion of the *never takers* by

$$
\begin{aligned}
\operatorname{pr}(D=0 \mid Z=1) & =\operatorname{pr}(U=\mathrm{n} \mid Z=1)=\operatorname{pr}(U=\mathrm{n})=\pi_{\mathrm{n}},
\end{aligned}
$$

- the proportion of the *always takers* by

$$
\begin{aligned}
\operatorname{pr}(D=1 \mid Z=0) & =\operatorname{pr}(U=\mathrm{a} \mid Z=0)=\operatorname{pr}(U=\mathrm{a})=\pi_{\mathrm{a}} .
\end{aligned}
$$

- the proportion of compliers is

$$
\begin{aligned}
\pi_{\mathrm{c}} & =\operatorname{pr}(U=\mathrm{c})=1-\pi_{\mathrm{n}}-\pi_{\mathrm{a}} \\\
& =1-\operatorname{pr}(D=0 \mid Z=1)-\operatorname{pr}(D=1 \mid Z=0) \\\
& =E(D \mid Z=1)-E(D \mid Z=0)=\tau_D,
\end{aligned}
$$

**Remark:** Although we do not know individual latent compliance types for all units, we can identify the proportions of never takers, always takers, and compliers.

---




{{< slide background-image="body1.jpg" >}}

### Proof of Theorem 22.1 (Part II)

Under the three assumptions, we have

$$
\mu_{1 \mathrm{a}}=\mu_{0 \mathrm{a}} \equiv \mu_{\mathrm{a}}, \quad \mu_{1 \mathrm{n}}=\mu_{0 \mathrm{n}} \equiv \mu_{\mathrm{n}} .
$$

- The observed group $(Z=1, D=0)$ only has never takers, so

$$
E(Y \mid Z=1, D=0)=E\{Y(1) \mid Z=1, U=\mathrm{n}\}=E\{Y(1) \mid U=\mathrm{n}\}=\mu_{\mathrm{n}} .
$$

- The observed group $(Z=0, D=1)$ only has always takers, so

$$
E(Y \mid Z=0, D=1)=E\{Y(0) \mid Z=0, U=\mathrm{a}\}=E\{Y(0) \mid U=\mathrm{a}\}=\mu_{\mathrm{a}}
$$


$$~~$$


<font color="red">How about the observed groups $(Z=1, D=1)$ and $(Z=0, D=0)$ ?</font>

---


{{< slide background-image="body1.jpg" >}}

#### The Observed Group $(Z=1, D=1)$

The observed group $(Z=1, D=1)$ has both *compliers* and *always takers*, so
$$
\small
\begin{aligned}
E(Y \mid Z=1, D=1)= & E\\{Y(1) \mid Z=1, D(1)=1\\} \\\
= & E\\{Y(1) \mid D(1)=1\\} \\\
= & \operatorname{pr}\\{D(0)=1 \mid D(1)=1\\} E\\{Y(1) \mid D(1)=1, D(0)=1\\} \\\
& +\operatorname{pr}\\{D(0)=0 \mid D(1)=1\\} E\\{Y(1) \mid D(1)=1, D(0)=0\\} \\\
= & \frac{\pi_{\mathrm{c}}}{\pi_{\mathrm{c}}+\pi_{\mathrm{a}}} \mu_{1 \mathrm{c}}+\frac{\pi_{\mathrm{a}}}{\pi_{\mathrm{c}}+\pi_{\mathrm{a}}} \mu_{\mathrm{a}} .
\end{aligned}
$$

Solve the linear equation above to obtain
$$
\small
\begin{aligned}
\mu_{1 \mathrm{c}}= & \pi_{\mathrm{c}}^{-1}\left\\{\left(\pi_{\mathrm{c}}+\pi_{\mathrm{a}}\right) E(Y \mid Z=1, D=1)-\pi_{\mathrm{a}} E(Y \mid Z=0, D=1)\right\\} \\\
= & \pi_{\mathrm{c}}^{-1}\\{\operatorname{pr}(D=1 \mid Z=1) E(Y \mid Z=1, D=1) \\\
& \quad-\operatorname{pr}(D=1 \mid Z=0) E(Y \mid Z=0, D=1)\\} \\\
= & \pi_{\mathrm{c}}^{-1}\\{E(D Y \mid Z=1)-E(D Y \mid Z=0)\\} .
\end{aligned}
$$

Based on the formulas of $\mu_{1 \mathrm{c}}$ and $\mu_{0 \mathrm{c}}$ in Theorem 22.1, we have

$$
\tau_{\mathrm{c}}=\mu_{1 \mathrm{c}}-\mu_{0 \mathrm{c}}=\{E(Y \mid Z=1)-E(Y \mid Z=0)\} / \pi_{\mathrm{c}}.
$$


---


{{< slide background-image="body1.jpg" >}}

## Testable implications

- Is there any additional value of the this detour for deriving the formula of $\tau_c$ ?
- For binary outcome, the following inequalities must be true:

$$
0 \leq \mu_{1 \mathrm{c}} \leq 1, \quad 0 \leq \mu_{0 \mathrm{c}} \leq 1,
$$

- It implies four inequalities

$$
\small
\begin{aligned}
E(D Y \mid Z=1)-E(D Y \mid Z=0) & \geq 0, \\\
E(D Y \mid Z=1)-E(D Y \mid Z=0) & \leq E(D \mid Z=1)-E(D \mid Z=0), \\\
E\\{(1-D) Y \mid Z=0\\}-E\\{(1-D) Y \mid Z=1\\} & \geq 0, \\\
E\\{(1-D) Y \mid Z=0\\}-E\\{(1-D) Y \mid Z=1\\} & \leq E(D \mid Z=1)-E(D \mid Z=0) .
\end{aligned}
$$

<mark>**Theorem 22.2 (Instrumental Variable Inequalities)**</mark> With a binary outcome $Y$, the three assumptions imply

$$
E(Q \mid Z=1)-E(Q \mid Z=0) \geq 0,
$$

where $Q=D Y, D(1-Y),(D-1) Y$ and $D+Y-D Y$.


---


{{< slide background-image="body1.jpg" >}}

### Remarks

- Under the three assumptions, the difference in means for $Q=$ $D Y, D(1-Y),(D-1) Y$ and $D+Y-D Y$ must all be non-negative. 
- Importantly, these implications only involve the distribution of the **observed** variables.
- Rejection of the IV inequalities leads to rejection of the IV assumptions.

- Balke and Pearl (1997) derived more general IV inequalities without assuming monotonicity.

---


{{< slide background-image="body1.jpg" >}}

### Eample 1.

- Investigators et al. (2014) assess the effectiveness of the *emergency endovascular* versus *the open surgical repair strategies* for patients with a clinical diagnosis of ruptured aortic aneurism.
- Patients are randomized to either the emergency endovascular or the open repair strategy.
- The primary outcome is the survival status after 30 days
- Let $Z$ be the treatment assigned, with $Z=1$ for the endovascular strategy and $Z=0$ for the open repair.
- Let $D$ be the treatment received.
- Let $Y$ be the survival status, with $Y=1$ for dead, and $Y=0$ for alive. 
- The estimate of $\tau_{\mathrm{c}}$ is 0.131 with $95 \\%$ confidence interval $(-0.036,0.298)$ including 0 . 
- Using the function "``IVbinary``" above, we can obtain $\hat\mu_{1c}=0.708$ and $\hat\mu_{0c}=0.629$
- There is no evidence of violating the IV assumptions.
---



{{< slide background-image="body1.jpg" >}}

### Example 2.

- In Hirano et al. (2000), physicians are randomly selected to receive a letter encouraging them to inoculate patients at risk for flu. 
- The treatment is the actual flu shot, and the outcome is an indicator for flu-related hospital visits. 
- However, some patients do not comply with their assignments. Let $Z_i$ be the indicator of encouragement to receive the flu shot, with $Z=1$ if the physician receives the encouragement letter, and $Z=0$ otherwise. 
- Let $D$ be the treatment received. 
- Let $Y$ be the outcome, with $Y=0$ if for a flu-related hospitalization during the winter, and $Y=1$ otherwise. 
- The estimate of $\tau_{\mathrm{c}}$ is 0.116 with $95 \%$ confidence interval $(-0.061,0.293)$ including 0 . 
- Using the function above, we can obtain $\hat\mu_{1c}=-0.0045$ and $\hat\mu_{0c}=0.1200$
- Since $\hat\mu_{1c}<0$, there is evidence of violating the IV assumptions.

