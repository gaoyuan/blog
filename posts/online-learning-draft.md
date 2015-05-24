---
title:
date: '2015-04-29'
description:
categories:
tags:
  - machine learning
  - online convex optimization
  - stochastic optimization

type: draft
---

Online learning is an area of machine learning where the data comes in a sequential fashion. This makes the problem particular interesting because it is like playing a game, where you are constantly making decisions as data streams in. The general model of online learning consists of the following steps:

 1. given current data $x\_t$
 2. perform action $u\_t$
 3. suffer loss $l\_t(u\_t)$
 4. set $t = t + 1$, go to 1

The simple model depicted above turns out to be super powerful and general. Here are some examples:

 * **Online Regression**. $x\_t \in \mathbb{R}^p$ is a data point. $u\_t \in \mathbb{R}$ is the prediction. $l\_t(u\_t) = (u\_t - y\_t)^2$, where $y\_t \in \mathbb{R}$ is the true value. Here we used the square loss, but other loss functions can be used as well, such as absolute loss.
 * **Online Classification**. $x\_t \in \mathbb{R}^p$ is a data point. $u\_t \in \\{1, 2, \ldots, K\\}$ is the prediction. $l\_t(u\_t) = 1 - \chi\left(u\_t = y\_t\right)$, where $\chi(\cdot)$ is the indicator function and $y\_t \in \\{1, 2, \ldots, K\\}$ is the true label.
 * **Online Clustering**.  $x\_t \in \mathbb{R}^p$ is a data point. $u\_t$ is a partition of the set $\\{x\_1, x\_2, \ldots, x\_t\\}$. $y\_t$ is a human defined optimal partition of the set $\\{x\_1, x\_2, \ldots, x\_t\\}$. $l\_t(u\_t)$ is a measure of the difference between these two partitions.

The objective of an online learning model is to minimize the running loss
\begin{equation}
\sum\_{t = 1}^T l\_t(u\_t).
\end{equation}
According to the *no free lunch* theorem in machine learning, we have to come up with some hypothesis class to learn the best action sequences $u\_t$. Different hypothesis corresponds to different models. Below are some examples:

 * **Online Linear Least Squares**. $x\_t \in \mathbb{R}^p$ is a data point. $u\_t \in \mathbb{R}^p$ is the predicted weight vector. $l\_t(u\_t) = \left(\langle x\_t, u\_t \rangle - y\_t\right)^2$, where $y\_t \in \mathbb{R}$ is the true value.
 * **Online Lasso.** Same as the previous example except that $l\_t(u\_t) = \left(\langle x\_t, u\_t \rangle - y\_t\right)^2 + ||u\_t||\_1$. A somewhat equivalent idea is to impose the constraint in the hypothesis class, say let $u\_t \in \\{ u~| ||u||\_1 \le \gamma \\}$.
 * **Online Logistic Regression.** $l\_t(u\_t) = log(1 + exp(-y\_t\langle x\_t, u\_t \rangle))$, where $y\_t \in \\{+1, -1\\}$ is the true label. 

After the modelling process, the last thing is to come up with an optimization algorithm to minimize the objective. Unlike offline optimization algorithms where we can evaluate them by studying their convergence properties, in general for online learning an optimal solution does not even exist. For instance, we can take $l\_t(u\_t) = (-2)^tu\_t$. Let the hypothesis class to be $[-1, 1]$, then the optimal solution is -1 for t odd and 1 for t even. Therefore the optimal solution depends on the horizon T. Given a fixed T, there exists an optimal solution from the hypothesis class. It is then natural to define the concept of regret
\begin{equation}
R(T) = \sum\_{t = 1}^T l\_t(u\_t) - \min\_{u \in \mathcal{H}}\sum\_{t = 1}^T l\_t(u),
\end{equation}
where $\mathcal{H}$ is the hypothesis class. The regret basically describes the convergence in optimal value. Similarly, we can also define the regret in optimal solution
\begin{equation}
R\_s(T) = \sum\_{t=1}^T \lvert u\_t - argmin\_{u \in \mathcal{H}}\sum\_{\tau = 1}^t l\_\tau(u) \rvert.
\end{equation}
For loss functions with both strong convexity and smoothness guarantees, $R(T)$ and $R\_s(T)$ are equivalent up to a constant. For an online optimization algorithm, we hope that $R(T) = o(T)$. This implies the average regret tends to zero $\lim\_{T\rightarrow\infty}R(T)/T = 0$, or on average the online algorithm performs equally well with an offline algorithm. It is unrealistic to ask for $R(T)\rightarrow 0$, because the regret is an accumulated error. 

