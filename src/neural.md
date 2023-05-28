# Neural Applications

HMMs can only do so much. Predicting secondary protein structure is difficult as the data is non-local, which HMMs don't deal well with.

Neural Networks can be used.

NETtalk is a MLP with windowed input that is used to predict protein structure, it uses:
- Mean squared erorr as a loss function
- Windowed input (target and ±n context words). This allows input of an arbitrary length sequence
- One hot encoding for representation

Profiling homologous proteins (same tertiary structure and approximately same secondary structure). Allows relationships between different proteins to be expressed in the coding. +5% accuracy gains

One hot encoding is used for amino acids as using a scalar number would imply that amino acid 1 and 2 are more related to 1 and 20. Amino acids are not related so one hot encoding is used. 

The distribution of secondary structure types is not balanced, Using balanced training data will reduce the overall performance slightly. 

Using structural context, e.g. alpha helices are ~ 10 amino acids long and beta sheets are ~ 6 long, can be used to check valid predictions

**PSI BLAST** is a MLP based local alignment searcher. This can be used with **PSI PRED** to predict secondary structures ~77%


Cross entropy loss + softmax is better than MLE for multi-class classification as it deals better with minority classes:
- Softmax: \\( P(i) = \frac{e^{x_i}}{\sum_j e^{x_j}} \\)
- Cross entropy Loss:  \\( H(y, q) = - \sum^N_{i=1} y_i \log q_i \\)

#### Deep learning

To get better results we need deep neural nets (DNNs). Where are neural networks with 3 or more layers. 

**LSTMs** are recurrent memory cells with allow networks to learn long range dependencies. This makes them good at language modelling, protein homology detection.

**Deep belief nets** learn a layer at a time, which is then fixed before learning the next layer. This has applications for estimating protein model quality and stability predictions. 

**Convolutional neural nets** are inspired by the visual cortex. Uses filters and weight sharing. This is mainly used in computer vision which has many uses is biology such as classifying medical imagery and gene expressions

**Auto encoders** are used to encode information, such as word or DNA to vectors by learning a vector embedding for them. These representations can carry semantic meaning unlike other encoding forms like one hot encoding

**Attention** and **Transformers** are a modern architecture of DNNs where the idea is that some words are more important than others. Skip connections are also used to preserve information

CASP is a competition ran twice yearly that tasks people to create the best model for predicting protein structures

**Residual Nets** such as AlphaFold are the latest in this field and variations have one every year since 2017

AlphaFold is good because:
- input:
	- Multiple sequence alignment
	- pairwise templates
- Embeddings combine:
	- sequence alignments
	- coevolutionary features
	- template angles
	- template pairwise features
- Evoformer:
	- treats structure as spacial graph
	- Builds graph using embeddings
	- combines pairwise and msa embeddings
	- 48 blocks
	- attention
- structure:
	- uses attention
+ Recycling
 




