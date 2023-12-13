================
MaSuRCA Assembly
================


.. contents::
   :depth: 3
..

MaSuRCA
=======

Assemble the sea cuke genome using the MaSuRCA assembler

Background
----------

Excerpt from the MaSuRCA
`website <http://www.genome.umd.edu/masurca.html>`__

MaSuRCA is whole genome assembly software. It combines the efficiency of
the de Bruijn graph and Overlap-Layout-Consensus (OLC) approaches.
MaSuRCA can assemble data sets containing only short reads from Illumina
sequencing or a mixture of short reads and long reads (Sanger, 454,
Pacbio and Nanopore).

More information can be found on their `github
page <https://github.com/alekseyzimin/masurca>`__

Installation
------------

MaSuRCA was installed using conda

installation

.. code:: bash

   conda create -n masurca
   conda activate masurca 
   conda install -c bioconda masurca 

Command
-------

Running MaSuRCA requires a bit more work then the normal assembler.
First, MaSuRCA is run on config.txt file which has been previously
edited to the desired parameters. The config file I used can be found
`here </assets/images/masurca/masurca_config.txt>`__. This creates an
assemble.sh file. Executing the assemble.sh file runs MaSuRCA.

::

   ./assemble.sh


=======================
Platanus-Allee Assembly
=======================


.. contents::
   :depth: 3
..

Platanus-Allee
==============

Assembly of hetereozygous genomes

Background
----------

Excerpt from the Platanus-Allee
`website <http://platanus.bio.titech.ac.jp/platanus2>`__

We are pleased to announce that our novel genome assembler
“Platanus-allee” has now been launched. Platanus-allee is an assembler
derived from Platanus assembler, however, it was developed with another
concept. Platanus-allee tries to construct each haplotype sequence from
the beginning and pair them as homologous chromosomes, while Platanus
constructs consensus sequence of homologous chromosomes at first and
tries to split into each haplotype sequence. Therefore, Platanus-allee
marks better performance for highly heterozygous species genome or
highly diverged genomic regions. However, for low heterozygous species
genome (as a guide < 1.0 %), Platanus assembler would mark better
performance than Platanus-allee. This may be caused by the sequence
coverage for the target sequence. Platanus-allee targets each haplotype,
therefore, simply speaking, it requires a double number of reads.

Installation
------------

Platanus-Allee does not have a conda package. It requires downloading
the prebuilt binary packages from their website.

I downloaded v.2.0.2 Linux 64 bit binary (precompiled) and uncompressed
it.

Command
-------

::

   ./platanus_allee assemble -t 70 -m 352 -f ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 

   ./platanus_allee consensus -t 70 -c out_primaryBubble.fa out_nonBubbleHomoCandidate.fa -IP1 ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 

   ./platanus_allee phase -t 70 -c out_contig.fa out_junctionKmer.fa -IP1 ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 

Results
-------

Resource usage for the contig assembly, consensus, and phasing steps
respectively

::

   #Assembly
   #### PROCESS INFORMATION ####
   VmPeak:         314.208 GByte
   VmHWM:           21.938 GByte

   #Phasing
   #### PROCESS INFORMATION ####
   VmPeak:          44.281 GByte
   VmHWM:            2.374 GByte

   #Consensus
   #### PROCESS INFORMATION ####
   VmPeak:           0.023 GByte
   VmHWM:            0.005 GByte

Optimal kmer calculations

::

   K=32, KMER_COVERAGE=89.9424 (>= 6), COVERAGE_CUTOFF=6
   K=52, KMER_COVERAGE=73.8309, COVERAGE_CUTOFF=6, PROB_SPLIT=10e-inf
   K=72, KMER_COVERAGE=57.7195, COVERAGE_CUTOFF=6, PROB_SPLIT=10e-inf
   K=92, KMER_COVERAGE=41.608, COVERAGE_CUTOFF=6, PROB_SPLIT=10e-10.6771
   K=107, KMER_COVERAGE=29.5244, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.1335
   K=109, KMER_COVERAGE=27.9132, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.1843
   K=110, KMER_COVERAGE=27.1077, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.0229

| Logs file can be found below
| `assemble.log </assets/images/platanus-allee/assemble.log>`__
| `consensus.log </assets/images/platanus-allee/consensus.log>`__
| `phase.log </assets/images/platanus-allee/phase.log>`__
