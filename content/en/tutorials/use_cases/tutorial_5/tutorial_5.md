---
title: 'Tutorial 5'
description: 'Our latest updates and news'
type: docs # <- important!  "blog"
lastmod: 2025-10-10
cascade:
  disableToc: true
---

<div style="text-align: justify;max-width: 100%; width: 100%; margin: 0; padding: 0;">

<!--

title: "Use Case 7.5 – Polymer Electrolyte Fuel Cell (PEMFC)" linkTitle: "7.5
Polymer Electrolyte Fuel Cell" weight: 75 description: "Demonstration of a
multi-physics electrochemical simulation using multiRegionFoam for a single
polymer electrolyte fuel cell (PEMFC) channel." --- -->

# 7.5 Polymer Electrolyte Fuel Cell

This use case demonstrates the capability of **multiRegionFoam** to simulate
**complex multi-physical systems**, focusing on a **single Polymer Electrolyte
Membrane Fuel Cell (PEMFC)** channel assembly.

A **PEM fuel cell** is a device that converts chemical energy from hydrogen and
oxygen into **electricity, heat, and water** through electrochemical
reactions.  
The general configuration and physical components of a typical PEMFC are shown
in **Figure 7.5.1**.

<br>

![Image 7.5.1 – General structure of a PEM fuel cell](images/usecase7_5_1.png)

<br>

## Modeling Approach

The computational model includes multiple coupled regions:

- **Gas diffusion layers (GDLs)**
- **Catalyst layers (anode and cathode)**
- **Polymer electrolyte membrane (PEM)**
- **Flow channels for reactant gases**

Each region solves its corresponding set of governing equations, which are
coupled through **electrochemical and thermal interfaces**.  
The general modeling approach is illustrated schematically in **Figure 7.5.2**.

![Image 7.5.2 – General modeling approach of the PEM fuel cell](images/usecase7_5_2.png)

The governing equations for all regions are summarized in **Table 16**.  
They include fluid flow, species transport, electrochemical potential, ionic
transport, and heat transfer equations.

<br>

{{< center >}}

| **Equation Type**        | **Region(s)**     | **Expression / Description**                                                                                                                                                                                                                                                             |
| ------------------------ | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Continuity**           | Fluid             | ∇ · (ρU) = R                                                                                                                                                                                                                                                                             |
| **Momentum**             | Fluid             | ∇ · (ρUU) = −∇p + ∇ · (μ∇U) + ρg + S<sub>Darcy</sub>                                                                                                                                                                                                                                     |
| **Species**              | Fluid             | ∇ · (ρUY<sub>i</sub>) = ∇ · (ρ D<sub>eff,i</sub> ∇Y<sub>i</sub>) + R<sub>i</sub>                                                                                                                                                                                                         |
| **Nernst potential**     | Anode             | E<sub>Nernst,ae</sub> = Σ<sub>i</sub> [ −(H<sub>i</sub>−T S<sub>i</sub>)ω − R<sub>g</sub>Tω ln p<sub>i</sub> ] / (zF)                                                                                                                                                                    |
| **Overpotential**        | Electrodes        | j<sub>ae</sub> = i<sub>ae,0</sub> [ ∏<sub>reac,i</sub>( C<sub>i</sub>/C<sub>ref</sub> )<sup>ξ</sup> exp(−α<sub>ae</sub> zF η<sub>ae</sub>/R<sub>g</sub>T) − ∏<sub>prod,i</sub>( C<sub>i</sub>/C<sub>ref</sub> )<sup>ξ</sup> exp((1 − α<sub>ae</sub>) zF η<sub>ae</sub>/R<sub>g</sub>T) ] |
| **Electrical potential** | Solid (electrode) | ∇ · (σ<sub>E</sub>∇Φ<sub>E</sub>) = J<sub>E</sub>                                                                                                                                                                                                                                        |
| **Ionic potential**      | Membrane          | ∇ · (σ<sub>I</sub>∇Φ<sub>I</sub>) = J<sub>I</sub>                                                                                                                                                                                                                                        |
| **Dissolved water**      | Membrane          | ∇ ( i/F n<sub>d</sub> ) = ρ<sub>m</sub>E<sub>W</sub> ∇ · (D<sub>eff,m</sub> ∇λ) + R<sub>λ</sub>                                                                                                                                                                                          |
| **Energy equation**      | All regions       | ρ c<sub>p</sub> U · ∇T = ∇ · (k<sub>eff</sub> ∇T) + Q                                                                                                                                                                                                                                    |

