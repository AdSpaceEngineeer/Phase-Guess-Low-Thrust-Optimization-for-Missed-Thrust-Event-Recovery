# A Novel "Phase Guess" Approach with Sequentially Linearized Dynamics to Build a Low-Fidelity Solution Space of Free-Final Time Low Thrust Optimized Trajectories

---

**1. OVERVIEW:** This paper attempts to overcome the challenges of solving free-final time low thrust trajectory optimization with a novel indirect approach. This approach gets a computationally expedient low fidelity solution space to reduce the search space in time, fuel spend and final state error for more complex algorithms. The fundamental idea is that for a minimum time problem optimized on Pontryagin's Minimum Principle (PMP), the Lagrangians (or co-states) can be expressed as a simple harmonic oscillator function ("SHOF") in time. The method suggests a structured way to sequentially linearize the dynamics, prioritize a Lagrangian to "guess" a suitable phase, and derive phases for the other Lagrangians to fulfill transversality conditions.

---

**2. MOTIVATION:** Low thrust trajectory optimization poses significant challenges due to the inherent nonlinearity of state equations and free-final time constraints. In deep space missions, unforeseen missed thrust events (MTE) force the spacecraft into a ballistic trajectory, injecting uncertainty into the time spent in this trajectory. The astrodynamics community traditionally relies on direct numerical approaches, such as Particle Swarm Optimization (PSO) or Sims-Flanagan Transcription (SMT), which grapple with computational expense. Recent homotopy-based approaches take 200 to 2000 seconds per month-long time of flight (TOF). The aim of this tool is to keep the computation time to under 1 second per 1-month TOF (currently at 1.6875 per month on a i5 processor) without the need for multiple cores.

---

**3. DATASET:**

**3.a.** The approach is tested on a hypothetical scenario of a missed thrust event (MTE) in Psyche mission approaching Mars for a flyby maneuver. Trajectory positions were derived from NASA's 'Eye on the Solar System' interactive. 
**3.b.** Spacecraft specifications:
* **Spacecraft weight:** 2,747 kg (payload plus fuel).
* **Max thrust per engine:** 240mN (total 4 engines).
* **Ejection velocity:** 83 km/s.
* **Fuel Consumption Rate:** 0.35 kilograms to 1.3 kilograms of xenon a day.

[Repo nav link to Dataset: [Psyche255_Mars499_ephemerides_202510to202603.xlsx](https://github.com/AdSpaceEngineeer/Phase-Guess-Low-Thrust-Optimization-for-Missed-Thrust-Event-Recovery/blob/main/Psyche255_ephemerals_202510to202603.xlsx)]

---

**4. METHOD - Sequentially Linearized Dynamics and Phase Guess**
We apply sequentially linearized dynamics to solve the optimal control problem. 
**4.a. State Equations:**
The state of the spacecraft is expressed in two dimensions with the equation $X=AX+BU$.
* **Cost Function:** Solves for a minimum time optimization problem.
* **Control Profile:** Uses a Bang-Bang Control Profile bounded by $u_{max}$.

[Repo nav link to Base Code: [python_code](https://github.com/AdSpaceEngineeer/Phase-Guess-Low-Thrust-Optimization-for-Missed-Thrust-Event-Recovery/blob/main/python_code)]

**4.b. Phase Guess Approach:**
Given the split boundary conditions, we use a structured logic to make an initial guess.
* **Euler-Lagrange Equations:** Lagrangians can be expressed as SHOFs: $\lambda_{i}=C_{i}cos(\sqrt{-g}t+\varphi_{i})$.
* **Prioritization:** The dimension with the largest ideal (uncontrolled) thrust acceleration Q is prioritized.
* **Phase Guess & Transversality:** The guess for $\varphi_{i}$ is either $\frac{\pi}{2}$ or $-\frac{\pi}{2}$, and the phase for the other Lagrangian is derived from the Transversality Equation.

---

**5. RESULTS**
The desired outcome is to build a contour plot of days ballistic, the distance from nominal flyby position, and the expected fuel consumption. 
* **Simulation Sweep:** Simulations were run over MTE dates ranging from Nov 15, 2025 to Feb 15, 2026.
* **Computational Speed:** The simulations achieved execution times ranging from 7.527 seconds to 10.369 seconds per run.
* **Distance and Fidelity:** Distances from the fixed final state are relatively large due to conservatively assumed values of the velocity vector and max time of flight, making this a low-fidelity space builder.

[Repo nav link to Plots: [python_code_3Dimensional_3Bperturbation_SearchSpacePlots](https://github.com/AdSpaceEngineeer/Phase-Guess-Low-Thrust-Optimization-for-Missed-Thrust-Event-Recovery/blob/main/python_code_3Dimensional_3Bperturbation_SearchSpacePlots)]

---

**6. USAGE**
To reproduce the phase guess approach and results using the 7-step algorithm:

**6.1.** Clone the repository.

**6.2.** Install dependencies (NumPy, SciPy, Matplotlib).

**6.3.** **Load Initial State:** Get the initial state and final-fixed state of the spacecraft. Compute instantaneous values of state equations constants.

**6.4.** **Determine Priorities:** Determine the dimension of which Q has the largest magnitude. Set the SHOF phase and compute the other from the Transversality equation.

**6.5.** **Propagate:** Propagate state until constants need updating, then loop. End propagation until final-fixed state is reached.

---

**7. CALL TO ACTION FOR FUTURE WORK**

**7.1.** The Phase Guess approach exhibits promise in deep space mission applications.

**7.2.** Future improvements to this repo could include:
* **i) Shooting Method Integration:** To address constraints in offering a logically optimal control law, a shooting method could facilitate determining values for all Lagrangian SHOF phases.
* **ii) Intermediate Flybys:** Optimize trajectories to give scientific teams the leverage to tweak parameters for incorporating intermediate planetary flybys.
* **iii) 3D & 3B Perturbations:** Evaluate performance under complex dynamics incorporating third-body perturbations in three-dimensional settings.

---

**8. REFERENCES:**
[1] Woollands R. et al., "Efficient Computation of Optimal Low Thrust Gravity Perturbed Orbit Transfers," 2019.
[2] Patrick B., "High Fidelity Low Thrust Trajectory Optimization to the Lunar Gateway," 2023.
[3] Jagannatha B. et al., "Preliminary Design of Low Energy Low Thrust Transfers to Halo Orbits using Feedback Control," 2021.
[4] Laipert F. et al., "A Monte Carlo Approach to Measuring Trajectory Performance Subject to Missed Thrust," 2018.
[5] Venigalla C. et al., "Multi-Objective Low-Thrust Trajectory Optimization with Robustness to Missed Thrust Events," 2022.
[6] Englander J. et al., "Trajectory Optimization for Missions to Small Bodies with a Focus on Scientific Merit," 2017.
[7] Pascarella A. et al., "Low-thrust trajectory optimization for the solar system pony express," 2022.
[8] Zhang J. et al., "Solution space exploration of low-thrust minimum-time trajectory optimization by combining two homotopies," 2022.
[9] Lee J., Ahn J., "Free Final-Time Low Thrust Trajectory Optimization Using Homotopy Approach," 2022.
[10] Donald E. Kirk, "Optimal Control Theory - An Introduction," 2003 edition.
[11] Longuski, Guzman & Prussing, "Optimal Control with Aerospace Applications," 2014 edition.
