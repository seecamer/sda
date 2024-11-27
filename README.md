# White Dwarf

This project analyzes the mass-radius relation for white dwarfs using a degenerate, non-relativistic equation of state.

## Contents

1. `README.md`: This file.
2. `astro_const.py`: Module containing physical constants, uses `astropy`.
3. `eos.py`: Code for the equation of state. Includes:
   - `pressure(rho, mue)`: Computes pressure (`P`) as a function of density (`rho`) and mean molecular weight (`mue`).
   - `density(P, mue)`: Computes density (`rho`) as a function of pressure (`P`) and `mue`.
4. `test_eos.py`: Unit test for the equation of state; compares computed values from `eos.py` and `eos_table.txt`.  
5. `eos_table.txt`: Comparison data for the equation of state, used by `test_eos.py`.
6. `structure.py`: Code to integrate stellar structure equations. Includes:
   - `stellar_derivatives(m, z, mue)`: Computes the derivatives of radius and pressure with respect to mass.
   - `central_values(Pc, delta_m, mue)`: Initializes boundary conditions for integration using the central pressure (`Pc`).
   - `lengthscales(m, z, mue)`: Calculates radial and pressure length scales for adaptive step size control.
   - `integrate(Pc, delta_m, eta, xi, mue)`: Integrates the stellar structure equations to calculate mass, radius, and pressure profiles.
   - `pressure_guess(M, mue)`: Provides an initial estimate of the central pressure (`Pc`) for a given mass (`M`).
7. `observations.py`: Module for handling observational data. Includes:
   - `MassRadiusObservations`: A class to read, store, and process observational data from `Joyce.txt`.
   - `make_observation_plot(ax, observations)`: Plots observed mass-radius data next to theoretical predictions.
8. `Joyce.txt`: Table 4, Joyce et al. (2018). Data for `observations.py`.


# Report: Mass-Radius Relation for White Dwarfs

## 1. Introduction

White dwarfs are stellar remnants stabilized by electron degeneracy pressure, forming a unique mass-radius relationship. This project uses a numerical approach to calculate the mass-radius relation for white dwarfs using a non-relativistic degenerate equation of state (EOS). The computed results are compared with observational data from Joyce et al. (2018).

---

## 2. Testing the Equation of State (EOS)

The EOS describes the relationship between pressure (P) and density (rho) for a degenerate electron gas. Two functions were used:
1. `pressure(rho, mue)`: Computes P given the density rho and mean molecular weight per electron (mue).
2. `density(p, mue)`: Computes rho given the pressure P and mue.

These functions were verified using the provided test script, `test_eos.py`, which compares the values against reference data in `eos_table.txt`. Our results were within the specified tolerance, confirming the accuracy of the EOS.

---

## 3. Convergence of Numerical Integration

### 3.1 Rationale for Parameters (delta_m, eta, xi)

- **delta_m**: The initial mass step near the core was set to 1e-10 solar masses. This made sure that the integration begins in a region where the density and pressure are nearly constant, providing stability in the solution.
- **eta**: The stopping condition for integration was defined as eta = 1e-10, a fraction of the central pressure, making sure that the integration at the stellar surface.
- **xi**: The integration step size was adjusted using a scaling factor of xi = 0.01 to balance computational efficiency and accuracy.

### 3.2 Scaling of Central Pressure and Density

- **Central Pressure (P_c)**: The initial guess for P_c was based on scaling relations derived from the virial theorem, where P_c is proportional to (G * M^2) / R^4.
- **Central Density (rho_c)**: Similarly, rho_c was estimated using the relation rho_c is proportional to M / R^3.

---

## 4. Results and Visualization

The mass-radius relation was computed for white dwarfs with masses ranging from 0.1 to 1.0 solar masses. The following plot shows the results:

![Mass-Radius Relation](MR_Joyce.png)


---

## 5. Discussion: Realism of the White Dwarf Model

### Strengths
1. The model accurately reproduces the mass-radius relation for white dwarfs with masses below 1.0 solar masses.
2. The use of a non-relativistic degenerate EOS gives a reasonable approximation for most white dwarfs.

### Weaknesses
1. Relativistic corrections are not included and become more important for high-mass white dwarfs.
2. The model assumes a uniform carbon-oxygen composition, ignoring variations that might affect the results.
3. Effects such as rotation and magnetic fields are not considered, which could modify the mass-radius relation.

---

## 6. Running the Code
1. Download `astro_const.py`, `eos.py`, `structure.py`, `observations.py`, `Joyce.txt`, `ode.py`, and `Questions_Answered.ipynb`.
2. Open `Questions_Answered.ipynb` and run the cell.
3. This will return the stellar structure test as well as the Mass-Radius table and plot.

---