{{< /center >}}

_For detailed formulations, refer to Zhang et al. [82]._

{{< center >}}

<p style="font-style: italic; margin-top: 0.5em;"><strong>Table 1:</strong>Governing Equations for the Single-Phase PEM Fuel Cell</p>

{{< /center >}}

---

## Simulation Setup

The simulation was configured to represent **steady-state operation** of a
single cell channel.  
**Monolithic coupling** was employed to ensure consistency across
**electrochemical**, **thermal**, and **fluid** fields.

The model accounts for:

- **Mass transport** in gas diffusion layers
- **Electrochemical reactions** in catalyst layers
- **Ionic conduction** in the polymer membrane
- **Heat transfer** across all regions

Boundary conditions for electrical and thermal interfaces were defined to
reflect **current collector contact**, **reactant flow inlets**, and **coolant
flow outlets**.

---

## Results

The simulation demonstrates the distribution of **temperature**, **current
density**, and **species concentrations** across the cell.

- **Figure 7.5.3** illustrates the temperature field and current density
  distribution within the PEMFC assembly.
- **Figure 7.5.4** compares the **computed polarization curve** with
  experimental data, showing excellent agreement.

![Image 7.5.3 – Temperature and current density distribution in the PEMFC](images/usecase7_5_3.png)

![Image 7.5.4 – Comparison of computed and experimental polarization curves](images/usecase7_5_4.png)

These results confirm that the **multiRegionFoam framework** can successfully
handle **multi-physics coupling** in electrochemical systems, including **fluid
flow**, **heat transfer**, and **electrochemical kinetics**.

---

## Discussion

- The **monolithic coupling approach** provided stable convergence across all
  physical fields.
- The flexible region-based model enabled simultaneous solution of **thermal**,
  **electric**, and **ionic potentials**.
- The results are consistent with prior open-source implementations such as
  **openFuelCell** [80] and **openFuelCell2** [82].
- This case highlights the **extendability** of multiRegionFoam for **fuel
  cells**, **electrolyzers**, and **battery systems**.

---

<!-- ## Assets to Add

| **Asset ID** | **Description**                                | **Suggested Filename**    |
| ------------ | ---------------------------------------------- | ------------------------- |
| Image 7.5.1  | Structure and components of a PEM fuel cell    | `images/usecase7_5_1.png` |
| Image 7.5.2  | General modeling approach of the PEM fuel cell | `images/usecase7_5_2.png` |
| Image 7.5.3  | Temperature and current density fields         | `images/usecase7_5_3.png` |
| Image 7.5.4  | Polarization curve comparison                  | `images/usecase7_5_4.png` |
| Table 16     | Governing equations for PEM fuel cell          | —                         |

--- -->

## References

[80] **openFuelCell.** *https://openfuelcell.sourceforge.io/*  
[81] Zhang S. (2020). _Modeling and Simulation of Polymer Electrolyte Fuel
Cells._ Springer, Forschungszentrum Jülich.  
[82] Zhang S., Hess S., Marschall H., Reimer U., Beale S., Lehnert W. (2023).
_openFuelCell2: A new computational tool for fuel cells, electrolyzers, and
other electrochemical devices and processes._ Rochester.
https://doi.org/10.2139/ssrn.4540105  
[83] Beale S. B., Lehnert W. (eds.) (2022). _Electrochemical Cell Calculations
with OpenFOAM._ Lecture Notes in Energy, Vol. 42, Springer, Cham.  
[84] Steven B., Werner L. (2023). _A Simple Electrochemical Cell Model._ In:
_Electrochemical Cell Calculation with OpenFOAM._ Lecture Notes in Energy, Vol.
42, pp. 21–57. Springer, Cham. https://doi.org/10.1007/978-3-030-92178-1  
[85] Beale S. B., Choi H.-W., Pharoah J. G., Roth H. K., Jasak H., Jeon D.-H.
(2015). _Open-source computational model of a solid oxide fuel cell._ _Comput.
Phys. Commun._ 200: 15–26. https://doi.org/10.1016/j.cpc.2015.10.007  
[86] Bard A. J., Faulkner L. R. (2001). _Electrochemical Methods: Fundamentals
and Applications_, 2nd edn. Wiley, New York.  
[87] Bockris J. O., Reddy A. K. N., Gamboa-Aldeco M. E. (2001). _Modern
Electrochemistry 2A: Fundamentals of Electrodics._ Springer, New York.

---

</div>
