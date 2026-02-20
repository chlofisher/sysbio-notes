---
title: Flux Balance Analysis
tags: 
    - metabolism
    - modelling
    - linear-algebra
    - ode
---
Flux balance analysis (FBA) is a linear, steady state mathematical model of the flow of metabolites through a metabolic network. Modelling metabolic networks using mass-action kinetics ODE models is intractable for systems consisting of more than a few reactions. FBA simplifies these calculations by assuming that the system is at steady state. Using FBA, we can calculate the steady state solution to a metabolic system which optimises a linear objective.

Let a system consist of $n$ metabolites, affected by $k$ reactions. For a vector of metabolite concentrations $\mathbf{x} \in \mathbb{R}^n$, the system is in steady state if

$$
\dot{\mathbf{x}} = 0.
$$

Flux, $\mathbf{v} \in \mathbb{R}^k$, is defined as the rate of each reaction in the system. Flux is linearly related to the derivative of $x$ by the stoichiometric matrix, $S \in \mathbb{R}^{n \times k}$. 

$$
\dot{\mathbf{x}} = S\mathbf{v} = 0
$$

$S_{ij}$ is the stoichiometric coefficient of metabolite $i$ in reaction $j$, with positive coefficients denoting products and negative coefficients reactants. Thus, the assumption that the system is in steady state constrains the possible flux vectors to the null space of $S$. Since for most physical metabolic networks, $k > n$, there is not a unique solution to the steady state condition. 

Finding solutions optimising for a specific requirement (e.g., maximise $Z = \mathbf{c} \cdot \mathbf{v}$) can be formulated as a linear programming problem and solved with the simplex algorithm. Additional constraints can come from defining maximum and minimum values for individual fluxes.

## Example

$$
\begin{align*}
A + B &\xrightarrow{v_1} C \\
C &\xrightarrow{v_2} D \\
C &\xrightarrow{v_3} E \\
D + E &\xrightarrow{v_4} F \\
\end{align*}
$$

$$
\mathbf{v} = \begin{bmatrix}v_1\\v_2\\v_3\\v_3\end{bmatrix},  
S = \begin{bmatrix}
-1 & 0 & 0 & 0 \\
-1 & 0 & 0 & 0 \\
 1 & -1 & -1 & 0 \\
 0 & 1 & 0 & -1 \\
0 & 0 & 1 & -1 \\
0 & 0 & 0 & 1 \\

\end{bmatrix}
$$
$$
S\mathbf{v} = \begin{bmatrix}-v_1 \\ -v_2 \\ v_1 - v_2 - v_3 \\ v_2 - v_4 \\ v_3 - v_4 \\ v_4\end{bmatrix} = 0
$$


---
## References
Orth, J., Thiele, I. & Palsson, B. What is flux balance analysis?. Nat Biotechnol **28**, 245–248 (2010). https://doi.org/10.1038/nbt.1614
