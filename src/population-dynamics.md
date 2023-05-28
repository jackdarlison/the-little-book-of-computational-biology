# Population Dynamics

Population dynamics looks at how population levels vary of time given certain factors. Formed by combining biostatistics and Mendelian inheritance

Prior to Mendel it was thought characteristic were blended, **Mendelian Inheritance** is the idea that the parents characteristics are independent and children independent parts of both not a mix. 

Biostatistics cover the statistics population dynamics:
- Wright found:
	- Inbreeding gave more variation
	- Small isolated populations led to more variation towards a local optima
- Fisher found: 
	- Large populations don't need to worry about local optima
	- Sufficiently large populations will always move "uphill"
- Haldne found:
	- certain features are inherited together (e.g. genetic linkage)
	- Provided evidence for inheritance via chromosomes

Modern synthesis of evolution brings together:
- Natural selection
- Population genetics
- Medelian genetics

We know that:
- Genes can mutate
- Alleles are exchanged during sexual reproduction
- Evolutionary fitness measures the long term number of offspring
- Epistasis, genes interact
- Linkage disequilibrium
- Generations overlap

### Fisher Wright model

The FW model looks at how a single mutation can take over in a population.

This model assumes:
- genes can be changed from one allele to another via mutation
- Alleles are exchanged during sexual reproduction
- Fitness is a measure of the expected number of offspring
- no epistasis
- linkage equilibrium
- generations do not overlap


To make models with FW we make further assumptions:
- No gender
- Haploid (one copy of genes)
- Two states normal (P) and mutant (Q)
- Q has selective advantage of 1+s
- P mutates to Q at rate m
- Q mutates to P at rate n
- constant population size (N) (for infinite populations we can approximate using differential equation)

Each generation follows:
- individuals produce seeds
- seeds mutate according to m and n
- sample N of the seeds


In reality we don't use exact numbers for seeds, only the proportion of mutants compared to normal, the model:
- Find the proportion of mutants after seeding: \\( p_s = \frac{(1+s)x}{N+sx} \\) where x is the number of mutants in this generation
- Mutate the seeds and find the new proportion of mutants: \\( p_{sm} = (1-n)p_s + m(1 - p_s) \\)
- Sample N individuals, using a binomial with the proportion of mutants: \\( P(x(t+1) = x) = \binom{N}{x} p^x_{sm}(1-p_{sm})^{N-x} \\)


If we presume that the population is infinite we can just use proportions instead of numbers: \\[ p_x(t+1) = \frac{(1-n)(1+s)p_x(t) + m(1 - p_x(t))}{1 + sp_x(t)} \\]

Which can be approximated by a differential equation if the effects of selection and mutation are small: \\[ \frac{dp_x}{dt} \approx p_x(t+1) - p_x(t) \\]

The problem with this is that if mutants are fitter, they will grow exponentially at the start. ODEs have much more rapid take over times than finite population models if low mutation rates

Finite models are stochastic, where as infinite populations are deterministic.

### Markov Models

Evolving populations can be modelled using a Markov chains. 

To do this we define:
- A transition matrix \\( W_{x', x} \\) where each cell represents the probability from x to x' mutants.
- The probability of x mutants are updated with \\( p(t+1) = Wp(t) \\)
- The start prob for no mutants:  \\( p(t=0) = [1, 0, 0, ...] \\)

Thus, \\(p(t) = W^tp(0) \\)

This model creates a probability distribution over the number of mutants for each time step. 

The transition matrix is given: 

\\[ W_{x', x} = \binom{N}{x'} p_sm(x)^{x'}(1-p_{sm}(x)^{N-x'}) \\]

Where:

\\[ p_{sm}(x) = \frac{(1-n)(1+s)x + m(N-x)}{N+sx} \\]

Markov models provide an exact analysis of population dynamics. However, as the  matrix is the size of the states + 1 squared, this model is infeasible for large populations.  

### Stochastic Differential Equations

To model the randomness inherent in population dynamics, stochastic differential equations are used. These work best with weak selection and small mutation rates

Presume a random walk (±1). The expectation of distance travelled is zero, as well as the expectation of number of steps in total. However, the expectation of the squared distance is somehow equal √N 

With random time steps dt and for a walk with distance x, the change is distance is measured:

\\[ dx = adt + bdW(t) \\]

where:
- a is the drift, i.e the expected change
- b is the variance, due to random processes
- W(t) is a random process drawing from a normal distribution with mean 0 and variance dt: i.e.  N(0, dt)

This is closely related to a Maxwell-Boltzmann distribution.

Applying this to evolution:
- Time steps are generations
- Distance is the number of mutants in the population
- dx is the change in mutants in each time step

We get:

\\[ X(t+dt) = X(t) + a(X(t))dt + b(X(t))dW(t) \\]

Where:
- Expected change / drift: a(X(t)) = (p_{sm}(X(t)) - X(t))dt
- Variance: b(X(t)) = \frac{p_{sm}(X(t))(1-p_{sm}(X(t)))}{N}dt

If s, m, and n are small then \\( p_{sm} \approx X(t) \\) and can be substituted in variance.

We get a stochastic differential equation or SDE:

\\[ dX(t) = a(X(t))dt + b(X(t))dW(t) \\]

This is known as an Itô process which can be solved. There is no closed from solution for the differential of W(t) as this will also be stochastic. We can however solve the probability distribution: \\( f(x, t)dx = P(x \le X(t) \lt x + dx) \\)

The solution to this is given by the **Fokker-Planck** equation (i.e. Forward Kolmogorov):

\\[ \frac{\partial f(x, t)}{\partial t} = -\frac{a(x)\partial f(x, t)}{\partial x} + \frac{1}{2} \frac{\partial^2(b^2(x)f(x, t))}{\partial x^2} \\]

However diffusion doesn't go backwards so we need to the Backward Kolmogorov:

\\[ \frac{\partial f(x, -t)}{\partial t} = -a(x)\frac{\partial f(x, -t)}{\partial x} + \frac{1}{2} b^2(x)\frac{\partial^2(f(x, -t))}{\partial x^2} \\]

This can be used to work out how long it might take to get to an absorbing state and how likely this state may be

Fokker-Planck shifts the SDE into a PDE. This is hard to solve directly but can use numerical methods. This is almost identical to Markov Chain analysis and can scale well to large numbers

### Steady State Distribution

Solving the PDE is hard in the previous model. We can approximate this by solving for when it equals 0, i.e. when the distribution stops changing at t tends to ∞. This can be done as solving for t tends to ∞ does not depend on t. 

It is solved using the equation:

\\[ f_{eq}(x) = \frac{x^{2Nm-1}(1-x)^{Nn-1}e^{2Nsx}}{\text{Betaln}(2Nm, 2Nn)\text{Hyp1f1}(2Nm; 2N(m + n); 2Nsx)} \\]

This is a valid approximation as evolution is slow, which means the steady state is often seen. 

