---
layout: post
title: Estimating Genome Size Using Jellyfish and GenomeScope2
date: 2024-11-17
description: A guide to estimating genome size using k-mer distributions.
tags: bioinformatics genomics visualisation kmer
---

Understanding genome size is a fundamental step in genomics. It provides crucial insights into an organism's biology, helps in planning sequencing projects, and is essential for interpreting downstream analyses. 

Estimating genome size from sequencing data is an efficient and cost-effective method that uses k-mer distributions to infer genome characteristics, including size, heterozygosity, and repeat content. 

In this blog post, we’ll walk through a workflow for genome size estimation using **Jellyfish** and **GenomeScope2**.

<br>

## Background

### Why Estimate Genome Size?

Genome size, most commonly measured in base pairs, varies widely across organisms. Accurate estimates guide sequencing depth requirements, inform assembly strategies, and provide insights into evolutionary biology, such as genome duplication or repeat content.

<br>

### Theory Behind the Tools

**K-mers** are sequences of length *k* bases (e.g., 21 bases for *k = 21*). If *k* is sufficiently large to ensure uniqueness and the genome length greatly exceeds *k*, the total number of k-mers approximately equals the genome size. Sequencing errors and repeats influence k-mer counts, creating distinct patterns in k-mer frequency histograms.

**Jellyfish** efficiently counts k-mers in sequencing reads, producing a k-mer histogram that reflects the genome’s complexity. The peak of the histogram corresponds to the most frequent k-mer coverage, while the distribution of peaks reveals features such as heterozygosity, repeats, and sequencing errors.

**GenomeScope2** interprets k-mer histograms to estimate genome size, heterozygosity, and repeat content. It uses a statistical model to fit the histogram, assuming a specific ploidy. The output includes plots visualising the k-mer frequency distribution, estimated genome size, heterozygosity, and repeat content, providing an intuitive overview of the genome’s structure and complexity.

<br>

## The Workflow

As an example, we'll use a script that I wrote for analysing sequencing data from haploid fungal isolates, which is optimised for execution on a Slurm-based HPC cluster.

Here is the script we’ll use:

```bash
#!/bin/bash
#SBATCH -J jellyfish
#SBATCH --partition=short
#SBATCH --mem=4G
#SBATCH --cpus-per-task=2

# INPUTS
Reads=$1
Prefix=$2
OutDir=$3
KMER=$4

source activate jellyfish

echo "Running jellyfish with the following parameters:"
echo "Reads: $Reads"
echo "Prefix: $Prefix"
echo "Kmer Size: $KMER"
echo "Output Directory: $OutDir"

mkdir -p $OutDir

zcat $Reads | jellyfish count -C -m $KMER -s 1G -t 4 -o $OutDir/$Prefix.jf /dev/fd/0

jellyfish histo -t 4 $OutDir/$Prefix.jf > $OutDir/$Prefix.histo

genomescope2 --input $OutDir/$Prefix.histo --kmer_length $KMER --ploidy 1 --max_kmercov 10000 --output $OutDir --name_prefix $Prefix

echo ""
echo "Run complete."
# If you're organism is a diploid, you can also upload $Prefix.histo to <http://genomescope.org/> to determine estimated genome size and heterozygosity rate.

```

<br>

### Inputs and Parameters

1. **Reads:** Path to the gzipped sequencing reads.
2. **Prefix:** Name prefix for output files.
3. **OutDir:** Directory to store output files.
4. **KMER:** K-mer size, typically 21-31 for most genomes.

<br>

### Outputs

- `Prefix.jf`: Jellyfish database of k-mers.
- `Prefix.histo`: K-mer frequency histogram.
- GenomeScope2 results, including plots and summary files documenting genome size estimate, heterozygosity, and repeat content.

<br>

## Step-by-Step Breakdown

### Step 1: K-mer Counting with Jellyfish

Jellyfish efficiently counts k-mers from sequencing reads. The script uses:

- `C`: Counts canonical k-mers (ignores reverse complements).
- `m`: Specifies the k-mer length.
- `s`: Sets the hash size (1G here).
- `t`: Threads for parallel processing.

<br>

### Step 2: Generate K-mer Histogram

The histogram shows k-mer frequencies. A typical histogram includes:

- A sharp peak at low frequencies (sequencing errors).
- A main peak (true genomic k-mers).
- Secondary peaks (repeats or heterozygous k-mers).

<br>

### Step 3: Analyze with GenomeScope2

GenomeScope2 fits a model to the k-mer histogram:

- Estimates genome size.
- Identifies heterozygosity and repeat content.
- Supports different ploidies.

The results include detailed plots, such as the k-mer frequency distribution, and numerical summaries, making it easy to interpret the genome's complexity and structure.

<br>

## Running the Script

### Example Command

```bash
sbatch genome_size_estimation.sh reads.fq.gz sample_name ./output_dir 21

```

- `reads.fq.gz`: Input sequencing reads.
- `sample_name`: Prefix for output files.
- `./output_dir`: Directory for results.
- `21`: K-mer size.

<br>

### Interpreting Results

The GenomeScope2 output includes:

- **Genome Size:** Estimated size in base pairs.
- **Heterozygosity:** Proportion of heterozygous sites.
- **Repeat Content:** Percentage of the genome that is repetitive.

<br>

## Additional Tips

1. **Choosing K-mer Size:**
    - Smaller k-mers capture more data but may inflate error peaks.
    - Larger k-mers reduce errors but require higher coverage.
2. **Quality Control:** Ensure input reads are high quality and adapter-trimmed.
3. **Ploidy Consideration:** For diploid organisms, adjust `-ploidy` to 2 in GenomeScope2.

<br>

By integrating Jellyfish and GenomeScope2, you can quickly estimate genome size and other critical genomic properties. This workflow is especially useful for non-model organisms or projects in the early stages of genomic analysis.

<br>

## Support  

If you find this blog post helpful, please consider [buying me a coffee on Ko-fi](https://ko-fi.com/jordanprice). Your support is very much appreciated!  

<p style='text-align: center'>
    <a href='https://ko-fi.com/jordanprice' target='_blank'>
        <img height='36' style='border:0px;height:36px;' src='https://storage.ko-fi.com/cdn/kofi2.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' />
    </a>  
</p> 