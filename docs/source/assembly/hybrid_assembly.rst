--------------

title: “Hybrid assembly using Platanus-allee and MaSuRCA” permalink:
/hybrid_genome_assembly/ layout: single toc: true toc_sticky: true

sidebar: nav: sidebar-main header: overlay_image:
/assets/images/home/cuke1.jpg

excerpt: “The Short and Long”

--------------

Introduction
============

*Apostichopus japonicus*, also called the Japanese sea cucumber is found
in the eastern coast of russia, china, korea, and around japan. It is an
important fishery and model organism for studying regeneration. The
official NCBI reference genome was published in
`2017 <https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2003790>`__
with another lower quality assembly published a year before this.

While I was investigating the structure of telomerase in *Apostichopus
californicus* I found that the telomerase in *Apostichopus japonicus*
was fragmented due to numerous assembly errors. There were enough errors
that it was not going to be an easy process to manually correct them. So
I decided it would make for a good hybrid genome assembly example.

I looked for the raw data from the paper and found that they uploaded
the rna-seq expression data but none of the data for the genome
assembly. Thankfully, another study, `Sea cucumber genome provides
insights into saponin biosynthesis and aestivation
regulation <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6018497/>`__
published shortly after this one also included a large amount of raw
sequence data that they used for genome assembly. Amusingly, they never
made their genome assembly public.

Hybrid genome assembly projects use multiple data types in the assembly
process. This can range from several different lengths of illumina short
read data to oxford nanopore or pacbio long read data, 10x-chromium,
optical mapping, hi-c etc. Essentially, if you are using more than one
data type you will be doing hybrid assembly. For the last ten or so
years, this has been the standard for achieving the best possible genome
assembly.

The Data
========

See below for data links and data type.

| 180bp paired end -
  `SRR6251237 <https://www.ncbi.nlm.nih.gov/sra/SRR6251237>`__
| 350bp paired end -
  `SRR6255867 <https://www.ncbi.nlm.nih.gov/sra/SRR6255867>`__
| 450bp paired end -
  `SRR6255870 <https://www.ncbi.nlm.nih.gov/sra/SRR6255870>`__
| 500bp paired end -
  `SRR6255868 <https://www.ncbi.nlm.nih.gov/sra/SRR6255868>`__
| 5kb long insert mate-pair -
  `SRR6257769 <https://www.ncbi.nlm.nih.gov/sra/SRR6257769>`__
| 10kb long insert mate-pair -
  `SRR6257771 <https://www.ncbi.nlm.nih.gov/sra/SRR6257771>`__
| 15kb long insert mate-pair -
  `SRR6257773 <https://www.ncbi.nlm.nih.gov/sra/SRR6257773>`__
| 20kb long-insert mate-pair -
  `SRR6257777 <https://www.ncbi.nlm.nih.gov/sra/SRR6257777>`__
| pacbio clr long read -
  `SRR6282347 <https://www.ncbi.nlm.nih.gov/sra/SRR6282347>`__

Hybrid Genome Assemblers
========================

There a number of genome assembly tools that accept more than one data
type. Two of my favorite are
`platanus-allee <https://www.nature.com/articles/s41467-019-09575-2>`__
and
`MaSuRCA <https://academic.oup.com/bioinformatics/article/29/21/2669/195975?login=true>`__.
Both MaSuRCA and Platanus-allee require at least short read data but
also accept long read data, and in the case of platanus-allee,
10x-chromium data. One significant difference between the two assembler
is regarding genome phasing. A phased genome is when the assembler is
able to distinguish the haplotypes of each chromosome pair. In the past,
assemblers were designed to ignore ploidy levels as the sequence data
was inadequate for telling chromosomes apart. This is not the case
anymore. With long read sequencing and hi-c it is now possible to
completely disintangle homologous chromsomes, although this is just
beginning to become standard practice.

Platanus-allee was designed specifically to be able to “phase” a genome
assembly and then, if you wanted, collapse the phased assembly into a
haploid assembly. To the best of my knowledge MaSuRCA does not have this
ability yet. However, MaSuRCA is significantly faster and at least in
this project, produced a more contigious assembly. Both tools are
designed to be used with heterozygous genomes, which is important as the
*Apostichopus japonicus* has a heterzygousity of 1.59%, meaning that
both sets of chromosomes are different enough to confuse a genome
assembler. This is important to know as genome assemblers will sometimes
duplicate heterozygous genes in the haploid genome assembly because it
can’t tell the genes are variants.

Platanus-allee assembler
------------------------

