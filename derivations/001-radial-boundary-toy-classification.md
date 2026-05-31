---
title: "001 — A Radial Decision Boundary for a Toy Classification Model"
date: 2026-05-31
---

[← Back to derivations](../derivations.md)

*31 May 2026*

A placeholder file for derivations and notes on the radial-boundary toy classification problem.


## Questions

<ol>
	<li>
		What is the probability that a random point $(X=(X_1,X_2))$ distributed according to a distribution $P_X$ centered at the origin lies within the Euclidean ball of radius $r$ ($\mathcal B_2(0,r)$) centered at the origin in the following cases?
		<ol type="a">
			<li>When $P_X$ is the uniform distribution on the square $[-a,a]^2$ with independent coordinates for some $a > r \ge 0$.</li>
			<li>When $P_X$ is the bivariate normal distribution with mean zero and covariance matrix $\sigma^2 I_2$ for some $\sigma > r \ge 0$.</li>
		</ol>
	</li>
	<li>
		We consider the following binary classification model: we assign a label $Y$ to the random vector $X$ based on whether $X$ lies within the ball $\mathcal B_2(0,r)$ or not with a probability $p>0$. More precisely, we define $Y$ as follows:

$$Y = \begin{cases}
f(X) & \text{with probability } p  \\
1 - f(X) & \text{with probability } 1-p
\end{cases}$$

where $f(X) = \mathbf{1}_{X \in \mathcal B_2(0,r)}$ is the indicator function that takes the value 1 if $X$ lies within the ball $\mathcal B_2(0,r)$ and 0 otherwise.

<ol type="a">
	<li>What is the probability that $Y=0$ in this model for the cases considered in question 1?</li>
	<li>What is the Bayes optimal classifier for this model and what is the corresponding Bayes risk, under the 0-1 loss?</li>
</ol>
	</li>
</ol>


## Setup

Let $X=(X_1,X_2)$ be a random vector in $\mathbb{R}^2$ with distribution $P_X$. We are interested in the probability that $X$ lies within the Euclidean ball of radius \(r\) centered at the origin, denoted by $\mathcal B_2(0,r) = \{x \in \mathbb{R}^2 : \|x\|_2^2 =  x_1^2 + x_2^2 \le r^2\}$. Let $Y$ be a binary random variable defined as
$$Y = \begin{cases}
0 & \text{if } X \in \mathcal B_2(0,r) \\
1 & \text{otherwise}
\end{cases}$$

We want to compute $\mathbb{P}(Y=0)$. This is equivalent to computing $\mathbb{P}(X \in \mathcal B_2(0,r))$.

## Derivation

### Noiseless model

The following two cases assume the noiseless setting where the label is the indicator of membership in the ball: $Y = \mathbf{1}_{X \in \mathcal B_2(0,r)}$.

<ol type="a">
    <li>Uniform distribution on the square $[-a,a]^2$ with independent coordinates for some $a > r \ge 0$.</li>

Since $X_1$ and $X_2$ are independent and uniformly distributed on $[-a,a]$, the joint density function of $X$ is given by
$$f_X(x_1,x_2) = \frac{1}{4a^2} \quad \text{for } (x_1,x_2) \in [-a,a]^2$$
and $f_X(x_1,x_2) = 0$ otherwise.
The probability that $X$ lies within the ball $\mathcal B_2(0,r)$ is given by the integral of the density function over the region defined by the ball:
$$\begin{align*}
\mathbb{P}(Y=0) &= \int_{\mathcal B_2(0,r)} f_X(x_1,x_2) \, dx_1 dx_2 \\
&= \int_{x_1=-a}^{a} \int_{x_2=-a}^{a} \frac{1}{4a^2} \mathbf{1}_{x_1^2 + x_2^2 \le r^2} \, dx_1 dx_2 \\
&= \int_0^{2\pi} \int_0^r \frac{1}{4a^2} \rho \, d\rho d\theta \\
&= \frac{\pi r^2}{4a^2}.
\end{align*}$$


