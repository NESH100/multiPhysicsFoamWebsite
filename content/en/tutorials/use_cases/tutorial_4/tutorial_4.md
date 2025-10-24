---
title: 'Tutorial 4'
description: 'Our latest updates and news'
type: blog # <- important!  "blog"
lastmod: 2025-10-10
weight: 50
cascade:
  disableToc: true
---

<!-- ---
title: "Use Case 7.4 – Channel Flow Around an Elastic Object"
linkTitle: "7.4 Channel Flow Around an Elastic Object"
weight: 74
description: "Demonstration of fluid–structure interaction (FSI) using the ALE interface-tracking method in multiRegionFoam."
--- -->

<div style="text-align: justify;max-width: 95%; width: 100%; margin: 0; padding: 0;">

# 7.4 Channel Flow Around an Elastic Object

This use case extends the **Arbitrary Lagrangian–Eulerian (ALE)**
interface-tracking method, originally developed for multiphase flow, to handle
**fluid–structure interaction (FSI)** problems within **multiRegionFoam**.

A frequently examined validation case involves **laminar incompressible flow**
around a **cylinder with an attached elastic plate**, as detailed by **Hron and
Turek [77]**.  
The configuration, shown in **Figure 7.4.1**, consists of a **horizontal
channel** containing a **rigid cylinder** with a **flexible plate** fixed to its
right-hand side.

---

## Simulation Setup

From the **left side** of the channel, the fluid enters with a **parabolic
velocity profile** whose maximum inflow velocity is **1.5 × Ū**, where _Ū_ is
the mean inflow velocity.

Three flow cases are typically studied:

- **FSI1** – Steady-state regime
- **FSI2** – Transition regime
- **FSI3** – Unsteady periodic regime

The **FSI3 case** is discussed here in detail.

The mean inflow velocity and other **fluid and solid properties** are summarized
in **Table 13**, while the **boundary conditions** are listed in **Table 14**.

![Image 7.4.1.2 – Velocity field visualisation for the rising bubble with rb = 0.9](https://ianussimulation-my.sharepoint.com/:i:/r/personal/c_habes_ianus-simulation_de/Documents/Projects/multiPhysicsFoam/Images/Figures_Paper_multiRegionFoam/RB09.0019.png?csf=1&web=1&e=q9K3gu)

![Image 7.4.1.2 – Computational domain for the FSI3 case with zoom on the structure part](images/usecase7_4_1.png)

Three **mesh refinement levels** were used:

- The **coarse mesh**, shown in Figure 7.4.1, has **5336 cells** for the fluid
  region and **630 cells** for the solid plate.
- Two finer meshes were generated to verify **grid convergence** of the
  simulation results.

<br>

{{< center >}}

| **Property**         | **Symbol** | **Unit** | **Solid** | **Fluid** |
| -------------------- | ---------- | -------- | --------- | --------- |
| Kinematic viscosity  | ν_f        | m²/s     | –         | 1 × 10⁻³  |
| Mean inflow velocity | Ū          | m/s      | –         | 2         |
| Density              | ρ          | kg/m³    | 103       | 103       |
| Young’s modulus      | E_s        | kg/ms²   | 5.6 × 10⁶ | –         |
| Poisson’s ratio      | ν_s        | –        | 0.4       | –         |

{{< /center >}}

{{< center >}}

<p style="font-style: italic; margin-top: 0.5em;"><strong>Table 1:</strong>Fluid and Solid Properties for the FSI3 Case</p>

{{< /center >}}

---

{{< center >}}

| **Boundary**    | **Velocity**         | **Kinematic Pressure** | **Traction**      |
| --------------- | -------------------- | ---------------------- | ----------------- |
| **Fluid**       |                      |                        |                   |
| interfaceShadow | `movingWallVelocity` | `zeroGradient`         | –                 |
| inlet           | `parabolicProfile`   | `zeroGradient`         | –                 |
| outlet          | `zeroGradient`       | `fixedValue (0)`       | –                 |
| walls           | `noSlip`             | `zeroGradient`         | –                 |
| **Solid**       |                      |                        |                   |
| interface       | `coupledMotion`      | –                      | `coupledTraction` |
| fixedEnd        | `fixedValue (0 0 0)` | –                      | –                 |

{{< /center >}}

{{< center >}}

<p style="font-style: italic; margin-top: 0.5em;"><strong>Table 2:</strong>Boundary Conditions for the FSI3 Case</p>

{{< /center >}}

---

## Results

The **velocity and pressure fields** in the fluid region, as well as the
**displacement and stress fields** in the solid domain, are shown in **Figure
7.4.2**.  
The **x- and y-displacements** of **point A** (the free end of the elastic
plate) for the **finest mesh** are depicted in **Figure 7.4.3**.

![Image 7.4.2 – Velocity and pressure fields (fluid) with displacement and stress fields (structure)](images/usecase7_4_2.png)

![Image 7.4.3 – x and y displacement of point A for the finest mesh](images/usecase7_4_3.png)

The computed oscillation amplitudes and frequencies closely match the reference
data of **Hron and Turek [77]**, confirming the accuracy of the coupled FSI
implementation.

---

## Discussion

- The **ALE-based interface-tracking approach** was successfully adapted for
  **fluid–structure interaction**, preserving numerical stability despite large
  mesh deformations.
- The **monolithic coupling** strategy demonstrated high robustness and
  convergence rates across all tested mesh resolutions.
- The **periodic motion** of the flexible plate in **FSI3** aligns with
  benchmark data, validating the model for both steady and unsteady FSI regimes.

---

<!-- ## Assets to Add

| **Asset ID** | **Description**                                            | **Suggested Filename**    |
| ------------ | ---------------------------------------------------------- | ------------------------- |
| Image 7.4.1  | Computational domain for the FSI3 case (zoom on structure) | `images/usecase7_4_1.png` |
| Image 7.4.2  | Velocity, pressure, displacement, and stress fields        | `images/usecase7_4_2.png` |
| Image 7.4.3  | Displacement of point A (x and y) for finest mesh          | `images/usecase7_4_3.png` |
| Table 13     | Fluid and solid properties for FSI3                        | —                         |
| Table 14     | Boundary conditions for FSI3                               | —                         |

--- -->

## References

[77] Turek, S., & Hron, J. (2006). _Proposal for numerical benchmarking of
fluid–structure interaction between an elastic object and laminar incompressible
flow._  
In H.-J. Bungartz & M. Schäfer (Eds.), **Fluid–Structure Interaction** (pp.
371–385). Springer, Berlin.  
DOI: [10.1007/3-540-34596-5_21](https://doi.org/10.1007/3-540-34596-5_21)

---

</div>
