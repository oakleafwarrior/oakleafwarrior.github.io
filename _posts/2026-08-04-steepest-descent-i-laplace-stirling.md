---
title: "Steepest Descent I: Laplace's Method and Stirling's Formula"
# summary: "The first in a series of "
tags: [math, random matrix theory]
math: true
abstract: >
  A short note on the method of steepest descent for asymptotic evaluation of integrals.
---

## Introduction

In random matrix theory, we are often confronted with contour integrals of the form $\oiint e^{nf(z)}dz$ where we would like to take $N \to \infty$ to understand local correlation structure of eigenvalues.
Often the integrals are quite formidable, so in this note we will walk through how to solve integrals like these using the method of steepest descent.
This will be the first of a series of posts on using the method of steepest descent.
In this series, we will go through a few examples increasing in difficulty, starting with integrals on $\RR$ to double contour integrals on $\CC^2$. 
We will build up to a result on the correlation structure of the largest eigenvalues as $N \to \infty$.
It is quite a fun way of computing integrals and I hope to showcase that.

This post covers Laplace's method, the $\RR$ analog of steepest descent, and we prove Stirling's approximation.

Generally, the formula for steepest descent in $\RR$ is

$$
\begin{equation}
    \int_a^b e^{nf(x)}dx = e^{nf(x_0)} \sqrt{\frac{2\pi}{n|f''(x_0)|}}(1 + o(1)).
\label{eq:laplace_general}
\end{equation}
$$

and in $\CC$,

$$
\begin{equation}
    \int_\gamma e^{nf(z)}dz = e^{nf(x_0)} \sqrt{\frac{2\pi}{n|f''(z_0)}} e^{i\alpha}(1 + o(1))
\label{eq:steepest_descent_general}
\end{equation}
$$

and if the critical point is of order $m$, we get $n^{-1/(m+1)}$ instead of $n^{-1/2}$.

When applying steepest descent we generally follow these steps.

1. Express the desired formula as an integral.
2. Find an adequate $f$ to to transform the integrand into $\exp(nf(z))$.
3. Localize near the critical point of $f$ with only $o(1)$ loss.
4. Bound the tails.

While proving Stirling's formula, we will apply Equation $\eqref{eq:laplace_general}$.
In future posts, we will see the standard form of Equation $\eqref{eq:steepest_descent_general}$ applied for the LCLT, and the order $2$ critical point for edge limits of GUE.



## Stirling's Formula

