+++
title = "Rollout: Approximate Dynamic Programming"
date = 2018-02-27T14:29:56-05:00
draft = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = ""
caption = ""
preview = true

+++

_Life can only be understood going backwards, but it must be lived going forwards - Kierkegaard_.

Dynamic Programming is a mathematical technique that is used in several fields of research including economics, finance, engineering. It deals with making decisions over different stages of the problem in order to minimize (or maximize) a corresponding cost function (or reward). The outcome of a decision at a given stage can be fully predicted (deterministic) or can only be partially predicted (with randomness involved). One key aspect of such problems is the trade off in optimizing the cost incurred at the current stage and the cost to be incurred at a future stage.

Dynamic Programming is very helpful in many contexts by providing an exact solution, however, when the problem size increases, it requires enormous amount of storage and computational time (often referred to as the _curse of dimensionality_) which makes it infeasible to apply to practical situations. Often in such situations, one may not desire an exact solution, but an approximately optimal (or suboptimal) solution. Then there must be a better class of techniques that can capture this trade off between feasibility and sub optimality. These techniques are collectively referred to as _Approximate Dynamic Programming_ or _Reinforcement Learning_ (in the field of AI) or _Neuro Dynamic Programming_ (in the field of automatic control), all of which essentially mean the same.

In this blog post, we shall look at a specific technique called _Rollout Algorithm_ that is quite extensively used (with surprisingly good results). In fact, [Mastering the game of Go, DeepMind](https://www.nature.com/articles/nature16961) uses a variant of _Rollout_ called _Monte Carlo Tree Search_. We shall first formulate a DP problem, state the DP algorithm and illustrate with an example the infeasibility of DP. Using the same example, we shall study how _Rollout_ offers an effective solution that optimizes the trade-off between accuracy and computation. This post is entirely based on my reading of the book [Approximate Dynamic Programming](http://web.mit.edu/dimitrib/www/dpchapter.html).

DP formulation
======
In this post, we shall focus only on finite horizon problems (problems involving a finite number of stages or time steps at which a decision is taken). We denote the state of the system at time $k$ as $x{_k}$ (belongs to a space $S{_k}$ ), and the decision (or control) taken as $u{_k}$ (belongs to a space $U{_k} (x{_k}) $ ), and a random variable $w{_k}$ (which is independent of other $w{_i}$) that affects the system. The system has the form

$$x\_{k+1} = f\_{k}(x\_{k}, u\_{k}, w\_{k})$$

where $k$ takes a value between $0$ and $N$ (finite horizon). At each stage, there is a cost incurred $g{_k}(x{_k}, u{_k}, w{_k})$ which accumulates over time. The total cost incurred is then

$$ g\_{N}(x\_{N}) + \sum\_{k=0}^{N-1}g\_{k}(x\_{k}, u\_{k}, w\_{k})$$

where $g {_N}(x{_N})$ is a terminal cost incurred at the final stage (it only depends on the state, and no action is to be taken). The objective of the problem is to find an optimal policy

$$\pi:= \{\mu\_{0},..,\mu\_{k},.. \mu\_{N-1}\}$$

(where $\mu\_{k} = u\_{k}(x\_{k})$) that minimizes the expected cost (we take an expectation due to the random variable $w\_{k}$). Given an initial state $x\_{0}$, the expected cost of following a policy $\pi$ starting from $x\_0$ is denoted as

$$ J\_{\pi}(x\_{0}) = \mathbb{E}\big[g\_{N}(x\_{N}) + \sum\_{k=0}^{N-1}g\_{k}(x\_{k}, u\_{k}, w\_{k})]$$

An optimal policy $\pi^{\*}$ is the one that minimizes this expected cost; i.e,

$$ J\_{\pi^{\*}}(x\_{0}) = \min\_{\pi \in \Pi} J\_{\pi}(x\_{0})$$

where $\Pi$ is the space of admissible policies.

Bellman's principle of optimality and the DP algorithm
======
Let $\pi^{\*}=\{\mu\_{0}^{\*},..,\mu\_{k}^{\*},.. \mu\_{N-1}^{\*}\}$ be an optimal policy of the problem stated above, and assume that when using $\pi^{\*}$, a given state $x_{i}$ occurs at time $i$ with positive probability. Consider the subproblem, whereby we are at $x\_{i}$ at time $i$ and wish to minimze the "cost-to-go" from time $i$ to time $N$

$$ \mathbb{E}\bigg[g\_{N}(x\_{N}) + \sum\_{k=i}^{N-1}g\_{k}(x\_{k}, u\_{k}, w\_{k})\bigg]$$

Then the truncated policy $\{\mu\_{i}^{\*},\mu\_{i+1}^{\*},.. \mu\_{N-1}^{\*}\}$ is optimal for this subproblem.

Intuitively, let us look at the following problem where we start from A and we need to reach the destination E, and the edge weights are the costs incurred for traversing that edge. The path that minimizes the cost is A-B-D-E. Equivalently, the decision (corresponds to choosing the next node) taken at each time step are B at k=0, D at k=1 and E at k=2. If the policy determined by the path A-B-D-E is optimal, then the truncated path B-D-E is also optimal. If it were not, then we can further reduce the cost by choosing the optimal path which contradicts the fact that A-B-D-E is the path with the minimum cost.

![DP](/img/DP1.png)

## DP Algorithm

The DP algorithm proceeds as follows. We start from the last step of decision making ($N-1$) and proceed backwards performing the minimization

$$ J\_{N}(x\_{N}) = g\_{N}(x\_{N}) $$

$$ J\_{k}(x\_{k}) =\min\_{u\_{k} \in U\_{k}(x\_{k})}\mathbb{E}\_{w\_{k}}\bigg[g\_{N}(x\_{N}) + \sum\_{k=0}^{N-1}g\_{k}(x\_{k}, u\_{k}, w\_{k})\bigg], k = 0,1,..,N-1$$

where the expectation is taken with respect to the probability distribution of $w\_{k}$ which is assumed to depend on $x\_{k}, u\_{k}$. Furthermore, if
$\mu^{\*}\_{k}=u\_{k}^{\*}(x\_{k})$ minimizes the above equation, then the policy $\pi^{\*}=\{\mu\_{0}{\*},...,\mu\_{k}{\*},...,\mu\_{N-1}{\*}\}$ is optimal.


Example
======


One-step lookahead, multi-step lookahead
======


Rollout
======


Conclusion
==========

References
----------
