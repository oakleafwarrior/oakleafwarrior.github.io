---
title: "Steepest Descent II: The Local Central Limit Theorem"
tags: [math, random matrix theory]
math: true
abstract: >
  The second in a series on the method of steepest descent: we move from Laplace's method on $\mathbb{R}$ to a contour integral in $\mathbb{C}$ and prove the local central limit theorem for the binomial distribution.
---

## Introduction

In [Steepest Descent I]({% post_url 2026-08-04-steepest-descent-i-laplace-stirling %}) we introduced Laplace's method (the real integral version) and used it to prove Stirling's formula. There, all we needed to do was localize near the maximum of $f$. In this post, we do proper steepest descent, which becomes significantly more complicated as you have to choose a contour.

The general formula in $\CC$ we wrote down but didn't yet need is

$$
\begin{equation}
    \int_\gamma e^{nf(z)}dz = e^{nf(z_0)} \sqrt{\frac{2\pi}{n|f''(z_0)|}} e^{i\alpha}(1 + o(1))
\label{eq:steepest_descent_general}
\end{equation}
$$

The complication is that $\gamma$ is now a contour we have to choose. On a bad contour, $e^{nf(z)}$ oscillates instead of decaying, and Laplace's argument falls apart. The method is called "steepest descent" because we choose a contour such that $\Re f$ decreases as fast as possible away from $z_0$ and does not oscillate. We take $\gamma$ through the critical point $z_0$ along the direction where $\Im f$ stays constant so there is no oscillation and $\Re f$ decays. We will prove the local central limit theorem using steepest descent and walk through the construction of such a contour.

So, the steps are similar to those in [Steepest Descent I]({% post_url 2026-08-04-steepest-descent-i-laplace-stirling %}), but with the addition of choosing a contour. 

1. Express the desired formula as an integral.
2. Find an adequate $f$ to to transform the integrand into $\exp(nf(z))$.
3. Find an appropriate contour.
4. Localize near the critical point of $f$ with only $o(1)$ loss.
5. Bound the tails.


## Local CLT for Binomial Random Variables

**Theorem (Local Central Limit Theorem).** Let $p \in (0,1)$ and $x \in \RR$. For $n \in \ZZ_+$, let $k$ be the nearest integer to $pn + x\sqrt{n}$. Then

$$
\begin{equation}
    p^k(1-p)^{n-k} {n \choose k} = \frac{1}{\sqrt{2 \pi n p(1-p)}} \exp \lp -\frac{x^2}{2p(1-p)} \rp (1 + o(1)).
\label{eq:local_clt}
\end{equation}
$$

This theorem predates the more familiar Central Limit Theorem — De Moivre proved it for $p=1/2$, and Laplace in general. It says something sharper than the CLT: not just that a properly-normalized binomial random variable converges in distribution to a Gaussian, but that its individual point masses converge to the values of the Gaussian density itself, at scale $n^{-1/2}$. Hence "local."

**Proof.**

**Step 1:**
We observe that $p^k(1-p)^{n-k}{n \choose k}$ is the coefficient of $z^k$ in

$$
(pz+(1-p))^n = \sum_{j=0}^n {n \choose j} (pz)^j (1-p)^{n-j}.
$$

Dividing by $z^{k+1}$ turns this into the coefficient of $z^{-1}$, i.e. a residue, and the Residue Theorem lets us write it as a contour integral around $0$:

$$
p^k(1-p)^{n-k}{n \choose k} = \frac{1}{2\pi \ii} \oint_{\{0\}} \frac{(pz+(1-p))^n}{z^{k+1}} dz.
$$

We recast this as

$$
\begin{equation}
    I(n) = \frac{1}{2\pi \ii}\oint_{\{0\}} \exp(n f(z)) z^{np-k-1}dz
\label{eq:clt_int}
\end{equation}
$$

where $f(z) = \log(pz + (1 - p)) - p \log(z)$. We'll treat $z^{np-k-1}$ separately — its exponent grows only like $O(\sqrt{n})$, far slower than the $nf(z)$ term that will dominate everything.

**Step 2:**
As with Laplace's method, we want to localize near a critical point of $f$. But we're now integrating over a closed contour in $\CC$ rather than an interval in $\RR$, so we also have to choose that contour, and worry about the geometry and oscillation of the integrand along it. We first fix a branch of $\log$: cut along the negative real axis, so $\log(re^{\ii\theta}) = \log r + \ii\theta$ for $\theta \in (-\pi,\pi)$.

$$
f'(z) = \frac{p}{pz + (1 - p)} - \frac{p}{z},
$$