`Platanus-allee <http://platanus.bio.titech.ac.jp/platanus2>`__ has
three stages. The first stage is diploid contig assembly. The second
stage is where the assembly is collapsed into a haploid assembly. The
final stage is scaffolding where linkage information is used to stitch
pieces together and then gap filled.

.. code:: bash

   ./platanus_allee assemble \
       -f /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_2.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_2.fastq \
       -o ajap_step10 \
       -s 10 \
       -t 75 \
       -m 360 2>assemble.log

.. code:: bash

   ./platanus_allee phase \
       -o ajap_step10 \
       -c /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64/ajap_step10_contig.fa \
       -IP1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
       -IP2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_2.fastq \
       -IP3 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_2.fastq \
       -OP1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_2.fastq \
       -OP2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_2.fastq \
       -OP3 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_2.fastq \
       -OP4 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_2.fastq \
       -p /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
       -mapper /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64/minimap2-2.0-r191/minimap2 \
       -t 70 2>phase.log

.. code:: bash

   ./platanus_allee consensus \
       -o ajap_step10_consensus \
       -c /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64/ajap_step10_consensusInput.fa \
       -IP1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
       -IP2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_2.fastq \
       -IP3 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_2.fastq \
       -OP1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_2.fastq \
       -OP2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_2.fastq \
       -OP3 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_2.fastq \
       -OP4 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_1.fastq \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_2.fastq \
       -p /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
       -mapper /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64m/inimap2-2.0-r191/minimap2 \
       -t 70 2>consensus.log

Run Time: almost three weeks

.. code:: bash

   Started platanus-allee assemble: 14 Oct 2021
   Started platanus-allee phase: 19 Oct 2021 
   Started platanus-allee consensus: 02 Nov 2021

Memory usage was surprisingly not bad compared to when I have run it
with just short read data. It peaked at 317 gigabytes of ram used. When
running with just illumina data is maxed out my server ram (~400gb) and
crashed. The author stated they thought it was probably a memory leak.
Never figured it out and had to switch to using an older version of
platanus-allee.

MaSuRCA assembler
-----------------

MaSuRCA has a different requirement for running it. It requires a
configuration bash script. The authors provide a template with some
instructions and the user edits the configuration template to suite
their needs. See below for the one I tweaked and used.

`MaSuRCA Configuration Script </masurca_config/>`__

After modifying the config file, you then execute it using bash
``./bin/MaSuRCA MaSuRCA_config_japonicus.txt``. This will output another
shell script file called “assemble.sh”. You’ll do the same to this one:
``./assemble.sh``. And viola - sit back and wait.

Quick notes: - I recently reinstalled ubuntu on my server. So some stuff
was missing when I tried to run MaSuRCA including numactl, boost, and
bzip libraries. - I also didn’t keep track of memory usage, but it
roughly matched what the authors say will be required. - Additionally,
MaSuRCA will use a lot of hard drive space. For this run it was a little
over a terabyte.

`MaSuRCA command line output </masurca_output/>`__

Final stats

.. code:: bash

   N50 811009
   Sequence 632348718
   Average 436404
   E-size 1.20851e+06
   Count 1449

Run time: Started the assembly on Nov 11 and finished on Nov 17 so about
7 days.

Summary Statistics
==================

MaSuRCA
-------

MaSuRCA outputs a few different scaffolds. The final results are in
primary.genome.scf.fasta. Redundant or haplotype variant scaffold
sequences are in alternative.genome.scf.fasta. Then there is
scaffolds.ref.fa, which I am not entirely sure what it is, but I
included it here as I was curious if it was just the two previously
mentioned scaffolds combined.

.. code:: bash

   conda create -n bbmap
   conda activate bbmap
   conda install -c bioconda bbmap 

   # running it on a single genome would use stats.sh
   stats.sh genome_of_interest

   # but I have three genome assemblies, so
   statswrapper.sh \
       in=/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/primary.genome.scf.fasta,\
       /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/scaffolds.ref.fa,\
       /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/alternative.genome.scf.fasta \
       format=6

   #n_scaffolds    n_contigs   scaf_bp contig_bp   gap_pct scaf_N50    scaf_L50    ctg_N50 ctg_L50 scaf_N90    scaf_L90    ctg_N90 ctg_L90 scaf_max    ctg_max scaf_n_gt50K    scaf_pct_gt50K  gc_avg  gc_std  filename
   1449    2535    632348718   632240118   0.017   214 811009  257 647248  737 272952  978 169229  6648447 6586636 1092    99.572  0.37229 0.03548 /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/primary.genome.scf.fasta
   7683    9989    958223014   957992414   0.024   483 466004  599 349301  2778    58960   3831    42594   6648447 6586636 3065    91.631  0.37278 0.03083 /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/scaffolds.ref.fa
   6363    7585    325904099   325781899   0.037   831 111108  1032    86740   3307    21871   4140    18511   692521  692521  1973    76.215  0.37373 0.03116 /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/alternative.genome.scf.fasta

