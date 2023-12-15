Diamond Blastx
==============

Using known genes from a database to predict function/family of
predicted genes

Background
----------

Excerpt from the Diamond
`website <http://www.diamondsearch.org/index.php>`__

DIAMOND is a sequence aligner for protein and translated DNA searches,
designed for high performance analysis of big sequence data. The key
features are:

-  Pairwise alignment of proteins and translated DNA at 500x-20,000x
   speed of BLAST.
-  Frameshift alignments for long read analysis.
-  Low resource requirements and suitable for running on standard
   desktops or laptops.
-  Various output formats, including BLAST pairwise, tabular and XML, as
   well as taxonomic classification.

Installation
------------

The refseq non-redundant gene database was downloaded from ncbi.

Diamond conda version

.. code:: bash

   diamond                   0.9.29               h56fc30b_0    bioconda

installation

.. code:: bash

    conda install -c bioconda diamond 

Commands
--------

command to blast the predicted proteins against nr.

.. code:: bash

   # making diamond blast database
   diamond makedb --in nr.gz -d nr

   diamond blastp \
       -f 100 \
       -b25.0 \
       -c 1 \
       --threads 75 \
       -d /home/jon/Working_Files/nr.dmnd \
       -q /home/jon/Working_Files/braker2/braker_denovo_pcali_ajap_redun_plat/augustus.ab_initio.aa \
       -o /home/jon/Working_Files/blast

   diamond view \
       -f 6 \
       -a blast.daa > blast.tbl

diamond blastp of gene predictions from braker2 with rna-seq gene
predictions

.. code:: bash

   diamond blastp \
       -f 100 \
       -b25.0 \
       -c 1 \
       --threads 75 \
       -d /home/jon/Working_Files/nr.dmnd \
       -q /home/jon/Working_Files/braker2/braker_rna_pcali_platredun/augustus.hints.aa \
       -o /home/jon/Working_Files/blast

   diamond view \
       -f 6 \
       -a blast.daa > blast.tbl

diamond blastp of gene predictions from braker2 with rna-seq and *A.
japonicus* proteins

.. code:: bash

   diamond blastp \
       -f 100 \
       -b25.0 \
       -c 1 \
       --threads 75 \
       -d /home/jon/Working_Files/nr.dmnd \
       -q /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/pcali-rna.ajap-prot.augustus.hints.aa \
       -o /home/jon/Working_Files/blast

   diamond view \
       -f 6 \
       -a blast.daa > pcali-rna.ajap-prot.tbl

Results
-------
