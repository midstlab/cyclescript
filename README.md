# cyclescript
merging FEP outputs from NAMD at different time points and calculating free energy cost of mutations for individual and merged sets

______________________

This repository contains all FEP files for the manuscript entitled, 
'A thermodynamic cycle to predict the competitive inhibition outcomes of an evolving enzyme'
coauthored by Ebru Cetin, Haleh Abdizadeh, Ali Rana Atilgan, and Canan Atilgan.

This study presents a thermodynamic cycle informed by free energy perturbation (FEP) simulations to assess the relative 
efficiency of an enzyme in the presence of a competitive inhibitor. By relating this cycle to the biochemical constants 
Kₘ and Kᵢ, we provide a framework for understanding the impact of mutations on enzyme inhibition mechanisms.
We apply our approach to 13 E. coli DHFR variants that arise under the evolutionary pressure of trimethoprim, 
demonstrating strong agreement between our predictions and experimental data. 

It is these FEP data and the codes for BAR calculations on these systems which is provided here.

# Data and code arrangement

All fepout files obtained from NAMD program are shared. There is a directory reserved for each mutant.
Each one contains two subdirectories 'dhf' and tmpp' for the substrate-bound and 'inhibitor-bound' DHFR,
respectively.

The file names have the following information:
substrate : mutation : time point at the MD simulation which forms the starting structure for FEP simulation : direction of FEP

For example, dhf_i5f_50ns_fw.fepout file contains the FEP simulation results for DHF-bound DHFR undergoing I to F mutation at position 5.
The 50 ns time point in a 200 ns MD simulation of the wild type DHFR is the starting point; and this is the forward simulation.

The code that crawls into the subdirectory and populates the data is provided as a jupyter-notebook (bar_analysis.ipynb)
in each mutant directory.

Outputs: 
1. calculated free energies for dhf/tmp bound cases in .dat files. The first number on line 2 is for the BAR calculated
   free energy in kcal/mol on the populated data, the following numbers are the BAR calculated free energies in individual
   runs. Line 3 has the error estimate, calculated from the RMSD values of individual runs.
   
2. .png files displaying the overlaps of the populated FEP trajectories in 32 lambda windows in forward and backward runs

# Special cases

1. The charge changing simulations have separate charge creation (cre) and charge annihilation (ann) .fepout files. 
The file naming includes this information, e.g. dhf_l28r_50ns_ann_fw.fepout
The code is modified to handle this new naming; bar_analysis_withchargechange.ipynb

2. For the W30R systems, a set of outputs for a R30W is also included. The charge creation in one system at lambda = i
   in foreward direction is equivelent to charge annihilation at lambda = 32-i in the reverse system. Data are reaaranged
   accordingly and these files have '10' appended to the beginning of the file name. E.g., dhf_w30r_10050ns_ann_bw.fepout
   has the data collected in the dhf_w30r_10050ns_cre_fw.fepout file reports the rearranged data from the original
   dhf_r30w_50ns_ann_bw.fepout file.

3. Double mutant outputs are FEP simulations starting from a time point in the 200 ns simulation of the first mutant.
   E.g., dhf_l28r-a26t_50ns_fw.fepout contains the FEP simulation starting from the 50 ns time point of a L28R mutant simulation.
   To get the total free energy cost for the L28R-A26T system, calculated values from this set should be added to the cost
   of the L28R mutation calculated as a single mutation.

# Code requirements

The codes work with python 3.10.0. 

imports alchemlyb, seaborn, numpy, pandas, matplotlib.pyplot
