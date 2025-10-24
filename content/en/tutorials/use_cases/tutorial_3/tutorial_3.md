---
title: 'Tutorial 3'
description: 'Our latest updates and news'
type: blog # <- important!  "blog"
lastmod: 2025-10-10
cascade:
  disableToc: true
weight: 40
---

<!-- ---
title: "Use Case 7.3 – Air Bubble Rising in Water"
linkTitle: "7.3 Air Bubble Rising in Water"
weight: 73
description: "Demonstration of the Arbitrary Lagrangian–Eulerian (ALE) interface-tracking method in multiRegionFoam using a rising air bubble case."
--- -->

<div style="text-align: justify;max-width: 95%; width: 100%; margin: 0; padding: 0;">

# 7.3 Air Bubble Rising in Water

This example demonstrates the use of **multiRegionFoam** to implement a
**moving-mesh Arbitrary Lagrangian–Eulerian (ALE)** interface-tracking method,
originally developed by **Hirt et al. [74]** and implemented in **OpenFOAM** by
**Tuković and Jasak [75]**.

A single **air bubble** rising in still pure water is considered, following the
setup of **Duineveld [76]** whose experimental data serve for validation.  
The bubble initially has a **spherical shape** of radius _r_b_ and accelerates
from rest to its **terminal rise velocity**.

<br>

![Image 7.3.1 – Computational domain for the rising-bubble simulation](images/usecase7_3_1.png)

<br>

The computational domain comprises two regions:  
the **bubble region** and the **outer fluid region**, represented by a sphere of
radius _20 r_b_.  
The meshes consist of **polyhedral cells** inside the bubble and **prismatic
cells with polyhedral bases** in the water, as shown in **Figure 7.3.2**.  
The interface between the two fluids is defined by coincident patches — the
_interfaceShadow_ (bubble side) and _interface_ (liquid side) — where **coupled
boundary conditions** are imposed as specified in **Table 10**.

---

<br>

## Simulation Setup

Bubbles with initial radii  
_r_b = 0.5, 0.6, 0.7, 0.8, and 0.9 mm_  
were investigated.  
The physical properties of air and water are given in **Table 11**.

Simulations were performed with a **time step size of Δt = 1 × 10⁻⁵ s** until
the **terminal rise velocities** were reached.  
The numerical schemes employed are listed in **Table 12**.

---

