# Sequence Matching

Sequence matching is useful for:
- Finding similarity in Protein/DNA between related species 
- Transfer knowledge about known sequences to new sequences
- Compare sequences with other species, although need to know which part of the sequence to match

To reassemble sequences after sequencing, we need to find best matches at both the DNA and Protein levels

Mutations in proteins are more likely to have adverse effects than DNA, meaning they are more highly conserved. This means they are unlikely to change between generations, species, or even kingdoms. DNA mutation are les conserved as the often have no effect

Some coding sequences are so important they are conserved in all living things, i.e the coding for proteins used in transcription and translation. All inherited from the universal common ancestor. 

Some amino acids are more similar to each other, meaning they have a higher chance of being substituted with each other. 

BLOSUM 50 is a data set of substitution rates, using known alignments it is calculated by:
- Eliminating sequences with higher than a given similarity
- Calculating frequency of pairs of amino acids
- Calculate the ratio of occurrence of each amino acid combination.
- Calculate log odds of each pair, i.e. how often they occur as a pair compared to how often then would be expected if independent: \\[ 2\log_2(\frac{P_{ab}}{P_aP_b}) \\]

The BLOSUM 50 table can be used to look up the probability of the \\(i\\)th amino mutating to the \\(j\\)th amino acid

It is also a possibility that there could be a gap, i.e. a insertion or deletion. 

As there is an infeasible number of possible inexact matches to check, so we use dynamic programming to build up an optimal set of partial solutions until we have a solution for the entire string. Each step builds upon the previous optimal solutions. Dynamic programming reduces the complexity from O(\\(2^n\\)) to O(\\(n^2\\))

### Exact Methods

**Needleman-Wunsch** is an algorithm for computing global alignments between two strings. 
- BLOSUM 50 is used for substitution costs, a to b
- Linear cost (d = -8) is used for insertion and deletion

We use the costs to build up a cost matrix \\(F\\) where each cell (n, m) gives the best alignment for strings \\(a_{0:n} \\) and \\(b_{0:m}\\). The top left is the start, which is filled with 0. This is done recursively with the equation: \\[ F_{i, j} = \max \begin{cases} F_{i-1, j-1} + S_{a_i, b_j} \\\\ F_{i-1, j} + d \\\\ F_{i, j-1} + d \end{cases}\\]

Corresponding to substitution, a gap, b gap respectively. 

At each cell a pointer is kept to indicate which previous state the cell came from (i.e. the action used). After the table is filled we start at the bottom right and follow the back pointers to find the best alignment. 

NW global matching has a time complexity of O(A size x B size) which is not an issue. The main problem is it has the same space complexity making it a problem for long sequences. 

*Hirschberg* is another global matching algorithm which recursively splits up the string making it have linear space complexity. Is still quadratic in time, but in practise tends to take twice as long as NW

**Smith-Waterman** is an algorithm for local matching, finds the best aligned substring within two strings. 

It is very similar to NW, with the added option of ignoring an operation for a cost of 0: \\[ F_{i, j} = \max \begin{cases} 0 \\\\ F_{i-1, j-1} + S_{a_i, b_j} \\\\ F_{i-1, j} + d \\\\ F_{i, j-1} + d \end{cases} \\]

The table is filled in the same way, using back pointers to the previous cell if the operation wasn't ignore. 

Once it is filled, we start from the highest value in the table rather than the last value, and follow the points until we reach a 0 to find the local alignment. 

To deal with multiple best matches, caused by repeats of one region within another, smith-waterman can be adapted. 

Both NW and SW use linear gap costs, i.e. each site is independent. This is problematic as in reality mutations can occur in sequences.

To fix this we add an affine (linear, i.e. y = mx +c) gap cost function evaluated over all previous possible gap sizes: \\( \gamma(g) = -d - e(g-1) \\). d=12 and e=3 gives a good model which penalises all gaps but larger slightly more punished. 

To implement this however, three matrices are needed. It is easier to visualise these using finite state automata, where the states are the matrices and arcs are the operation costs

### Approximate Methods

Proteins we have chains of 100s of amino acids, meaning the quadratic time of these algorithms is possible, but slow.

DNA however is 10^5 to 10^11 bases long, meaning quadratic time is not feasible. To achieve this we need approximate methods

**FASTA** is a heuristic method that finds candidate subsequences for alignments. Smith-waterman is then used of the subsequences. It has however been superseded by BLASTA

**BLASTA** is another heuristic method which:
- filters out low complexity regions or repeated sequences
- make k-letter exact matches word list from the sequence
- evaluates of *significant* matches
- Combines high scoring matches to find gapped regions.
- Finishes off with Smith Waterman

The find the significance of a match, i.e. the probability of a match given sequences a and b we can use the logistic equation on the probabilities: 

\\[ P(M|a, b) = \sigma (\log\frac{P(a, b|M)}{P(a, b|R)} + \log\frac{P(M)}{P(R)}) \\]

Another approach is finding the likelihood of a particular score being random rather than a match is to use *extreme value distributions*.

![EVD](Y3/3212/the-little-book-of-computational-biology/src/images/EVD.png)

Where we sample multiple distributions, and take the most extreme value of each distribution to create an extreme value distribution.

EVDs are more accurate the longer the string.

