# The Implementations of GPU-vaccum/MSES Project

Xiping Gong

01/01 - 01/31/2020


## Summary of what you have done since your last report

1. Built a clean and separated GBMV OpenMM plugin

The tutorial can be found in [the github website](https://github.com/XipingGong/gbmvtutorial),
also the GBMV plugin has been pushed into the CHARMM program.

2. Implemented the separted/block vacuum calculation 

The energy of system for MSES simulations can be written as follows,

ETOT = E(AT) + E(CG) + EMSES(AT,CG)

where the E(AT) and E(CG) are the energies of AT and CG systems, respectively.
The EMSES is the coupling energy between AT and CG, which can be defined by users.
Currently, the nonbonded term including the vdW and electrostatic terms have
the block function, while this function does not suit the GBMV/SA implicit solvent 
simulations, meaning the E(AT) and E(CG) only include the classic CHARMM force fields
of proteins.

3. Implemented the MSES coupling term in the OpenMM MSES plugin

This part implemented the EMSES(AT,CG) GPU calculations. The interaction definition can be
found in [the MSES paper](https://pubs.acs.org/doi/abs/10.1021/ct500031v)
The tutorial can be found in [the github website](https://github.com/XipingGong/msestutorial),
and the MSES plugin has been also updated in the CHARMM program.

## The motivation/goals of what you have done

The goal is to achieve the GBMV/MSES GPU calculations and accelerate the conformational
transitions of disordered proteins.


## Analysis/conclusion of the results: progress and remaining problems

![Figure 1: The force difference between CPU and GPU calculations](msesforce_202001.jpg)

It can be seen from the Figure 1 that the force error between the 
CPU and GPU (CUDA) calculations are almost closer to zero, which means
that the GPU implementation of MSES coupling term has been done 
successfully.

## Future plan and why

The future plan is to implement the separated GBMV2/SA calculations of different blocks,
because in real systems, we must consider the solvent effect on the conformational 
transitions of proteins, which is quite important to the MSES simulations of 
flexible protein simulations.

Two weeks (2/1 - 2/14): To refine the MSES implementations and 
implement and examine the GBMV/MSES GPU calculations.

Two weeks (2/15 - 2/29): Several systems should be tested, including the (AAQAA)3, 3gb1, and
so on;