but the columns are off so let’s make it pretty. I pasted the results
into a spreadsheet and then copied/pasted into this websites spreadsheet
to markdown `converting
tool <https://tabletomarkdown.com/convert-spreadsheet-to-markdown/>`__:

+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---------------------------+
| # | n | s | c | g | s | s | c | c | s | s | c | c | s | c | s | s | g | g | filename                  |
| n | _ | c | o | a | c | c | t | t | c | c | t | t | c | t | c | c | c | c |                           |
| _ | c | a | n | p | a | a | g | g | a | a | g | g | a | g | a | a | _ | _ |                           |
| s | o | f | t | _ | f | f | _ | _ | f | f | _ | _ | f | _ | f | f | a | s |                           |
| c | n | _ | i | p | _ | _ | N | L | _ | _ | N | L | _ | m | _ | _ | v | t |                           |
| a | t | b | g | c | N | L | 5 | 5 | N | L | 9 | 9 | m | a | n | p | g | d |                           |
| f | i | p | _ | t | 5 | 5 | 0 | 0 | 9 | 9 | 0 | 0 | a | x | _ | c |   |   |                           |
| f | g |   | b |   | 0 | 0 |   |   | 0 | 0 |   |   | x |   | g | t |   |   |                           |
| o | s |   | p |   |   |   |   |   |   |   |   |   |   |   | t | _ |   |   |                           |
| l |   |   |   |   |   |   |   |   |   |   |   |   |   |   | 5 | g |   |   |                           |
| d |   |   |   |   |   |   |   |   |   |   |   |   |   |   | 0 | t |   |   |                           |
| s |   |   |   |   |   |   |   |   |   |   |   |   |   |   | K | 5 |   |   |                           |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   | 0 |   |   |                           |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   | K |   |   |                           |
+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===========================+
| 1 | 2 | 6 | 6 | 0 | 2 | 8 | 2 | 6 | 7 | 2 | 9 | 1 | 6 | 6 | 1 | 9 | 0 | 0 | /home/j                   |
| 4 | 5 | 3 | 3 | . | 1 | 1 | 5 | 4 | 3 | 7 | 7 | 6 | 6 | 5 | 0 | 9 | . | . | on/Working_Files/japonicu |
| 4 | 3 | 2 | 2 | 0 | 4 | 1 | 7 | 7 | 7 | 2 | 8 | 9 | 4 | 8 | 9 | . | 3 | 0 | s_genome_project/MaSuRCA- |
| 9 | 5 | 3 | 2 | 1 |   | 0 |   | 2 |   | 9 |   | 2 | 8 | 6 | 2 | 5 | 7 | 3 | 4.0.5/CA.mr.67.17.15.0.02 |
|   |   | 4 | 4 | 7 |   | 0 |   | 4 |   | 5 |   | 2 | 4 | 6 |   | 7 | 2 | 5 | /primary.genome.scf.fasta |
|   |   | 8 | 0 |   |   | 9 |   | 8 |   | 2 |   | 9 | 4 | 3 |   | 2 | 2 | 4 |                           |
|   |   | 7 | 1 |   |   |   |   |   |   |   |   |   | 7 | 6 |   |   | 9 | 8 |                           |
|   |   | 1 | 1 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                           |
|   |   | 8 | 8 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                           |
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---------------------------+
| 7 | 9 | 9 | 9 | 0 | 4 | 4 | 5 | 3 | 2 | 5 | 3 | 4 | 6 | 6 | 3 | 9 | 0 | 0 | /home/jon/Working_Files/  |
| 6 | 9 | 5 | 5 | . | 8 | 6 | 9 | 4 | 7 | 8 | 8 | 2 | 6 | 5 | 0 | 1 | . | . | japonicus_genome_project/ |
| 8 | 8 | 8 | 7 | 0 | 3 | 6 | 9 | 9 | 7 | 9 | 3 | 5 | 4 | 8 | 6 | . | 3 | 0 | MaSuRCA-4.0.5/CA.mr.67.17 |
| 3 | 9 | 2 | 9 | 2 |   | 0 |   | 3 | 8 | 6 | 1 | 9 | 8 | 6 | 5 | 6 | 7 | 3 | .15.0.02/scaffolds.ref.fa |
|   |   | 2 | 9 | 4 |   | 0 |   | 0 |   | 0 |   | 4 | 4 | 6 |   | 3 | 2 | 0 |                           |
|   |   | 3 | 2 |   |   | 4 |   | 1 |   |   |   |   | 4 | 3 |   | 1 | 7 | 8 |                           |
|   |   | 0 | 4 |   |   |   |   |   |   |   |   |   | 7 | 6 |   |   | 8 | 3 |                           |
|   |   | 1 | 1 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                           |
|   |   | 4 | 4 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                           |
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---------------------------+
| 6 | 7 | 3 | 3 | 0 | 8 | 1 | 1 | 8 | 3 | 2 | 4 | 1 | 6 | 6 | 1 | 7 | 0 | 0 | /home/jon/W               |
| 3 | 5 | 2 | 2 | . | 3 | 1 | 0 | 6 | 3 | 1 | 1 | 8 | 9 | 9 | 9 | 6 | . | . | orking_Files/japonicus_ge |
| 6 | 8 | 5 | 5 | 0 | 1 | 1 | 3 | 7 | 0 | 8 | 4 | 5 | 2 | 2 | 7 | . | 3 | 0 | nome_project/MaSuRCA-4.0. |
| 3 | 5 | 9 | 7 | 3 |   | 1 | 2 | 4 | 7 | 7 | 0 | 1 | 5 | 5 | 3 | 2 | 7 | 3 | 5/CA.mr.67.17.15.0.02/alt |
|   |   | 0 | 8 | 7 |   | 0 |   | 0 |   | 1 |   | 1 | 2 | 2 |   | 1 | 3 | 1 | ernative.genome.scf.fasta |
|   |   | 4 | 1 |   |   | 8 |   |   |   |   |   |   | 1 | 1 |   | 5 | 7 | 1 |                           |
|   |   | 0 | 8 |   |   |   |   |   |   |   |   |   |   |   |   |   | 3 | 6 |                           |
|   |   | 9 | 9 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                           |
|   |   | 9 | 9 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                           |
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---------------------------+

