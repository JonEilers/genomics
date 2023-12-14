================
Genome Polishing
================


Introduction
============

Genome assembly algorithms are not perfect. Especially long read genome
assemblies that have noisy data. Insertions and deletions (aka indels)
and adapter contamination can sneak their way into the genome assembly.
This can be problem as it can mess up de novo gene prediction resulting
in truncated genes or shifted reading frames. If the naive molecular
geneticist then downloads these genes and tries to use them for wet lab
research they’ll find themselves very frustrated and confused.

But there is an easy way to deal with this problem. It’s called genome
assembly polishing. There are numerous assembly polishing tools out
there now. Some are better than others and there are a number of papers
comparing them. `Racon <https://github.com/isovic/racon>`__ for example
is known to “over polish” assembly and introduce errors if used too
liberally. Others such as
`nextpolish <https://github.com/Nextomics/NextPolish>`__ seems to only
need to be used once and are better for homozygous genomic regions or
genomes, while tools such as
`hapo-g <https://github.com/institut-de-genomique/HAPO-G>`__ are better
suited for heterozygous genomes and require multiple rounds of polishing
to achieve maximum effectiveness. So proceed with care, but definitely
polish your assembly.

Installation of Hapo-g, Inspector, and Merqury
==============================================

Installing Hapo-G

.. code:: bash

   conda create -n hapog
   conda activate hapog
   conda install -c lbgb_cea hapog

Installing Merqury

.. code:: bash

   conda create -n merqury
   conda activate merqury
   conda install -c bioconda merqury

Installling inspector

.. code:: bash

   conda create -n inspector
   conda activate inspector
   conda install -c bioconda inspector

Datasets used for polishing
===========================

Genome assembly: the primary assembly output by Masurca as assemblying
using all the files from the bioproject
`PRJNA413998 <https://www.ncbi.nlm.nih.gov/bioproject/PRJNA413998/>`__

| Reads for polishing were:
| Long reads:
  `SRR6282347 <https://www.ncbi.nlm.nih.gov/sra/SRR6282347>`__
| Short reads:
  `SRR6255867 <https://www.ncbi.nlm.nih.gov/sra/?term=SRR6255867>`__

these can be downloaded using prefetch or fasterq-dump. Both tools are
part of NCBI’s sratoolkit which can be installed using conda.

Polishing Assembly Using Hapo-G
===============================

Note: the hapo-g
`paper <https://academic.oup.com/nargab/article/3/2/lqab034/6262629?login=false>`__
recommends using nextpolish for the first round and then doing 6 more
rounds of hapo-g. I did not do this.

Additionally, while they don’t say it in the paper or on github, long
read assembly polishing should probably only be done with either
polished long reads or highly accurate long reads such as pacbio Hifi.

Polishing Using Long reads (Not HiFi)
-------------------------------------

running hapo-g

.. code:: bash

   # round 1 long reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \   
     --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
     -o round_1_long \              
     -t 36 \                     
     -u                         

   # round 2 long reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_1_long/hapog_results/hapog.fasta \ 
     --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \ 
     -o round_2_long \ 
     -t 36 \ 
     -u   

   # round 3 long reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_2_long/hapog_results/hapog.fasta \ 
     --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \ 
     -o round_3_long \ 
     -t 36 \ 
     -u 

   # round 4 long reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_3_long/hapog_results/hapog.fasta \
     --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
     -o round_4_long \
     -t 36 \
     -u 

   # round 5 long reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_4_long/hapog_results/hapog.fasta \
     --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
     -o round_5_long \
     -t 36 \
     -u 

   # round 6 long reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_5_long/hapog_results/hapog.fasta \
     --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
     -o round_6_long \
     -t 36 \
     -u 

Polishing Using Short Reads
---------------------------

short read

