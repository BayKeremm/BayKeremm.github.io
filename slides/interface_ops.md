# On being 2-manifold
<div style="width: 70%; margin: 0 auto; text-align: center;">
    <img src="figures/slechte_dingen.png" alt="Nonmanifold features" style="width: 85%;" />
</div>

---

# Vertex repair

<!-- Link of a vertex in tetrahedral mesh is a triangulation of a sphere. Labels of tets -->
<!-- partition this sphere. Manifold when there is one inside and one outside partition -->
$$
\operatorname{Lk(v_j)} = \text{triangulation of a sphere}
$$


<div style="width: 70%; margin: 0 auto; text-align: center;">
    <img src="figures/vertex_repair.png" alt="Nonmanifold features" style="width: 75%;" />
</div>

Iterate and change labels until one connected component of inside and outside.

---

# Edge repair
<!---->
<!-- Link of an edge in a tet mesh is a cycle of tets. Count of label changes in the cycle -->
<!--   needs to be 2 for a manifold edge. -->
$$
\operatorname{Lk(e_{ij})} = \text{cycle of tetrahedra}
$$

<div style="width: 70%; margin: 0 auto; text-align: center;">
    <img src="figures/edge_repair.png" alt="Nonmanifold features" style="width: 75%;" />
</div>

Iterate and change labels until one connected component of inside and outside.