**Geometric interpretation:** The probability $\mathbb{P}(Y=0)$ can be interpreted as the ratio of the area of the circle defined by the ball $\mathcal B_2(0,r)$ to the area of the square $[-a,a]^2$. The area of the circle is $\pi r^2$ and the area of the square is $4a^2$, hence the probability is $\frac{\pi r^2}{4a^2}$. More precisely, 
$$\begin{align*}
\mathbb{P}(Y=0) &= \frac{\lambda(\mathcal B_2(0,r) \cap [-a,a]^2)}{\lambda([-a,a]^2)} \\
&= \frac{\pi r^2}{4a^2},
\end{align*}$$
where $\lambda$ denotes the Lebesgue measure on $\mathbb{R}^2$.

</li>

<li> Bivariate normal distribution $\mathcal{N}(0,\sigma^2 I_2)$

Let $X \sim \mathcal{N}(0, \sigma^2 I_2)$. The probability that $X$ lies within the ball $\mathcal B_2(0,r)$ is given by
$$\mathbb{P}(Y=0) = \mathbb{P}(\|X\|_2 \le r) = \mathbb{P}\left(\frac{\|X\|_2^2}{\sigma^2} \le \frac{r^2}{\sigma^2}\right).$$
Since $\frac{\|X\|_2^2}{\sigma^2} = \frac{X_1^2 + X_2^2}{\sigma^2}$ follows a chi-squared distribution with 2 degrees of freedom, we have
$$\mathbb{P}(Y=0) = F_{\chi^2_2}\left(\frac{r^2}{\sigma^2}\right),$$
where $F_{\chi^2_2}$ is the cumulative distribution function of the chi-squared distribution with 2 degrees of freedom.

The other calculation approach is the same as in case 1, but with the density function of the bivariate normal distribution:
$$f_X(x_1,x_2) = \frac{1}{2\pi\sigma^2} \exp\left(-\frac{x_1^2 + x_2^2}{2\sigma^2}\right).$$
The probability can be computed as
$$\begin{align*}
\mathbb{P}(Y=0) &= \int_{\mathcal B_2(0,r)} f_X(x_1,x_2) \, dx_1 dx_2 \\
&= \int_0^{2\pi} \int_0^r \frac{1}{2\pi\sigma^2} \exp\left(-\frac{\rho^2}{2\sigma^2}\right) \rho \, d\rho d\theta \\
&= 1 - \exp\left(-\frac{r^2}{2\sigma^2}\right).
\end{align*}$$

</li>
</ol>

### Noisy model — label noise p

The following assumes the noisy label model described in Question 2: with probability $p$ the label equals $f(X)=\mathbf{1}_{X\in\mathcal B_2(0,r)}$, and with probability $1-p$ it is flipped.

<ol type="a">
<li>Probability that $Y=0$.

Recall the noiseless (true) label is $Y_0=0$ when $X\in\mathcal B_2(0,r)$ and $Y_0=1$ otherwise. Writing $f(x)=\mathbf{1}_{x\in\mathcal B_2(0,r)}$, we have $Y_0=1-f(x)$. The observed label equals the true label with probability $p$ and is flipped with probability $1-p$. Hence
$$\mathbb{P}(Y=0 \mid X=x) = p(1-f(x)) + (1-p)f(x).$$

Integrating over $X$ gives
$$\begin{aligned}
\mathbb{P}(Y=0) &= \int_{\mathbb{R}^2} \mathbb{P}(Y=0 \mid X=x) f_X(x) \, dx \\
&= p\int (1-f(x))f_X(x) \,dx + (1-p)\int f(x)f_X(x) \,dx.
\end{aligned}$$

Let $q=\mathbb{P}(X\in\mathcal B_2(0,r))$. Then the above simplifies to
$$\mathbb{P}(Y=0) = p(1-q) + (1-p)q = p + (1-2p)q.$$

For the uniform case $q=\pi r^2/(4a^2)$, so
$$\mathbb{P}(Y=0) = p + (1-2p)\frac{\pi r^2}{4a^2}.$$

For the Gaussian case $q=1-\exp(-r^2/(2\sigma^2))$, so
$$\mathbb{P}(Y=0) = p + (1-2p)\left(1- e^{-r^2/(2\sigma^2)}\right).$$

</li>
<li>Bayes optimal classifier and Bayes risk.

[To be completed.]

</li>
</ol>
