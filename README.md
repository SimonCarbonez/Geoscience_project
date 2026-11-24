# Geoscience project - Simon Carbonez

This document holds my observations/results, plans, for the project 

## MGAR time discretization 

To see the impact of MGAR time discretization, i ran it with different time steps. Runs start from the end-of-rain MGAR state and iterate through the first hour of redistribution (I don't use the Gamma correction because it doesn't really matter here). The reference is RK4 with Δt = 0.25 s.

* Euler is first order: when Δt is halved, the errors in θ and Z are also +- halved. At dt = 600 s the error is about 4·10⁻³ in θ and 2 mm in Z. At dt = 1 s it is about 6·10⁻⁶ in θ and 0.003 mm in Z.
* RK4 is fourth order: halving Δt divides the error by about 16. Even at dt = 600 s, RK4 is more accurate than Euler at dt = 1 s. With a dt of 10s and smaller the curves flatten as we reach machine precision
* Work: RK4 costs 4 right-hand-side evaluations per step and Euler costs 1. RK4 still needs far fewer evaluations for the same accuracy.

### Richards sensitivity: grid versus time step
To compare the grid vs time step sensitivity i made heatmaps for different cases. 
We can see that the grid controls the accuracy. The infiltration error at 2 h falls from about 3 mm (4 cm cells) to below 0.04 mm (0.25 cm cells). Each halving of dz roughly halves the error, so the convergence is about first order.

The time step barely matters. Along any row, the infiltration at 2 h changes by at most 0.4 mm between max dt = 600 s and 30 s. That is a lot less than the change from the grid.

So going from dt = 120 s to 30 s triples the run time for almost no gain. Halving dz roughly doubles the cost and halves the error. Refining the grid is the better choice, aalso as the Richard step are adaptive in time, they become smaller if needed.

Strange things : why do big dt seem to produce a smaller error for coarse grids than larger dt???

## Does the solution of Richard -> MGAR

I tried to see if the solutions from Richard would converge to the same as the one for MGAR if i kept refining the grid. 
The results table show that it is not the case which I expected because of the different natures of the methods.

## To do 


- MGAR in rain period
- Replace the impermeable BC in richards because at the moment models can diverge (if a caase where the whole 2.5m box gets filled up (a voir si possible d'en arriver la))

- Can the MGAR be represented as a dynamical system? 
- Energy usgae of Richard vs MGAR (note to self : voir LINMA2710)
