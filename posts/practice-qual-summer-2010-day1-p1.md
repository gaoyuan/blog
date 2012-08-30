---
title:
date: '2012-08-30'
description:
categories:
tags:
  - quals
  - dynamical system
  - fixed point
  - stability
---

> Consider the following nonlinear system:
\begin{equation}
\cases{
    x' = y + x^2y\cr
    y' = -x + 2xy
}
\end{equation}
Find the fixed points of this system. Determine their stability. Assuming that there are no limit cycles, give a sketch of the phase portrait of the nonlinear system (Put this sketch on a separate page, and make it BIG). If you encounter marginal cases for the fixed points, assume that the linearization gives you the correct result.

By setting $x' = y' = 0$, we get the only possible fixed point $(0, 0)$. The Jacobian of the vector field $(y + x^2y, -x + 2xy)$ is
\begin{equation}
\left(
\begin{matrix}
    2xy & 1 + x^2 \cr
    -1 + 2y & 2x
\end{matrix}
\right)
\end{equation}
At the origin, the Jacobian becomes
\begin{equation}
\left(
\begin{matrix}
    0 & 1\cr
    -1 & 0
\end{matrix}
\right)
\end{equation}
The two eigenvalues of the Jacobian are both purely imaginary. So we cannot say it is stable or unstable. By plotting the vector field around the origin, we can see that it is a center.

<img src="{{urls.media}}/fig5.png" width=600px height=600px />

Note that when $y = 1/2$, $y' = 0$. Therefore if you start with a state with $y \ge 1/2$, you will never be absorbed into the origin. This can be seen if we plot the vector field in a larger domain.

<img src="{{urls.media}}/fig6.png" width=600px height=600px />

