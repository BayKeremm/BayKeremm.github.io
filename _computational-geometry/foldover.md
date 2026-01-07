---
layout: post
title: "Making sense of Foldover-free maps"
order: 2
---
This is my attempt to make sense of the derivations in the article "Foldover-free maps in 50 lines of code" from Garanzha and collaborators. [The article](https://dl.acm.org/doi/10.1145/3450626.3459847).

# Reminders
In a [previous post](https://baykeremm.github.io/computational-geometry/ref-to-phy/) I derived:

$$
J_{t} = \sum_{k=0}^3 \underline{u}_{k} \otimes \nabla \lambda_{k}(\underline{x})
$$

Where $$\underline{u}_{k}$$ and $\nabla \lambda_{k}(\underline{x})$ come from to the tetrahedron $t$

$$
\begin{align}
J_{t} 
&= 
\begin{bmatrix}
\underline{a}_{1} & \underline{a}_{2} & \underline{a}_{3} 
\end{bmatrix}  \\
&=
\sum_{k=0}^3 \underline{u}_{k} (\nabla \lambda_{k}(\underline{x}))^\top \\
&= 
\sum_{k=0}^3 \underline{u}_{k}  
 \begin{bmatrix}
\frac{\partial \lambda_{j}}{\partial x_{1}}& \frac{\partial \lambda_{j}}{\partial x_{2}} & \frac{\partial \lambda_{j}}{\partial x_{3}} 
\end{bmatrix}
\end{align}
$$

Which says

$$
\underline{a}_{i} = \sum_{j=0}^3 \underline{u}_{j}  \frac{\partial\lambda_{j}}{\partial x_{i}}
$$

And we can use this to compute: 

$$
\frac{\partial \underline{a}_{i}}{\partial \underline{u}_{j}} = \frac{\partial\lambda_{j}}{\partial x_{i}}I
$$

The scalar $\frac{\partial\lambda_{j}}{\partial x_{i}}$ is called $z_{ji}$ in the article.

### Useful to remember from matrix theory
If $A$ is a square matrix, then the *minor* of the entry in the i-th row and j-th column (also called (i,j) minor or a first minor) is the determinant of the sub-matrix formed by deleting i-th row and j-th column and denoted by $M_{ij}$. The $(i,j)$ *cofactor* is obtained by multiplying the minor by $(-1)^{i+j}$.

This is used for example in Laplace's formula for the expansion of determinants, which is basically how I learned to compute determinants of larger matrix in terms of smaller ones. In words the determinant can be written as the sum of cofactors of any row or column of the matrix multiplied by the entries that generated them. 

Define the cofactor as 

$$
C_{ij} = (-1)^{i+j} M_{ij}
$$

then the cofactor expansion along the j-th column 

$$
\det A=\sum_{i=1}^n a_{ij}C_{ij}
$$

or the cofactor expansion along the i-th row 

$$
\det A=\sum_{j=1}^n a_{ij}C_{ij}
$$

Define the cofactor matrix 

$$
C = \begin{bmatrix}
C_{11} & \cdots & C_{1n} \\
\vdots & \ddots & \vdots \\
C_{n1} & \cdots & C_{nn} \\
\end{bmatrix}
$$

Then the inverse is given by 

$$
A^{-1} = \frac{1}{\det A} C^\top
$$

The special name for cofactor matrix transposed is *adjugate matrix*. 

Using the Laplace's expansion we can find the derivative below as 

$$
\frac{d}{dJ} \det J = \mathrm{C(J)}
$$ 

Where $C(J)$ is the cofactor matrix of $J$.

---

# Understanding the gradient expression
Here are the definitions from the article:

$$
\begin{align}
\phi(J) &= f_{\epsilon}(J) + \lambda g_{\epsilon}(J) \\
&= \frac{tr J^\top J}{\chi(\det J, \epsilon)^{2/3}} + \lambda \frac{\det^2J + 1}{\chi(\det J, \epsilon)}
\end{align}
$$

where 

$$
\chi(D,\epsilon) := \frac{D+\sqrt{ \epsilon^2+D^2 }}{2}
$$


and the problem we want to solve

$$
\lim_{\epsilon \rightarrow 0^+}
\underset{U}{\arg\min} \ F(U,\epsilon)
$$

discretized as 

$$
    F(U,\epsilon) :=  \sum_{t\in \mathcal{T}} \phi(J_{t}) \text{vol}(T_t)
$$

$U$ are basically the physical domain positions of the vertices, which are our inputs. 


I am interested in understanding the expression of the additive contribution to the gradient of $F$ from the tetrahedron $t$ with local indices $0 \dots 3$ and global indices $g_{0} \dots g_{3}$

$$
(\nabla F)_{g_{j}} = ?
$$

First 

$$
\frac{\partial \phi(J_{t})}{\partial  \underline{u}_{j}} = \sum_{i=1}^3 \frac{\partial \phi(J_{t})}{\partial  \underline{a}_{i}} \frac{\partial \underline{a}_{i}}{\partial  \underline{u}_{j}}
$$

Define matrix (apparently this is called Piola-Kirchhoff stress tensor [link](https://en.wikipedia.org/wiki/Piola%E2%80%93Kirchhoff_stress_tensors))

$$ 
P = \frac{\partial \phi(J_{t})}{\partial J_{t}} = 
\begin{bmatrix}
\frac{\partial \phi(J_{t})}{\partial  \underline{a}_{1}} & \frac{\partial \phi(J_{t})}{\partial  \underline{a}_{2}} & \frac{\partial \phi(J_{t})}{\partial  \underline{a}_{3}}
\end{bmatrix}
$$

$$
\frac{\partial \phi(J_{t})}{\partial  \underline{u}_{j}} = 
\sum_{i=1}^3  P(:,i) 
\frac{\partial\lambda_{j}}{\partial x_{i}}
$$


Which is equal to in matrix notation

$$
\frac{\partial \phi(J_{t})}{\partial  \underline{u}_{j}} =
P \nabla \lambda_{j}
$$

Then this is the additive contribution from index $j$ from tetrahedron $t$:

$$
(\nabla F)_{g_{j}} +=  \frac{\partial F}{\partial \underline{u}_{j}} = 
 Vol(t) P \nabla \lambda_{j}
$$

To find the matrix $P$ we need some calculus: 
For a scalar function $f$ derivative wrt to a matrix is: 

$$
\frac{\partial f}{\partial A} = 
\begin{bmatrix}
\frac{\partial f}{\partial A_{11}} &  \frac{\partial f}{\partial A_{12}} & \cdots &  \frac{\partial f}{\partial A_{1n}} \\
\vdots & \vdots & & \vdots  \\
\frac{\partial f}{\partial A_{m1}} &  \frac{\partial f}{\partial A_{m2}} & \cdots &  \frac{\partial f}{\partial A_{mn}} \\
\end{bmatrix}
$$

The result is the same size as $A$.
Then using that we can find the derivative of the below expression:

$$
\begin{align}
\mathrm{tr}J^\top J &=  \|J\|_{F}^2 = \sum_{i=1}^3 \sum_{j=1}^3 (J_{ij})^2 \\
\frac{d}{dJ} \mathrm{tr}J^\top J &=  2J\\
\end{align}
$$

--- 

Change variables to:

$$
\begin{align}
I_{2} &= \mathrm{tr}J^\top J \\
I_{3} &= \det J
\end{align}
$$

Then the first term becomes 

$$
 \frac{\partial\phi}{\partial I_{2}}  \frac{I_{2}}{\partial J} = 
 \frac{1}{\chi(I_{3}, \epsilon)^{2/3}} 2J
$$


Moving onto a bit trickier territory on first term:

$$
\frac{\partial}{\partial I_{3}} \frac{ I_{2}}{\chi(I_{3}, \epsilon)^{2/3}} = -\frac{2}{3}I_{2} \frac{\partial \chi(I_{3}, \epsilon)}{\partial I_{3}} (\chi(I_{3}, \epsilon))^{-5/3}
$$

second term: 

$$
\frac{\partial}{\partial {I_{3}} }\frac{I_{3}^2+1}{\chi(I_{3},\epsilon)} =
\frac{
(2I_{3})\chi(I_{3},\epsilon) - 
\frac{\partial \chi(I_{3}, \epsilon)}{\partial I_{3}} (I_{3}^2+1)
}{
\chi(I_{3}, \epsilon)^2
}
$$



Now combining: 

$$
\begin{align}
P &= 
 \frac{\partial\phi}{\partial I_{2}}  \frac{I_{2}}{\partial J} + \frac{\partial\phi}{\partial I_{3}}  \frac{I_{3}}{\partial J} \\
&= 
 \frac{\partial\phi}{\partial I_{2}}  2J + \frac{\partial\phi}{\partial I_{3}}  \mathrm{Cof(J)} \\
\end{align}
$$

$$
\begin{align}
P &= 
 \frac{1}{\chi(I_{3}, \epsilon)^{2/3}} 2J + 
\left( 
-\frac{2}{3} I_{2} \frac{\partial \chi(I_{3})}{\partial I_{3}} \chi(I_{3})^{-5/3} 
+\lambda
\frac{2I_{3}\chi(I_{3}) - \frac{\partial \chi(I_{3})}{\partial I_{3}} (I_{3}^2+1)}{\chi(I_{3})^2}
\right) \mathrm{Cof}(J)
\end{align}
$$


## Comparison of results 

What I derived: 

$$
\begin{align}
(\nabla F)_{g_{j}} & +=  \frac{\partial F}{\partial \underline{u}_{j}}  \\
&=  Vol(t) P \nabla \lambda_{j} \\
&= Vol(t) \sum_{i=1}^3  P(:,i) 
\frac{\partial\lambda_{j}}{\partial x_{i}}
\end{align}
$$

What paper has: 

$$
(\nabla F)_{g_{j}} += \frac{\det S}{d!} \sum_{i=1}^3z_{ji} \frac{\partial \phi}{\partial  \underline{a}_{i}}
$$
- $z_{ji} =\frac{\partial\lambda_{j}}{\partial x_{i}}$
- $\frac{\partial \phi}{\partial  \underline{a}_{i}} = P(:,i)$

---

What I have for matrix $P$

$$
\begin{align}
P &= 
 \frac{1}{\chi(I_{3}, \epsilon)^{2/3}} 2J + 
\left( 
-\frac{2}{3} I_{2} \frac{\partial \chi(I_{3})}{\partial I_{3}} \chi(I_{3})^{-5/3} 
+\lambda
\frac{2I_{3}\chi(I_{3}) - \frac{\partial \chi(I_{3})}{\partial I_{3}} (I_{3}^2+1)}{\chi(I_{3})^2}
\right) \mathrm{Cof}(J)
\end{align}
$$



what paper has (which contains my interpretation on what chi needs to take as parameter cus they do not provide it)

$$
\frac{\partial \phi}{\partial  \underline{a}_{i}} = \frac{2}{\chi(I_{3})^{2/3}} \underline{a}_{i} - \frac{1}{\chi(I_{3})} 
\left(
\frac{2}{3} \frac{I_{2}}{\chi(I_{3})^{2/3}} \frac{\partial \chi(I_{3})}{\partial I_{3}} - 2\lambda I_{3} +\lambda \frac{I_{3}^2+1}{\chi(I_{3})} \frac{\partial \chi(I_{3})}{\partial I_{3}}
\right) \underline{b}_{i}
$$


- $\underline{a}_{i} = J(:,i)$
- $\underline{b}_{i} = \mathrm{Cof}(J)(:,i)$

And it all corresponds.

# Disclaimer
I used gemini "Guided Learning" mode to check my work. It helped me derive the expressions step by step and said where to look if I went wrong, 
so it was very useful.
