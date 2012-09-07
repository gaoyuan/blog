---
title:
date: '2012-09-06'
description:
categories:
tags:
  - quals
  - analysis
  - kernel
  - partial differentiation
---

> Assume that $k:[0,1]\times[0,1] \rightarrow \mathcal{R}$ is continuously differentiable and that $f:[0,1] \rightarrow \mathcal{R}$is continuous. Using the definition of the derivative, and the derivative with respect to $x$ of
\begin{equation}
F(x) = \int\_0^x k(x,y)f(y)dy, x \in (0,1),
\end{equation}
As an example, let $k(x,y) = x^2y$ and $f(x)=x$. Calculate $F'(x)$ in two different ways: (i) using the formula you found, and (ii) by calculating $F(x)$ and taking a derivative.

By the definition of derivative,
\begin{equation}
F'(x) = \lim\_{\epsilon\rightarrow 0}\frac{F(x+\epsilon)-F(x)}{\epsilon} = \left(\int\_0^{x+\epsilon}k(x+\epsilon,y)f(y)dy - \int\_0^x k(x,y)f(y)dy\right)/\epsilon
\end{equation}
\begin{equation}
=\lim\_{\epsilon\rightarrow 0}\left(\int\_0^{x+\epsilon}k(x+\epsilon,y)f(y)dy - \int\_0^{x+\epsilon}k(x,y)f(y)dy + \int\_0^{x+\epsilon}k(x,y)f(y)dy - \int\_0^x k(x,y)f(y)dy\right)/\epsilon
\end{equation}
\begin{equation}
=\lim\_{\epsilon\rightarrow 0}\left(\int\_0^{x+\epsilon}\frac{k(x+\epsilon,y)-k(x,y)}{\epsilon} f(y)dy  + \frac{1}{\epsilon}\int\_x^{x+\epsilon}k(x,y)f(y)dy \right) = \int\_0^xk\_x(x,y)f(y)dy + k(x,x)f(x)
\end{equation}
In the derivation above, we used the continuously differentiability of $k$ and the continuity of $f$ in exchanging the integral and limit at the second last step.

For the example $k(x,y)=x^2y$ and $f(x)=x$, we first use our formula:
\begin{equation}
\int\_0^xk\_x(x,y)f(y)dy + k(x,x)f(x) = \int\_0^x2xy^2dy + x^4 = \frac{5}{3}x^4.
\end{equation}
If we calculate it directly, we get
\begin{equation}
\int\_0^xk(x,y)f(y)dy = \int\_0^xx^2y^2dy = \frac{1}{3}x^5. 
\end{equation}
Then by taking the derivative of $\frac{1}{3}x^5$ respect to $x$ we get the same solution.






