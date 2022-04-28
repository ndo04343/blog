---
keywords: fastai
description: Optimization theory summary note.
title: "[OptimizationTheory] CH03. Convex Optimization"
toc: false
badges: false
comments: false
categories: [optimization-theory]
hide_{github,colab,binder,deepnote}_badge: true
nb_path: _notebooks/ch03-convex-optimization.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/ch03-convex-optimization.ipynb
-->

<div class="container" id="notebook-container">
        
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="3.1.-Introduction">3.1. Introduction<a class="anchor-link" href="#3.1.-Introduction"> </a></h4><p>Consider the objective function that is differentiable in optimization problem. If $\mathbf{w}^*$ is optimal, $\nabla_\mathbf{w} L$ must be zero vector. This condition is called gradient necessary condition. In optimization problem, we have to find global optimal. However, we only find local optimal everytime. Also, if we find the solution of $\nabla_\mathbf{w} L = \mathbf{0}$, there is no guarantee that the solution is a global optimal. <br><br></p>
<p>Here is something to think about.</p>
<blockquote><p>[Dauphin14] Y. Dauphin, R. Pascanu, C. Gulcehre, K. Cho, S. Ganguli, Y. Bengio. Identifying and attacking the saddle point problem in high-dimensional non-convex optimization.</p>
</blockquote>
<p>Above paper suggest that local minima problem is actually a very rare case that does not occur in a high dimensional space. First reason is gradient necessary condition. Every elements in gradient must be a zero at some $\mathbf{w}^*$. But this is very rare case. Also, in every direction, gradient must form a convex shape in a high-dimensional space. But, the probability of that happening is close to zero.</p>
<blockquote><p>Intuitively, in high dimensions, the chance that all the directions around a critical point lead upward is exponentially small w.r.t. the number of dimensions, unless the critical point is the global minimum or stands at an error level close to it, i.e., it is unlikely one can find a way to go further down.</p>
</blockquote>
<p>In this context, since both local minima and the global minima are same, it can be seen that a convex function can be good loss function.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="3.2.-Convex-Optimization">3.2. Convex Optimization<a class="anchor-link" href="#3.2.-Convex-Optimization"> </a></h4><h5 id="Definition.3.1.-Convex-Set">Definition.3.1. Convex Set<a class="anchor-link" href="#Definition.3.1.-Convex-Set"> </a></h5><p>A set $S$ is said to be convex if</p>
$$
\mathbf{x}, \mathbf{y} \in S, \,\ \text{then} \,\ t\mathbf{x} + (1 - t)\mathbf{y} \in S, \,\ \text{for} \,\ t \in [0, 1]
$$
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h5 id="Definition.3.2.-Convex-Function">Definition.3.2. Convex Function<a class="anchor-link" href="#Definition.3.2.-Convex-Function"> </a></h5><p>A function $f: S \rightarrow \mathbb{R}$ is said to be convex if</p>
$$
^\forall \mathbf{x}_1, \mathbf{x}_2 \in S, \,\ f(t\mathbf{x}_1 + (1 - t)\mathbf{x}_2) \le tf(\mathbf{x}_1) + (1 - t)f(\mathbf{x}_2) \,\ \text{for} \,\ t \in [0, 1]
$$<p>where $S$ is convex subset of a real vector space.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h5 id="Theorem.3.1.">Theorem.3.1.<a class="anchor-link" href="#Theorem.3.1."> </a></h5><p>In convex function, some local minimum is a global minimum.</p>
<p><strong>Proof.</strong><br>
Trivial(proof by contradiction).</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h5 id="Theorem.3.2.">Theorem.3.2.<a class="anchor-link" href="#Theorem.3.2."> </a></h5><p>A twice differentiable function $f: \mathbb{R}^n \rightarrow \mathbb{R}$ is a convex function if and only if $\nabla^2 f \succeq 0$.</p>
<p><strong>Proof.</strong><br>
Suppose $f$ has a positive semidefinite hessian matrix.<br>
Then for some $\mathbf{x}_0, \mathbf{x}_1$ in the domain, and $t \in [0, 1]$, we have</p>
$$
g(t) = f(t\mathbf{x}_0 + (1- t)\mathbf{x}_1)
$$<p>which have the first and second derivative</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
$$
\begin{matrix}
\frac{dg}{dt} = (\mathbf{x}_0 - \mathbf{x}_1)^T \nabla_\mathbf{x} f(t\mathbf{x}_0 + (1 - t)\mathbf{x}_1) \\
\frac{d^2g}{dt^2} = (\mathbf{x}_0 - \mathbf{x}_1)^T \nabla_\mathbf{x}^2 f(t\mathbf{x}_0 + (1 - t)\mathbf{x}_1)(\mathbf{x}_0 - \mathbf{x}_1) \\
\end{matrix}
$$
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Since the hessian matrix of $f$ is positive semidefinite, $\frac{d^2g}{dt^2} \ge 0$ for $t \in [0, 1]$.<br>
Then we can get</p>
$$
\begin{matrix}
g(0) \ge g(t) + g^\prime(t)(-t) \\
g(1) \ge g(t) + g^\prime(t)(1 - t) \\
\end{matrix} \quad (\because \,\ \text{Taylor's theorem})
$$
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Then 
$$
g(t) \le tg(1) + (1 - t)g(0)
$$</p>
$$
\therefore \,\ ^\forall \mathbf{x}_0, \mathbf{x}_1 \in D, \,\ f(t\mathbf{x}_0 + (1 - t)\mathbf{x}_1) \le tf(\mathbf{x}_0) + (1 - t)\mathbf{x}_1 \quad \blacksquare
$$
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h5 id="Definition.3.3.-Convex-Optimization-Problem">Definition.3.3. Convex Optimization Problem<a class="anchor-link" href="#Definition.3.3.-Convex-Optimization-Problem"> </a></h5><p>A convex optimization problem is an optimization problem in which the objective function is a convex function and the feasible set is a convex set.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="3.3.-Properties">3.3. Properties<a class="anchor-link" href="#3.3.-Properties"> </a></h4><p>Followings are convex function:</p>
<ul>
<li>Exponential function</li>
<li>Power function(in some case)</li>
<li>Logarithmic function</li>
<li>Affine function</li>
<li>Quadratic function </li>
<li>Mean square error</li>
<li>Max function</li>
<li>Norm function</li>
<li>Softmax function</li>
</ul>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h5 id="Theorem.3.3.">Theorem.3.3.<a class="anchor-link" href="#Theorem.3.3."> </a></h5><p>Let $f(\mathbf{x}) = h(g(\mathbf{x})) = h(g_1(\mathbf{x}), \cdots, g_k(\mathbf{x}))$ <br>
where $g: \mathbb{R}^n \rightarrow \mathbb{R}^k, \,\ h : \mathbb{R}^k \rightarrow \mathbb{R}, \,\ \text{and} \,\ f : \mathbb{R}^n \rightarrow \mathbb{R}$.<br>
Then</p>
<ul>
<li>$f$ is convex if $h$ is convex and nondecreasing in each argument, g is convex.</li>
<li>$f$ is convex if $h$ is convex and nonincreasing in each argument, g is concave.</li>
</ul>
<p><strong>Proof.</strong><br>
Trivial(by chain rule and above theorems).</p>

</div>
</div>
</div>
</div>
 
