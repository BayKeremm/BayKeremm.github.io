---
layout: post
title: "Introduction to Conjugate Gradient - 1"
order: 3
---

Part 1 of my notes from the very good article "An Introduction to the Conjugate Gradient Method Without the Agonizing Pain" by Jonathan Richard Shewchuck (1994) for future reference.

---

## The setting

$$
\begin{bmatrix}
A_{11} & A_{12} & \cdots & A_{1n} \\
A_{21} & A_{22} & \cdots & A_{2n} \\
\vdots & \vdots & \ddots & \vdots  \\
A_{n1} & A_{n2} & \cdots & A_{nn} \\
\end{bmatrix} 
\begin{bmatrix}
x_{1} \\
x_{2} \\
\vdots \\
x_{n} 
\end{bmatrix}
=
\begin{bmatrix}
b_{1} \\
b_{2} \\
\vdots \\
b_{n} 
\end{bmatrix}
$$

The Conjugate Gradient (CG) method is a popular method for solving large systems of linear equations. $x$ is an unknown, $b$ is a known vector, and $A$ is a known, square, symmetric, positive-definite (or positive-indefinite) matrix. 

CG is an iterative method and it is suited for use with sparse matrices. If $A$ is **dense** the best course of action is likely to factor $A$ and solve the equation by backsubstitution. 


**Aside:** An interesting intuition for such an equation is that the solution $x$ lies at the intersection point of $n$ hyperplanes, each having dimension $n-1$. 

---

**Definition: Positive-definite**
A matrix $A$ is *positive-definite* if, for every nonzero vector $x$,

$$
x^TAx > 0
$$

---

## The Quadratic Form
A *quadratic form* is simply a scalar, quadratic function of a vector with the form 

$$
f(x) = \frac{1}{2}x^TAx -b^Tx +c
$$

The *gradient* of a quadratic form is the column vector

$$
\nabla f(x) = 
\begin{bmatrix}
\frac{\partial}{\partial x_{1}}f(x) \\
\frac{\partial}{\partial x_{2}}f(x) \\
\vdots \\
\frac{\partial}{\partial x_{n}}f(x) \\
\end{bmatrix}
$$

which points to the direction of the greatest increase of $f(x)$. 

$$
\begin{align}
\frac{\partial}{\partial x_{k}} x^TAx &= \frac{\partial}{\partial x_{k}} \left( \sum_{i=1}^n \sum_{j=1}^n x_{i}Ax_{j}\right) \\

&=  \underbrace{\sum_{j=1}^n A_{kj}x_{j}}_{i=k}  + \underbrace{\sum_{i=1}^n x_{i}A_{ik}}_{j=k} \\
&= A(k,:) x + A(:,k)^Tx \\
\end{align}
$$

The $k$-th component of the gradient is dot product of the row $k$ with $x$ and the dot product of column $k$ with x

$$
\nabla x^TAx = Ax + A^Tx
$$

Now we can find 

$$
\nabla f(x) = \frac{1}{2} Ax + \frac{1}{2} A^Tx - b
$$

and if $A=A^T$ 

$$
\nabla f(x) =  Ax - b
$$

If you set the gradient to zero it is the linear system I would like to solve. Which means that the solution to $Ax=b$ is a *critical point* of $f(x)$. 

If $A$ is symmetric and positive-definite then the solution $x$ to $Ax=b$ is something special. 
Say we pick a point as $p=x+e$,

$$
\begin{align}
f(x+e) &= \frac{1}{2} (x+e)^TA(x+e) - b^T(x+e) + c \\
&= \frac{1}{2}(\underline{x^TAx}+x^TAe+e^TAx+e^TAe) \underline{-b^Tx}-b^Te +\underline{c}  \\
&= f(x) + e^T\underbrace{Ax}_{b} + \frac{1}{2} e^TAe - b^Te \quad \quad (\text{used }A^T=A) \\
&= f(x) + \frac{1}{2}e^TAe
\end{align}
$$

A is PD, which means this result is $f(x) + \text{positive}$ which means $x$ is a critical point that *minimizes* $f$. 

---
**Intuitions**
- If $A$ is PD $\rightarrow$ the $f(x)$ is a paraboloid. 
- If $A$ is negative-definite $\rightarrow$ upside down of the case of $A$ is PD
- If $A$ is singular $\rightarrow$ No solution is unique, the set of solutions is a line or hyperplane having uniform value for $f$. 
- $A$ none of the above $\rightarrow$ $x$ is a saddle point (where CG and steepest descent do not like)

The values $b$ and $c$ in the quadratic form determine the minimum point of the paraboloid but do not affect its shape.

## The Method of Steepest Descent

