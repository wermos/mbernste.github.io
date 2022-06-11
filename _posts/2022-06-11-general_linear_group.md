---
title: 'Systems of linear equations and elementary row operations'
date: 2022-06-11
permalink: /posts/systems_linear_equations/
tags:
  - tutorial
  - mathematics
  - linear algebra
---

_THIS POST IS CURRENTLY UNDER CONSTRUCTION_

Introduction
------------

Many curricula in linear algebra begin by introducing systems of linear equations. In my [blog posts on linear agebra](https://mbernste.github.io/posts/), I take an alternative pedogocial route that is a bit more abstract. Instead of starting with systems of linear equations, I start with [vector spaces](https://mbernste.github.io/posts/vector_spaces/), [matrices](https://mbernste.github.io/posts/matrices/), and [linear transformations](https://mbernste.github.io/posts/matrices_linear_transformations/). Advantages to this approach are 1) right from the start, we delve into the deep mathematical structures at the heart of linear algebra and 2) we keep everything abstract and thus, very generalizable to specific applications and problems. 

In this blog post, we will begin by taking a step back from the very abstract and discuss the relationship between matrices and systems of linear equations. Specifically, we will show how many ideas we have discussed so far, such as invertible matrices, relate to description of solutions to systems of linear equations.  Let's dig in!

Systems of linear equations
---------------------------

A **system of linear equations** is a set of [linear equations](https://en.wikipedia.org/wiki/Linear_equation) that all utilize the same set of variables, but each equation differs by the coefficients that multiply those variables. 

For example, say we have three variables, $x_1, x_2$, and $x_3$. A system of linear equations involving these three variables can be written as:

$$\begin{align*}3 x_1 + 2 x_2 - x_3 &= 1 \\ 2 x_1 + -2 x_2 + 4 x_3 &= -2 \\ -x_1 + 0.5 x_2 + - x_3 &= 0 \end{align*}$$

A **solution** to this system of linear equations is an assignment to the variables $x_1, x_2, x_3$, such that all of the equations are simultaneously true. In our case, a solution would be given by $(x_1, x_2, x_3) = (1, -2, -2)$.

More abstractly, we could write a system of linear equations as 

$$\begin{align*}a_{1,1}x_1 + a_{1,2}x_2 + a_{1,3}x_3 &= b_1 \\ a_{2,1}x_1 + a_{2,2}x_2 + a_{2,3}x_3 &= b_2 \\ a_{3,1}x_1 + a_{3,2}x_2 + a_{3,3}x_3 &= b_3 \end{align*}$$

where $a_{1,1}, \dots, a_{3,3}$ are the coefficients and $b_1, b_2,$ and $b_3$ are the constant terms, all treated as fixed.

The solutions to a system of linear equations
---------------------------------------------

Now, a natural question is: given a system of linear equations, how many solutions does it have? 

In some cases, the system could have one solution. For example, the following system has only one solution:

$$\begin{align*}3 x_1 + 2 x_2 - x_3 &= 1 \\ 2 x_1 + -2 x_2 + 4 x_3 &= -2 \\ -x_1 + 0.5 x_2 + - x_3 &= 0 \end{align*}$$

That solution, as mentioned in the previous section, is $(x_1, x_2, x_3) = (1, -2, -2)$.

However, in other cases, a system may not only have one solution.  Some systems of linear equations have an infinite number of solutions. Take for example, the following system:

$$\begin{align*}3 x_1 + 2 x_2 + 0x_3 &= 1 \\ 2 x_1 + -2 x_2 + 0x_3 &= -2 \\ -x_1 + 0.5 x_2 + + 0x_3 &= 0 \end{align*}$$

In still other cases, the system could have no solutions at all! For example:

$$\begin{align*}3 x_1 + 2 x_2 + 0x_3 &= 1 \\ 2 x_1 + -2 x_2 + 0x_3 &= -2 \\ -x_1 + 0.5 x_2 + + 0x_3 &= 0 \end{align*}$$

How can we tell how many solutions a given system of linear equations has? We will use the theory of linear algebra to answer this question!

Representing systems of linear equations using matrices
-------------------------------------------------------

First, note that we can write a system of linear equations much more succinctly using [matrix-vector](https://mbernste.github.io/posts/matrix_vector_mult/) multiplication. That is,

$$\begin{bmatrix}a_{1,1} && a_{1,2} && a_{1,3} \\ a_{2,1} && a_{2,2} && a_{2,3} \\ a_{3,1} && a_{3,2} && a_{3,3} \end{bmatrix}  \begin{bmatrix}x_1 \\ x_2 \\ x_3\end{bmatrix} = \begin{bmatrix}b_1 \\ b_2 \\ b_3\end{bmatrix}$$

If we let the matrix of coefficients be $\boldsymbol{A}$, the vector of variables be $\boldsymbol{x}$, and the vector of constants be $\boldsymbol{b}$, then we could write this even more succinctly as:

$$\boldsymbol{Ax} = \boldsymbol{b}$$

This is an important point: any system of linear equations can be written succintly as an equation using matrix-vector multiplication. By viewing systems of linear equations through this lense, we can reason about the number of solutions to a system of linear equations using properties of the matrix $\boldsymbol{A}$!

Given this newfound representation for systems of linear equations, let us employ the insights that we have gained from our study of [matrix-vector multiplication from a previous blog post](https://mbernste.github.io/posts/matrix_vector_mult/). 

As we saw, matrix-vector multiplication between a matrix $\boldsymbol{A}$ and vector $\boldsymbol{x}$ can be understood as taking a linear combination of the column vectors of $\boldsymbol{A}$ using the elements of $\boldsymbol{x}$ as the coefficients:

<center><img src="https://raw.githubusercontent.com/mbernste/mbernste.github.io/master/images/matrix_vec_mult_as_lin_comb.png" alt="drawing" width="700"/></center>

Thus, we see that the solution to a system of linear equations is any set of weights for which, if we take a weighted sum of the columns of $\boldsymbol{A}$, we get the vector $\boldsymbol{b}$!








