# The Implementations of GPU-GBMV2/SA-MSES

Xiping Gong

02/01 - 02/28/2020


## Summary of what you have done since your last report

1. Implemented the GPU-GBMV2/SA-MSES model 

The energy of system for MSES simulations can be written as follows,

ETOT = E(AT) + E(CG) + EMSES(AT,CG)

where the E(AT) and E(CG) are the energies of AT and CG systems, respectively.
The EMSES is the coupling energy between AT and CG, which can be defined by users.
The GBMV2/SA implicit solvent model was included into the AT system, 
while the CG model does not consider the GBMV2/SA model. The contact list file
can be used to specify the EMSES interactions, for example, 

> ! EMSES-DIST <- SCAL * Ei

> ! Ei = FORC * 0.5 * di * di, if di <= RSWI

> ! Ei = FORC * (A + B/di^SEXP + FMAX * di), if di > RSWI
>
> ! where, A and B are determined by making sure Ei and derivative are continuous functions at the RSWI point.
>
> ! TYPE AT1(segid resid atomName) AT2 AT3 AT4 FORC(kcal/mol/Ang^2) FMAX(kcal/mol/Ang) RSWI(Ang) SEXP SCAL
>  
> DIST AT 1  CA AT 4  CA  CG 1  CA CG 4  CA 1.0 0.5 2.0 1.0 1.0

> where, 'DIST' means the distance-based pair interaction model will be used, the expression can be found in 
> [this MSES paper](https://pubs.acs.org/doi/abs/10.1021/ct500031v); 'AT 1 CA' represents one atom, where 'AT' is the
> segment id, '1' is the residue id, and 'CA' is the atom type name, so 'AT 1  CA AT 4  CA' is two atoms in the 
> AT system, while 'CG 1  CA CG 4  CA' is two atoms in the CG system, and then we can calculate their distances 
> and difference between AT and CG system, because these four atoms are the alpha-C atoms, so it is expected that
> the difference should be closer to zero, which means the distance between two AT atoms is closer to that between
> two CG atoms, and it helps the AT system to follow the CG systems. The last five numbers are five parameters
> whose meanings can be found in the [MSES paper](https://pubs.acs.org/doi/abs/10.1021/ct500031v). Additionally, the
> '!' is used to comment, similar to the CHARMM usage. 
> Currently, only 'DIST' type interaction is supported by the OpenMM MSES plugin, in the future, other useful interactions
> might be added in this plugin.

## The motivation/goals of what you have done

The goal is to achieve the GBMV2/SA-MSES GPU calculations and accelerate the conformational
transitions of disordered proteins, and reproduce the [CPU results](https://onlinelibrary.wiley.com/doi/full/10.1002/jcc.24734).

## Analysis/conclusion of the results: progress and remaining problems


## Future plan and why

The future plan is to test more flexible proteins, to see whether the GPU-based GBMV2/SA-MSES model can sample
fastly their conformational space, and to make sure whether the GBMV2/SA model can provide an accurate description 
on most of flexible proteins.

Several proteins include the (AAQAA)3, GB1p, GB1m1, GB1m3 and KID.



