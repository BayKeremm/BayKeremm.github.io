### Move nodes towards the target surface
<div style="display: flex; justify-content: center;">
  <img src="figures/cycle_of_life_deform_small.jpg" alt="New approach pipeline" style="width: 85%;" />
</div>
---
# Deformation

After segmentation, we extract an interface, but it still only approximates the target surface.

<div style="height: 1rem;"></div>

<div style="display: flex; justify-content: center; margin: 1.5rem 0 2rem;">
  <img src="figures/new_deform.png" alt="deformation" style="width: 80%;" />
</div>

<div style="height: 0.75rem;"></div>

Deformation moves the extracted interface closer to the target while keeping tetrahedra valid.

---

# Deformation Energy
Add one term that pulls interface vertices toward the target surface.

<div style="display: grid; grid-template-columns: 1.35fr 0.85fr; gap: 2.75rem; align-items: center; margin-top: 1.75rem;">
  <div style="font-size: 0.9em;">
    $$
    \begin{aligned}
    E(U) ={}&
    \underbrace{(1-\theta)E_{\mathrm{shape}}(U)
    + \theta E_{\mathrm{volume}}(U)}_{\text{keep tetrahedra well-shaped}} \\
    &+ \underbrace{\lambda \sum_{u \in \Gamma_h}
    \left\langle u - p(u), n_u \right\rangle^2}_{\text{attract interface to target}}
    \end{aligned}
    $$
  </div>
  <div style="font-size: 0.78em; line-height: 1.6;">
    <div style="font-weight: 700; margin-bottom: 0.75rem;">Notation</div>
    <ul style="margin: 0; padding-left: 1.1em;">
      <li>\(U\): vertex positions</li>
      <li>\(\Gamma_h\): interface</li>
      <li>\(p(u)\): closest target point</li>
      <li>\(n_u\): target normal</li>
      <li>\(\lambda\): attraction strength</li>
    </ul>
  </div>
</div>

<div style="margin-top: 2.25rem; padding: 0.85rem 1rem; border-left: 0.25rem solid #64748b; background: rgba(100, 116, 139, 0.12); font-size: 0.86em; line-height: 1.45;">
  The attraction term is a <strong>point-to-plane distance</strong>: it moves each interface vertex along the target normal while the shape and volume terms prevent invalid tetrahedra.
</div>

