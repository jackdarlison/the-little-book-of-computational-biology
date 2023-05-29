# Evolutionary Trees

**Phylogeny** is the study of the evolution of a species or group. This is usually represented using a tree, which help groups the different kinds of living things 

The Tree is built up from external features as well as DNA. However, feature based trees can be misleading due to convergent evolution 

We can see using Mitochondrial DNA that all Humans share a common ancestor from around 155,000 years ago. Mitochondrial DNA only comes from the mother, thus it is asexually passed on. Cross breeding with other hominids meant over time only homo-sapians were left. 

Statistically asexual reproduction creates to a single line from one original ancestor to the entire current generation.

Entire Life **Phylogenetic** trees are not actually trees as horizontal gene transfer is present in Bacteria and Archaea. These reproduce asexually but can exchange DNA to evolve. 

However As most multicellular organisms cannot interbreed, we can model them using trees. 

To use DNA to build up a phylogeny tree we look at Homologous (the same) genes. Genes that diverge due to speciation are orthologues whereas genes that diverge due to supplication are paralogues. To understand how species relate to each other we need to use orthologues.

To build a tree we need a molecular clock. To do this we presume that mutations occur at a constant rate; they don't due to selection and random processes so it is best to use large sequences.

A perfect molecular clock would give us an ultra metric structure, i.e. long-sided isosceles. Defined as for any three nodes, the maximum distance between two is **not** unique

#### Distance Based Trees

These trees explain a table of distances between DNA, e.g. sequence edit distance

Unweighted Pairwise Group Method using Arithmetic Averaging (**UPGMA**) is a method of creating a distance based tree. All nodes begin in single node clusters then we repeat until all merged:
- Calculate the distance between clusters: \\( d_{kl} = \frac{1}{|C_k||C_l|}\sum_{p\in C_k; q\in C_l}d_pq \\)
- Repeat Select the two clusters
- Remove all nodes and replace with a joined cluster
- Distance to remaining clusters are: \\( d_{kl} = \frac{d_il|C_i| + d_{jl}|C_j|}{|C_i| + |C_j|} \\)

For a distance metric between two sequences we could use the negative log of (1 - number of mismatched letters): \\( d_{ij} = -\log(1-p_{ij}) \\) 

UPGMA imposes the ultra metric structure on trees. however, we can not always assume that trees will be ultra-metric.

UPGMA gives a good idea of evolutionary time

Another distance based tree are **Additive Trees**, which don't assume a perfect molecular clock and thus don;t assume the tree is ultra-metric.

Instead we assume **Additivity**, which assumes that mutations accumulate but not at a constant rate. In addition, mutations are independent of each other. 

For additive trees the closest nodes are not necessarily neighbours, so we cant just pick the closest two pairs and join them. We need to compensate for high weighted arcs. To do this we subtract the averaged distances from each node to all other nodes:

\\[ D_{ab} = d_{ab} - r_a - r_b; \text{ where }r_i = \frac{1}{|L| - 2}\sum_{n\in L}d_{in} \\]

We then repeat until all nodes joined:
- Create new node k and join the two closest nodes to it (smallest \\(D_{ij} \\))
- Calculate the distances \\( d_{ki} \\) between k and all other nodes
- Calculate distances from k to both i and j using \\( d_{ki} = \frac{1}{2}(d_{ij} + r_i - r_j) \\)
- Calculate the weighted distance \\( D_{ki} \\) between k and all other nodes
- Remove i and j from the distance matrix and add k

Additivity requires that mutations accumulate. However this does not happen where backward mutations exist and two species find the same mutation. Thus, it is better to use parts of the genome which are known to not have backward mutations

Additive trees give a good idea of evolutionary distance. 

#### Sequence Based Trees

We want to find the tree that explains the evolutionary history of a set of sequences with as few mutations as possible 

We can use maximum parsimony trees for this, which uses aligned sequences. However, there is no efficient algorithm for this so need to iterate through all possible trees

We want to find the lowest cost tree. we need to create a cost:
- Assume each site is independent
- Assumes a binary tree where leaves are sequences
- Total cost is the sum of the costs at each site (this could be number of substitutions are BLOSUM50 costs, etc. )

We then enumerate all the trees to find the one with the lowest cost. However this is costly. We can use Branch and Bound (i.e. pruning) to reduce the search space. This involves removing subtrees which have a higher cost than the cheapest found as it can never become cheaper