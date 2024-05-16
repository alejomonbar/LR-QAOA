# LR-QAOA

Link: https://arxiv.org/abs/2405.09169


The quantum approximate optimization algorithm (QAOA) is a promising algorithm for solving combinatorial optimization problems (COPs). In this algorithm, there are alternating layers consisting of a mixer and a problem Hamiltonian. Each layer $i=0,\ldots,p-1$ is parameterized by $\beta_i$ and $\gamma_i$. How to find these parameters has been an open question with the majority of the research focused on finding them using classical algorithms. In this work, we present evidence that fixed linear ramp schedules constitute a universal set of QAOA parameters, i.e., a set of $\gamma$ and $\beta$ parameters that rapidly approximate the optimal solution, $x^{\*}$, independently of the COP selected, and that the success probability of finding it, $probability(x^\*)$, increases with the number of QAOA layers $p$. We simulate linear ramp QAOA protocols (LR-QAOA) involving up to $N_q=42$ qubits and $p = 400$ layers on random instances of 9 different COPs. The results suggest that $probability(x^\*) \approx 1/2^{(\eta N_q / p)}$ for a constant $\eta$. We extend the analysis in 4 COPs with $p=N_q$ and show that $probability(x^*)$ seems to be constant for general cases.  For example, when implementing LR-QAOA with $p=42$, the $probability(x^\*)$ for 42-qubit Weighted MaxCut problems (W-MaxCut) increases from $2/2^{42}\approx 10^{-13}$ to an average of 0.13. We compare LR-QAOA, simulated annealing (SA), and branch-and-bound (B\&B) finding a fundamental improvement in LR-QAOA. We test LR-QAOA on real hardware using IonQ Aria, Quantinuum H2-1, IBM Brisbane, IBM Kyoto, and IBM Osaka, encoding random weighted MaxCut (W-MaxCut) problems from 5 to 109 qubits and $p=3$ to $100$. We find that even for the largest case, $N_q=109$ qubits and $p=100$, information about the LR-QAOA optimization protocol is still present. The circuit involved requires 21200 CNOT gates and a time of $\approx 132 \ \mu s$. The resilience of LR-QAOA to noise is attributed to its characteristic of bringing the system towards a minimal energy state despite noise driving it towards a maximally mixed state. These results show that LR-QAOA effectively finds high-quality solutions for COPs and suggests an advantage of quantum computation for combinatorial optimization in the near future.

# Formulation
QAOA consists of alternating layers that encode the problem of interest along with a mixer element in charge of amplifying the desired solutions with low energy. In this case, the COP cost Hamiltonian is given by

$$H_C = \sum_i h_i \sigma_z^i + \sum_{i, j > i} J_{ij} \sigma_z^i \sigma_z^j,\tag{1}$$

where $\sigma_z^i$ is the Pauli-z term of qubit i, and $h_i$ and $J_{ij}$ are coefficients associated with the problem. Usually, $H_C$ is derived from the quadratic unconstrained binary optimization (QUBO) formulation \cite{Lucas2014, Montanez-Barrera2024, Montanez-Barrera2022}. The QUBO to $H_C$ transformation usually includes a constant term that does not affect the QAOA formulation and is left out for simplicity. $H_C$ is encoded into a parametric unitary gate given by

$$U_C(H_C, \gamma)=e^{-j \gamma_i H_C},\tag{2}$$

 where $\gamma_i$ is a parameter that in our case comes from the linear ramp schedule. Following this, in every second layer, a unitary operator is applied given by 

$$U(H_B, \beta)=e^{j \beta_i H_B},\tag{3}$$

where $\beta_i$ is taken from the linear ramp schedule and 

$$H_B = \sum_{i=0}^{N_q-1} \sigma_i^x, \tag{4}$$

with $\sigma_i^x$ the Pauli-x term of qubit $i$. Here, $R_X(-2\beta_i) = e^{j\beta_i \sigma^x}$, $p$ is the number of repetitions of the unitary gates of Eqs.~2 and 3, and the initial state is a superposition state $| + \rangle^{\otimes N_q}$. Repeated preparation and measurement of the final QAOA state yields a set of candidate solution samples, which are expected to give the optimal solution or some low-energy solution.

LR-QAOA is characterized by three parameters $\Delta_\beta$, $\Delta_\gamma$, and the number of layers $p$. The $\beta_i$ and $\gamma_i$ parameters are given by 

$$\beta_i = \left(1-\frac{i}{p}\right)\Delta_\beta\ \ \mathrm{and} \ \ \gamma_i = \frac{i+1}{p}\Delta_\gamma,\tag{4}$$

for $i=0, ..., p-1$. 