.. code:: bash


   # round 1 short reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
     --pe1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
     --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
     -o round_1_short \
     -t 36 \
     -u  

   # round 2 short reads 
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_1_short/hapog_results/hapog.fasta \
     --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
     --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
     -o round_2_short \
     -t 36 \
     -u 

   # round 3 short reads 
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_2_short/hapog_results/hapog.fasta \
     --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
     --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
     -o round_3_short \
     -t 36 \
     -u

   # round 4 short reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_3_short/hapog_results/hapog.fasta \
     --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
     --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
     -o round_4_short \
     -t 36 \
     -u

   # round 5 short reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_4_short/hapog_results/hapog.fasta \
     --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
     --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
     -o round_5_short \
     -t 36 \
     -u  

   # round 6 short reads
   hapog.py \
     --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_5_short/hapog_results/hapog.fasta \
     --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
     --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
     -o round_6_short \
     -t 36 \
     -u

hapo-g doesn’t provide any result summaries. However, we can run
inspector or mercury on the final results to get a QV and inspector
produces some summary statistics. hapo-g also outputs a summary tsv file
which can be dumped into excel or pandas to generate some graphs of what
those changes look like.

Checking changes
================

running inspector on polished assemblies

.. code:: bash

   conda activate inspector

   # Before polishing masurca assembly
   inspector.py \
       -c /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
       -r /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
       -t 40 \
       --datatype clr 

   # inspector results for long read polishing
   inspector.py \
       -c /home/jon/Working_Files/japonicus_genome_project/hapog/long_read/round_6_long/hapog_results/hapog.fasta \
       -r /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
       -t 40 \
       --datatype clr 

   # inspector results for short read polishing
   inspector.py \
       -c /home/jon/Working_Files/japonicus_genome_project/hapog/short_read/round_6_short/hapog_results/hapog.fasta \
       -r /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
       -t 40 \
       --datatype clr 

running merqury on polished assemblies

