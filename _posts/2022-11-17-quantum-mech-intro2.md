---
title: 'Intro to Quantum Mechanics (Part I)'
date: 2022-11-20
permalink: /posts/2022/11/quantum2/
tags:
  - Physics
  - Statistics
---

This is Part II of my blog post series giving an introduction to the basic principles of quantum mechanics. In this post, we will cover the importance of linearity in quantum mechanics and introduce Dirac's notation. We will also show how the wave function can be interpreted as a probability distribution.

As mentioned in Part I, you may find it helpful to review the following background concepts prior to reading this post series:

### Background Reading:
- Linear Algebra:
	- [MIT Open Courseware Linear Algebra](https://ocw.mit.edu/courses/18-06-linear-algebra-spring-2010/) (Dr. Gilbert Strang)
	- [Essence of Linear Algebra Video Series](https://www.3blue1brown.com/topics/linear-algebra) (3Blue1Brown)
- Linear Differential Equations:
	- [Linear Differential Equations Notes](https://tutorial.math.lamar.edu/classes/de/linear.aspx) (Paul's Online Notes)
	- [Differential Equations Video Series](https://www.3blue1brown.com/topics/differential-equations) (3Blue1Brown)
- Statistics:
	- [Probability and Statistics](https://phys.libretexts.org/Courses/University_of_California_Davis/UCD:_Physics_9HE_-_Modern_Physics/01:_Mathematical_Background/1.3:_Probability_and_Statistics) (LibreTexts Physics) 

## Linearity and the Wave Function
In the previous post, we derived the more general form of the Schrodinger equation, which employs the Hamiltonian operator $\hat{\mathbf{H}}$ to describe the time-dynamics of a wave function $\psi(x,t)$:

$$\boxed{\dfrac{\partial}{\partial t}\psi(x,t) = \hat{\mathbf{H}}\psi(x,t)}$$

In one dimension, the Hamiltonian takes the form:

$$\hat{\mathbf{H}} \equiv \frac{\hat{\mathbf{p}}^2}{2m} + V(x) \equiv \frac{-\hbar^2}{2m}\dfrac{\partial^2}{\partial x^2} + V(x),$$

where $V(x)$ is some known potential function. Earlier, we remarked that the Hamiltonian operator $\hat{\mathbf{H}}$ was _linear_. If an operator $\hat{\mathbf{O}}$ is linear, it means that it distributes over scalar multiplication and addition. Specifically, for any two wave functions $\psi_a(x,t)$, $\psi_b(x,t)$, and any two scalars $a, b$ we have:

$$\hat{\mathbf{O}}[a\psi_a(x,t) + b\psi_b(x,t)] = a[\hat{\mathbf{O}}\psi_a(x,t)] + b[\hat{\mathbf{O}}\psi_b(x,t)]$$

From this definition, we see that linearity is quite a useful property. For example, consider a complex wave function as some linear combination of simpler wave functions
$$\psi = \sum_n a_n\psi_n,$$

In this case, we say that $\psi$ is a _superposition_ of the wave functions $\psi_n$. Suppose we know that that applying $\hat{\mathbf{O}}$ to each wave function $\psi_n$ gives us another wave function $\widetilde{\psi_n}$:
$$\hat{\mathbf{O}}\psi_n = \widetilde{\psi_n}.$$ 

Then, applying $\hat{\mathbf{O}}$ to a linear combination of the $\psi_n$ wave functions is equivalent to the same linear combination of the results of applying $\hat{\mathbf{O}}$ directly to the $\psi_n$ wave functions:

$$\hat{\mathbf{O}}\psi = \hat{\mathbf{O}}\left[\sum_n a_n\psi_n \right] = \sum_n a_n\hat{\mathbf{O}}\psi_n = \sum_n a_n\widetilde{\psi_n}$$

So far, we have encountered two linear operators: the Hamiltonaian $\hat{\mathbf{H}}$ and the momentum operator $\hat{\mathbf{p}}$, but these are not the only linear operators in quantum mechanics. Remarkably,  linearity an essential property that _all operators representing physically observable quantities_ must have in quantum mechanics. While linearity may seem like just a useful mathematical property, the fact that quantum operators are linear has significant ramifications in the nature of physical reality. For example, linearity implies that the wave function of one particle cannot be "copied" onto the wave function of another particle. We will explore this result in more detail in a future post.


## Operators in Matrix Form
If you think back to your linear algebra course, you may recall that linear transformations can be realized as a matrix acting on set of _complete orthonormal_ basis vectors. For example, a rotation of a 2D vector $\mathbf{x} = \begin{pmatrix} x_1 & x_2 \end{pmatrix}^T$ by an angle $\theta$ can be written as the product of a rotation matrix $\mathbf{R}_\theta$ times $\mathbf{x}$:

$$\mathbf{x'} = \mathbf{R}_\theta\mathbf{x} = \begin{pmatrix} \cos(\theta) & -\sin(\theta) \\ \sin(\theta) & \cos(\theta) \end{pmatrix}\begin{pmatrix} x_1 \\ x_2 \end{pmatrix} = \begin{pmatrix} x_1\cos(\theta) - x_2\sin(\theta) \\ x_1\sin(\theta) + x_2\cos(\theta)\end{pmatrix}$$

This matrix corresponds to a rotation operation $\mathbf{R}_\theta$, which rotates a vector counter-clockwise about the origin by an angle $\theta$, as depicted below:

[Image here]

In the matrix representation of $\mathbf{R}_\theta$ and vector representation of $\mathbf{x}$ and $\mathbf{x}'$, we use the natural basis $\mathbf{e_1}, \mathbf{e_2}$, where

$$\mathbf{e_1} = \begin{pmatrix} 1 \\ 0 \end{pmatrix},\quad \text{ and }\quad \mathbf{e_2} = \begin{pmatrix} 0 \\ 1 \end{pmatrix}.$$

This basis is _complete_ because every vector in $\mathbb{R}^2$ can be written as a linear combination of $\mathbf{e_1}$ and $\mathbf{e_2}$. This basis is _orthonormal_ because every vector in the basis has unit length, and is orthogonal to (i.e. has an inner product of zero with) every other basis vector:

$$\lVert \mathbf{e_1} \rVert = \lVert \mathbf{e_2} \lVert = 1, \qquad \text{ and }\quad  \mathbf{e_1}^T\mathbf{e_2} = 0$$

Since $\mathbf{e_1}, \mathbf{e_2}$ is complete and orthonormal, every
 vector in $\mathbb{R}^2$ can be represented uniquely in this basis. However, this is not the only complete orthonormal basis in which we could represent $\mathbf{x}$. For example, we could just as easily use the $45^\circ$ rotated basis with unit vectors $\mathbf{u_1} = \mathbf{R}_{(\pi/2)}\mathbf{e_1}$ and $\mathbf{u_2} = \mathbf{R}_{(\pi/2)}\mathbf{e_2}$. In this basis, we uniquely represent vectors in $\mathbb{R}^2$ using the coordinates $(u_1, u_2)$, as illustrated below:

[Image here]

The 


So what does this have to do with quantum mechanical operators? So far, we have seen operators that act on _functions_, not vectors. Remarkably, it is the case that most well-behaved functions can actually be represented as vectors. At first glance, this may seem impossible, since we typically think of functions as continuous objects and vectors as discrete objects.

## Dirac Wave Function Notation







