---
title:
date: '2012-08-30'
description:
categories:
tags:
  - quals
  - numerical methods
  - forward Euler
  - backward Euler
---

> Consider the ordinary differential equation
\begin{equation}
\frac{dy}{dt} = -100y
\end{equation}
Your unsophisticated friend is using the forward Euler method to solve this equation over the time interval [0, 5]. He observes a discrete approximation to the solution $y(t)$ whose absolute value increases over time. Is this possible? If so, characterize the time step that he is using. Now answer the same question using the backward Euler method.

* For the forward Euler method, we have $y\_{n+1} = y\_n + hf(t\_n)$, where $h$ is the time spacing. In our case it will become $y\_{n+1} = y\_n + h(-100y\_n) = (1-100h)y\_n$. To make the absolute value of $y(t)$ increases over time, we have $|y\_{n+1}| > |y\_n|$, which gives us $1-100h > 1$ or $1-100h < -1$. Since the timestep $h$ is positive, only the latter case is possible. In that case $h > 0.02$.

* For the backward Euler method, we have $y\_{n+1} = y\_n + hf(t\_{n+1})$. In our case it will become $y\_{n+1} = y\_n + h(-100y\_{n+1})$. To make the absolute value of $y(t)$ increases over time, we have $|y\_{n+1}| > |y\_n|$, which gives us $0 < 1+100h < 1$ or $-1 < 1+100h < 0$. Since the timestep $h$ is positive, neither case is possible. Therefore it is impossible if backward Euler method is used.

Now we make some plots of the forward Euler method. Note that if we set $h = 0.02$, the absolute value of $y(t)$ will remain the same. In fact it will be oscillating because $y\_{n+1} = -y\_n$. Below is a plot of the case. We initialize $y(0) = 1$.

![]({{urls.media}}/fig1.png)

If we set $h > 0.02$, it will be like

![]({{urls.media}}/fig2.png)

If we set $h$ slightly smaller than $0.02$, it is still not working correctly, but the amplitude of the solution will not increase.

![]({{urls.media}}/fig3.png)

If we set $h$ small enough, we get the correct behavior

![]({{urls.media}}/fig4.png)

