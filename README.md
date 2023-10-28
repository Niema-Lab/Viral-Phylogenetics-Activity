# Viral Phylogenetics
In this activity, you will perform a viral phylogenetics analysis on real viral genome sequences. Specifically, in the [`datasets`](datasets) folder, we have provided whole genome sequence datasets for two viruses of interest: [HIV-1](datasets/HIV-1) and [SARS-CoV-2](datasets/SARS-CoV-2). Each dataset contains 10 genome sequences collected from real people, found in the `_sequences.fas` file, as well as each sequence's collection date (i.e., the date on which the viral sample was collected from the patient), found in the `_dates.txt` file.

We will be performing our viral phylogenetics analyses using ViralWasm-Epi, a web-based tool for performing viral molecular epidemiological analyses:

https://niema-lab.github.io/ViralWasm-Epi

## Step 1: Multiple Sequence Alignment (MSA)
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

Thus, as a first step, we need to perform **Multiple Sequence Alignment (MSA)**: we need to line up our viral genome sequences. While this may seem like a simple problem at a glance, it's actually an incredibly computationally-intensive task to perform on large datasets. Fortunately for us, this dataset is small, so MSA can be performed quite quickly.

## Step 2: Phylogenetic Inference
Once we have our Multiple Sequence Alignment (MSA), we can perform **Phylogenetic Inference**: using mathematical models of evolution, we can find the **phylogeny** (i.e., evolutionary tree) that best explains the evolutionary relationships between the sequences in our MSA. For reasons that are out of the scope of this workshop, the trees we produce when we perform phylogenetic inference are **unrooted**: they describe *relative* evolutionary relationships between the sequences (which appear at the tips, or **leaves**, of the tree), but they don't know the directionality of time and thus don't know where the **root** (i.e. the **Most Recent Common Ancestor (MRCA)**) exists.

Also, because we didn't provide the phylogenetic inference methods any temporal information (just information about mutations, represented within the MSA), the lengths of the branches in the evolutionary trees produced by phylogenetic inference methods are in unit of **mutations** (specifically, "average number of mutations per position of the sequence," or "number of mutations divided by sequence length").

## Step 3: Rooting + Dating
As mentioned, the trees produced by phylogenetic inference methods are unrooted, and the branch lengths are in unit of mutations (not time). However, if we know the collection dates of the sequences in our dataset, we can use mathematical models of evolution to determine the most likely root as well as the most likely mutation rates across the branches in the tree. In other words, we can "root" the tree (i.e., find the most likely position of the Most Recent Common Ancestor (MRCA)) as well as "date" the tree (scale the branch lengths to be in unit of time).

# Perform the Analysis!
As mentioned, you will be using [ViralWasm-Epi](https://niema-lab.github.io/ViralWasm-Epi) to perform your viral phylogenetics analysis. Download the [HIV-1](datasets/HIV-1) or [SARS-CoV-2](datasets/SARS-CoV-2) dataset and follow these steps:

1. Go to the ViralWasm-Epi website: https://niema-lab.github.io/ViralWasm-Epi
2. For "Input Sequence File", select the `_sequences.fas` file from the dataset that you selected
3. Make sure that the "Skip Sequence Alignment" box is **not** checked (because we need to perform alignment)
4. For "Select Preloaded Reference Sequence", from the dropdown, select either `HIV-1` or `SARS-CoV-2` (depending on which of the datasets you selected)
5. Check the "Omit Reference Sequence from Output" box
6. Check the "Perform Phylogenetic Inference" box
  1. Make sure that the "Use Generalized Time Reversible (GTR) Model" and "Gamma Likelihoods" boxes are checked
7. Check the "Perform Tree Rooting and Dating" box
  1. For "Input Date File", select the `_dates.txt` file from the dataset that you selected
  2. Make sure that the "Infer Root" box is checked
  3. Keep all the other tree rooting/dating parameters as their default values
8. Click the blue "Run ViralWasm-Epi" button at the bottom
