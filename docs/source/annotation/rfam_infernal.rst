rfam/Infernal and tRNAscan-SE
=============================

Identify non-coding rna in the genome assembly and annotate them

Background
----------

Excerpt from rfam `website <https://rfam.xfam.org/>`__

The Rfam database is a collection of RNA families, each represented by
multiple sequence alignments, consensus secondary structures and
covariance models (CMs). Less…

The families in Rfam break down into three broad functional classes:
non-coding RNA genes, structured cis-regulatory elements and
self-splicing RNAs. Typically these functional RNAs often have a
conserved secondary structure which may be better preserved than the RNA
sequence. The CMs used to describe each family are a slightly more
complicated relative of the profile hidden Markov models (HMMs) used by
Pfam. CMs can simultaneously model RNA sequence and the structure in an
elegant and accurate fashion.

Rfam families are frequently built from external sources, we ask that if
you find a particular family useful for your work that you cite both
Rfam and the primary source of our data.

Excerpt from Infernal `website <http://eddylab.org/infernal/>`__

Infernal (“INFERence of RNA ALignment”) is for searching DNA sequence
databases for RNA structure and sequence similarities. It is an
implementation of a special case of profile stochastic context-free
grammars called covariance models (CMs). A CM is like a sequence
profile, but it scores a combination of sequence consensus and RNA
secondary structure consensus, so in many cases, it is more capable of
identifying RNA homologs that conserve their secondary structure more
than their primary sequence.

Excerpt from tRNAscan-SE
`publication <https://www.ncbi.nlm.nih.gov/pubmed/31020551>`__

Transfer RNAs are the largest, most complex non-coding RNA family,
universal to all living organisms. tRNAscan-SE has been the de facto
tool for predicting tRNA genes in whole genomes. The newly developed
version 2.0 has incorporated advanced methodologies with improved
probabilistic search software and a suite of new gene models, enabling
better functional classification of predicted genes.

tRNAscan-SE website can be found
`here <http://lowelab.ucsc.edu/tRNAscan-SE/index.html>`__

Installation
------------

tRNAscan-SE conda version

.. code:: bash

   trnascan-se               2.0.5           pl526h516909a_0    bioconda

installed trnascan, barrnap, snoscan using conda

::

    conda install -c bioconda trnascan-se 
    conda install -c bioconda barrnap 
    conda install -c bioconda snoscan 

Infernal conda version

.. code:: bash

   infernal                  1.1.2                h14c3975_2    bioconda

installing infernal using conda, note this is version 1.1.2

.. code:: bash

    conda install -c bioconda infernal 

downloading the datasets from rfam and unpacking them

.. code:: bash

   wget ftp://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/Rfam.cm.gz
   gunzip Rfam.cm.gz
   wget ftp://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/Rfam.clanin

Commands
--------

indexing the rfam file using infernal

::

   cmpress Rfam.cm

checking the pcali genome size using easel tools. This is for the genome
after reads less than 1kbp were removed

::

   esl-seqstat /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa

running infernal

::

   cmscan -Z 1579.280944 --cut_ga --rfam --nohmmonly --tblout pcali_plat_redun_fil_1k_genome.tblout --fmt 2 --clanin Rfam.clanin Rfam.cm /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa > cali_plat_redun_fil_1k_genome.cmscan

running tRNAscan

::

   tRNAscan-SE -d -l trnascan-se.log -a trna_pcali_plat_redun_fil_1k_genome.fa  -E -o trna_pcali_plat_redun_fil_1k_genome -f trna_pcali_plat_redun_fil_1k_genome_struct -m stats_trna_pcali_plat_redun_fil_1k_genome /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa

Results
-------

checking tRNA count in the Rfam/Infernal output. Yikes, that is
suspicious. Note that is double the actual number as tRNA appears twice
in the respective row

::

   grep -o "tRNA" pcali_plat_redun_fil_1k_genome.deoverlapped.tblout | wc -l
   7282

tRNAscan is able to predict if a tRNA is a psuedo-gene or not. This
greatly reduced the number of actual tRNA and increased the number of
psuedo-genes which is pretty neat. The data is in a table format and I
didn’t save the analysis when I was looking at the table though.
