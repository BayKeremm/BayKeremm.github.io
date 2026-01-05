---
layout: post
title: "Reference Domain to Physical Domain"
order: 1
---

Say $\underline{u}(\underline{x})$ takes vertices from the reference domain to the physical domain.
We are interested in tetrahedra mappings. 

Reference tetrahedron: $$\underline{x}_{0}, \underline{x}_{1}, \underline{x}_{2}, \underline{x}_{3}$$

Physical tetrahedron: $$\underline{u}_{0}, \underline{u}_{1}, \underline{u}_{2}, \underline{u}_{3}$$

I know these vectors, reference ones I define, and the physical ones are the mesh tetrahedra

<p style="text-align: center;">
  <img src="/assets/images/ref_to_phy.png" alt="drawing" width="400"/>
</p>

Since this transformation is affine (since in the physical domain it stays as a tetrahedron) it should be along the lines of

$$
\underline{u}(\underline{x}) = J\underline{x}+ \underline{b}
$$

which can be put into matrix form by subtracting the equation of vector zero from other ones:

$$
\begin{bmatrix}
\underline{u}_{1}-\underline{u}_{0} &\underline{u}_{2}-\underline{u}_{0} & \underline{u}_{3}-\underline{u}_{0}
\end{bmatrix} 
= J
\begin{bmatrix}
\underline{x}_{1}-\underline{x}_{0} &\underline{x}_{2}-\underline{x}_{0} & \underline{x}_{3}-\underline{x}_{0}
\end{bmatrix} 
$$

Reminding myself that a column of left hand side is found by taking the full matrix A and the corresponding column of the matrix that contains edges of the reference tetrahedron. 

Here $J$ is the Jacobian matrix for the affine transformation $\underline{u}(\underline{x})$. 

**Aside**
Affine transformation is a combination of a linear transformation (rotation, scaling, or shearing) and a translation. 
Take a point in the tetrahedron:

$$
\underline{p} = \sum_{i=0}^3 \lambda_{i}* \underline{x}_{i}
$$

Noting that $\sum_{i} \lambda_{i} = 1$ applying the mapping

$$
\begin{align}
\underline{p}' &= \underline{u}(\underline{p}) = J\left( \sum_{i=0}^3 \lambda_{i}* \underline{x}_{i} \right)+b  \\
&= \left( \sum_{i=0}^3 \lambda_{i}* J\underline{x}_{i} \right)+ \left( \sum_{i=0}^3 \lambda_{i} \right)b  \\
&= \sum_{i=0}^3 \lambda_{i} \left( J \underline{x}_{i}+b \right) \\
&= \sum_{i=0}^3 \lambda_{i} \underline{u}_{i} \\
\end{align}
$$

The barycentric coordinates are the same.  Note that it is more like $\lambda_{i} = \lambda_{i}(\underline{p})$  barycentric coordinate is a function of position.  What type of function is that? 
More clearly pick $\underline{x}_{0}$

$$
\begin{align}
\underline{x}- \underline{x}_{0} &= \left( 1-\sum_{j=1}^3 \lambda_{j} \right) \underline{x}_{0}+\sum_{i=1}^3 \lambda_{i} \underline{x}_{i} - \underline{x}_{0} \\
&= \lambda_{1}(\underline{x}_{1}-\underline{x}_{0}) + \lambda_{2}(\underline{x}_{2}-\underline{x}_{0}) + \lambda_{3}(\underline{x}_{3}-\underline{x}_{0})  \\
&= 
  \begin{bmatrix}
\lambda_{1}  &
\lambda_{2}  &
\lambda_{3}  
\end{bmatrix}  
\begin{bmatrix}
\underline{x}_{1}-\underline{x}_{0} &\underline{x}_{2}-\underline{x}_{0} & \underline{x}_{3}-\underline{x}_{0}
\end{bmatrix}
\end{align}
$$

Then 

$$
\begin{bmatrix}
\lambda_{1}   \\
\lambda_{2}  \\
\lambda_{3}  
\end{bmatrix}   = M^{-1}(\underline{x}-\underline{x}_{0})
$$

$M$ is constant, and each $\lambda_{i}$ is 

$$
\lambda_{i}(\underline{p}) = M^{-1}(i,:) \underline{x} + \text{constant}
$$

Which is an affine function. 

**A more geometric intuition is**: Think of a facet ($$\underline{x_{1}}, \underline{x_{2}}, \underline{x_{3}}$$) and the fourth point ($$\underline{x}_{0}$$), on the facet $$\lambda_{0} = 0$$, now go to the fourth point we get $$\lambda_{0} = 1$$ the rest $$0$$. The $$\lambda_{0}$$ represents the scaled height of the fourth point above the floor defined by the facet. 