**Idea:** Take an arbitrary start point and slide down to the bottom of the paraboloid through series of steps that update the starting point. The step is taken in the direction of the steepest descent which is $-\nabla f(x_{(i)})= b-Ax_{(i)}$. 

- The *error* $e_{(i)}=x_{(i)}-x$ tells how far we are away from the solution 
- The *residual* $r_{(i)}=b-Ax_{(i)}$ indicates how far we are from the correct value of $b$

$$
\begin{align}
r_{(i)}&=b-Ax_{(i)} \\
&= Ax-Ax_{(i)} \\
&=Ae_{(i)}
\end{align}
$$

Think of the residual is the error transformed by $A$ into the same space as $b$ (for linear problems).

Also 

$$
r_{(i)} = b-Ax_{(i)} = -f'(x_{(i)})
$$

which suggests we can think of the residual as the direction of the steepest descent. 

**To keep in mind:** Residual = direction of the steepest descent

### Line search 
Suppose we start with $x_{(0)}$, the first step is along the direction $r_{(0)}$. 

$$
x_{(1)} = x_{(0)} + \alpha r_{(0)} 
$$

The question line search answers is "what is $\alpha$?"

**Observation:** When $A$ is positive definite, the $f(x)$ is a paraboloid. Now, we draw a line along the direction of $r_{(0)}$ that starts at $x_{(0)}$ and extends in the direction of the negative gradient. This is like getting a cross section of the paraboloid which is a parabola defined by the intersection of the line and the paraboloid. 

Now, the goal is to select the minimum of this parabola (cross section) which corresponds to the directional derivative of $f$ wrt to $\alpha$ being zero. 

$$
\begin{align}
f(x_{(1)}) &= f(x_{(0)}+\alpha r_{(0)}) \\
\frac{d}{d\alpha} f(x_{(1)}) &= f'(x_{(1)})^T \frac{d}{d\alpha}x_{(1)} \\
&= f'(x_{(1)})^T r_{(0)} \\
&= 0
\end{align}
$$

Meaning, $\alpha$ should be chosen so that $r_{(0)}$ and $f'(x_{(1)})$ are orthogonal. 

**Another perspective on why orthogonality at the minimum:** The slope of the parabola at any point and the gradient of the quadratic form are related. The slope at any point is equal to the magnitude of the gradient projected onto the line. When this projection is not zero, that means we can actually go a bit further to reach a lower value for $f$. So when the projection is 0, this means we are at the minimum of the parabola. 

Therefore $\alpha$ is chosen to satisfy this orthogonality.  From definition $f'(x_{(1)}) = -r_{(1)}$

$$
\begin{align}
r_{(1)}^T r_{(0)} &= 0 \\
(b-Ax_{(1)})^Tr_{(0)} &= 0 \\
(b-A(x_{(0)}+\alpha r_{(0)}))^Tr_{(0)} &= 0 \\
(b-Ax_{(0)})^Tr_{(0)}-\alpha (Ar_{(0)})^Tr_{(0)} &= 0 \\
(b-Ax_{(0)})^Tr_{(0)} &= \alpha (Ar_{(0)})^Tr_{(0)}  \\
r_{(0)}^Tr_{(0)} &= \alpha (Ar_{(0)})^Tr_{(0)}  \\
\alpha &= \frac{r_{(0)}^Tr_{(0)}}{r_{(0)}^TAr_{(0)}}
\end{align}
$$

### Putting together 
The method of steepest descent is then

$$
\begin{align}
r_{(i)} &= b - Ax_{(i)}  \\
\alpha_{(i)} &= \frac{r_{(i)}^Tr_{(i)}}{r_{(i)}^TAr_{(i)}} \\
x_{(i+1)} &= x_{(i)} + \alpha_{(i)}r_{(i)} \\
(x_{(i+1)} -x &= x_{(i)} -x + \alpha_{(i)}r_{(i)} )\\
(e_{(i+1)}  &= e_{(i)} + \alpha_{(i)}r_{(i)} )\\
\end{align}
$$

---

## Convergence of Steepest descent 

### Instant convergence
In the case where $e_{(i)}$ is an eigenvector with eigenvalue $\lambda_{e}$ 

$$
\begin{align}
r_{(i)} = -A e_{(i)} &= -\lambda_{e} e_{(i)} \\
e_{(i+1)} &= e_{(i)} + \frac{r_{(i)}^Tr_{(i)}}{r_{(i)}^TAr_{(i)}} r_{(i)} \\
&= e_{(i)}+\frac{r_{(i)}^Tr_{(i)}}{r_{(i)}^TA(-\lambda_{e}Ae_{(i)})} (-\lambda_{e}e_{(i)}) \\
&= e_{(i)}+\frac{r_{(i)}^Tr_{(i)}}{\lambda_{e}r_{(i)}^Tr_{(i)}} (-\lambda_{e}e_{(i)}) \\
&= 0
\end{align}
$$

