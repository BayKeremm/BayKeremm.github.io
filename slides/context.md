<div style="margin-top: 2.3rem; text-align: center;">

# Conformal Meshes in X-Mesh 3D

### Volume Fitting to Target Surface Mesh

<div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 0.65rem; width: 76%; margin: 1.5rem auto 1.8rem; align-items: end;">
  <div style="height: 190px; display: flex; align-items: center; justify-content: center;">
    <img src="figures/blue_bimba_1.png" alt="Red Bimba input" style="max-width: 100%; max-height: 100%; object-fit: contain;" />
  </div>
  <div style="height: 190px; display: flex; align-items: center; justify-content: center;">
    <img src="figures/red_bimba_1.png" alt="Blue Bimba input" style="max-width: 100%; max-height: 100%; object-fit: contain;" />
  </div>
  <div style="height: 190px; display: flex; align-items: center; justify-content: center;">
    <img src="figures/blue_bimba_2.png" alt="Red Bimba result" style="max-width: 100%; max-height: 100%; object-fit: contain;" />
  </div>
  <div style="height: 190px; display: flex; align-items: center; justify-content: center;">
    <img src="figures/red_bimba_2.png" alt="Blue Bimba result" style="max-width: 100%; max-height: 100%; object-fit: contain;" />
  </div>
</div>

<div style="height: 0.75rem;"></div>

Kerem Okyay

May 2026
</div>

---

# The problem context

We have a tetrahedral mesh (domain) and a surface triangulation (explicit interface).

These two meshes are independent.

<div style="height: 1.25rem;"></div>

<div style="display: flex; gap: 5rem; justify-content: center; align-items: center; margin-top: 2rem;">
  <img src="figures/domain.png" alt="Domain mesh" style="width: 30%;" />
  <img src="figures/target_surface.png" alt="Target surface" style="width: 30%;" />
</div>

---

# The Goal

Make the domain conformal to the target surface **by only moving the vertices**.

<div style="height: 1.25rem;"></div>

<div style="display: flex; gap: 5rem; justify-content: center; align-items: center; margin-top: 2rem;">
  <img src="figures/new_spot_domain.png" alt="Cross section spot" style="width: 60%;" />
  <img src="figures/new_spot.png" alt="Approximation spot" style="width: 60%;" />
</div>

Highly constrained problem.

---
# Stages of the pipeline
<div style="display: flex; justify-content: center;">
  <img src="figures/cycle_of_life_small.jpg" alt="New approach pipeline" style="width: 90%;" />
</div>