which vanishes exactly when $p(1-p)(z-1) = 0$, i.e. at $z_c = 1$. Furthermore,

$$
f''(z_c) = \left. -\frac{p^2}{(pz + (1 - p))^2} + \frac{p}{z^2} \right|_{z = 1} = p(1-p) > 0.
$$

Since $f(1) = \log(1) - p\log(1) = 0$, Taylor expanding gives

$$
f(z) = \frac{p(1 - p)}{2} (z-1)^2 + O\lp (z-1)^3 \rp.
$$

Because $f$ is holomorphic, the Cauchy–Riemann equations force this critical point to be a saddle: $f''(z_c)$ is real and positive, so $\Re f$ falls off in the imaginary direction and rises in the real direction near $z_c$. The figure below shows the local picture.

{% include figure.html
   src="/assets/img/posts/steepest-descent/fig04-clt-saddle.png"
   caption="The Cauchy–Riemann equations ensure that critical points of any holomorphic function are saddle points. Here $f''(1)=p(1-p)>0$ is real, so near $z_c=1$ one has $\Re(f(z)-f(z_c)) \approx \tfrac{p(1-p)}{2}((x-1)^2-y^2)$. The light grey curves are level curves of $\Re f$; the dashed diagonals are the degenerate level curve $\Re f=\Re f(z_c)$ through the saddle itself. $\Re f$ falls fastest in the imaginary direction and rises fastest in the real one — so to localize near $z_c$, our contour must pass through it vertically. Claude helped generate the tikz."
   alt="A saddle point at z=1 in the complex plane, with hyperbolic level curves of Re f, shaded ascent and descent sectors, a vertical steepest-descent path, and the real axis marked as the direction of steepest ascent."
   width="78%" %}