So for a vertex $i$: 

$$
\lambda_{i}(\underline{x}) = \frac{\text{volume of tet formed by } \underline{x} \text{ and face opposite to i}}{\text{volume of the full tetrahedron}}
$$

Let:
- $$F_{i}$$  facet opposite to vertex $i$
- $$\underline{n}_{i}$$ area weighted normal of $$F_{i}$$

$$
\underline{n}_{i} = \frac{1}{2} (\underline{e}_{0} \times  \underline{e}_{1})
$$

- $$A_{i} = \|\underline{n}_{i}\|$$ area of $$F_{i}$$
- Normalized normal $$\underline{\bar{n}}_{i}$$

$$\underline{\bar{n}}_{i} = \frac{\underline{n}_{i}}{A_{i}}$$

- $$V$$ is the volume of the tetrahedron ($$\frac{1}{3} \times(\text{face area}) \times (\text{height)}$$)


Given a point $$\underline{x}$$, its signed distance to $$F_{i}$$ is 

$$
h_{F_{i}}(\underline{x}) = \underline{\bar{n}}_{i} \cdot (\underline{x}-\underline{x}_{i})
$$

Then we have 

$$
\begin{align}
\text{Vol}(\underline{x}, F_{i}) &= \frac{1}{3} A_{i}h_{F_{i}}(\underline{x}) \\
&= \frac{1}{3} \underline{n}_{i} (\underline{x}-\underline{x}_{i})
\end{align}
$$

Finally using the geometric intuition

$$
\lambda_i(\underline{x}) = \frac{1}{3V} (\underline{n}_{i} \cdot (\underline{x} - \underline{x}_{i}))
$$

Now I can compute the $\nabla \lambda_{i}$

$$
\nabla \lambda_{i} = \frac{\underline{n}_{i}}{3V} = \frac{\underline{e}_{0} \times  \underline{e}_{1}}{6V}
$$

**Intuition that comes:** A barycentric coordinate is just the signed distance to the opposite face, normalized by the tetrahedron height.

### Jacobian matrix computation

$$
\underline{u} = 
\sum_{i=0}^3 \lambda_{i}(\underline{x}) * \underline{u}_{i}
$$

The Jacobian matrix definition is 

$$
J = 
\begin{bmatrix}
\frac{\partial u_{1}}{\partial x_{1}} & \frac{\partial u_{1}}{\partial x_{2}} & \frac{\partial u_{1}}{\partial x_{3}}  \\

\frac{\partial u_{2}}{\partial x_{1}} & \frac{\partial u_{2}}{\partial x_{2}} & \frac{\partial u_{2}}{\partial x_{3}}  \\

\frac{\partial u_{3}}{\partial x_{1}} & \frac{\partial u_{3}}{\partial x_{2}} & \frac{\partial u_{3}}{\partial x_{3}}  \\
\end{bmatrix} =
\frac{\partial \underline{u}}{\partial \underline{x}}
$$

$$
\begin{align}
\frac{\partial \underline{u}}{\partial \underline{x}} &= 
\frac{\partial}{\partial \underline{x}} \left( 
\sum_{i=0}^3 \lambda_{i}(\underline{x}) * \underline{u}_{i}
\right)  \\
\end{align}
$$

Focus on $u_{1}$ from $\underline{u} = (u_{1}, u_{2}, u_{3})$

$$
u_{1} = \sum_{k=0}^3\lambda_{k}(\underline{x})u_{k1}
$$

And focus on $x_{2}$ from $\underline{x} = (x_{1}, x_{2}, x_{3})$

$$
J_{12} = \frac{\partial u_{1}}{\partial x_{2}} =  \sum_{k=0}^3
 u_{k1} \frac{\partial\lambda_{k} (\underline{x})}{\partial x_{2}} 
$$

This tells me that all four vertices $\underline{u}_{k}$ contribute at row 1 column 2, and similarly actually at each entry of $J$ they all contribute.

$$
J_{mn} = \frac{\partial u_{m}}{\partial x_{n}} =  \sum_{k=0}^3
 u_{km} \frac{\partial\lambda_{k} (\underline{x})}{\partial x_{n}} 
$$

So we can write

$$
J = \sum_{k=0}^3 \underline{u}_{k} \otimes \nabla \lambda_{k}(\underline{x})
$$

Evet.
