---
title: 'Intro to Quantum Mechanics (Part II)'
date: 2022-11-20
permalink: /posts/2022/11/quantum2/
tags:
  - Physics
  - Statistics
---

This is Part II of my blog post series giving an introduction to the basic principles of quantum mechanics. In this post, we will cover the importance of linearity in quantum mechanics and develop some intuition regarding the relationship between linear operators (which act on functions) and matrices (which act on vectors).

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

For waves in one dimension, the Hamiltonian takes the form

$$\hat{\mathbf{H}} \equiv \frac{\hat{\mathbf{p}}^2}{2m} + V(x) \equiv \frac{-\hbar^2}{2m}\dfrac{\partial^2}{\partial x^2} + V(x),$$

where $V(x)$ is some known potential function. Earlier, we remarked that the Hamiltonian operator $\hat{\mathbf{H}}$ was _linear_. Let's take some time to discuss what this means. If an operator $\hat{\mathbf{O}}$ is linear, it must (by definition) distribute over scalar multiplication and addition. This means that for any two wave functions $\psi_1(x,t)$, $\psi_2(x,t)$ and any two scalars $a_1, a_2$  $\hat{\mathbf{O}}$ must satisfy

$$\hat{\mathbf{O}}[a_1\psi_1(x,t) + a_2\psi_2(x,t)] = a_1[\hat{\mathbf{O}}\psi_1(x,t)] + a_2[\hat{\mathbf{O}}\psi_2(x,t)].$$

From this definition, we see that linearity is quite a useful property. For example, consider a complex wave function as some linear combination of simpler wave functions
$$\psi = \sum_n a_n\psi_n,$$

In this case, we say that $\psi$ is a _superposition_ of the wave functions $\psi_n$. Suppose we know that that applying $\hat{\mathbf{O}}$ to each wave function $\psi_n$ gives us another wave function $\widetilde{\psi_n}$:
$$\hat{\mathbf{O}}\psi_n = \widetilde{\psi_n}.$$ 

Then, applying $\hat{\mathbf{O}}$ to a linear combination of the $\psi_n$ wave functions is equivalent to the same linear combination of the results of applying $\hat{\mathbf{O}}$ directly to the $\psi_n$ wave functions:

$$\hat{\mathbf{O}}\psi = \hat{\mathbf{O}}\left[\sum_n a_n\psi_n \right] = \sum_n a_n\hat{\mathbf{O}}\psi_n = \sum_n a_n\widetilde{\psi_n}$$

In the previous blog post, we encountered two linear operators: the Hamiltonaian $\hat{\mathbf{H}}$ and the momentum operator $\hat{\mathbf{p}}$, but these are not the only linear operators in quantum mechanics. Remarkably,  linearity an essential property that _all operators representing physically observable quantities_ must have in quantum mechanics. While linearity may seem like just a useful mathematical property, the fact that quantum operators are linear has significant ramifications in the nature of physical reality. For example, linearity implies that the wave function of one particle cannot be "copied" onto the wave function of another particle. (Perhaps we will explore this in more detail in a future post!) In this post, we will focus on building some connections between the linear algebra of vectors and matrices, and linear operators. In fact, we will show that in many situations, linear operators can be represented as matrices and vice versa. This means that all of our understanding of linear algebra can, in fact, be translated to an understanding of linear operator theory. Before we begin, let's briefly review the basic concepts of vectors and vector bases from linear algebra.


## Bases for Vectors and Functions
If you think back to your linear algebra course, you may recall that linear transformations can be realized as a _matrix_ represented in a _complete orthonormal_ basis of vectors. For example, a rotation of a 2D vector $\mathbf{x} = \begin{pmatrix} x_1 & x_2 \end{pmatrix}^T \in \mathbb{R}^2$ by an angle $\theta$ can be written as the product of a rotation matrix $\mathbf{R}_\theta$ times $\mathbf{x}$:

