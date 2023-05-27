# Hidden Markov Models

In machine learning we often need to determine the probability of target \\(y\\) given the features \\(x\\), i.e. P(y|x). This is known as discrimination

To model a sequence of states, the probability of the current state depends on all previous states. However, we can simplify this by the **Markov Assumption** which states that the current state only depends on the pervious state. This is overly simply for biological sequences

We can extend this by creating a finite automata, at each time step:
- Model emits a an output (e.g. base, codon..)
- Model transmits to new states

Each state has a certain probability of emitting different outputs, as well as certain probabilities of moving to new states, thus the next state depends only on the current. To use the model we only ever see the emissions, not the states, thus it is given a name: **Hidden Markov Model** 

![Dice HMM](Y3/3212/the-little-book-of-computational-biology/src/images/dice-HMM.png)

Consider the HMM for a a pair of dice, one fair and one loaded. The arcs are the transmission probabilities between states and the nodes contain the emission probabilities of each state.

The parameters are given:
- Emission in state i: \\( e_i(l_n) = P(l_n|q_t = i) \\)
- Transition in state i to j: \\( q_{ij} = P(q_t = j|q_{t-1} = i) \\)

Where:
- \\(l_n \\) is a letter from an alphabet
- \\( q_t \\) is the state at time t

To use a HMM, given an observed sequence of emissions we want to find the most probable sequence of states that generated them. 

To do this the **Viterbi** algorithm is used, which is an instance of dynamic programming. 

Where \\( v_i(t) \\) is the probability of being in state i at time t and \\( v_0(0) = 1 \\) and all others at time 0 are 0, indicating the start state.  Viterbi follows:
- For each time step:
	- For all states:
		- compute the probability of each state at this time step by multiplying the observed emission probability by the highest previous state and transmission to current state value.
		- This is: \\( \log v_i(t) = \log e_i(l_t) + \max_j(\log v_j(t-1) + \log a_{ij}) \\)
	- Keep a pointer to the previous most likely state: \\( ptr_t(i) = \underset{j}{argmax}(\log v_j(t-1) + \log a_{ij} ) \\)

Note: Log probabilities are used to prevent numerical underflow

Following the pointers backwards allows us find the most likely sequence of states which generated the observed sequence.

However, this can be inaccurate. It is better to ask what the probability of being in a certain state at time t given sequence x. i.e. \\( P(q_t = k | x) \\)

To do this we use Viterbi but replace the max with sum and changing the backwards section. This allows us to calculate the probability of a sequence, summed over all the possible paths. We can then plot the probabilities of all states at time t being a certain state. 

In biology, HMMs are used to inform unseen sequences based on the relative probabilities of each residue in a sequence. E.g. if a protein is a kinase protein, secondary structure probabilities, sections of genes.

#### Training 

However this relies on us knowing the probabilities within biology, which in reality we don't. To do this we need to train the model on known sequence and state paths. 

Given a sequence and state path we can count:
- The number of transitions between states: \\( A_{ij} \\)
- The count of emissions in each state: \\( E_i(l) \\)

Using these we can estimate the parameter probabilities in HMMs: \\[ e_i(l) = \frac{E_i(l)}{\sum_{l\in A}E_i(l)};\\ a_{ij} = \frac{A_{ij}}{\sum_{k \in Q} A_{ik}} \\]

We may need to add pseudo-counts (i.e. Add-k Smoothing?) to allow transitions we don't see much in the training data. 

To train the model:
- Make initial guess at transition and emission probabilities
- repeat, until good estimation:
	- Calculate posterior probabilities - Expectation
	- Calculate new transition and emission probabilities given previous - Maximisation

#### Designing

Designing the structure of a HMM (number of states, allowed transitions, order) is challenging as:
- Too few states: underfitting, fail to model significant differences
- Too many states: overfitting and poor generalisation, model noise
- Too many transitions can overfit, should represent real structure
- Markov property can impose too short range of a structure, can extend to higher-order dependencies 

GENESCAN is a fifth-order Markov model which was used to find genes. By looking for areas with a high "CG" pair content


