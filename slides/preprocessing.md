
# Redistribute vertices around the target surface
<div style="display: flex; justify-content: center;">
  <img src="figures/cycle_of_life_preprocess_small.jpg" alt="New approach pipeline" style="width: 90%;" />
</div>

---

# Why preprocess the domain?

<div style="height: 1rem;"></div>

- **Goal**: For finer target features, we want more nodes around the interface.
- **Means:** Redistribute tetrahedra before segmentation.

<div style="height: 2rem;"></div>

<div style="display: flex; gap: 10rem; justify-content: center; align-items: center; margin-top: 1rem;">
  <img src="figures/new_bob_domain.png" alt="Without preprocessing" style="width: 35%;" />
  <img src="figures/new_bob.png" alt="With preprocessing" style="width: 35%;" />
</div>


---
# Adaptive sizing field

Prescribe **smaller** target tetrahedra near the interface.

<div style="width: 60%; margin: 1.5rem auto; text-align: center;">
  <img src="figures/sized_tets.png" alt="sized tets" style="width: 100%;" />
</div>
Then optimize for the energy with $\theta = 0.5$
<div style="display: flex; justify-content: center; align-items: center; gap: 2rem; margin-top: 1.5rem;">

  <!-- Left: mapping -->
  <div style="width: 40%; text-align: center;">
    <img src="figures/mapping.png" alt="mapping" style="width: 100%;" />
  </div>

  <!-- Right: equations -->
  <div style="width: 40%; text-align: center; font-size: 0.95em;">

$$
E_{\text{shape}} = \frac{\operatorname{tr}(J^T J)}{(\operatorname{det} J)^{2/3}}, 
\quad
E_{\text{volume}} = \frac{(\operatorname{det} J)^2 + 1}{\operatorname{det} J}
$$

$$
E = (1-\theta)E_{\text{shape}} + \theta  E_{\text{volume}}
$$

  </div>
</div>

<div style="margin-top: 0.75rem; font-size: 0.8em; color: #64748b; text-align: center;">
  Garanzha et al., <a href="https://doi.org/10.1145/3450626.3459847">Foldover-free maps in 50 lines of code</a>, 2021.
</div>


---

 # Remarks 

<div style="display: flex; justify-content: center;">
  <img src="figures/2_prep_with_torus.png" alt="New approach pipeline" style="width: 40%;" />
</div>

- Hard to get the collar region around the interface.
- For thinner shapes the tetrahedra inside can be stretched