$$\mathbf{x'} = \mathbf{R}_\theta\mathbf{x} = \begin{pmatrix} \cos(\theta) & -\sin(\theta) \\ \sin(\theta) & \cos(\theta) \end{pmatrix}\begin{pmatrix} x_1 \\ x_2 \end{pmatrix} = \begin{pmatrix} x_1\cos(\theta) - x_2\sin(\theta) \\ x_1\sin(\theta) + x_2\cos(\theta)\end{pmatrix}$$

This matrix corresponds to a rotation operation $\mathbf{R}_\theta$, which rotates a vector counter-clockwise about the origin by an angle $\theta$, as depicted below for the vector $\mathbf{x} = \begin{pmatrix} 1 & 2 \end{pmatrix}^{T}$:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/rotation-matrix-1.svg'>
</p>
</div>

In the matrix representation of $\mathbf{R}_\theta$ and vector representation of $\mathbf{x}$ and $\mathbf{x}'$, we use the natural basis $\mathbf{e_1}, \mathbf{e_2}$, where

$$\mathbf{e_1} = \begin{pmatrix} 1 \\ 0 \end{pmatrix},\quad \text{ and }\quad \mathbf{e_2} = \begin{pmatrix} 0 \\ 1 \end{pmatrix}.$$

This basis is _complete_ because every vector in $\mathbb{R}^2$ can be written as a linear combination of $\mathbf{e_1}$ and $\mathbf{e_2}$. This basis is _orthonormal_ because every vector in the basis has unit length (i.e. $\lVert \mathbf{e}_n\lVert = 1$) and is orthogonal to (i.e. has an inner product of zero with) every other basis vector:

$$\lVert \mathbf{e_1} \rVert = \lVert \mathbf{e_2} \lVert = 1, \qquad \text{ and }\qquad  \mathbf{e_1}^T\mathbf{e_2} = 0$$

Since $\mathbf{e_1}, \mathbf{e_2}$ is complete and orthonormal, every vector in $\mathbb{R}^2$ can be represented in this basis as a unique linear combination $\mathbf{x} = x_1\mathbf{e}_1 + x_2\mathbf{e}_2$. As a consequence of orthonormality, the coeffiecients $x_1$ and $x_2$ are obtained by evaluating the inner product between the vector $\mathbf{x}$ and each unit basis vector $\mathbf{e_n}$: $x_n = \mathbf{x}^T\mathbf{e}_n$. Visually, this inner product can be represented as the "projection" or "component" of $\mathbf{x}$ in the direction of the basis vector $\mathbf{e}_n$:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/rotation-matrix-2.svg'>
</p>
</div>

## The Fourier Series for Periodic Functions on $[-\ell,\ell]$

So what does all this matrix algebra have to do with quantum mechanical operators? So far, we have encountered operators like the Hamiltonian $\hat{\mathbf{H}}$ that act on _functions_, not vectors. Remarkably, it is the case that most well-behaved functions can actually be represented as vectors, even though we might typically think of functions as continuous objects and vectors as discrete objects. 

As a simple motivating example, let's consider the set of 1-dimensional periodic wave functions $\psi(x, t)$ at a fixed time $t = 0$ that are periodic on the interval $[-\ell, \ell]$ for some length scale $\ell > 0$. In the remainder of this post, we will write $\psi(x)$ instead of $\psi(x,0)$, since $t = 0$ is implied. For now, we are also considering only functions with period $2\ell$, satisfying $\psi(x) = \psi(x + 2\ell N)$ for all integers $N$. As an example, consider the following real wave wave function $\psi(x)$:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/fourier_function.svg'>
</p>
</div>


It turns out that we can "discretize" periodic wave functions by writing them as a [Fourier series](https://en.wikipedia.org/wiki/Fourier_series) in terms of a complete orthogonal basis of harmonic wave functions. For functions with spatial period $2\ell$, the Fourier series takes the form

$$\psi(x,0) = \sum_{n=-\infty}^{\infty} c_n e^{i2\pi n x/(2\ell)},$$

where $n$ is restricted to only integer values ($n$ = ..., -2, -1, 0, 1, 2, ... etc.). The constants $c_n$ are complex numbers called _Fourier coefficients_. If the initial wave function $\psi(x,0)$ is entirely real-valued, then the coefficients $c_n$ are such that $c_n = (c_{-n})^{\*}$. (Here, we use $^{*}$ to denote the [complex conjugate](https://en.wikipedia.org/wiki/Complex_conjugate)). In this case, we can expand $\psi(x,0)$ as a series of sine and cosine functions

$$\psi(x,0) = a_0 + \sum_{n=1}^{\infty} a_n\sqrt{2}\cos(2\pi n x) + b_n\sqrt{2} \sin(2\pi n x),$$

where $a_n$ and $b_n$ are real coefficients obtained from the $c_n$:

$$a_0 = c_0, \qquad a_n = \frac{c_n + c_{-n}}{\sqrt{2}}, \qquad b_n = \frac{i(c_n - c_{-n})}{\sqrt{2}}$$

By using a Fourier series, we can decompose any initial wave function $\psi(x,0)$ as a superposition of harmonic waves $e^{i2\pi n x}$ or even sines and cosines with real coefficients when $\psi$ is real-valued. If we truncate this infinite series summation to only include the terms where $\lvert n\lvert \le n_{\max}$, we can obtain a close approximation to the original wave function $\psi(x)$ that becomes more accurate as more terms are included:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/fourier_series.svg'>
</p>
</div>

Using the complex coefficients $c_n$ (or, if you prefer, the real $a_n$ and $b_n$ coefficients for a real $\psi$), we can represent an initial wave function $\psi$ as an "infinite-dimensional" discrete vector representation. Here, we will denote this representation as $\vec{\psi}$:

$$\psi(x,0) \quad\Leftrightarrow\quad \vec{\psi} = \begin{pmatrix} \vdots \\ c_{-2} \\ c_{-1} \\ c_0 \\ c_1 \\ c_2 \\ \vdots\end{pmatrix} \quad\qquad \text{or}\quad\qquad  \psi_{\text{real}}(x,0) \quad\Leftrightarrow\quad\vec{\psi}_{\text{real}} = \begin{pmatrix} \vdots \\ b_{2} \\ b_{1} \\ a_0 \\ a_1 \\ a_2 \\ \vdots\end{pmatrix}$$

In this vector representation, each basis vector $\mathbf{e_n}$ corresponds to a single harmonic wave $e^{i2\pi nx}$ or a real sine/cosine function ($\cos(2\pi n),\ \sin(2\pi n)$). These two sets of functions all exhibit orthogonality with respect to an inner product called the [$L^2$ inner product](https://mathworld.wolfram.com/L2-Space.html). The $L^2$ inner product of two (complex-valued) functions $f(x)$ and $g(x)$ with period interval $[-\ell, \ell]$ is denoted by $\langle f \vert g\rangle_{[-\ell, \ell]}$ and is computed by integrating the product of the complex conjugate of the left function with the right over the period:

$$\boxed{\langle f | g\rangle_{[-\ell, \ell]} = \int_{-\ell}^{\ell} f(x)^* g(x)\ dx}$$

Though it may seem unintuitive at first, the $L^2$ inner product has many of the same properties of inner products you may be familiar with, such as the real vector inner product (i.e. $\mathbf{a}^{T}\mathbf{b}$) for $\mathbf{a},\mathbf{b}$ in $\mathbb{R}^n$, and the complex-valued vector inner product for $\mathbf{a},\mathbf{b}$ in $\mathbb{C}^n$, given by

$$\mathbf{a}^{\dagger}\mathbf{b} = (\mathbf{a}^*)^T\mathbf{b} = \sum_{i=1}^n \mathbf{a}_i^*\mathbf{b}_i, $$


where $\mathbf{a}^{\dagger} = (\mathbf{a}^*)^T$ denotes the complex conjugate transpose of $\mathbf{a}$. We can use the $L^2$ inner product to compute a function norm, much like how we compute the vector norm $\lVert \mathbf{a} \rVert = \sqrt{\mathbf{a}^{\dagger}\mathbf{a}}$:

$$\lVert f \rVert_{[-\ell,\ell]} = \sqrt{\langle f | f\rangle_{[-\ell,\ell]}} = \int_{-\ell}^{\ell} |f(x)|^2\ dx$$

The $L^2$ inner product on $[-\ell,\ell]$ can be evaluated for linear combinations of functions much like the regular vector inner product; however, we must be careful to remember the complex conjugation of the left-hand arguments:

$$\begin{aligned}\langle (a_1f_1(x) + a_2f_2(x))| (b_1g_1(x) + b_2 g_2(x))\rangle_{[-\ell, \ell]} &= a_1^*b_1\langle f_1 | g_1 \rangle_{[-\ell, \ell]} \ + a_1^*b_2\langle f_1 | g_2 \rangle_{[-\ell, \ell]}  \\ &\quad + a_2^*b_1\langle f_2 | g_1 \rangle_{[-\ell, \ell]} \ + a_2^*b_2\langle f_2 | g_2 \rangle_{[-\ell, \ell]} \end{aligned}$$

Now that we have a basic understanding of $L^2$ inner products, we are equipped to discuss the relationship between matrices and linear operators in greater detail.

### Orthonormality and the Fourier Series

Earlier, we said that the Fourier series representation let us expand any well-behaved complex-valued function as a infinite vector of coefficients $c_n$ for each $n$ in $\{ .., -1, 0, 1, 2, ...\}$. We also claimed that the basis functions $f_n(x) = e^{i2\pi n x/\ell}$ served as an orthogonal basis for these kinds of functions, which we will now verify is indeed true. After evaluating the the $L^2$ inner product on $[-\ell, \ell]$, for any two $f_n, f_{n'}$ with $n \neq n'$, we obtain

$$\begin{aligned} \langle f_n | f_{n'} \rangle_{[-\ell, \ell]} &= \int_{-\ell}^{\ell} \left(e^{-i2\pi nx/(2\ell)}\right)\left(e^{i2\pi n'x/(2\ell)}\right)\ dx \\ &= \int_{-\ell}^{\ell} e^{i\pi x(n-n')/\ell}\ dx \\ &=  \frac{\ell}{\pi(n-n')}\left[ \frac{e^{i\pi x (n-n')/\ell}}{i}\right]_{x=-\ell}^{x=\ell}\\ &= \frac{2\ell}{\pi}\sin(\pi(n-n')) \\
&= 0 \end{aligned}$$

so the basis functions $f_n$ are indeed orthogonal. Using the $L^2$ norm we defined above, we can also normalize these functions to create a basis of functions that are orthonormal with respect to the $L^2$ norm, which we will denote by $\phi_n(x) = f_n(x)/\lVert f_n(x) \rVert_{[-\ell,\ell]}$. We find that the norm of each function is, in fact, only a constant factor:

$$\lVert f_n \rVert_{[-\ell,\ell]} = \sqrt{\int_{-\ell}^{\ell} \left(e^{-i2\pi nx/(2\ell)}\right)\left(e^{i2\pi nx/(2\ell)}\right)\ dx} = \sqrt{\int_{-\ell}^{\ell} 1\ dx} = \sqrt{2\ell}$$

If we re-scale the coefficients $c_n$ in our complex Fourier series such that $c_n^{(new)} = \sqrt{2\ell} c_n^{(old)}$, we can re-write the expansion of any function $\psi(x)$ as an _orthonormal Fourier series_:

$$\boxed{ \psi(x) = \sum_{n=-\infty}^{\infty} c_n \phi_n(x) = \sum_{n=-\infty}^{\infty}c_n \frac{e^{i2\pi nx/(2\ell)}}{\sqrt{2\ell}} }$$

For real-valued wave functions $\psi(x)$, we can also re-scale the real coefficients $a_n^{(new)} = \sqrt{2\ell} a_n^{(old)}$ and $b_n^{(new)} = \sqrt{2\ell}b_n^{(old)}$ to obtain the orthonormal Fourier series

$$\boxed{ \psi_{\text{real}}(x) = a_0 + \sum_{n=1}^{\infty} a_n C_n(x) + b_n S_n(s) }$$

where $C_n$ and $S_n$ are the basis functions

$$C_n(x) = \frac{1}{\sqrt{\ell}}\cos(2\pi n x),\qquad S_n(x) = \frac{1}{\sqrt{\ell}}\sin(2\pi n x).$$


In this new form of the Fourier series, the basis functions $\phi_n$ (or $C_n$ and $S_n$ for the real case) are all _orthonormal_, meaning they are orthogonal but they also satisfy
$$\lVert \phi_n \rVert_{[-\ell,\ell]} = \lVert C_n \rVert_{[-\ell,\ell]} = \lVert S_n \rVert_{[-\ell,\ell]} = 1.$$

We can denote the orthonormality of of a set of basis functions more compactly using the [Kronecker delta](https://en.wikipedia.org/wiki/Kronecker_delta):

$$\langle \phi_n | \phi_{n'} \rangle_{[-\ell, \ell]} = \delta_{n,n'} = \begin{cases} 0, & \text{if } n \neq n' \\ 1, & \text{if } n = n' \end{cases}$$

So why is orthonormality important here? To better understand this, let's return to the $2\times 2$ rotation matrix $\mathbf{R}\_\theta$ we were considering earlier. The matrix $\mathbf{R}\_{\theta}$ acts on the vector space $\mathbb{R}^2$, which is spanned by the set of basis vectors $\{\mathbf{e_1},\mathbf{e_2}\}$. These basis vectors are orthonormal ($\mathbf{e}\_n^T \mathbf{e}\_{n'} = \delta\_{n,n'}$). By taking the inner product with any vector $\mathbf{x} = ( x_1\ x_2 )^T$ in $\mathbb{R}^2$, we obtain the individual vector components $\mathbf{x}^T\mathbf{e}_1 = x_1$, and  $\mathbf{x}^T \mathbf{e}_2 = x_2$. In an analogous manner, we can obtain the coefficients $c_n$ of an orthonormal Fourier series expansion of a $2\ell$-periodic function $\psi(x)$ by computing inner products with respect to the orthonormal Fourier basis:

$$\boxed{ c_n = \langle \phi_n | \psi \rangle_{[-\ell, \ell]}}$$

Likewise, for real functions $\psi_{\text{real}}(x)$ the real coefficients of an orthonormal sine+cosine Fourier series are obtained as:

$$\boxed{ a_n = \langle C_n | \psi \rangle_{[-\ell, \ell]}, \qquad b_n = \langle S_n | \psi \rangle_{[-\ell, \ell]}}$$

These Fourier coefficients can be interpreted as the "projection" of $\psi(x)$ onto the "direction" (or, more appropriately, the "spatial wave frequency") associated with the basis function $\phi_n(x)$:

## Matrix Representation of Operators

Now we know how to compute Fourier coefficients, we are able to convert any initial $2\ell$-periodic wave function $\psi(x, 0)$ to an infinite vector of Fourier coefficients $\vec{\psi} = \begin{pmatrix} \dots & c_{-1} & c_0 & c_1 & \dots \end{pmatrix}^T$. Since we have an understanding of how this mapping of periodic functions to vectors works, we can now consider the question of how linear operators (like $\hat{\mathbf{H}}$ and $\hat{\mathbf{p}}$) that act on functions can be mapped to matrices that act on vector representations of functions (like $\vec{\psi}$). For simplicity, we will continue to restrict our analysis to the set of functions $\psi(x)$ that are $2\ell$-periodic. (In a future post, we will consider the case of general 1D wave functions.)

### The Momentum Operator
 First, we will recall the momentum operator, which we introduced in the previous post as

$$\hat{\mathbf{p}} = -i\hbar \dfrac{\partial}{\partial x}.$$

(recall $\hbar = h/(2\pi)$ is the reduced [Planck constant](https://en.wikipedia.org/wiki/Planck_constant).) We can verify that this operator is linear by considering how it acts on a superposition of wave functions $\psi_1(x)$ and $\psi_2(x)$:

$$\begin{aligned}\hat{\mathbf{p}}[a\psi_a(x) + b\psi_b(x)] &= -i\hbar\dfrac{\partial}{\partial x}[a_1 \psi_1(x) + a_2\psi_2(x)] \\
&= -a_1 i\hbar \dfrac{\partial}{\partial x}\psi_1(x) - a_2i\hbar \dfrac{\partial}{\partial x}\psi_2(x) \\ &= a_1 \hat{\mathbf{p}}\psi_1(x) + a_2\hat{\mathbf{p}}\psi_2(x)\end{aligned}$$

It follows that if we decompose a wave function $\psi(x)$ as a superposition of orthonormal Fourier basis functions $\phi_n(x)$ with coefficients $c_n$, we have:

$$\hat{\mathbf{p}}\psi(x) = \hat{\mathbf{p}}\left[ \sum_{n=-\infty}^{\infty} c_n \phi_n(x)\right] = \sum_{n=-\infty}^{\infty} c_n[\hat{\mathbf{p}}\phi_n(x)]$$

In order to know the matrix form of $\hat{\mathbf{p}}$, we must consider how $\hat{\mathbf{p}}$ acts on each basis state $\phi_n(x)$. We compute

$$\begin{aligned} \hat{\mathbf{p}}\phi_n(x) &= -i\hbar\dfrac{\partial}{\partial x}\left[ \frac{e^{i2\pi n x/(2\ell)}}{\sqrt{2\ell}} \right] \\ &= -i\hbar(i2\pi n/(2\ell))\left[ \frac{e^{i2\pi n x/(2\ell)}}{\sqrt{2\ell}}\right] \\
&= \frac{2\pi n \hbar}{2\ell}\phi_n \end{aligned}$$

and observe that applying the $\hat{\mathbf{p}}$ operator to each basis function $\phi_n(x)$ only adds a factor of $2\pi n \hbar/(2\ell)$. We can re-write this factor in terms of the unreduced Planck constant $h$ and the wavelength $\lambda_n = 2\ell/n$ of each basis function $\phi_n$, which yields the relation

$$\hat{\mathbf{p}}\phi_n(x) = (h/\lambda_n)\phi_n(x)$$

Writing the action of $\hat{\mathbf{p}}$ like this illustrates a re-arranged form of the Planck equation ($\lambda = h/p$), which we learned about in the previous post. When $\hat{\mathbf{p}}$ is applied to a function $\phi(x)$, the vector of Fourier coefficients must transform as:

$$\hat{\mathbf{p}}\psi(x)\qquad \Leftrightarrow\qquad \mathbf{P}\vec{\psi} = \begin{pmatrix} \vdots \\ (h/\lambda_{-1})c_{-1} \\ (h/\lambda_{0})c_{0} \\ (h/\lambda_{1})c_{1} \\ \vdots  \end{pmatrix}$$

where $\mathbf{P}$ is the matrix representation of $\hat{\mathbf{p}}$. We conclude that $\mathbf{P}$ must be a diagonal matrix, taking the form

$$\mathbf{P} = \begin{pmatrix}
\ddots & & & & \\
& h/\lambda_{-1} & & & \\
& & h/\lambda_0 & & \\
& & & h/\lambda_1 & \\
& & & & \ddots
\end{pmatrix},$$

where all off-diagonal entries are zero.

### The Free Hamiltonian Operator

We now know a matrix form of the momentum operator for $2\ell$-periodic functions, which we expanded in the orthonormal basis of the $\phi_n(x)$ functions. As a second example, let's consider the Hamiltonian operator $\hat{\mathbf{H}}$ without an external potential ($V(x) = 0$). This is often called the "Free Hamiltonian" and describes the propagation of wave functions for particles with a mass $m$ that are not spatially confined:

$$\hat{\mathbf{H}}_{\text{free}} = \frac{\mathbf{p}^2}{2m}$$

Although we could go through the same process we used to obtain the matrix form of $\hat{\mathbf{p}}$, it is easier to simply write down the matrix form in terms of the matrix $\mathbf{P}$ we solved for earlier. Since $\mathbf{P}$ is diagonal, taking the square of the matrix is  straightforward:

$$\mathbf{H}_{\text{free}} = \frac{1}{2m}\mathbf{P}^2 = \frac{1}{2m}\begin{pmatrix}
\ddots & & & & \\
& h^2/(2m\lambda_{-1}^2) & & & \\
& & h^2/(2m \lambda_0^2) & & \\
& & & h^2/(2m \lambda_1^2) & \\
& & & & \ddots
\end{pmatrix}.$$

## Conclusion
In this post, we gave a basic introduction the concept of linearity in quantum mechanics and developed some intuition regarding the connection between matrices acting on vectors and linear operators acting on functions. Specifically, we focused on the simple example of 1D wave functions with periodicity $2\ell$ and showed how these functions can be represented as discrete vectors of Fourier coefficients. We also showed how
linear operators acting on these functions can be represented as matrices acting in the basis of Fourier coefficients. This mapping between functions and vectors, as well as between linear operators and matrices will allow us to translate our knowledge of linear algebra into knowledge about linear operators.

In the next post, we will build upon this discussion, introducing the concept of wave function normalization and Dirac notation. We will also introduce the concepts of eigenfunctions, diagonalization, and unitary transformations.