Note: as previously noted elsewhere, the N50 and L50 are swapped in this
output. I really wish the authors of BBtools would fix this.

So the scaffold count for the first and second entries add up close to
the 2nd entry but it’s not a perfect match so I am hestitant to say the
scaffold.ref.fa assembly is actually just the primary and alternative
genomes merged.

Platanus-allee
--------------

At the end of the consensus step Platanus-allee outputs a scaffold
assembly. See below for how that turned out.

Quickly comparing the MaSuRCA and Platanus-allee assemlbies.

.. code:: bash

   statswrapper.sh \
       in=/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/primary.genome.scf.fasta,\
       /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64/ajap_step10_consensus_consensusScaffold.fa,\
       format=6

+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------------------------------+
| n | n | s | c | g | s | s | c | c | s | s | c | c | s | c | s | s | g | g | filename                       |
| _ | _ | c | o | a | c | c | t | t | c | c | t | t | c | t | c | c | c | c |                                |
| s | c | a | n | p | a | a | g | g | a | a | g | g | a | g | a | a | _ | _ |                                |
| c | o | f | t | _ | f | f | _ | _ | f | f | _ | _ | f | _ | f | f | a | s |                                |
| a | n | _ | i | p | _ | _ | N | L | _ | _ | N | L | _ | m | _ | _ | v | t |                                |
| f | t | b | g | c | N | L | 5 | 5 | N | L | 9 | 9 | m | a | n | p | g | d |                                |
| f | i | p | _ | t | 5 | 5 | 0 | 0 | 9 | 9 | 0 | 0 | a | x | _ | c |   |   |                                |
| o | g |   | b |   | 0 | 0 |   |   | 0 | 0 |   |   | x |   | g | t |   |   |                                |
| l | s |   | p |   |   |   |   |   |   |   |   |   |   |   | t | _ |   |   |                                |
| d |   |   |   |   |   |   |   |   |   |   |   |   |   |   | 5 | g |   |   |                                |
| s |   |   |   |   |   |   |   |   |   |   |   |   |   |   | 0 | t |   |   |                                |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   | K | 5 |   |   |                                |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   | 0 |   |   |                                |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   | K |   |   |                                |
+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+===+================================+
| 1 | 2 | 6 | 6 | 0 | 2 | 8 | 2 | 6 | 7 | 2 | 9 | 1 | 6 | 6 | 1 | 9 | 0 | 0 | /home/jon/Working              |
| 4 | 5 | 3 | 3 | . | 1 | 1 | 5 | 4 | 3 | 7 | 7 | 6 | 6 | 5 | 0 | 9 | . | . | _Files/japonicus_genome_projec |
| 4 | 3 | 2 | 2 | 0 | 4 | 1 | 7 | 7 | 7 | 2 | 8 | 9 | 4 | 8 | 9 | . | 3 | 0 | t/MaSuRCA-4.0.5/CA.mr.67.17.15 |
| 9 | 5 | 3 | 2 | 1 |   | 0 |   | 2 |   | 9 |   | 2 | 8 | 6 | 2 | 5 | 7 | 3 | .0.02/primary.genome.scf.fasta |
|   |   | 4 | 4 | 7 |   | 0 |   | 4 |   | 5 |   | 2 | 4 | 6 |   | 7 | 2 | 5 |                                |
|   |   | 8 | 0 |   |   | 9 |   | 8 |   | 2 |   | 9 | 4 | 3 |   | 2 | 2 | 4 |                                |
|   |   | 7 | 1 |   |   |   |   |   |   |   |   |   | 7 | 6 |   |   | 9 | 8 |                                |
|   |   | 1 | 1 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                                |
|   |   | 8 | 8 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                                |
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------------------------------+
| 3 | 5 | 1 | 8 | 1 | 8 | 1 | 4 | 5 | 5 | 1 | 2 | 8 | 4 | 1 | 1 | 5 | 0 | 0 | /home/jon/Working_Fil          |
| 3 | 2 | 0 | 7 | 3 | 2 | 8 | 2 | 1 | 0 | 8 | 0 | 2 | 1 | 4 | 6 | 7 | . | . | es/japonicus_genome_project/pl |
| 5 | 1 | 0 | 6 | . | 3 | 7 | 7 | 7 | 8 | 4 | 2 | 5 | 6 | 3 | 5 | . | 3 | 0 | atanus-allee/Platanus_allee_v2 |
| 2 | 9 | 7 | 0 | 0 |   | 2 | 3 | 7 | 7 | 5 | 7 |   | 1 | 8 | 4 | 9 | 7 | 7 | .2.2_Linux_x86_64/ajap_step10_ |
| 5 | 1 | 4 | 1 | 4 |   | 0 | 9 |   | 1 |   | 9 |   | 9 | 5 |   | 7 | 5 | 1 | consensus_consensusScaffold.fa |
| 2 | 2 | 1 | 3 | 3 |   | 3 |   |   |   |   | 2 |   | 9 | 9 |   | 7 | 2 | 8 |                                |
|   |   | 2 | 8 |   |   |   |   |   |   |   |   |   | 6 |   |   |   | 9 | 3 |                                |
|   |   | 6 | 3 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                                |
|   |   | 5 | 6 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                                |
|   |   | 8 |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |                                |
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------------------------------+

