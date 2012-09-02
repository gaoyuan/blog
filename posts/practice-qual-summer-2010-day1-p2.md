---
title:
date: '2012-09-02'
description:
categories:
tags:
  - quals
  - Gram-Schmidt process
  - vector space
  - orthogonal
---

>Define the inner product of two functions $f(x)$ and $g(x)$ defined on the interval [0,1] by
\begin{equation}
\langle f,g \rangle = \int_0^1 f(x)g(x)dx
\end{equation}
We say $f$ and $g$ are orthogonal if $\langle f,g \rangle = 0$.
Let $\mathcal{P}$ be the space of cubic polynomials $p(x)$ satisfying $p(1)=p'(1)=0$. This is a two-dimensional linear function space. Determine an orthogonal basis for this space with respect to the inner product above. Note: Orthogonal is enough, it need not be orthonormal.

First we find two vectors in the space and then we use Gram-Schmidt process to orthogonalize them. 

The general way of finding the two vectors are to assume its form $p(x)=ax^3+bx^2+cx+d$. The two conditions $p(1)=p'(1)=0$ gives us two constrains so that only two variables among $a,b,c$ and $d$ are flexible. That is also the reason why the space is two dimensional. We have
\begin{equation}
\cases{
    a + b + c + d = 0\cr
    3a + 2b + c = 0
}
\end{equation}
We find two linear independent solutions
\begin{equation}
\cases{
    p\_1(x) = x^3 - 2x^2 + x\cr
    p\_2(x) = 2x^3 - 3x^2 + 1
}
\end{equation}
There are other ways to come up with the two vectors. For example, by observation $x-1$ is also in the space.

Anyway, we will project $p\_2(x)$ onto $p\_1(x)$ and then substract that part from $p\_2(x)$ to get a vector that is still in the space and is orthogonal to $p\_1(x)$. This is done by
\begin{equation}
p\_3(x) = p\_2(x) - \frac{\langle p\_1(x),p\_2(x)\rangle}{\langle p\_1(x), p\_1(x)\rangle}p\_1(x) = -\frac{7}{2}x^3+8x^2+1-\frac{11}{2}x
\end{equation}
One can verify that $\langle p\_1(x), p\_3(x) \rangle = 0$.

Note: Denote the angle between $p\_1(x)$ and $p\_2(x)$ as $\theta$. Then $\|p\_2\|cos\theta\vec{e}$ is the projection of $p\_2(x)$ onto $p\_1(x)$, where $\vec{e}$ is the unit vector with the same direction as $p\_1(x)$. 
\begin{equation}
\frac{\langle p\_1(x),p\_2(x)\rangle}{\langle p\_1(x), p\_1(x)\rangle}p\_1(x) = \frac{\|p\_1\|\|p\_2\|cos\theta}{\|p\_1\|^2}p\_1 = \|p\_2\|cos\theta\vec{e}
\end{equation}


