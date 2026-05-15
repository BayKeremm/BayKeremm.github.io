# Strengths
- Good with smooth shapes
- Target only needs to be a triangle soup.
    - No need to be closed 2-manifold free of intersections 
- Fast, scales linearly with the number of cells
- Produces closed 2-manifold surfaces.
<div style="display: flex; justify-content: center; margin: 1.5rem 0 2rem;">
  <img src="figures/teapot_better.png" style="width: 90%;" />
</div>

---

# Limitations
- Thin shapes are hard
- Topology unaware attraction to the plane
- Parameter dependence on $\lambda$ and $\alpha$
- Heuristic preprocessing strategy with adaptive sizing

<div style="display: flex; justify-content: center; align-items: center; gap: 2rem; width: 88%; margin: 1.3rem auto 1.6rem;">
  <div style="width: 48%; text-align: center;">
    <img src="figures/sized_tets.png" alt="sized tets" style="width: 100%;" />
  </div>
  <div style="width: 48%; text-align: center;">
    <img src="figures/thin_problem.png" alt="Thin problem" style="width: 100%;" />
  </div>
</div>


$$
E(U) = (1-\theta)E_{\mathrm{shape}}(U) + \theta E_{\mathrm{volume}}(U) + \mathbf{\lambda} \sum_{u \in \Gamma_h} \|<u - p(u), n_u>\|^2
$$

$$
E(\underline{x}) = \sum_i D_i(x_i) + \sum_{(i,j)}\mathbf{\alpha} w_{ij} |x_i - x_j|
$$


---

# Future work

- Preprocessing to put more nodes into thin shapes.
- **Semi-discrete optimal transport** seems promising
- Topology-aware deformation
- Moving onto 3D multiphase flow simulations

<div style="display: flex; justify-content: center; align-items: flex-start; gap: 1rem; width: 94%; margin: 1rem auto 0;">
  <div style="width: 31%; text-align: center;">
    <img src="figures/initial_domain.png" alt="Initial domain" style="width: 100%;" />
  </div>
  <div style="width: 31%; text-align: center;">
    <img src="figures/prepped_domain.png" alt="Current strategy" style="width: 100%;" />
  </div>
  <div style="width: 31%; text-align: center;">
    <img src="figures/OT_domain.png" alt="OT based sizing" style="width: 100%;" />
  </div>
</div>

---

# The End

<div style="width: 82%; margin: 1rem auto 0; text-align: center;">
  <video
    src="figures/lucy.mp4"
    data-autoplay
    loop
    muted
    playsinline
    controls
    style="width: 100%; max-height: 560px; object-fit: contain;"
  ></video>
</div>
<!-- 11,243,374 tetrahedra (30 mins to generate), 3 minutes to conform to Lucy -->

