---
author:
- Social Hackathon Group
title: |
    \
    Hackathon at CeMM - 24 April 2014 
...

Introduction
============

A hackathon is meant to synchronize a group of people and encourage
joint work on a single topic or theme. In industry such events are used
to kickstart development on a new project or to gather new ideas; the
goal is typically to produce a working example or prototype of an
application or algorithm. The hackathon at CeMM was meant to borrow some
of these concepts and apply them in a science-centered setting.

Theme
=====

The theme for the event was personal genomics and the analysis of
whole-genome sequencing data from a ‘normal’ individual. The topic is
anchored into current trends in science and society: sequencing is
becoming increasingly accessible and mainstream, so it is interesting to
ask what we can learn from analysis of a single genome.

In the literature and in industry, much effort has already been expended
on analysis of genomes and variants. One example is the service provided
by <span>23andMe.com</span> over the last several years. The objective
of the hackathon, however, was to attempt analysis of data produced
in-house.

Results
=======

We started with a joint brainstorming and discussion sesssion. Then, we
had an introduction to the available personal genome data provided by
the BSF. The data was stored under <span>/fhgfs/scratch/hackathon</span>
on the compute cluster and consisted of:

-   one alignment file (<span>bam</span>) - bwa, approximately 12x
    coverage.

-   one variant call file (<span>vcf</span>) - GATK, approximately 4.1M
    lines.

After the introduction to the data, we split up into smaller groups and
tried to tackle the genome analysis problem from different angles.
Throughout the event, we had spontaneous exchanges between the smaller
groups.

Global variant statistics
-------------------------

Variant call files for whole genome sequencing can contain millions of
identified items.

We ran the variant call file through annotation programs Annovar and
snpEff and looked through the summary tables, stratifying variants by
chromosomes, substitution type, genomic region, etc.

Repeat the analysis on sets of variants obtained from a merge of
multiple replicates (multiple replicates are available, although were
not merged at time of hackathon).

Prioritization of variants
--------------------------

Most variants in a genome are polymorphism that have no known or
expected phenotype. A challenge in annotation is to identify the handful
of variants that may be interesting from a medical viewpoint.

We created short lists of interesting variants based on output from
Annovar, polyPhen, and intersecting with database ClinVar. For example,
we produced a list of variants with STOP codons in protein coding genes
and inspected some of these variants in <span>IGV.</span>

Carry out more rigorous variant filtering. Study other variant classes.
Rank by variant quality score, conservation score, or predicted effect.

Quality control
---------------

A challenge in genome annotation is to distinguish true genetic
variation from technical artifacts introduced in the sequencing
chemistry (library prep) or informatics (e.g. alignment).

Starting from the provided alignment, we produced an alternative set of
variant calls with independent software. We compared calls of single
nucleotide substitutions (3.5M common calls, 100k-300k calls unique to
each method).

Compare indel calls. Compare with higher-coverage merged samples.

Pharmacogenomics
----------------

Although some variants (e.g. STOP codons) can be predicted to be
phenotypically interesting, the effect of many others is difficult to
judge. Pharmacogenomics connects a distinct genetic makeup to an
individual’s response to drugs by correlating genetic variants to a
drug’s efficacy or toxicity, ultimately optimizing drug treatment
towards personalized medicine.

We intersected a “core list” of genetic biomarkers from the PharmaADME
initiative with variants identified in our whole-genome sequencing data.
Beside relatively frequent variants, we identified variants in ABC, CYP,
SLC and SULT proteins that only have global allele frequencies of
10-20%.

Gather more information on how the identified variants modulate
pharmacodynamics and pharmacokinetics. Assemble a list of compounds
whose action and/or metabolism is improved/impaired by the identified
variants.

Protein interactions
--------------------

Some variants translate into changes in protein sequence and structure,
and this can have an impact on interaction with other proteins that
results in phenotypic changes.

We have intersected variants annotation with public protein interactions
databases and identified interactions, where both partners contain
variants classified as ‘probably damaging’ by PolyPhen. Such
interactions have increased probability to be disrupted.

Follow up on hits and interactors from the databases. Consider position
of variant within protein (surface, interaction site, binding motifs,
phosphorylation sites etc) and the type of protein-protein interaction
(physical association in a complex, direct binding) to rank affected
interactions. Integrate with network analysis methods to estimate the
impact of disrupting interaction on the cellular system.

Analysis of ancestry
--------------------

Some genetic variants are common in one population but not in another,
and this can be used to calculate the ancestry of a sample and in some
cases even to identify a donor personally.

We downloaded variant information on 12 different population from the
1000 Genomes project. First, we attempted PCA on variants from one
chromosome. Then, we used exonic variants and produced
multi-dimensional-scaling diagram using PLINK (Fig [ancestry]).

Repeat the analysis using a newer release of the 1000 Genomes project,
which contains more individuals and more population groups.

![[ancestry]<span>**Ancestry clustering. **</span> Multi-dimensional
scaling plot produced using PLINK. Personal genome PGA1 (black triangle)
clusters with samples of European descent. ](ancestry_MDS.pdf)

Structural variants
-------------------

Structural variants are changes in a genome that comprise large regions,
e.g. copy number of transposons. Such variants can be detected via
analysis of coverage or via search of abnormally mapping mate pairs.

This type of analysis requires higher coverage than 10x in the available
<span>bam</span> file.

Revisited this idea after merging multiple replicates together (data
available from the BSF, but not merged at the time of event)

