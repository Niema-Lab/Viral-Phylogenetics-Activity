# Viral Phylogenetics
In this activity, you will perform a viral phylogenetics analysis on real viral genome sequences. Specifically, in the [`datasets`](datasets) folder, we have provided whole genome sequence datasets for two viruses of interest: [HIV-1](datasets/HIV-1) and [SARS-CoV-2](datasets/SARS-CoV-2). Each dataset contains 10 genome sequences collected from real people, found in the `sequences.fas` file, as well as each sequence's collection date (i.e., the date on which the viral sample was collected from the patient), found in the `dates.txt` file.

# Step 1: Multiple Sequence Alignment
Much like how human genomes "evolve" (i.e., mutate) over time (e.g. between parent and offspring), viral genomes similarly mutate as the viruses replicate. Recall that the most common types of mutations are **substitutions** (in which one nucleotide is replaced with another), **insertions** (in which one or more nucleotides are inserted into the genome), and **deletions** (in which one or more nucleotides are deleted from the genome). Also, recall that these mutations are **heritable** (i.e., they pass down from parent to offspring).

If we were to sequence multiple viral genomes that descended from a common ancestor, any substitutions that occurred between the common ancestor and the present-day sequences would still result in sequences that line up with the ancestral sequence (and with each other). For example, imagine an ancestral viral genome sequence is `ACGTAC`, and imagine the first `A` mutated into a `G`, resulting in a descendant viral genome sequence of `GCGTAC`. The two genome sequences still line up properly (i.e., if I write them on top of one another, each column of the sequences corresponds to a single letter in the ancestral sequence):

```
123456 # position in ancestral sequence
ACGTAC # ancestral sequence
GCGTAC # descendant sequence
123456 # position in ancestral sequence
```

However, any insertions or deletions (commonly lumped together to create the colloquial term **indels**) would shift parts of the descendant sequence to the left (deletion) or right (insertion), breaking how the sequences line up. For example, imagine that the first `A` actually got deleted:

```
123456 # position in ancestral sequence
ACGTAC # ancestral sequence
CGTAC  # descendant sequence
23456  # position in ancestral sequence
```

Simiarly, if an `C` was inserted at the beginning of the sequence instead, the sequences would also not line up:

```
123456  # position in ancestral sequence
ACGTAC  # ancestral sequence
CACGTAC # descendant sequence
 123456 # position in ancestral sequence
```

Before we can infer the evolutionary relationships between genome sequences, we need to first line them up properly so that each column of the "alignment" corresponds to the same position in the ancestral genome sequence. The reason for this is that, by aligning the genome sequences in this way, all nucleotides that are in the same column in the alignment are **homologous** (i.e., they evolved from the same exact nucleotide position in the ancestral genome sequence), and this knowledge of homology is the key insight behind inferring the evolutionary relationships between the genome sequences. In the examples above, we can add gap characters (`-`) at various positions of the sequences to get them to line up properly:

```
-ACGTAC # ancestral sequence
-GCGTAC # descendant sequence 1
--CGTAC # descendant sequence 2
CACGTAC # descendant sequence 3
```

Thus, as a first step, we need to perform **Multiple Sequence Alignment**: we need to line up our viral genome sequences 
