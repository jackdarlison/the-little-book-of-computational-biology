# DNA

### Biology

DNA is a helix of antiparallel chains of nucleic acids, bonded via adenine (A) + thymine (T) or guanine (G) + cytosine (C) bonds

The human genome consists of 64 billion pairs. The majority of which resides in the nucleus of each cell, however, small amounts reside in the mitochondria. 

Sequences of DNA which have functional purposes are known as genes.

DNA creates structures called chromosomes, which are held together by histones (a protein). Humans have 23 pairs of chromosomes which reside in the nucleus of each cell. 22 of these are known as autosomes, numbered in decreasing size, and the 23rd are the sex chromosomes (Female is XX, male is XY)

DNA replication follows the process of:
- Helicase enzyme splits the two strands
- DNA polymerase (enzyme) and free nucleotides combine of the two strands

Mutations are alterations of the nucleotide sequence in the genome of an organism, caused by:
- DNA copying mistakes
- Environmental mutagens (e.g. radiation)
- Infection by retroviruses

Mutations consist of three types:
- Point mutations
- Frameshift (insertion or delations)
- chromosomal exchanges/alterations

Transposons are genes which can change position (usually during replication) in the genome. These can create or reverse mutations. 

Mutations can be hereditary (germline mutations) or occur during life (somatic mutations). Mutations can have negative, none, or positive benefits. 

RNA is a single strand of nucleic acid, the Thymine (T) base in DNA is replaced with Uracil (U) in RNA. RNA strands can associate (match) with single strands of DNA

There are three types of RNA:
- Messenger RNA, synthesised from DNA in nucleus and delivered to Ribosomes,  **Transcription**
- Ribosomal RNA, forms Ribosomes along with proteins
- Transfer RNA, used to create proteins, **Translation**

**Transcription** is the process of creating mRNA from DNA. The enzyme RNA polymerase unwinds the DNA into single strands, synthesises a strand of mRNA from one of the DNA strands, and finally recombines the DNA.

mRNA is transported to the ribosomes to undergo **Translation** which involves synthesising proteins based upon the template of the mRNA. tRNA consists of three nucleotide bases and are connected to an amino acid. The three bases in tRNA then match with sequences in the mRNA, thus the amino acids create a chain based on the sequence of mRNA. This is chain is later folded to create a specific protein.

The mRNA is **spliced** before translation. mRNA consists of two regions, the introns and exons. The introns are removed during the splicing process and exons combined, yielding sections of the mRNA called the *open reading frame* which are using in translation, there are untranslated regions either side of the open reading frames. 

**Codons** are the sequence of three nucleic bases that correspond to a specific amino acid. There are specific start (AUG) and stop (UAA, UAG, UGA) codons that indicate when to start and end translation for a specific protein. Codon tables map between  mRNA (*not tRNA*) and amino acids 

**Genes** are sequences of nucleotides which synthesise a specific gene product, i.e. a protein or RNA molecule. 

Only 2% of the genome are protein encoding exons


### Sequencing

The Humane Genome Project aims to sequence the entire genome, it is mostly finished. 

Sequencing the entire genome in one go is impossible, so smaller sequences are used and combined together on overlaps

DNA is sequenced by:
- separating the chromosomes
- Isolating the DNA
- fragmenting into manageable chunks using enzymes
- Inserted into bacteria, which replicates the DNA (Bacterial Artificial Chromosomes, BACS) so there is sufficient concentrations to run the sequencing on. 
- Copied DNA is sequenced and assembled into a physical map
- Gene locations are assessed, and genetic map produced

Three methods of sequencing:
- PacBio HiFi: 10-25kb with accuracy >99.5%
	- Florescence based
- Illumina: 0.25kb with accuracy >99.9%
	- Florescence based
- Oxford Nanopore: 1500kb with 87-98% accuracy
	- Electric current based

Reassembling sequences can be challenging as there may be ambiguities when aligning different sequences. Long repeats are especially ambiguous

### Tries

A Trie, or digital tree, is often used to store sets of words. In DNAs case there are only 4 letters. Each node is a letter, which branches to the next letter in the sequences. All sequences end with the $ character. 

Branches are usually collapsed if there is only one path. 

Tries provide a implementation for a set of words which has quick insertion, deletion and find. Making them useful for algorithms used for spell checking, completion, prefix and suffix matching.

With DNA they can be used to correct mis-reads by using known sequences tries and finding a good match. DNA uses a 5 way trie, 4 for the bases and 1 for end of sequence markers

There are many fixed and common sequences which can be used to match and check reads with, such as protein binding (promotor regions), stop and start codons, TATA box, ... Some areas of genes have much higher concentrations of certain bases, making them useful for finding areas of genes.


Tries can be quite memory intensive, Suffix trees can be built to use O(n) memory where n is the length of the text and be constructed in O(n) time

Suffix tries are useful for:
- Applications with multiple searches
- exact words matching is important.
- useful when looking for repeats