![Image 7.3.2 – Mesh for the rising-bubble simulation](https://ianussimulation-my.sharepoint.com/personal/c_habes_ianus-simulation_de/_layouts/15/onedrive.aspx?viewid=ee2713f1%2Dc698%2D489a%2Db26d%2D146ad61a188f&ga=1&id=%2Fpersonal%2Fc%5Fhabes%5Fianus%2Dsimulation%5Fde%2FDocuments%2FProjects%2FmultiPhysicsFoam%2FImages%2FFigures%5FPaper%5FmultiRegionFoam%2FRBmesh%2Epng&parent=%2Fpersonal%2Fc%5Fhabes%5Fianus%2Dsimulation%5Fde%2FDocuments%2FProjects%2FmultiPhysicsFoam%2FImages%2FFigures%5FPaper%5FmultiRegionFoam)

<br>

{{< center >}}

| **Boundary**        | **Pressure**                 | **Velocity**                 |
| ------------------- | ---------------------------- | ---------------------------- |
|                     |                              |                              |
| **Fluid A (Water)** |                              |                              |
| interface           | `regionCoupledPressureValue` | `regionCoupledVelocityFlux`  |
| space               | `zeroGradient`               | `inletOutlet`                |
|                     |                              |                              |
| **Fluid B (Air)**   |                              |                              |
| interfaceShadow     | `regionCoupledPressureFlux`  | `regionCoupledVelocityValue` |

{{< /center >}}

{{< center >}}

<p style="font-style: italic; margin-top: 0.5em;"><strong>Table 1:</strong>Boundary Conditions for the Rising Bubble Simulation Heat</p>

{{< /center >}}

---

{{< center >}}

| **Property**                 | **Fluid B (Air)** | **Fluid A (Water)** |
| ---------------------------- | ----------------- | ------------------- |
|                              |                   |                     |
| Density [kg/m³]              | 1.205             | 998.3               |
| Dynamic viscosity [kg/(m·s)] | 1.82 × 10⁻⁵       | 1.00 × 10⁻³         |
| Surface tension [N/m]        | 0.0727            | —                   |

{{< /center >}}

{{< center >}}

<p style="font-style: italic; margin-top: 0.5em;"><strong>Table 2:</strong>Table 11 – Physical Properties for the Rising Bubble Simulation</p>

{{< /center >}}

---

{{< center >}}

| **Scheme**             | **Setting**                       |
| ---------------------- | --------------------------------- |
| Time scheme            | `ddtScheme backward`              |
| Finite volume schemes  |                                   |
| `gradScheme`           | `Gauss linear`                    |
| `divScheme div(phi,U)` | `Gauss GammaVDC 0.5`              |
| `divScheme div(phi,T)` | `Gauss linearUpwind Gauss linear` |
| `laplacianScheme`      | `Gauss linear corrected`          |
| `interpolationScheme`  | `linear`                          |
| `snGradScheme`         | `corrected`                       |
| Finite area schemes    |                                   |
| `gradScheme`           | `Gauss linear`                    |
| `divScheme`            | `Gauss linear`                    |
| `interpolationScheme`  | `linear`                          |

{{< /center >}}

{{< center >}}

<p style="font-style: italic; margin-top: 0.5em;"><strong>Table 3:</strong>Numerical Schemes for the Rising Bubble Simulation</p>

{{< /center >}}

---

## Results and Discussion

The numerical results were compared with the **experimental data** from
**Duineveld [76]** and the **numerical results** of **Tuković and Jasak [75]**.

![Image 7.3.3 – Rise velocity over time for r_b = 0.5 mm, compared with experiment](images/usecase7_3_3.png)

**Figure 7.3.3** illustrates the temporal evolution of rise velocity for a
bubble of _r_b = 0.5 mm_ until the terminal velocity is reached.

![Image 7.3.4 – Comparison of terminal rise velocity for different bubble radii](images/usecase7_3_4.png)

**Figure 7.3.4** compares the **terminal rise velocities** for multiple bubble
sizes, showing excellent agreement with reference data.

![Image 7.3.5 – Final shape of the rising bubble with r_b = 0.9 mm](images/usecase7_3_5.png)

The final bubble shape at the terminal state for _r_b = 0.9 mm_ is shown in
**Figure 7.3.5**.

These results confirm that the implemented **ALE interface-tracking method**
within **multiRegionFoam** reliably reproduces the physical behavior of rising
bubbles, even under **significant mesh deformation**, demonstrating **numerical
robustness** and **accuracy**.

---

<!-- ## Assets to Add

| **Asset ID** | **Description**                                                     | **Suggested Filename**    |
| ------------ | ------------------------------------------------------------------- | ------------------------- |
| Image 7.3.1  | Sketch of the computational domain for the rising-bubble simulation | `images/usecase7_3_1.png` |
| Image 7.3.2  | Mesh for the rising-bubble simulation                               | `images/usecase7_3_2.png` |
| Image 7.3.3  | Rise velocity over time for r_b = 0.5 mm                            | `images/usecase7_3_3.png` |
| Image 7.3.4  | Comparison of terminal rise velocity for various radii              | `images/usecase7_3_4.png` |
| Image 7.3.5  | Terminal state of the bubble (r_b = 0.9 mm)                         | `images/usecase7_3_5.png` |
| Table 10     | Boundary conditions for the rising bubble simulation                | —                         |
| Table 11     | Physical properties of fluids                                       | —                         |
| Table 12     | Numerical schemes used in the simulation                            | —                         | -->

---

## References

[74] Hirt C. W., Amsden A. A., Cook J. L. (1974). _An arbitrary
Lagrangian–Eulerian computing method for all flow speeds._ _J. Comput. Phys._
14(3): 227–253. https://doi.org/10.1016/0021-9991(74)90051-5  
[75] Tuković Ž., Jasak H. (2012). _A moving mesh finite volume interface
tracking method for surface-tension-dominated interfacial fluid flow._ _Comput.
Fluids_ 55: 70–84. https://doi.org/10.1016/j.compfluid.2011.11.003  
[76] Duineveld P. C. (1995). _The rise velocity and shape of bubbles in pure
water at high Reynolds number._ _J. Fluid Mech._ 292: 325–336.
https://doi.org/10.1017/S0022112095001546

---

</div>
