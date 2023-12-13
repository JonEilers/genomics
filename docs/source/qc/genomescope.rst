Jellyfish and GenomeScope
-------------------------

Estimating genome characteristics

Background
~~~~~~~~~~

**Jellyfish** Excerpt from
`website <http://www.genome.umd.edu/jellyfish.html>`__

Jellyfish is a tool for fast, memory-efficient counting of k-mers in
DNA. A k-mer is a substring of length k, and counting the occurrences of
all such substrings is a central step in many analyses of DNA sequence.
JELLYFISH can count k-mers quickly by using an efficient encoding of a
hash table and by exploiting the “compare-and-swap” CPU instruction to
increase parallelism.

**GenomeScope** Excerpt from github
`website <https://github.com/schatzlab/GenomeScope>`__

Current developments in de novo assembly technologies have been focused
on relatively simple genomes. Even the human genome, with a
heterozygosity rate of only ~ 0.1% and 2n diploid structure, is
significantly simpler than many other species, especially plants.
However, genomics is rapidly advancing towards sequencing more complex
species such as pineapple, sugarcane, or wheat that have much higher
rates of heterozygosity (>1% for pineapple), much higher ploidy (8n for
sugarcane), and much larger genomes (16Gbp for wheat).

One of the first goals when sequencing a new species is determining the
overall characteristics of the genome structure, including the genome
size, abundance of repetitive elements, and the rate of heterozygosity.
These features are needed to study trends in genome evolution, and can
inform the parameters that should be used for the individual assembly
steps. They can also serve as an independent quality control during any
analysis, such as quantifying the quality of an assembly, or measuring
the expected number of heterozygous bases in the genome before mapping
any variants.

We have developed an analytical model and open-source software package
GenomeScope that can infer the global properties of a genome from
unassembled sequenced data. GenomeScope uses the k-mer count
distribution, e.g. from Jellyfish, and within seconds produces a report
and several informative plots describing the genome properties. We
validate the approach on simulated heterozygous genomes, as well as
synthetic crosses of related strains of microbial and eukaryotic genomes
with known reference genomes. GenomeScope was also applied to study the
characteristics of several novel species, including pineapple, pear, the
regenerative flatworm Macrostomum lignano, and the Asian sea bass.

Jellyfish Installation and Commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Installation into conda environment biotools

::

   conda activate biotools
   conda install -c bioconda jellyfish 

Jellyfish conda package information

::

   jellyfish                 2.2.10               h6bb024c_1    bioconda

Commands

::

   jellyfish count -C -m 21 -s 1000000000 -t 17 *.fq -o reads.jf
   jellyfish histo -t 17 reads.jf > reads.histo

GenomeScope Input
~~~~~~~~~~~~~~~~~

GenomeScope website can be found
`here <http://qb.cshl.edu/GenomeScope/>`__. The reads.histo file created
by jellyfish is uploaded to the website. The parameters I used for
GenomeScope were: \* kmer length: 21 \* Read length: 150 \* Max kmer
coverage: 1000

A more indepth breakdown of how GenomeScope works and how to interpret
the graphs can be found in the GenomeScope paper `Supplementary Notes
and
Figures <https://www.biorxiv.org/content/biorxiv/suppl/2017/02/28/075978.DC2/075978-1.pdf>`__

Results
~~~~~~~

{% include gallery %}

::

   GenomeScope version 1.0
   k = 21

   property                      min               max               
   Heterozygosity                3.6575%           3.7462%           
   Genome Haploid Length         476,443,101 bp    478,044,911 bp    
   Genome Repeat Length          86,075,406 bp     86,364,793 bp     
   Genome Unique Length          390,367,695 bp    391,680,118 bp    
   Model Fit                     93.7426%          95.1882%          
   Read Error Rate               0.218397%         0.218397%    

Thoughts
^^^^^^^^

GenomeScope is estimating the genome size of *P. californicus* to be
about 400 mb smaller than the 900mb published estimates. Heterozygosity
is higher than in *A. japonicuis*. Very strange, even after adjusting
the max kmer coverage the estimated genome size was still small.

{% include figure
image_path=“/assets/images/data_cleaning/GenomeScope/Apostichopusjaponicus.png”
caption=“Genome statistics from the paper `Zhang et al
2017 <https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2003790>`__”
%}

Model
^^^^^

::

   Formula: y ~ (((2 * (1 - d) * (1 - (1 - r)^k)) + (2 * d * (1 - (1 - r)^k)^2) + 
       (2 * d * ((1 - r)^k) * (1 - (1 - r)^k))) * dnbinom(x, size = kmercov/bias, 
       mu = kmercov) * length + (((1 - d) * ((1 - r)^k)) + (d * 
       (1 - (1 - r)^k)^2)) * dnbinom(x, size = kmercov * 2/bias, 
       mu = kmercov * 2) * length + (2 * d * ((1 - r)^k) * (1 - 
       (1 - r)^k)) * dnbinom(x, size = kmercov * 3/bias, mu = kmercov * 
       3) * length + (d * (1 - r)^(2 * k)) * dnbinom(x, size = kmercov * 
       4/bias, mu = kmercov * 4) * length)

   Parameters:
            Estimate Std. Error t value Pr(>|t|)    
   d       6.714e-02  6.585e-03    10.2   <2e-16 ***
   r       3.702e-02  2.217e-04   166.9   <2e-16 ***
   kmercov 5.760e+01  4.833e-02  1191.8   <2e-16 ***
   bias    1.877e+00  1.877e-02   100.0   <2e-16 ***
   length  4.103e+08  1.646e+06   249.3   <2e-16 ***
   ---
   Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

   Residual standard error: 172800 on 969 degrees of freedom

   Number of iterations to convergence: 8 
   Achieved convergence tolerance: 2.265e-06
