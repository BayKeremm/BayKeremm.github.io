# Classify tetrahedra "inside" or "outside"
<div style="display: flex; justify-content: center;">
  <img src="figures/cycle_of_life_segment_small.jpg" alt="New approach pipeline" style="width: 90%;" />
</div>

---

# Winding numbers
Winding numbers are a way to tell if we are inside a given shape or not.


<div style="display: flex; justify-content: center;">
  <img src="figures/winding_number.png" style="width: 90%;" />
</div>


<div style="margin-top: 0.75rem; font-size: 0.8em; color: #64748b; text-align: center;">
  Alec Jacobson, Ladislav Kavan, and Olga Sorkine-Hornung, <a href="https://doi.org/10.1145/2461912.2461916">Robust inside-outside segmentation using generalized winding numbers.</a>, 2013.
</div>


---
# Mesh Segmentation
<div style="text-align: center; margin-top: 0.7rem;">
  <div style="font-size: 1.2em; margin-bottom: 0.8rem;">Binary label per tetrahedron</div>
  <div style="display: inline-block; font-size: 1.45em; padding: 0.45rem 1.4rem; border-radius: 999px; background: #f8fafc; border: 1px solid #cbd5e1; box-shadow: 0 8px 22px rgba(15, 23, 42, 0.08);">
    $x_i \in \{0,1\}$
  </div>
  <div style="margin: 1.1rem auto 0; width: 48%;">
    <img src="figures/inside_outside.png" style="width: 100%; max-height: 330px; object-fit: contain; display: block;" />
  </div>
  <div style="margin-top: 0.9rem; font-size: 1.1em;">Find labels with a <strong>minimum cut</strong>.</div>
</div>

--

<div style="text-align: center; margin-top: 0.25rem;">
  <div style="font-size: 0.78em; letter-spacing: 0.09em; text-transform: uppercase; color: #64748b; margin-bottom: 0.35rem;">Step 1</div>
  <h2 style="margin: 0 0 0.45rem 0;">Construct the graph</h2>
  <div style="margin: 0 auto; width: 84%;">
    <img src="figures/pedagoji_1.png" style="width: 100%; max-height: 500px; object-fit: contain; display: block; transform: translateX(3%);" />
  </div>
</div>

--

<div style="text-align: center; margin-top: 0.1rem;">
  <div style="font-size: 0.78em; letter-spacing: 0.09em; text-transform: uppercase; color: #64748b; margin-bottom: 0.35rem;">Step 2</div>
  <h2 style="margin: 0 0 0.45rem 0;">Find minimum cut for $a = 0.01$</h2>
  <div style="margin: 0 auto; width: 86%;">
    <img src="figures/pedagoji_2.png" style="width: 100%; max-height: 480px; object-fit: contain; display: block; transform: translateX(3%);" />
  </div>
  <div style="display: inline-block; margin-top: 0.6rem; padding: 0.45rem 0.9rem; border-radius: 999px; background: #e0f2fe; color: #075985; font-weight: 700; box-shadow: 0 8px 20px rgba(15, 23, 42, 0.12);">
    $\text{cost}=0.7+2a$
  </div>
  <div style="margin-top: 0.65rem; font-size: 0.95em; color: #334155;">What if $a = 2$?</div>
</div>

--

<div style="text-align: center; margin-top: 0.1rem;">
  <div style="font-size: 0.78em; letter-spacing: 0.09em; text-transform: uppercase; color: #64748b; margin-bottom: 0.35rem;">Step 3</div>
  <h2 style="margin: 0 0 0.45rem 0;">$a = 2$</h2>
  <div style="margin: 0 auto; width: 78%;">
    <img src="figures/pedagoji_3.png" style="width: 100%; max-height: 480px; object-fit: contain; display: block; transform: translateX(3%);" />
  </div>
  <div style="display: inline-block; margin-top: 0.6rem; padding: 0.45rem 0.9rem; border-radius: 999px; background: #e0f2fe; color: #075985; font-weight: 700; box-shadow: 0 8px 20px rgba(15, 23, 42, 0.12);">
    $\text{cost}=1.6$
  </div>
</div>

---
# Mesh Segmentation
<div style="display: flex; gap: 4rem; justify-content: center; align-items: center; margin-top: 1.5rem;">
  <div style="width: 40%; text-align: left;">
    Assign each tetrahedron a binary label \(x_i \in \{0,1\}\) by minimizing:
    $$
    E(\underline{x}) = \underbrace{\sum_i D_i(x_i)}_{\text{Data term}} + \underbrace{\sum_{(i,j)} \alpha w_{ij} |x_i - x_j|}_{\text{Regularization term}}
    $$
  </div>
  <div style="width: 60%; text-align: center;">
    <img src="figures/flow_graph.png" alt="Flow graph" style="width: 100%;" />
  </div>
</div>

Popular way of segmenting images/meshes from 2000s.

---
# On being 2-manifold

$$
E(\underline{x}) = \sum_i D_i(x_i) + \sum_{(i,j)} \alpha w_{ij} |x_i - x_j|
$$

  Larger $\alpha$ favors a smaller interface.

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 0.7rem; width: 96%; margin: 0.8rem auto 0; align-items: end;">
  <div style="text-align: center;">
    <div style="height: 260px; display: flex; align-items: end; justify-content: center;">
      <img src="figures/no_alpha.png" style="max-width: 100%; max-height: 100%; object-fit: contain;" />
    </div>
    <div style="margin-top: 0.35rem; font-size: 0.72em; color: #000;">No regularization</div>
  </div>
  <div style="text-align: center;">
    <div style="height: 260px; display: flex; align-items: end; justify-content: center;">
      <img src="figures/alpha_1bucuk.png" style="max-width: 100%; max-height: 100%; object-fit: contain;" />
    </div>
    <div style="margin-top: 0.35rem; font-size: 0.72em; color: #000;">$\alpha = 1.5$</div>
  </div>
  <div style="text-align: center;">
    <div style="height: 260px; display: flex; align-items: end; justify-content: center;">
      <img src="figures/alpha_bes.png" style="max-width: 100%; max-height: 100%; object-fit: contain;" />
    </div>
    <div style="margin-top: 0.35rem; font-size: 0.72em; color: #000;">$\alpha = 5$</div>
  </div>
</div>

<div style="text-align: center; margin-top: 0.8rem; font-weight: 700; color: #991b1b;">
  No guarantee that the extracted interface is 2-manifold.
</div>