**Step 3:**
We want a contour through $z_c=1$ on which $\Im f$ stays constant (so the integrand doesn't oscillate) and $\Re f$ is maximized at $z_c$ (so Laplace's argument applies once we're on it). Consider the level set $\Im f(z) = \Im f(z_c) = 0$. The positive real ray $(0,\infty)$ trivially lies in it, since $f$ is real there, but we claim it also contains a loop around $0$, which we'll take as our contour.

$\Im f(z) = 0$ is equivalent to $g(z) := (pz + (1 - p))z^{-p} \in (0, \infty)$, a form that's easier to work with. Writing $z = re^{\ii \theta}$,

$$
g(z) = r^{-p} \lp p re^{\ii (1-p) \theta} + (1 - p)e^{-\ii p \theta} \rp,
$$

so

$$
\Im g(z) = r^{-p}\lp pr \sin((1-p)\theta) - (1 - p) \sin(p \theta) \rp.
$$

Setting $\Im g(z) = 0$ (and $r^{-p}\ne 0$) requires the two terms to balance:

$$
pr\sin((1-p)\theta) = (1-p)\sin(p\theta).
$$

At $\theta=0$ both sides vanish trivially which is the ray we already found. For $\theta \ne 0$: since $\theta \in (-\pi,\pi)$ forces $(1-p)\theta \in (-\pi,\pi)$ too, $\sin((1-p)\theta) \ne 0$, so we can solve for $r$:

$$
R(\theta) = \frac{(1 - p) \sin(p \theta)}{p\sin((1-p)\theta)}.
$$

$R(\theta) > 0$ for all $\theta$, so $\gamma(\theta) = R(\theta)e^{\ii \theta}$ is injective and winds once around $0$, and $R(0)=1$ (by L'Hôpital), so $\gamma$ passes through $z_c$ exactly as required. The figure below draws $\gamma$ for a few values of $p$.

{% include figure.html
   src="/assets/img/posts/steepest-descent/fig05-clt-contour.png"
   caption="The level set $\{\Im f=0\}$ contains, besides the ray $(0,\infty)$, a closed loop $\gamma(\theta)=R(\theta)e^{\ii\theta}$ around the pole at the origin. It passes vertically through the saddle at $z_c=1$, matching the figure above, and closes at $\theta=\pm\pi$, i.e. at $z=-\tfrac{1-p}{p}$: the zero of $pz+(1-p)$, where the true integrand vanishes and $\Re f\to-\infty$, pinching the loop shut. Claude helped generate the tikz."
   alt="Three closed contours in the complex plane through z=1, for p=0.4, p=0.8, and the degenerate p=1/2 case which is exactly the unit circle, each pinched shut on the negative real axis."
   width="78%" %}

It remains to check $\Re g$ (i.e. $g$ itself, since it's real on $\gamma$) is positive rather than negative there. Substituting $r=R(\theta)$ and using the sine addition formula $\sin(p\theta)\cos((1-p)\theta) + \cos(p\theta)\sin((1-p)\theta) = \sin\theta$,

$$
g(\gamma(\theta)) = R(\theta)^{-p}(1-p)\frac{\sin\theta}{\sin((1-p)\theta)},
$$

which is positive because $\sin\theta$ and $\sin((1-p)\theta)$ always share a sign for $\lvert\theta\rvert<\pi$. So $\gamma(\theta)$ genuinely lies in $\{\Im f = 0\}$.

<!-- *A remark on the branch cut: writing $f = \log(pz+(1-p)) - p\log z$ makes it look like there are two places for something to go wrong, but the actual integrand $(pz+(1-p))^n z^{-(k+1)}$ from Step 1 is single-valued, since $k+1$ is an integer — nothing is really broken. Still, it's worth noticing that $\gamma$ closes exactly at the point $z=-(1-p)/p$ on our cut, and that this is also the zero of $pz+(1-p)$. That's not a coincidence: it's precisely where the singular bookkeeping in $f$ stops mattering, because the true integrand vanishes there.* -->

We appeal to a general fact: along a level set of $\Im f$, $\Re f$ is monotonic. Write $f = u + \ii v$ with $z = x+\ii y$. The Cauchy–Riemann equations $u_x = v_y$, $u_y=-v_x$ give $\nabla v = (-u_y, u_x)$, so $\nabla u \cdot \nabla v = 0$ and $\lvert\nabla u\rvert = \lvert\nabla v\rvert = \lvert f'\rvert$. The tangent to a level curve of $v$ is orthogonal to $\nabla v$, hence parallel to $\nabla u$, so along $\gamma$,

$$
\frac{d}{d\theta}u(\gamma(\theta)) = \nabla u \cdot \gamma'(\theta) = \pm |\nabla u||\gamma'(\theta)|,
$$

which is nonzero away from the critical point. So $\Re f$ is monotonic on $(-\pi,0)$ and on $(0,\pi)$ separately. Since $\Re f(\gamma(\theta)) \to -\infty$ as $\theta \to \pm\pi$ (the pinch point from Step 3), $\Re f$ must rise from $-\infty$ up to $\theta=0$ and fall back to $-\infty$ past it. $\Re f$ attains its maximum exactly at $z_c$. The figure below plots this profile, together with the zones we'll use to bound the tail below.

{% include figure.html
   src="/assets/img/posts/steepest-descent/fig06-clt-profile.png"
   caption="$\Re f$ restricted to the contour above, at $p=0.4$. Because $\Re f$ is monotone along either branch of $\{\Im f=0\}\setminus\{z_c\}$, the profile has a single maximum at $\theta=0$, agreeing with $-\tfrac{p(1-p)}{2}\theta^2$ to third order there, and decaying to $-\infty$ at $\theta=\pm\pi$. The shading is the decomposition used to bound the tail: the localized integral $I_{\mathrm{loc}}$ over $\lvert \theta \rvert<\vep(n)=n^{-2/5}$, a middle zone where $\Re f<-\theta^2/4\cdot p(1-p)$, and an edge zone bounded away from $0$. Claude helped generate the tikz."
   alt="A plot of Re f along the descent contour against theta, showing a single hump maximized at theta=0, decaying to negative infinity at plus or minus pi, with a dashed quadratic model overlaid and the local, middle, and edge zones shaded."
   width="95%" %}

**Step 4:**
Write $u(\theta) = f(\gamma(\theta))$. Since $f(1)=0$, $f'(1)=0$, $f''(1) = p(1-p)$, and $\gamma'(0) = (R'(0)+\ii R(0))e^{\ii \cdot 0} = \ii$ (using that $R$ is even since it's a ratio of two odd functions, so $R'(0)=0$), we have $\gamma(\theta) = 1+\ii\theta + O(\theta^2)$ and

$$
\begin{equation}
    u(\theta) = \frac{p(1-p)}{2}(\gamma(\theta) - 1)^2 + O((\gamma(\theta) - 1)^3) = - \frac{p(1-p)}{2} \theta^2 + O(\theta^3).
\label{eq:clt_u_taylor}
\end{equation}
$$

As in the Stirling proof, we split at $\vep(n) = n^{-2/5}$ — the same balance survives unchanged, since the error term here is cubic exactly as it was there. Write $I(n) = I_{\mathrm{loc}}(n) + I_{\mathrm{tail}}(n)$, the integral \eqref{eq:clt_int} restricted to $\lvert\theta\rvert<\vep(n)$ and its complement.

For $I_{\mathrm{loc}}$, substitute $t = \theta\sqrt{n}$, so $\lvert t\rvert < \sqrt{n}\,\vep(n) = n^{1/10}$. Equation \eqref{eq:clt_u_taylor} gives

$$
n u \lp \frac{t}{\sqrt{n}} \rp = - \frac{p(1-p)}{2} t^2 + O \lp n^{-1/5} \rp,
$$

and, writing $np-k-1 = -x\sqrt{n} + \delta_n$ with $\lvert\delta_n\rvert<2$ (from the choice of $k$) and $\log\gamma(\theta) = \log R(\theta) + \ii \theta$,

$$
(np-k-1) \log \gamma \lp \frac{t}{\sqrt{n}} \rp = - \ii x t + O \lp n^{-1/10} \rp,
$$

while the Jacobian is $\gamma'(t/\sqrt{n}) = \ii(1+O(n^{-2/5}))$. Together,

$$
I_{\mathrm{loc}}(n) = \frac{1+O(n^{-1/5})}{\sqrt{n}} \int_{-n^{1/10}}^{n^{1/10}} \exp \lp - \frac{p(1-p)}{2}t^2 - \ii x t \rp dt.
$$

Extending the domain of integration to all of $\RR$ costs $O\lp e^{-p(1-p)n^{1/5}/2} \rp$, and evaluating the Fourier transform of the resulting Gaussian,

$$
\begin{equation}
    I_{\mathrm{loc}}(n) = \frac{1}{\sqrt{2\pi n p(1-p)}} \exp \lp -\frac{x^2}{2p(1-p)}\rp (1 + o(1)).
\label{eq:clt_center}
\end{equation}
$$

**Step 5:**
For the tail, uniformly bound the Jacobian $\lvert\gamma'(\theta)\rvert \le M$ (continuous on a compact set), and set $F(n,\theta) = nu(\theta) + (np-k-1)\log R(\theta)$, so

$$
|I_{\mathrm{tail}}(n)| \le M \sup_{\vep(n) < |\theta| < \pi} \exp(F(n,\theta)).
$$

In the middle zone $\vep(n)<\lvert\theta\rvert<\theta_0$, \eqref{eq:clt_u_taylor} gives $u(\theta) < -\tfrac{p(1-p)}{4}\theta^2$, and since $\lvert\log R(\theta)\rvert \le C_1\theta^2$ and $\lvert np-k-1\rvert \le (\lvert x\rvert+1)\sqrt{n}$, for $n$ sufficiently large

$$
|(np-k-1)\log R(\theta)| \le C_1(|x|+1)\sqrt{n}\,\theta^2 \le \frac{p(1-p)}{8}n\theta^2,
$$

so, using $\theta^2 \ge \vep(n)^2 = n^{-4/5}$ on this range,

$$
\sup_{\vep(n) < |\theta| < \theta_0} \exp(F(n,\theta)) \le \exp \lp -\frac{p(1-p)}{4}n\theta^2 + \frac{p(1-p)}{8}n\theta^2 \rp \le \exp \lp -\frac{p(1-p)}{8}n^{1/5} \rp.
$$

In the edge zone $\theta_0 < \lvert\theta\rvert < \pi$, there is $c<0$ with $u(\theta) < c$, and $R(\theta)$ is bounded away from $0$ and $\infty$, so $\lvert\log R(\theta)\rvert \le C_2$. Hence $F(n,\theta) \le cn + C_2(\lvert x\rvert+1)\sqrt{n} \le \tfrac{c}{2}n$ for $n$ large, and

$$
\sup_{\theta_0 < |\theta| < \pi} \exp(F(n,\theta)) \le e^{cn/2}.
$$

Both bounds decay faster than any power of $n$, so absorbing constants and prefactors into a single $c^*>0$,

$$
|I_{\mathrm{tail}}(n)| \le e^{-c^*n^{1/5}} = o(n^{-1/2}).
$$

Combining with \eqref{eq:clt_center}, whose leading term is already $\Theta(n^{-1/2})$, the tail is absorbed into the multiplicative error and

$$
I(n) = I_{\mathrm{loc}}(n) + I_{\mathrm{tail}}(n) = \frac{1}{\sqrt{2\pi n p(1-p)}} \exp \lp -\frac{x^2}{2p(1-p)}\rp (1 + o(1)),
$$

which is exactly \eqref{eq:local_clt}. $\blacksquare$
