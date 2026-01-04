---
layout: post
title: "Fundamental Theorem of Algebra"
order: 2
---
The fundamental theorem of algebra states that any polynomial $p$ with complex coefficients and of degree at least $1$ has at least one zero in $\mathbb{C}$ (Version 1). 
This is equivalent to saying, a polynomial of degree $n \ge 1$  with complex coefficients has, counting multiplicities, exactly $n$ zeros among the complex numbers (Version 2). 

#### Why equivalence?
Version 1 $\implies$ Version 2
Say order $n\ge 1$ polynomial $p(x)$ has root $r$, then this means $\exists \ q(x)$ of order $n-1$ such that 
$$
p(x) = q(x) (x-r)
$$
Then we can descent through the orders of $q(x)$ making the same action using Version 1, therefore induction on powers of $q(x)$ gives us the $n-1$ roots. Then we can see that $p(x)$
has $n$ roots.
Version 1 $\impliedby$ Version 2
If there are $n$ roots there is at least one

So if we have some proof action for one version that is enough


#### Version 1 proof
The fact I will take for granted is "Winding number is invariant under homotopy". 
What winding number and homotopy mean intuitively? Using Wikipedia I have the following.

**Winding number:** The winding number of a closed curve in the plane around a given point is an integer representing the total number of times that curve travels counterclockwise around the point. Imagine you are at the center and there is a running route around you and you follow your friend running, the winding number of the running route is how many times you made a full turn counterclockwise following your friend. 

**Homotopy:** In topology two continuous function from one spacpe to another called homotopic if one can be "continuously deformed" into the other, such a deformation being called a homotopy. 

The proof with this fact relies on tracking how the image of a circle in the complex plane winds around the origin as you change the circles radius. 

Assume monic polynomial of degree $n\ge 1$
$$
P(x) = x^n + a_{n-1} x^{n-1} + \cdots + a_{1}x + a_{0}
$$
and assume now $p(x)$ has no roots so $\nexists x \in \mathbb{C}$  such that $p(x) = 0$ 

If you pick $R \gg 0 \in \mathbb{R}$ and $x=Re^{i\theta}$ then 
$$
P(Re^{i\theta}) = {R^ne^{in\theta}} + a_{n-1} {R^{n-1}e^{i(n-1)\theta}} + \cdots + a_{1}Re^{i\theta} + a_{0}
$$
since $R$ is huge, the first term will dominate. 
$$
P(Re^{i\theta}) \approx {R^ne^{in\theta}} 
$$

Observation: 
if you take $\theta$ from $0$ to $2\pi$ the circle $e^{i\theta}$ makes one tour around the origin. 
Important to see here, $e^{i n \theta}$ then will make $n$ tours around the origin 
since we in a way accelerate with $n$. 

So we can say then when $R \gg 0$ the winding number of $P(R e^{i \theta})$ is $n$. 

Now look at the other extreme when $R \approx 0$ then we have
$$
P(Re^{i\theta}) \approx a_{0}
$$
And since we assumed there is no root of $P$ then we know that $a_{0} \neq 0$

And what would be the winding number for this case, well $0$ it does not go around the origin, it is just constant.

Now imagine we start with a huge $R$ and smoothly reduce $R$ towards $0$, 
we saw what the winding number would be at the extremes: 
- $R \gg 0$ winding number $n$
- $R \approx 0$ winding number $0$

Since we are smoothly reducing $R$ the output of $P$ (the curve it traces for our input of $R e^{i \theta}$, $\theta$ going from $0$ to $2\pi$) will also deform smoothly. 

The winding number has to be an integer from its definition, and for this homotopic deformation we have a winding number change as $R$ changes. 

So there must have been some origin crossings as we changed $R$ above. Which means that there must exist a $x \in \mathbb{C}$ where $P(x) = 0$
so there is at least one root for $P(x)$. 