Convex Loss Functions
--------------------------
Throughout the discussion we will stick to convex loss functions, i.e.,  $l\_t(\cdot)$ is convex for all $t$. The nice thing about a convex function is that it is bounded below by a line
\begin{equation}
l\_t(u\_t) - l\_t(u) \le \langle z\_t, u\_t - u \rangle,
\end{equation}
where $z\_t \in \partial l\_t(u\_t)$. In particular, if we choose $u^\* = argmin\_{u \in \mathcal{H}}\sum\_{t = 1}^T l\_t(u)$, this implies,
\begin{equation}
R(T) = \sum\_{t = 1}^T l\_t(u\_t) - \sum\_{t = 1}^T l\_t(u^\*) \le \sum\_{t = 1}^T \langle z\_t, u\_t \rangle - \langle z\_t, u^\* \rangle \le R\_L(T),
\end{equation}
where $R\_L(T)$ is the regret with respect to the linear loss function $l\_t(u\_t) = \langle z\_t, u\_t \rangle$. Therefore we can focus on the regret study of linear loss functions, as they automatically induce an upper bound of the regret of convex loss functions.

Algorithms
----------
A naive algorithm is called **follow-the-leader**, which is to choose the vector that has the minimum loss on all past rounds.
\begin{equation}
u\_t = argmin\_{u \in H}\sum\_{\tau=1}^{t-1}l\_{\tau}(u).
\end{equation}
According to this definition, we have
\begin{equation}
R(T) = \sum\_{t = 1}^T l\_t(u\_t) - \min\_{u \in \mathcal{H}}\sum\_{t = 1}^T l\_t(u) = \sum\_{t = 1}^T l\_t(u\_t) - \sum\_{t = 1}^T l\_t(u\_{T+1}) \le \sum\_{t = 1}^T \left(l\_t(u\_t) - l\_t(u\_{t+1})\right). 
\end{equation}
Note that here we are

Contrary to offline optimization where each iteration usually involves going through all the data, one iteration in an online optimization only observes one data. This makes online optimization algorithms perferable even in the offline case when the data size is large.


Connection to Reinforcement Learning
------------------------------------
Online learning bears a lot of resemblance to reinforcement learning (RL), in that they both embody the flavor of making decision on the fly. In a typical RL scenario, we have a state space $X$ and any at any state $x \in X$ there is a set of actions $U(x)$ available, so that applying $u \in U(x)$ will lead us to another state $x'$, according to the dynamics $x' = f(x, u)$. At the meantime, we suffer from a loss $l(x, u)$. The goal is to minimize the total cost $\sum\_{k=1}^n l(x\_k, u\_k)$. The infinite horizon formulation usually includes a discount factor $0 < \alpha < 1$ to mimic myopic behavior, so the total cost is $\sum\_{k=1}^{\infty} \alpha^kl(x\_k, u\_k)$. The key factor that distinguishes RL from optimal control theory is that the dynamics $f(x, u)$ (usually modelled as a Markov transition between states $P(x'|x, u)$) and loss function $l(x, u)$ are not known beforehand, something known as partial observability. So a RL model looks like:

 1. given current state $x\_t$
 2. choose an action $u\_t$
 3. receive $x\_{t+1} \sim P(x'|x\_t, u\_t)$ and $l(x\_t, u\_t)$
 4. suffer loss $l(x\_t, u\_t)$
 5. set $t = t + 1$, go to 1

Comparing the model to online learning, we find that RL is actually a special case of online learning where we assume the data $x\_{t+1}$ is influenced by our action/prediction at time $t$. Therefore reinforcement learning can be seen as an *active* form of learning, where we are using our actions to actively exploring the state space.

Problem to study:
1. Nesterov's accerlerated online gradient descent
2. Series Acceleration (Aitken)

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

