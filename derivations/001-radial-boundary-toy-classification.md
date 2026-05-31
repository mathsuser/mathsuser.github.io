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
    <p>
      What is the probability that a random point \(X=(X_1,X_2)\), distributed according to a distribution \(P_X\) centered at the origin, lies within the Euclidean ball
      \[
      \mathcal B_2(0,r)
      =
      \{x\in\mathbb R^2:\|x\|_2\le r\}
      \]
      in the following cases?
    </p>

    <ol type="a">
      <li>
        \(P_X\) is the uniform distribution on the square \([-a,a]^2\), with independent coordinates, for some \(a>r\ge 0\).
      </li>
      <li>
        \(P_X\) is the bivariate normal distribution \(\mathcal N(0,\sigma^2 I_2)\), for some \(\sigma>0\).
      </li>
    </ol>
  </li>

  <li>
    <p>
      Consider the following binary classification model. Let
      \[
      f(X)=\mathbf 1_{\{X\in \mathcal B_2(0,r)\}}.
      \]
      The observed label \(Y\) is defined by
      \[
      Y =
      \begin{cases}
      f(X), & \text{with probability } p,\\
      1-f(X), & \text{with probability } 1-p.
      \end{cases}
      \]
    </p>

    <ol type="a">
      <li>
        What is the probability that \(Y=0\) in this model for the cases considered in Question 1?
      </li>
      <li>
        What is the Bayes optimal classifier for this model under \(0\)-\(1\) loss, and what is the corresponding Bayes risk?
      </li>
    </ol>
  </li>
</ol>

## Setup

Let \(X=(X_1,X_2)\) be a random vector in \(\mathbb R^2\) with distribution \(P_X\). We are interested in the probability that \(X\) lies within the Euclidean ball of radius \(r\) centered at the origin,

\[
\mathcal B_2(0,r)
=
\{x\in\mathbb R^2:\|x\|_2^2=x_1^2+x_2^2\le r^2\}.
\]

Write

\[
q:=\mathbb P(X\in \mathcal B_2(0,r)).
\]

The first part of the note computes \(q\) for two choices of \(P_X\). The second part uses this quantity to compute \(\mathbb P(Y=0)\) under the noisy binary classification model.

## Derivation

### 1. Probability of the ball event

<ol type="a">
  <li>
    <p><strong>Uniform distribution on \([-a,a]^2\).</strong></p>

    <p>
      Assume \(X_1\) and \(X_2\) are independent and uniformly distributed on \([-a,a]\), with \(a>r\ge 0\). Then the joint density of \(X\) is
    </p>

    \[
    f_X(x_1,x_2)
    =
    \frac{1}{4a^2},
    \qquad
    (x_1,x_2)\in[-a,a]^2.
    \]

    <p>
      Since \(r\le a\), the ball \(\mathcal B_2(0,r)\) is contained in the square \([-a,a]^2\). Hence
    </p>

    \[
    \begin{aligned}
    q
    &=
    \mathbb P(X\in\mathcal B_2(0,r))\\
    &=
    \int_{\mathcal B_2(0,r)}
    \frac{1}{4a^2}\,dx_1dx_2\\
    &=
    \int_0^{2\pi}\int_0^r
    \frac{1}{4a^2}\rho\,d\rho d\theta\\
    &=
    \frac{\pi r^2}{4a^2}.
    \end{aligned}
    \]

    <p>
      Geometrically, this is the ratio between the area of the disk of radius \(r\) and the area of the square:
    </p>

    \[
    q
    =
    \frac{\lambda(\mathcal B_2(0,r))}
    {\lambda([-a,a]^2)}
    =
    \frac{\pi r^2}{4a^2}.
    \]
  </li>

  <li>
    <p><strong>Bivariate normal distribution \(\mathcal N(0,\sigma^2 I_2)\).</strong></p>

    <p>
      Let \(X\sim\mathcal N(0,\sigma^2 I_2)\). Then
    </p>

    \[
    \frac{\|X\|_2^2}{\sigma^2}
    =
    \frac{X_1^2+X_2^2}{\sigma^2}
    \sim \chi^2_2.
    \]

    <p>
      Therefore
    </p>

    \[
    q
    =
    \mathbb P(\|X\|_2\le r)
    =
    \mathbb P\left(
    \frac{\|X\|_2^2}{\sigma^2}
    \le
    \frac{r^2}{\sigma^2}
    \right)
    =
    F_{\chi^2_2}\left(\frac{r^2}{\sigma^2}\right).
    \]

    <p>
      Since a \(\chi^2_2\) random variable has distribution function
    </p>

    \[
    F_{\chi^2_2}(t)=1-e^{-t/2},
    \qquad t\ge 0,
    \]

    <p>
      we obtain
    </p>

    \[
    q
    =
    1-\exp\left(-\frac{r^2}{2\sigma^2}\right).
    \]

    <p>
      Equivalently, using polar coordinates and the Gaussian density,
    </p>

    \[
    f_X(x_1,x_2)
    =
    \frac{1}{2\pi\sigma^2}
    \exp\left(
    -\frac{x_1^2+x_2^2}{2\sigma^2}
    \right),
    \]

    <p>
      gives
    </p>

    \[
    \begin{aligned}
    q
    &=
    \int_{\mathcal B_2(0,r)} f_X(x_1,x_2)\,dx_1dx_2\\
    &=
    \int_0^{2\pi}\int_0^r
    \frac{1}{2\pi\sigma^2}
    \exp\left(-\frac{\rho^2}{2\sigma^2}\right)
    \rho\,d\rho d\theta\\
    &=
    1-\exp\left(-\frac{r^2}{2\sigma^2}\right).
    \end{aligned}
    \]
  </li>
</ol>

### 2. Noisy classification model

The noisy label model is

\[
Y =
\begin{cases}
f(X), & \text{with probability } p,\\
1-f(X), & \text{with probability } 1-p,
\end{cases}
\qquad
f(X)=\mathbf 1_{\{X\in\mathcal B_2(0,r)\}}.
\]

<ol type="a">
  <li>
    <p><strong>Probability that \(Y=0\).</strong></p>

    <p>
      Conditional on \(X=x\), we have
    </p>

    \[
    \mathbb P(Y=0\mid X=x)
    =
    p(1-f(x))+(1-p)f(x).
    \]

    <p>
      Taking expectation with respect to \(X\),
    </p>

    \[
    \begin{aligned}
    \mathbb P(Y=0)
    &=
    \mathbb E\big[\mathbb P(Y=0\mid X)\big]\\
    &=
    p\,\mathbb E[1-f(X)]
    +
    (1-p)\,\mathbb E[f(X)].
    \end{aligned}
    \]

    <p>
      Since
    </p>

    \[
    \mathbb E[f(X)]
    =
    \mathbb P(X\in\mathcal B_2(0,r))
    =
    q,
    \]

    <p>
      it follows that
    </p>

    \[
    \mathbb P(Y=0)
    =
    p(1-q)+(1-p)q
    =
    p+(1-2p)q.
    \]

    <p>
      Therefore, in the uniform case,
    </p>

    \[
    \mathbb P(Y=0)
    =
    p+(1-2p)\frac{\pi r^2}{4a^2}.
    \]

    <p>
      In the Gaussian case,
    </p>

    \[
    \mathbb P(Y=0)
    =
    p+(1-2p)
    \left(
    1-e^{-r^2/(2\sigma^2)}
    \right).
    \]
  </li>

  <li>
    <p><strong>Bayes optimal classifier and Bayes risk.</strong></p>

    <p>
      To be completed.
    </p>
  </li>
</ol>