.. code:: bash

   conda activate merqury

   # iterate over the fastq files in that specific folder and create meryl databases
   for file in /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/*.fastq
   do
       # 1. Build meryl dbs
       meryl k=20 count output $file.meryl $file
   done

   # 2. Merge the paired-end fastq meryl databases
   meryl union-sum \
     output ajap.rawdata.meryl \
     *.meryl

   # don't pass a folder directory to meryl - it doesn't like them. Only use a file name. So make sure you are in the directory containing the files

   # masurca assembly before polishing
   merqury.sh \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/ajap.rawdata.meryl \
       /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
       ajap_masurca_merqury_analysis

   # masurca assembly after long read polishing
   merqury.sh \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/ajap.rawdata.meryl \
       /home/jon/Working_Files/japonicus_genome_project/hapog/long_read/round_6_long/hapog_results/hapog.fasta \
       ajap_masurca_merqury_analysis


   # masurca assembly after short read polishing
   merqury.sh \
       /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/ajap.rawdata.meryl \
       /home/jon/Working_Files/japonicus_genome_project/hapog/short_read/round_6_short/hapog_results/hapog.fasta \
       ajap_masurca_merqury_analysis

Results
=======

I wasn’t sure what to expect from using merqury/inspector to check for
any improvements to the assembly, but the below results are interesting
to say the least. Before Diving in though, there are some caveats to
mention.

The QV scores produced by Inspector and Merqury are fundamentally
different. If you want more details you can read the papers or take a
peak at the math at the bottom of this `page </genome_quality/>`__ on
checking genome assembly quality. However, as the authors of tools note
- the QV scores do correlate with eachother. That being said, the below
table is interesting.

QV results
----------

========= =================== ============ =============
Tool      Before polishing QV long read QV short read QV
========= =================== ============ =============
Inspector 31.3246             31.3219      30.8105
Merqury   34.4081             34.2974      34.4798
========= =================== ============ =============

Based off of QV alone, the only polishing that “improved” the assembly
at all was using short reads and this was picked up by merqury. The
reason inspector might not show this is due to higher error rate in the
long reads or that the long reads are from a different organism and sea
cucumbers have higher heterozygosity which could result in the long
reads generating more changes in the assembly during polishing or just
not aligning as well when mapped by inspector. However, the long reads
were used in generating the assembly, so I dunno. Kinda weird though.

The QV score is OK, not ideal, but usable. QV (or `phred
score <https://en.wikipedia.org/wiki/Phred_quality_score>`__) of 30
means there is an error in the assemble every 1000 base pairs or that
the assembly is 99.9% accurate.

While I was hoping the QV score would change more than it did, I
shouldn’t be surprised as polishing is fixing small errors such as base
modifications, insertions, and deletion (aka indels). I am surprised
that both merqury and inspector show a slight decrease in QV with six
rounds of polishing using Hapo-g with the exception being short read
polishing. Although the author do state not to use long reads with a lot
of errors. This makes me think a round of polishing the long reads using
something like `consent <https://github.com/morispi/CONSENT>`__ before
using error prone reads with hapo-g might be the thing to do.

Inspector contig statistics
---------------------------

+------------------+---------------+----------------+-----------------+
| Statistics of    | before        | After Long     | After short     |
| contigs:         | polishing     | read Polishing | read polishing  |
|                  | (inspector)   | (6x)           | (6x)            |
+==================+===============+================+=================+
| Number of        | 1449          | 1449           | 1449            |
| contigs          |               |                |                 |
+------------------+---------------+----------------+-----------------+
| Number of        | 1173          | 1173           | 1173            |
| contigs > 10000  |               |                |                 |
| bp               |               |                |                 |
+------------------+---------------+----------------+-----------------+
| Number of        | 145           | 145            | 145             |
| contigs >1000000 |               |                |                 |
| bp               |               |                |                 |
+------------------+---------------+----------------+-----------------+
| Total length     | 632348718     | 632337298      | 632186223       |
+------------------+---------------+----------------+-----------------+
| Total length of  | 631958137     | 631946805      | 631796193       |
| contigs > 10000  |               |                |                 |
| bp               |               |                |                 |
+------------------+---------------+----------------+-----------------+
| Total length of  | 254446844     | 254442981      | 254378934       |
| contigs          |               |                |                 |
| >1000000bp       |               |                |                 |
+------------------+---------------+----------------+-----------------+
| Longest contig   | 6648447       | 6648345        | 6646963         |
+------------------+---------------+----------------+-----------------+
| Second longest   | 5259394       | 5259332        | 5258815         |
| contig length    |               |                |                 |
+------------------+---------------+----------------+-----------------+
| N50              | 811009        | 811002         | 810645          |
+------------------+---------------+----------------+-----------------+
| N50 of contigs   | 811009        | 811002         | 810645          |
| >1Mbp            |               |                |                 |
+------------------+---------------+----------------+-----------------+

Huh, the take away from this table is that polishing removed a fair
amount of reads. Short read polishing more so than long read polishing.
Enough was removed to impact the N50. I guess there were a lot of
artifacts in the assembly.

Inspector alignment statistics
------------------------------

+-------------------+---------------+----------------+-----------------+
| Read to Contig    | before        | After Long     | After short     |
| alignment:        | polishing     | read Polishing | read polishing  |
|                   | (inspector)   | (6x)           | (6x)            |
+===================+===============+================+=================+
| Mapping rate /%   | 93            | 93             | 93.14           |
+-------------------+---------------+----------------+-----------------+
| Split-read rate   | 30.24         | 30.26          | 30.44           |
| /%                |               |                |                 |
+-------------------+---------------+----------------+-----------------+
| Depth             | 37.9629       | 37.9649        | 38.0062         |
+-------------------+---------------+----------------+-----------------+
| Mapping rate in   | 36.99         | 36.99          | 37.03           |
| large contigs /%  |               |                |                 |
+-------------------+---------------+----------------+-----------------+
| Split-read rate   | 29.06         | 29.07          | 29.21           |
| in large contigs  |               |                |                 |
| /%                |               |                |                 |
+-------------------+---------------+----------------+-----------------+
| Depth in large    | 37.5496       | 37.5516        | 37.5809         |
| conigs            |               |                |                 |
+-------------------+---------------+----------------+-----------------+

It appears that short read polishing wins in the of the categories from
this table. Split reads happen the minimap splits the read into two
pieces in order to get it to align to the assembly correctly. I am
unsure what the implications are, normally this would be a good thing in
rna-seq mapping, but a higher split read rate for long read mapping
suggests to me that there are more insertions that are messing up the
alignment. Otherwise mapping rate is higher in short reads, depth is a
little higher (remember this is long reads mapped to an assembly
polished with short reads).

Inspector structural changes
----------------------------

+-----------+-----------------+-------------------+-------------------+
| S         | before          | After Long read   | After short read  |
| tructural | polishing       | Polishing (6x)    | polishing (6x)    |
| changes   | (inspector)     |                   |                   |
+===========+=================+===================+===================+
| S         | 562             | 557               | 595               |
| tructural |                 |                   |                   |
| error     |                 |                   |                   |
+-----------+-----------------+-------------------+-------------------+
| Expansion | 243             | 241               | 265               |
+-----------+-----------------+-------------------+-------------------+
| Collapse  | 119             | 116               | 122               |
+-----------+-----------------+-------------------+-------------------+
| Haplotype | 199             | 199               | 208               |
| switch    |                 |                   |                   |
+-----------+-----------------+-------------------+-------------------+
| Inversion | 1               | 1                 | 0                 |
+-----------+-----------------+-------------------+-------------------+

This table I am also not sure what to make of. Short read polishing made
the structural changes “worse”, with the exception of removing the
inversion. I don’t know if this is an artifact of using error prone long
reads with inspector or if short reads were unable to resolve some of
these problems due to their shortness.

Inspector Indels
----------------

+-------------------+---------------+----------------+-----------------+
| Indel differences | before        | After Long     | After short     |
|                   | polishing     | read Polishing | read polishing  |
|                   | (inspector)   | (6x)           | (6x)            |
+===================+===============+================+=================+
| Small-scale       | 55            | 5              | 610.71434787832 |
| assembly error    | 7.96733890302 | 59.45215198928 |                 |
| /per Mbp          |               |                |                 |
+-------------------+---------------+----------------+-----------------+
| Total small-scale | 352612        | 353544         | 385847          |
| assembly error    |               |                |                 |
+-------------------+---------------+----------------+-----------------+
| Base substitution | 282836        | 283402         | 325045          |
+-------------------+---------------+----------------+-----------------+
| Small-scale       | 39465         | 39378          | 35148           |
| expansion         |               |                |                 |
+-------------------+---------------+----------------+-----------------+
| Small-scale       | 30311         | 30764          | 25654           |
| collapse          |               |                |                 |
+-------------------+---------------+----------------+-----------------+

Short read polishing was able to decrease the number of small scale
collapse and expansions which is awesome. However, polishing with both
long and short read data appparently increased the total number of small
scale assembly errors and base substitutions. I would take this with a
grain of salt again as inspector was run with error prone pacbio reads.
The base substitution could be chalked up to more accurate illumina
reads correcting mistakes introduced by long reads during assembly.

Summary
=======

Polishing is a critical step in removing assembly errors. Polishing
tools are getting better and providing more feedback in the process such
as QV scores and information regarding what is being corrected. However,
it is difficult to interpret the results. When I see that there are
still “25,654” small-scale collapses in the assembly after 6 rounds of
polishing it makes me think I need to do more polishing.

The reality might be different though and I won’t know until further
analysis is done. Hapo-g, Inspector, and Mercury all output tsv files
containing changes or errors that were found. These files could be
loaded into pandas or excel and graphed. This would likely be a more
informative way of looking at the changes.

Additionally, the next steps in assessing the quality of the assembly is
to map rna-seq data, identify repetitive elements, and generate gene
models. The results of this can then be visualized and it will become
rapidly obvious if gene models are not matching gene expression data.
This is a time intenstive process though and it behoves the
bioinformatician to do what they can to insure a high quality assembly
before getting to that point.