**Theorem (Stirling's Approximation).** For $n$ large,

$$
\begin{equation}
    n! = \sqrt{2\pi n} \lp \frac{n}{e} \rp^n \lp 1 + o\lp 1 \rp \rp. 
    \label{eq:stirling}
\end{equation}
$$

Laplace's method is used when solving a real integral of the form $\int e^{nf(x)}dx$.
Heuristically, as $n \to \infty$, any contribution of $f(x)$ outside of a maximum, $x_0$ becomes vanishingly small, as we see in $\eqref{eq:laplace_general}$.
In the proof of $\eqref{eq:stirling}$ $f$ will be constructed from the Gamma function $\Gamma(x) = \int_0^\infty t^{x-1}e^{-t}dt$.
The figure below shows this concentration happening.

{% include figure.html
   src="/assets/img/posts/steepest-descent/fig01-laplace-concentration.png"
   caption="The integrand of $I(n)$ after normalising by its maximum, $e^{\,n(f(y)-f(1))}$ with $f(y)=\log y-y$. As $n$ grows the mass collapses onto the global maximum $y_0=1$ at scale $n^{-1/2}$ with Gaussian profile, $e^{-n(y-1)^2/2}$, from $f(y)=-1-\tfrac{(y-1)^2}{2}+O((y-1)^3)$. We see that the integrand is exponentially small outside of a fixed neighborhood of $y_0$. Claude helped generate the tikz."
   alt="Plot of the normalized Laplace integrand concentrating at y=1 as n grows from 5 to 100, compared against its Gaussian approximation."
   width="85%" %}

**Proof.**

**Step 1:**
We take for granted that $\Gamma(n+1) = n!$.
We want to find the $f$, so that

$$
n! = \Gamma(n+1) = \int_0^\infty x^{n}e^{-x}dx.
$$

$x^{n}e^{-x} = e^{n\log(x)-x}$.

**Step 2:**
We will want to change variables $x = ny$ so that $n\log(x)-x = n \log(ny) - ny = n (\log (n) + \log(y) - y)$.
Because we are interested in a maximum of $f$, we can ignore the $n \log(n)$ and our candidate is $f(y) = \log(y) - y$.
The integral we study is thus,

$$
I(n) = \int_0^\infty (yn)^{n}e^{-ny}ndy = n^{n+1} \int_0^\infty e^{n (\log(y) - y)}dy.
$$

We note $f'(y) = \tfrac{1}{y} - 1$ and $f''(y) = - \tfrac{1}{y^2}$, so $f$ has a global maximum at $y_0 = 1$.

We Taylor expand $f$:

$$
f(y) = -1 - \frac{(y-1)^2}{2} + O \lp (y-1)^3\rp.
$$

**Step 2:**
Let $\vep > 0$.
We split the integral up

$$
\begin{align}
    I(n) &= 
    \underbrace{n^{n}\int_{1-\vep}^{1+\vep} e^{\lp n \lp -1 - \frac{(y-1)^2}{2} + O \lp (y-1)^3 \rp \rp \rp}dy}_{\text{The mass of the integral:}\ I_1(n)} + \\
    &\underbrace{n^{n}\int_{0}^{1-\vep} e^{n(\log y - y)} dy}_{\text{Negligble as }n \to \infty: \ I_2(n)} + \underbrace{n^{n}\int_{1 + \vep}^\infty e^{n(\log y - y)} dy}_{\text{Negligible as } n \to \infty: \ I_3(n)}.
\label{eq:int_split}
\end{align}
$$

The figure below shows this split and the two scales that make it work.

{% include figure.html
   src="/assets/img/posts/steepest-descent/fig02-laplace-split.png"
   caption="The decomposition of the integral. The left shows each window. $I_1$ in $[1-\vep,1+\vep]$ and carrying the mass is shaded while the tails $I_2,I_3$ are in grey. The exponent $\vep=n^{-2/5}$ is chosen to sit between the two scales. The cubic error $nO(\vep^3)=O(n^{-1/5})$ decays, but $\vep$ outpaces the Gaussian width of $n^{-1/2}$. On the right, after the substitution $y=1+z\sqrt{n}$, the window becomes $\vert z \vert<n^{1/10}$, which has growth just fast enough to make the tail negligble. Claude helped generate the tikz."
   alt="Two panels: the split of the Laplace integral into center and tail regions at n=40, and the same window rescaled to show the Gaussian profile."
   width="95%" %}

The first term, under the substitution  $y = 1 + \frac{z}{\sqrt{n}}$ is

$$
   I_1(n) = n^{n+1}\frac{1}{\sqrt{n}}\int_{-\vep \sqrt{n}}^{\vep \sqrt{n}} e^{-n - \frac{z^2}{2} + n O(\vep^3)}dz.
$$

We need to take $\vep \to 0$, and we have $O(\vep^3)$ in the exponent, so lets set $\vep = n^{-2/5}$.
We make this choice in order that $n \vep^3 = n^{-1/5} \to 0$ as $n \to \infty$.
Then,

$$
    I_1(n) = \sqrt{n} \lp \frac{n}{e} \rp^n \lp \int_{-n^{1/10}}^{n^{1/10}} e^{-\frac{z^2}{2}} dz + o(1) \rp.
$$

Using the Gaussian integral we get the desired approximation

$$
\begin{equation}
    I_1(n) = \sqrt{2\pi n} \lp \frac{n}{e} \rp^n \lp 1 + o(1) \rp.
\label{eq:stirling_center}
\end{equation}
$$

**Step 4:**
However, we must also show that $I_2(n)$ and $I_3(n) \to 0$.
To do so, we use the fact that $f(1) = -1$ is a global maximum and $f(y)$ is strictly increasing on $(0,1)$ and strictly decreasing on $(1, \infty)$.
Set $\delta = \min(-1-f(1-\vep), -1-f(1+\vep)) > 0$ so that $f(y) < -1 - \delta$ for all $y \in (0,1 - \vep) \cup (1 + \vep,\infty)$.
Note that,

$$
    e^{nf(y)} = e^{f(y)} \cdot e^{(n-1)f(y)} \leq e^{f(y)} \cdot e^{(n-1)(-1-\delta)}.
$$

So,

$$
    I_2(n) \leq \int_{0}^{1-\vep} e^{f(y)} \cdot e^{(n-1)(-1-\delta)} dy = e^{(n-1)(-1-\delta)} \int_0^{1-\vep} ye^{-y} dy \leq e^{(n-1)(-1-\delta)}
$$

where we have used the fact that $\int_0^\infty ye^{-y} dy = 1$ and the integrand is strictly positive.
Similarly, $I_3(n) \leq e^{(n-1)(-1-\delta)}$.
Now, we must achieve a bound solely in $n$.
Using the Taylor expansion of $\log(1+\vep)$

$$
    -1 - f(1+\vep) = \vep - \log(1+\vep) = \frac{\vep^2}{2} - \frac{\vep^3}{3} + \cdots
$$

whereas,

$$
    -1 - f(1-\vep) = -\vep - \log(1-\vep) = \frac{\vep^2}{2} + \frac{\vep^3}{3} + \cdots
$$

so actually $\delta = \vep - \log(1+\vep)$.
We then bound, $g(t) = t - \log(1+t)$.

$$
    t - \log(1+t) = t - \int_0^t \frac{1}{1+u} du = \int_0^t \frac{u}{1+u} du.
$$

Since $\frac{u}{1+u} \leq u$, $t - \log(1+t) \leq \frac{t^2}{2}$.
Additionally, since $\frac{u}{1+u} \geq \frac{u}{1+t}$, $t - \log(1+t) \geq \frac{t^2}{2(1+t)}$.
Plugging in $\vep = n^{-2/5}$ we have, $\delta \geq \frac{n^{-4/5}}{4}$.
So,

$$
    I_2(n) \leq e^{(n-1)(-1 - n^{-4/5}/4)} \leq e \lp \frac{n}{e} \rp^n \cdot e^{-n^{1/5}/4} \cdot e^{-n^{-4/5}/4}
$$

and similarly with $I_3(n)$.
These both can be absorbed into the the $o(1)$ term in Equation $\eqref{eq:stirling_center}$ and we are done. $\blacksquare$
