---
layout: page
title: Mycoprotein - Molecular Genetics
description: Collaborative project with Quorn producer, Marlow Foods Ltd.
img: assets/img/cvariant_00.jpg
importance: 2
category: current
---

Quorn mycoprotein is a globally recognised, sustainable meat alternative derived from the filamentous fungus _Fusarium venenatum_. A major challenge in its large-scale industrial production is the spontaneous and consistent appearance of highly branched mutant strains, known as "colonial variants" or "**C-variants**". 

This dense, irregular growth pattern negatively affects the texture of the final product and forces the early termination of the fermentation process, limiting efficiency and sustainability. Despite being a long-standing problem for the industry, the precise genetic cause of this undesirable morphology was previously unknown. This project aimed to pinpoint the specific mutation responsible for the C-variant phenotype.

<br>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/cvariant_01.jpg" title="Fusarium venenatum colonies on agar plates" class="img-fluid" %}
    </div>
</div>
<div class="caption">
    Morphological diversity of F. venenatum strains isolated from commercial mycoprotein fermentations.
</div>

<br>

To investigate the genetic basis of this issue, we isolated and cultured the C-variant strains, as well as post-fermentation WT strains, from multiple independent commercial fermentation campaigns. Using whole-genome sequencing, we determined the genetic variation across these isolates and identified mutations in a single gene, jg4843, in 11 out of 12 C-variant samples. These mutations were completely absent in the wild-type strains. 

We identified this gene as the F. venenatum ortholog of LRG1, a protein known to regulate fungal growth and branching. The identified mutations were primarily located within the critical RhoGAP functional domain, which is essential for the gene's activity.

<br>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/cvariant_02.jpg" title="Missense mutations in LRG1" class="img-fluid" %}
    </div>
</div>
<div class="caption">
    Locations of missense mutations in the LRG1 protein.
</div>

<br>

To definitively prove that mutations in this gene were the cause, we used CRISPR/Cas9 to precisely introduce one of the non-synonymous SNPs found in a C-variant into the genome of a wild-type strain. This single nucleotide change was sufficient to completely recreate the characteristic dense, hyper-branching C-variant morphology. 

By identifying the mutation of the FvLRG1 gene as the causal event for C-variants in commercial fermentations, this research provides a clear target for developing strategies to prevent their appearance. This work paves the way for enhancing the efficiency, quality, and sustainability of mycoprotein production.

<br>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/cvariant_03.jpg" title="CRISPR base editing transformants" class="img-fluid" %}
    </div>
</div>
<div class="caption">
    A single non-synonymous SNP in the WT causes C-variant morphology.
</div>

<br>