The residual points directly to the center of the ellipsoids of the paraboloid defined by $f(x)$. 
By choosing $\alpha_{(i)} = \lambda_{e}^{-1}$ gives instant convergence. 

**More general analysis**
If $A$ is symmetric, then there exists a set of $n$ orthonormal eigenvectors of $A$. Then express the error as a linear combination of eigenvectors, 

$$
e_{(i)} = \sum_{j=1}^n \xi_{j}v_{j}
$$

Then below are some expression derived from the error above

$$
\begin{align}
r_{(i)} = -Ae_{(i)} &= -\sum_{j} \xi_{j} \lambda_{j}v_{j} \\
\|e_{(i)}\|^2 = e_{(i)}^Te_{(i)} &=  \sum_{j} \xi_{j}^2 \\
e_{(i)}^TAe_{(i)} &= \left( \sum_{j}\xi_{j}v_{j}^T \right)\sum_{j} \xi_{j} \lambda_{j}v_{j} \\
&= \sum_{j} \xi_{j}^2 \lambda_{j} \\
\|r_{(i)}\|^2 = r_{(i)}^Tr_{(i)} &=  \sum_{j} \xi_{j}^2 \lambda_{j}^2 \\
r_{(i)}^TAr_{(i)} &= \sum_{j} \xi_{j} \lambda_{j}v_{j} \left( \sum_{j} \xi_{j} \lambda_{j}^2v_{j} \right) \\
&= \sum_{j} \xi_{j}^2 \lambda_{j}^3
\end{align}
$$

Back to the analysis

$$
e_{(i+1)} = e_{(i)} + \frac{\sum_{j}\xi_{j}^2 \lambda_{j}^2}{\sum_{j}\xi_{j}^2 \lambda_{j}^3} r_{(i)}
$$

If all eigenvectors have a common eigenvalue $\lambda$ then 

$$
\begin{align}
e_{(i+1)} &= e_{(i)} + \frac{ \lambda^2\sum_{j}\xi_{j}^2 }{\lambda^3\sum_{j}\xi_{j}^2 } \underbrace{r_{(i)}}_{-\lambda e_{(i)}} \\
&= 0
\end{align}
$$

This is because when all the eigenvalues are equal, the ellipsoid is spherical, hence no matter what point you start at the residual must point to the center of the sphere. Thus, again instant convergence. 

**Note:** However if there are several unequal, nonzero eigenvalues no choice of $\alpha_{(i)}$ will be able to get us to the center, then the best choice of direction has to be a compromise.
Longer components have the precedence, and that means that it is possible that some shorter components at a given iteration become larger components at a later iteration. 
But for not indeterminate increase. This is the reason why methods steepest descent and conjugate gradients are called *roughers*. 

### General convergence

**Definition: Energy norm** 

$$
\|e\|_{A} = (e^TAe)^{1/2}
$$

Minimizing $$\|e_{(i)}\|_{A}$$ is equivalent to minimizing $$f(x_{(i)})$$, why? Because,

$$
f(x_{(i)}) = f(x) + \frac{1}{2} \underbrace{(x_{(i)}-x)^TA(x_{(i)}-x)}_{\|e_{(i)}\|_{A}^2}
$$

With this we have 

$$
\begin{align}
\|e_{(i+1)}\|_{A}^2 &= e_{(i+1)}^TAe_{(i+1)} \\
&= (e_{(i)}+\alpha r_{(i)})^T A (e_{(i)}+\alpha r_{(i)}) \\

&=  \|e_{(i)}\|_{A}^2 + 2\alpha_{(i)} r_{(i)}^TAe_{(i)} + \alpha_{(i)}^2r_{(i)}^TAr_{(i)} \\
\end{align}
$$

Then focusing on 

$$
\begin{align}
2\alpha_{(i)} r_{(i)}^T\underbrace{Ae_{(i)}}_{-r_{(i)}} + \alpha_{(i)}^2r_{(i)}^TAr_{(i)} &= 
2 \frac{r_{(i)}^T r_{(i)}}{r_{(i)}^TAr_{(i)}} -r_{(i)}^Tr_{(i)} + 
 \left( \frac{r_{(i)}^T r_{(i)}}{r_{(i)}^TAr_{(i)}}\right)^2r_{(i)}^TAr_{(i)} \\


 &=  -\frac{(r_{(i)}^T r_{(i)})^2}{r_{(i)}^TAr_{(i)}}
\end{align}
$$

Plugging back in 