As I’ve noted previously when using this tool, the L50 and N50 are
switched. So the platanus-allee assembly has over 300k scaffolds, a
scaffold n50 of 187k bp but a contig n50 of ~5k. That’s not great,
especially compared to MaSuRCA assembly. The low n50 might be due to a
few hundred megabases of the assembly being reads shorter than 1kb as
platanus-allee seems to try to keep enough reads so the assembly
represents the whole genome and not just parts that it could assembly.
But that’s just a guess. I really won’t be able to tell until I view
these assemblies using a snailplot.

Summary
=======

So the platanus-allee assembly has a lot more scaffolds/contigs, aka
fragmented, but also contains more data in its assembly. I think this is
because it tries to capture the whole genome in the assembly regardless
of if it is contigious. MaSuRCA removes stuff shorter than a few hundred
base pair or at least doesn’t include very many short reads. Some might
say that mean it loses information. While this is true, that extra
information isn’t very useful (I think?) and will greatly slow down
steps later during genome masking, gene prediction, and annotation.
MaSuRCA was also significantly faster than Platanus-allee.

Even though it seems fairly obvious which assembly is better, a “best
assembly” can’t be declared without further evaluation of more assembly
characteristics. This includes using tools such as Inspector or Mercury
to look at assembly kmer properties, Blobtoolkit for generating graphs
to look at genome contiguity and contamination, and Busco to check for
gene fragmentation.