Telomeres
---------

Telomores are sequences on the ends of chromosome; their length is known
to diminish with age. It is thus conceivable to determine the age of a
genome through analysis of coverage on telomeric regions.

We found software in the litererature presenting a classifier of age
based on telomere length (<span>github.com/zd1/telseq</span>). Attempted
to install the software, but many dependencies were missing.

Run the program on the bam file and estimate age of individual.

Visualization
-------------

Visualization is an important tool for exploring large datasets as well
as communicating results. Recently, several frameworks have been
developed to enable creation of dynamic and interactive figures.

We looked at examples of visualizations produced using
<span>D3.js</span> and selected one example that could be useful for
portraying genomic data
(<span>github.com/vogievetsky/KoalasToTheMax</span>). We managed to run
it locally but not to modify it in substantial ways.

Learn more about the <span>D3.js</span> framework. Build simple examples
from the ground up.

23andMe
-------

Genome analysis is now a service offered by several companies. Data from
our donor was also previously analyzed by <span>23andMe</span>.

During a break toward the end of the evening, we had a look altogether
at results produced by <span>23andMe</span>. One variant in gene HBB
caught our attention (SNP rs11549407 linked with beta thalessemia), but
when we first looked for it in the sequencing data, we did not
immediately see it. However, upon manual inspection, a variant at the
same locus was labelled with a different id (rs76728603) (Fig. [HBB])

Think about synonyms in databases and how they affect our understanding
of the genome. Study the professional website and gather
ideas/inspiration for future work.

![[HBB]<span>**Heterozygous variant in gene HBB (chr11). **</span>
Substitution variant in gene HBB is visible in alignment and was
(marginally) detected by variant calling software. However, dbSNP ids
associated to the variant by <span>23andMe</span> and our pipelines were
not verbatim identical. ](HBB_snp.png)

Discussion
==========

We attempted analysis of one personal genome, “PGA1,” sequenced by the
BSF. Along the way we looked at the genome from multiple different
angles.

Understandly, in the short timeframe, concrete results arose primarily
from efforts using familiar and established tools - for example we
categorized variants by their predicted phenotype and studied a few
individual cases. These are case studies for how personal genome
sequencing can give insight into health.

Some efforts identified a number of new directions that have not been
attempted/established at CeMM so far - for example study of telomere
length or presentation of data interactively. These efforts raise
awareness of developments in the field and could be followed up on.

Commendably, some efforts began during the hackathon and were completed
in later days - for example analysis of ancestry.

Participants
============

In alphabetical order:

-   Christoph Bock

-   Doris Chen

-   Katrin Hoermann

-   Johanna Klughammer

-   Tomasz Konopka

-   Alexandra Kornienko

-   Angelo Nuzzo

-   Adrian Cesar Razquin

-   Fiorella Schischlik

-   Andreas Schoenegger

-   Michael Schuster

-   Alexey Stukalov

Others expressed interest in the event/topic but could not attend on
this occasion.

Acknowledgements
================

Thanks to Christoph for suggesting the topic; Mischa, Joachim and IT for
help with the setup on the cluster; Angelo and BSF for the introduction
to the data. Special thanks also to Giulio for donating sample “PGA1”.

Supplement
==========

Miscellaneous additional text and analysis details.

Data files
----------

During the course of the event, we created multiple code and derived
data files. All the information is stored under
<span>/fhgfs/scratch/hackathon</span>.

Variant Filtering
-----------------

After it turned out that the provided bam file did not provide the
proper coverage for CNV analysis, I decided to work on in-depth
annotation of the vcf file (which is anyway something I am interested at
the moment). For that purpose I installed an updated version of Annovar
and related databases (e.g. ClinVar) and ran annotation by the
table\_annovar.pl script from Annovar (after conversion into Annovar
format); command:

    /fhgfs/groups/lab_kralovics/Doris/bin/perl_tools/annovar/table_annovar.pl 
      analysis_ready_vcf.annovar

    /fhgfs/prod/ngs_resources/genomes/hg19/forAnnovar -buildver hg19 
      -out analysis_ready_vcf -remove -protocol 
      refGene,phastConsElements46way,genomicSuperDups,esp6500si_all,
      1000g2012apr_all,snp137,ljb2_all,cosmic64,clinvar_20140211 
      -operation g,r,r,f,f,f,f,f,f -nastring NA -csvout 2>&1 | 
      tee tableAnnovar-9_140424.log )

Since this results in a huge file, I extracted the ones which were
flagged to be “exonic” or “splicing” variants, in total around 21k
(/fhgfs/scratch/hackathon/variant\_annotation/annovar/annovar-201308\_not-filtered
/analysis\_ready\_vcf\_hg19\_multianno\_exonic-splicing.csv). To further
reduce the number of variants, one can additionally exclude synonymous
and non-pathogenic (as annotated by ClinVar) ones and retain only those
with ClinVar annotation, resulting in 122 variants (including one
pointing to beta-thalassemia); s.a.

    /fhgfs/scratch/hackathon/variant\_annotation/annovar/annovar-201308\_not-
    filtered/analysis\_ready\_vcf\_hg19\_multianno\_exonic-splicing\_CLNDBN\_
    woSyn-NonPatho.txt

Two important things are missing: (1) Addition of quality information to
the annotation tables (2) Filtering according (1).


#### Added to repository
File converted from TeX with pandoc: `pandoc -s Hackathon_April2014.tex -o README.md` and added to repository on 2015-03-30.