$$
\begin{align}
\|e_{(i+1)}\|_{A}^2 
&= \|e_{(i)}\|_{A}^2 -\frac{(r_{(i)}^T r_{(i)})^2}{r_{(i)}^TAr_{(i)}} \\
&= \|e_{(i)}\|_{A}^2 \left(1 - \frac{(r_{(i)}^T r_{(i)})^2}{r_{(i)}^TAr_{(i)}\|e_{(i)}\|_{A}^2}\right)\\
&= \|e_{(i)}\|_{A}^2 \left(1 - \frac{(\sum_{j} \xi_{j}^2 \lambda_{j}^2)^2}{(\sum_{j} \xi_{j}^2 \lambda_{j}^3) \sum_{j} \xi_{j}^2 \lambda_{j}}\right)\\
&= \|e_{(i)}\|_{A}^2 \omega^2
\end{align}
$$

The convergence analysis depends on finding an upper bound for $\omega$. But this shows already that the weights $\xi_{j}$ and eigenvalues $\lambda_{j}$ affect convergence. 

### For n=2

**Definition: Spectral condition number**
Assume $\lambda_{1} \ge \lambda_{2}$ 

$$
\kappa = \frac{\lambda_{1}}{\lambda_{2}} \ge 1
$$

**Definition: Slope of** $e_{(i)}$
Relative to the coordinate system defined by the eigenvectors 

$$
\mu = \frac{\xi_{2}}{\xi_{1}}
$$

Analysis continues to find an upper bound (worst case) for $\omega$ in the case when $n=2$

$$
\begin{align}
\omega^2 &= \left(1 - \frac{(\sum_{j} \xi_{j}^2 \lambda_{j}^2)^2}{(\sum_{j} \xi_{j}^2 \lambda_{j}^3) \sum_{j} \xi_{j}^2 \lambda_{j}}\right) \\
&= 1 - \frac{(\xi_{1}^2 \lambda_{1}^2+\xi_{2}^2 \lambda_{2}^2)^2}{(\xi_{1}^2\lambda_{1}^3+\xi_{2}^2\lambda_{2}^3)(\xi_{1}^2\lambda_{1}+\xi_{2}^2\lambda_{2})} (\frac{\xi_{1}^2\lambda_{2}^2}{\xi_{1}^2\lambda_{2}^2})^2  \\
&= 1 - \frac{(\kappa^2+\mu^2)^2}{(\xi_{1}^2\lambda_{1}^3+\xi_{2}^2\lambda_{2}^3)(\xi_{1}^2\lambda_{1}+\xi_{2}^2\lambda_{2})} \frac{1}{\frac{1}{\xi_{1}^2\lambda_{2}^3}} \frac{1}{\frac{1}{\xi_{1}^2\lambda_{2}^1}} \\
&= 1 - \frac{(\kappa^2+\mu^2)^2}{(\kappa^3+\mu^2)(\kappa+\mu^2)}  \\
\end{align}
$$

Because $A$ is fixed $\kappa$ is constant. Did the computation, $\omega$ maximizes when $\mu = \pm \kappa$. 

$$
\begin{align}
\omega^2 &\le 1 - \frac{4\kappa^4}{\kappa^5+2\kappa^4+\kappa^3} \\
&= \frac{(k-1)^2}{(k+1)^2} \\
\omega &\le \frac{\kappa-1}{\kappa+1}
\end{align}
$$

**Remarks:** The more *ill-conditioned* the matrix is (that means the larger its condition number $\kappa$), the slower the convergence of steepest descent. 

**Foreshadowing:** This upper bound for $\omega$ is also valid for $n>2$ if the condition number of a symmetric, PD matrix is defined to be 

$$
\kappa = \frac{\lambda_{\max}}{\lambda_{\min}}
$$

which is the ratio of the largest to smallest eigenvalue. 

### Convergence results for Steepest Descent 
Two results from the analysis so far

$$
\|e_{(i)}\|_{A} \le \left(\frac{k-1}{k+1}\right)^i \|e_{(0)}\|_{A}
$$

and from earlier observation $f(x_{(i)})= f(x) + \frac{1}{2}e_{(i)}^TAe_{(i)}$ we have

$$
\begin{align}
\frac{f(x_{(i)})-f(x)}{f(x_{(i)})-f(x_{(0)})} &= \frac{\frac{1}{2}e_{(i)}^TAe_{(i)}}{\frac{1}{2}e_{(0)}^TAe_{(0)}} \\
&\le \left(\frac{k-1}{k+1}\right)^{2i}
\end{align}
$$

---

TODOs to support understanding:
- Jacobi method and the difference between smoothers and roughers
- eigenvectors = principal axis of ellipses 
- proof of if $A$ is symmetric, then there exists a set of $n$ orthonormal eigenvectors of $A$.
