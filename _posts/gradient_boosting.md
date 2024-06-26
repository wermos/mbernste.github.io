---
title: 'Gradient boosting'
date: 2022-03-04
permalink: /posts/gradient_boosting/
tags:
  - tutorial
  - machine learning
---

_THIS POST IS CURRENTLY UNDER CONSTRUCTION_

Boosting is a machine learning paradigm that entails constructing an ensemble of models in an iterative fashion where upon each iteration, a new model is added to the ensemble that compensates for the weaknesses of the previously selected models. 

Introduction
------------

Gradient boosting is a machine learning paradigm that involves training an ensemble of weak models to form a strong model. To understand gradient boosting, one must understand two central ideas: **functional gradient descent** and **boosting**.  

When I first learned about gradient boosting, I was confused as to how it related to the standard gradient descent learning algorithm used to fit the parameters in  parameterized machine learning models. Though the word "gradient" appears in the term "gradient boosting", the gradient boosting is, in fact, quite different from the standard gradient descent algorithm. To understand gradient boosting, one must understand a fairly sophisticated method called "functional" gradient descent, which, as far as I know, was introduced by [Mason _et al.](https://proceedings.neurips.cc/paper/1999/file/96a93ba89a5b5c6c226e49b88973f46e-Paper.pdf) in 1999.

In this post, we will first review the supervised learning task, boosting, and gradient descent. Then we will discuss "functional" gradient descent. Finally, we show how all of these ideas are brought together to form the gradient boosting paradigm.

Review of the supervised learning task
--------------------------------------

Before we get started, let's review the basic supervised learning task so that we are on the same page regarding both the problem setting and mathematical notation. 

In supervised learning, we assume that there exists a function $f^*$ that maps each element in a set $\mathcal{X}$ to a target element in a set $\mathcal{Y}$. That is,

$$f^*: \mathcal{X} \rightarrow \mathcal{Y}$$

For example, $\mathcal{X}$ may be the set of images of animals and $\mathcal{Y}$ may be the set of animal labels (e.g., "tiger" or "snake").  More specifically, the set of objects $\mathcal{X}$ and labels $\mathcal{Y}$ completely depend on the problem being solved. Other examples of $\mathcal{X}$ are text documents (e.g., articles),  numerical vectors (e.g.,those describing scientific measurements), or images. The space $\mathcal{Y}$ usually consists of a set of finite labels (in the case of classification, such as the aforementioned scenario in which we are classifying images according the animal label) or real numbers (in the case of regression).

Importantly, we don't have access to this "latent" function $f^\*$, but rather, we have access to a finite set of pairs $\{(x_i, y_i)}_{i=1, \dots, n}$ where $x_i \in \mathcal{X}$ and $y_i := f^\*(x_i) \in \mathcal{Y}$.  Our goal is to approximate the true $f^*$ by leveraging this finite sample. 

To perform this approximation, we conser a space of functions $\mathcal{H}$ and search for a function $f \in \mathcal{H}$ that minimizes another function, $\ell$, called the [loss function](https://en.wikipedia.org/wiki/Loss_function), which tracks the correspondence between each $y_i$ and each $f(y_i)$. That is, $L$function $\ell(y_i, f(x_i))$ tells us how much $f(x_i)$ deviates from $y_i$.

Specifically, we seek to find an $f \in \mathcal{H}$ that minimizes the average value of the loss function over all of the samples:

$$\hat{f} := \text{arg min}_{f \in \mathcal{H}} \frac{1}{n} \sum_{i=1}^n \ell(y_i, f(x_i))$$

The hope is then that $\hat{f}$ is a good approximation to the "true" latent function $f^*$.

The gradient boosting algorithm
-------------------------------

Before explaining the gradient boosting algorithm itself, let's review the concept of **boosting**. Boosting is a machine learning paradigm that involves constructing an accurate function $f^*$ from a set of innacurate functions. Specifically, we consider a space of simple, innacurate models $\mathcal{H}\text{simple}$, and then let the $\mathcal{H}$ be the set of all linear combinations of functions in $\mathcal{H}\_{\text{simple}}$, which we denote as $\text{lin} \ \mathcal{H}\_{\text{simple}}$.  Said more succintly, we let $\mathcal{H} := \text{lin} \ \mathcal{H}\_{\text{simple}}$. 

Said differently, each function $f \in \mathcal{H}$ has the form:

$$f(x) = \sum_{t=1}^T \alpha_t h_t(x)$$

where for each $t = 1, \dots, T$, $h_t \in \mathcal{H}_{\text{simple}}$ and $T$ is some arbitrary number of inaccurate functions that we select from $\mathcal{H}\_{\text{simple}}$.

The most common category of simple functions, $\mathcal{H}_{\text{simple}}$, that are considered in practice are sets of very small [decision trees](https://en.wikipedia.org/wiki/Decision_tree) with low depth or few features. Generally, as will be discussed below, each model in $\mathcal{H}\_{\text{simple}}$ should perform very poorly on its own. Our goal then is to add a set of weak models together in order to form a strong model. 

Gradient boosting is a specific boosting paradigm that entails constructing $f$ iteratively by adding a new function from $\mathcal{H}_{\text{simple}}$ to $f$ one at a time. 


$$h_t(x_i) = \frac{\partial \ell(y_i, f(x_i))}{\partial f(x_i)}$$

That is, for any given $x_i$, $$h_t(x_i)$$ gives you the 

For simplicity of notation, let

$$r_i := \frac{\partial \ell(y_i, f(x_i)}{\partial f(x_i)}$$

Then, we simply need a function $h_t$ for which $h_t(x_i) = r_i$. How do we actually find such a function? One idea is to train a model to fit the pairs $(x_i, r_i)$! Once we fit this model, we can update our current estimate $f_{t-1}$ via

$$f_t := f_{t-1} + h_t$$

At the end of $T$ iterations, our full model $\hat{f}$ will have the form

$$\hat{f}(x) = \sum_{t=1}^T \alpha h_t(x)$$

Notice that $f(x)$ is actually an ensemble of models $h_1, \dots, h_T$!


Gradient descent
----------------

Now, we'll review the basic ideas behind **gradient descent**. Gradient descent is a numerical approach for solving an optimization problem that involves starting with an initial guess of the solution to the problem and then iteratively improving that solution by taking small steps in the direction of steepest descent down the objective function's surface.

Say we have an objective function $g : \mathcal{\mathbb{R}^m} \rightarrow \mathbb{R}$ and we wish to find the argument that minimizes $g$. That is, we wish to find the solution to the optimization problem

$$\hat{\boldsymbol{x}} := \text{arg min}_{\boldsymbol{x} \in \mathbb{R}^m} g(x)$$

In gradient descent, we find the $\boldsymbol{x}$ that minimizes $g$ by "walking" down $g$'s surface:

<center><img src="https://raw.githubusercontent.com/mbernste/mbernste.github.io/master/images/gradient_descent_error_surface.png" alt="drawing" width="500"/></center>

More rigorously, gradient descent works as follows: We start with an initial guess of the solution $\boldsymbol{x}\_0 \in \mathbb{R}^m$ and iteratively update our estimate by following the direction of steepest descent.  Recall, the direction of steepest _ascent_ is given by the function's [gradient](https://en.wikipedia.org/wiki/Gradient), $\nabla g(\boldsymbol{x})$, and so the direction of steepest _descent_ is simply the direction of the negative gradient. Thus, at the $t$th iteration, the updated solution is given by

$$\boldsymbol{x}_t := \boldsymbol{x}_{t-1} - \alpha \nabla g(\boldsymbol{x}_{t-1})$$

The parameter, $\alpha$, is called the **learning rate** and simply dictates how far we will step in the direction of the negative gradient at each iteration.

Notice that gradient descent can really only be applied when $g$ is differentiable. Otherwise, we cannot compute a gradient and therefore do not how to update the solution! (A variant of gradient descent called [subgradient descent](https://en.wikipedia.org/wiki/Subgradient_method) can be applied to cases where $g$ is not differentiable, but we'll save that discussion for later.)

Functional gradient descent
---------------------------

Before introducing "functional gradient descent", let me spoil the punchline: gradient boosting can be understood as a gradient descent algorithm for solving for 

$$\hat{f} := \text{arg min}_{f \in \mathcal{H}} L(f)$$

where 

$$L(f) := \frac{1}{n} \sum_{i=1}^n \ell(y_i, f(x_i))$$

Now this may seem a bit weird. In our review of gradient descent above, it was assumed that the objective function was numeric. That is, $g$ mapped vectors of real numbers to numbers: 

$$g: \mathbb{R}^m \rightarrow \mathbb{R}$$ 

Here, our objective function $L$ maps functions to real numbers: 

$$L : \mathcal{H} \rightarrow \mathbb{R}$$ 

So how do we perform gradient descent on $L$? What does it even mean to compute a "gradient" on a function of functions like $L$?

This is where functional gradient descent comes in. As you may have guessed, functional gradient descent is a generalization of gradient descent from functions of numbers to functions of functions. To undestand functional gradient descent, we must take a brief forray into an area called the [calculus of variations](https://en.wikipedia.org/wiki/Calculus_of_variations). 

The calculus of variations is all about generalizing the ideas in calculus from functions of numbers to functions of functions. In the calculus of variations, functions are referred to as [functionals](https://en.wikipedia.org/wiki/Functional_(mathematics)). The derivative of a functional is called a **functional derivative**.

Functionals? Functional derivatives? Let's step through this slowly.

As previously stated, a functional is a function of functions. Specifically, it is a function that maps functions to real numbers. The function $L$ above can be viewed as a functional because it takes a function $f \in \mathcal{H}$ and maps it to a real number. That real number, of course, corresponds to the average value of the loss function when evaluating $f$ on all of the $(x_i, y_i)$ pairs.

Now, we need a way to think about the derivative of $L$ at a given $f$. Recall that for a plain old function on the real numbers $g: \mathbb{R} \rightarrow \mathbb{R}$, the derivative of $g$ at $x$, denoted $g'(x)$ tells us how much $g$ is changing at $f$. Said differently, it asks, if we change $x$ by an infinitesimal amount, how much does $g$ change? The functional derivative of $L$ addresses this same question. That is, if we change $f$ by an infinitesimal amount, how does $L$ change? This concept is rigorously addressed by the functional derivative, denoted $\nabla L(f)$.

Now, to define the functional derivative, we must ask the question, what does it mean to "change $f$ by an infinitesimal amount"? To define this rigorously, we'll consider an arbitrary real number $\epsilon \in \mathbb{R}$ and an arbitrary function $\eta: \mathcal{X} \rightarrow \mathcal{Y}$. Then, we'll consider the functional evaluated at $f + \epsilon\eta$:

$$L(f + \epsilon \eta)$$

Notice that $f + \epsilon \eta$ is a function and thus, can be "fed" to $L$. Let's now go a step further and consder $f$ and $\eta$ to be fixed, but we'll let $\epsilon$ vary. That is, we will let $\epsilon$ be a variable that is free to be changed. We thus see that $L(f + \epsilon \eta)$ is a function of $epsilon$ and thus, an ordinary function mapping numbers to numbers. That is, we can let $\mathcal{L}(\epsilon) := L(f + \epsilon \eta)$ and realize that $\mathcal{L} : \mathbb{R} \rightarrow \mathbb{R}$.  



Calculating the pseudo-residuals for common loss functions
----------------------------------------------------------


References
----------

Mason, L. _et al._ (1999). [Boosting Algorithms as Gradient Descent](https://proceedings.neurips.cc/paper/1999/file/96a93ba89a5b5c6c226e49b88973f46e-Paper.pdf). Proceedings of the 12th International Conference on Neural Information Processing Systems.

