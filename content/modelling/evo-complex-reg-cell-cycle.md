---
title: The Evolution of Complex Regulation of Cell-Cycle-Control
tags:
    - modelling
    - evolution
    - networks
---

von der Dunk et al. set out to determine the relationship between genome size, regulatory network complexity and fitness over varying conditions of nutrient abundance. 
While a larger genome facilitates a more complex regulatory network (as genomes grow larger, an increasing proportion of genes are dedicated to regulation), it also increases the cost of replication. 
The cell cycle was chosen as the model system for this study, as it "provides an intrinsic and dynamic fitness criterion for a cell's regulatory network". 

## The model

The cell cycle in *C. crescentus*, which is controlled by five genes (CtrA, GcrA, DnaA, CcrM and SciP), is used as a model system. The model used is an elaborate, multi-level model: a mutable genome model gives rise to a regulatory network, which controls the cell-cycle state of each cell, which in turn determines cell fate in an agent-based model of cells on a grid.

The population of bacteria is modelled as an agent-based model on a 2D grid. Each cell contains a genome, which defines a regulatory network. Expression of the five cell cycle genes defines the cell's state. 

The cell cyle is modelled as a walk through the state space of a boolean network. corresponding to activation of CtrA, GcrA, DnaA, CcrM and SciP.
Within this network, absent any external regulation, paths through state space cycle through G1, S, G2, M phases. Cells reaching M phase after progressing through all four states in the correct order will divide (creating a new cell in an adjacent grid space), and those prematurely reaching M phase die.

DNA replication in S phase is a key part of the model. The rate of DNA replication is proportional to nutrient abundance, $n$. Each time step spent in S phase, $n$ units of the genome are replicated. During replication, the parts of the genome which have already been replicated are able to participate in the regulatory network, effectively doubling their copy number[^1]. 

Nutrient abundance, $n$, is modelled as a function of the occupancy of the surrounding cells: a site with $x$ neighbours has nutrient level $n = n_0 (1 - x/8)$, meaning that a cell can not divide at all if it is surrounded by other cells. This, along with the fact that nascent cells can kill the cell previously occupying their grid square, simulates crowding and gives an upper limit to population size. Initial nutrient concentration, $n_0$ is a gradient from low to high abundance.

Cells die with an instantaneous death rate $\delta$.

The genome is modelled as a series of genes and binding sites arranged on a "string". Binding sites upstream of gene $i$ affect its expression ($\epsilon_i$), such that

$$
\epsilon_i =
\begin{cases} 
1 & \text{if } \sum_b w_{x \rightarrow b} > \theta_i, \\
0 & \text{otherwise}
\end{cases}
$$

where $\theta_i$ is the activation weight of the gene, and the weight of a binding site on its bound gene is $w_{x \rightarrow b} = w_x w_b$. Here, negative weights correspond to repression and positive weights to activation, such that a repressor of a represive binding site is overall activating.

Each gene and binding site is associated with a 20-bit binding sequence.
Binding of a gene to a binding site is determined randomly at each timestep, with a probability determined by the bitwise similarity between the binding sequence and the gene, such that genes which more closely match the binding sequence are more likely to bind. 

At each timestep, the following mutations can occur in the genome:
- Flipping of a bit in a binding sequence 
- Increasing or decreasing values of $\theta$ or $w$
- Duplicating, deleting, or relocating a gene/binding site
- Creation of a new gene/binding site with random properties

## Results

Cells were consistently able to evolve to inhabit regions of low nutrient density. Limited nutrients in more densely populated regions creates an evolutionary niche for cells able to adapt to nutrient limitation. Expansion into low nutrient density regions was correlated with increased genome size (despite this compounding the effect of slower replication). Cells with larger genomes were able to progress through the cell cycle faster.

Two main strategies evolved: phenotypic plasticity, allowing the cells to adapt to a wide range of conditions (generalists), and specialisation to lower nutrient concentrations (specialists). Generalist cells are viable over the whole range of nutrient concentrations, while specialist cells survive worse at high $n$. There was no correlation between plasticity and genome size.

Cells which adapted to low nutrient availability needed to delay entry into M phase and remain in S phase long enough to complete replication. Mechanistically, this was achieved by rerouting the cell cycle to either remain in S phase, or to exit and then re-enter S phase several times. This drastic rerouting of the cell cycle is unrealistic for a real organism, in which it is essential for cell cycle transitions to be commital and irreversible.

---
## References
1. von der Dunk, S. H. A., Snel, B. & Hogeweg, P. Evolution of Complex Regulation for Cell-Cycle Control. Genome Biol Evol **14**, evac056 (2022). https://doi.org/10.1093/gbe/evac056

[^1]: Due to the discretisation of time, at the limit of high nutrient availability, the time taken to complete S phase remains constant at a single timestep (and doesn't approach zero, as it would if, say the Gillespie method were used). This is a more realistic outcome, since replication time can only be inversely proportional to nutrient abundance when it is constrained by nutrient availability. A better simulation may model this non-linearity explicitly, e.g. using a Hill function (why should the maximum replication rate depend on the time step of the simulation?).

