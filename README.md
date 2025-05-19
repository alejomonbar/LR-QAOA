# LR-QAOA

Link: https://arxiv.org/abs/2405.09169

The Quantum Approximate Optimization Algorithm (QAOA) is a promising algorithm for solving combinatorial optimization problems (COPs), with performance governed by variational parameters $\{\gamma_i, \beta_i\}_{i=0}^{p-1}$.

While most prior work has focused on classically optimizing these parameters, we demonstrate that fixed linear ramp schedules—**linear ramp QAOA (LR-QAOA)**—can efficiently approximate optimal solutions across diverse COPs.

Simulations with up to $N_q = 42$ qubits and $p = 400$ layers suggest that the success probability scales as:

$$
P(x^*) \approx 2^{-\eta(p) N_q - C}
$$

where $\eta(p)$ decreases with increasing $p$. For example, in Weighted MaxCut instances, $\eta(10) = 0.22$ improves to $\eta(100) = 0.05$.

Comparisons with classical algorithms, including **simulated annealing**, **Tabu Search**, and **branch-and-bound**, show a scaling advantage for LR-QAOA.

We present LR-QAOA results on multiple QPUs (IonQ, Quantinuum, IBM) with up to $N_q = 109$ qubits, $p = 100$, and circuits requiring **21,200 CNOT gates**.

Finally, we introduce a **noise model based on two-qubit gate counts** that accurately reproduces the experimental behavior of LR-QAOA.




# Formulation

QAOA consists of alternating layers that encode the problem of interest along with a mixer element in charge of amplifying solutions with low energy. In this case, the COP cost Hamiltonian is given by

$$
H_C = \sum_i h_i \sigma_z^i + \sum_{i, j > i} J_{ij} \sigma_z^i \sigma_z^j,
$$

where $\sigma_z^i$ is the Pauli-z term of qubit $i$, and $h_i$ and $J_{ij}$ are coefficients associated with the problem. Usually, $H_C$ is derived from the quadratic unconstrained binary optimization (QUBO) formulation [Lucas2014, Montanez-Barrera2024, Montanez-Barrera2022]. The QUBO-to-$H_C$ transformation typically includes a constant term that does not affect the QAOA formulation and is omitted for simplicity. 

$H_C$ is encoded into a parametric unitary gate given by:

$$
U_C(H_C, \gamma_i)=e^{-j \gamma_i H_C},
$$

where $\gamma_i$ is a parameter that in our case comes from the linear ramp schedule. Following this, in every second part of a layer, a unitary operator is applied, given by:

$$
U(H_B, \beta_i)=e^{j \beta_i H_B},
$$

where $\beta_i$ is taken from the linear ramp schedule and 

$$
H_B = \sum_{i=0}^{N_q-1} \sigma_i^x,
$$

with $\sigma_i^x$ the Pauli-x term of qubit $i$. The general QAOA circuit is shown in Fig. `linear_ramp_schedule-(a)`. Here, $R_X(-2\beta_i) = e^{j\beta_i \sigma^x}$, $p$ is the number of repetitions of the unitary gates from the equations above, and the initial state is a superposition state $|+\rangle^{\otimes N_q}$. Repeated preparation and measurement of the final QAOA state yields a set of candidate solution samples, which are expected to give the optimal solution or some low-energy solution.

In Fig. `linear_ramp_schedule-(b)`, we show the LR-QAOA protocol. It is characterized by three parameters: $\Delta_\beta$, $\Delta_\gamma$, and the number of layers $p$. The $\beta_i$ and $\gamma_i$ parameters are given by:

$$
\beta_i = \left(1 - \frac{i}{p}\right)\Delta_\beta, \quad
\gamma_i = \frac{i + 1}{p} \Delta_\gamma,
$$

for $i = 0, ..., p - 1$. 

For our simulations, we scan over a set of $\Delta_{\gamma}$ and $\Delta_{\beta}$ values from one problem at each size and use the best value across the remaining cases. For the experimental results, we use:

$$
\Delta_\beta = 0.3, \quad \Delta_\gamma = 0.6.
$$
