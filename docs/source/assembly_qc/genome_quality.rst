--------------

title: “Genome Assembly Quality Assessment using Inspector and Merqury”
permalink: /genome_quality/ layout: single toc: true toc_sticky: true

excerpt: “Good day Sir, I am here to inspect your assembly.”

gallery_merqury: - url:
/assets/images/assembly/assembly_evaluation/c_heheva_merqury_analysis.raven_c_heheva.spectra-cn.fl.png
image_path:
/assets/images/assembly/assembly_evaluation/c_heheva_merqury_analysis.raven_c_heheva.spectra-cn.fl.png
title: “Raven Assembly K-mer Spectra” - url:
/assets/images/assembly/assembly_evaluation/shasta_c_heheva_merqury_analysis.Assembly.spectra-cn.fl.png
image_path:
/assets/images/assembly/assembly_evaluation/shasta_c_heheva_merqury_analysis.Assembly.spectra-cn.fl.png
title: “Shasta Assembly K-mer Spectra” - url:
/assets/images/assembly/assembly_evaluation/flye_c_heheva_merqury_analysis.assembly.spectra-cn.fl.png
image_path:
/assets/images/assembly/assembly_evaluation/flye_c_heheva_merqury_analysis.assembly.spectra-cn.fl.png
title: “Flye Assembly K-mer Spectra” - url:
/assets/images/assembly/assembly_evaluation/og_c_heheva_merqury_analysis.Chiridota_heheva.spectra-cn.fl.png
image_path:
/assets/images/assembly/assembly_evaluation/og_c_heheva_merqury_analysis.Chiridota_heheva.spectra-cn.fl.png
title: “Published Genome Assembly K-mer Spectra”

--------------

Introduction
============

There is a bad habit in the genome assembly world of not checking the
quality of your assembly before publishing it. Part of that is due to a
lack of tools.
`Inspector <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02527-4>`__
and
`Merqury <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9>`__
are two great tools to address this. Inspector uses long reads aligned
to the genome to check quality. Merqury uses highly accurate short reads
to analyze k-mer distribution in the assembly and raw data.

The Assemblies
==============

Three *Chiridota heheva* assemblies previously created using the
assemblers Shasta, Flye, and Raven will be used for comparison. Note
that these assemblies used long read nanopore data that was not error
corrected. Additionally, the assemblies have not been polished. For
reference I have also included the published version of the genome
`downloaded from
NCBI <https://www.ncbi.nlm.nih.gov/datasets/genomes/?taxon=2743191&utm_source=gquery&utm_medium=referral>`__.

Inspector
=========

Installation
------------

Installation is fairly straight forward. Further instructions can be
found at the Inspector
`github <https://github.com/ChongLab/Inspector>`__

.. code:: bash

   conda create -n inspector
   conda activate inspector
   conda install -c bioconda inspector

Running Inspector
-----------------

Running inspector requires a fasta/q file of long reads and a genome
assembly to align the reads too. Additionally you can set an output
directory and even add a reference genome to compare to.

.. code:: bash


   # Raven assembly
   inspector.py \
       -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/raven/raven_c_heheva.fasta \
       -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
       -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/raven \
       --datatype nanopore \
       -t 40 

   # Shasta assembly
   inspector.py \
       -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta/Assembly.fasta \
       -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
       -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/shasta \
       --datatype nanopore \
       -t 40 

   # Flye assembly
   inspector.py \
       -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye/assembly.fasta \
       -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
       -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/flye \
       --datatype nanopore \
       -t 40 

   # Published assembly
   inspector.py \
       -c /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/15302229/Chiridota_heheva.fa \
       -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
       -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/published \
       --datatype nanopore \
       -t 40 

Note: Minimap uses a lot of power and will max out the UPS at 70 cpus
(because m UPS doesn’t provide as much power as the server can use.
Price of being cheap.). This might be a python specific use of minimap.
I do need to get a larger ups at some point

Results
-------

Inspector outputs a text document containing a summary of the results.
Copying and pasting into excel and then into `excel to markdown table
converter website <https://tableconvert.com/excel-to-markdown>`__
generates the below table.

+--------------------------+--------+-----------+-----------+----------+
| Stastics of contigs:     | Pub    | Raven     | Shasta    | Flye     |
|                          | lished | Assembly  | Assembly  | Assembly |
+==========================+========+===========+===========+==========+
| Number of contigs        | 4610   | 3227      | 15199     | 16728    |
+--------------------------+--------+-----------+-----------+----------+
| Number of contigs >      | 3746   | 3223      | 11941     | 13396    |
| 10000 bp                 |        |           |           |          |
+--------------------------+--------+-----------+-----------+----------+
| Number of contigs        | 335    | 207       | 28        | 179      |
| >1000000 bp              |        |           |           |          |
+--------------------------+--------+-----------+-----------+----------+
| Total length             | 1106   | 1         | 1         | 15       |
|                          | 937763 | 221957363 | 209883028 | 40823993 |
+--------------------------+--------+-----------+-----------+----------+
| Total length of contigs  | 1101   | 1         | 1         | 15       |
| > 10000 bp               | 305091 | 221922362 | 196342865 | 21394030 |
+--------------------------+--------+-----------+-----------+----------+
| Total length of contigs  | 626    | 290685476 | 36833854  | 2        |
| >1000000bp               | 006340 |           |           | 67358448 |
+--------------------------+--------+-----------+-----------+----------+
| Longest contig           | 6      | 2901347   | 1748484   | 3605186  |
|                          | 953784 |           |           |          |
+--------------------------+--------+-----------+-----------+----------+
| Second longest contig    | 5      | 2793378   | 1705511   | 3518942  |
| length                   | 776503 |           |           |          |
+--------------------------+--------+-----------+-----------+----------+
| N50                      | 1      | 606319    | 201222    | 307072   |
|                          | 221604 |           |           |          |
+--------------------------+--------+-----------+-----------+----------+
| N50 of contigs >1Mbp     | 1      | 606319    | 201222    | 307072   |
|                          | 221604 |           |           |          |
+--------------------------+--------+-----------+-----------+----------+

The raven assembly is clearly the winner with regards to assembly
contiguity and N50. The shasta and raven assemblies more or less agree
on the size of the genome with Flye having a significantly larger
assembly, which is suspicious. However, Flye also has the first and
second longest contig. With just this information, Raven seems like it
is probably the better assembly and the Flye assembly is suspect. But
let’s take a look at some more information output by Inspector.

+--------------------------+-------+-----------+-----------+----------+
| Read to Contig           | Publ  | Raven     | Shasta    | Flye     |
| alignment:               | ished | Assembly  | Assembly  | Assembly |
+==========================+=======+===========+===========+==========+
| Mapping rate /%          | 98.51 | 98.43     | 98.38     | 98.75    |
+--------------------------+-------+-----------+-----------+----------+
| Split-read rate /%       | 28.97 | 27.4      | 30.49     | 21       |
+--------------------------+-------+-----------+-----------+----------+
| Depth                    | 38    | 34.5491   | 35.2633   | 27.7466  |
|                          | .3654 |           |           |          |
+--------------------------+-------+-----------+-----------+----------+
| Mapping rate in large    | 47.77 | 21.93     | 2.89      | 18.42    |
| contigs /%               |       |           |           |          |
+--------------------------+-------+-----------+-----------+----------+
| Split-read rate in large | 24.5  | 23.79     | 20.73     | 16.53    |
| contigs /%               |       |           |           |          |
+--------------------------+-------+-----------+-----------+----------+
| Depth in large contigs   | 33    | 32.8825   | 34.2379   | 29.7275  |
|                          | .2755 |           |           |          |
+--------------------------+-------+-----------+-----------+----------+

Long read mapping rate was close to the same for all three. Split-read
rate, which I am interpreting as long read sequences that had to be
split in order to map successfully is honestly kinda high for all three.
Flye has the lowest, but 21% seems like a lot of the reads are having to
be split in order to map successfully and they probably shouldn’t need
to be at all. For high quality genomes I would expect that number to be
low. This indicates to me two things: First that splitting might be due
to genome fragmentation, and second that there are a significant number
of errors in the assembly that are causing the reads to not map well,
resulting in needing to split the reads. Split read rate in large
contigs is lower than for the whole assembly and likely has a similar
explanation.

30X coverage is generally considered the minimum for assembling a long
read genome succesfully. All three assemblies are pretty close to this,
although Flye is a bit under probably due to a larger assembly size.
Coverage is higher in large contigs but also similar to overall assembly
coverage. I would view this as a good thing. If coverage was noticably
different in the large contigs relative to the rest of the assembly,
this might suggest higher assembly errors, greater fragmentation in the
rest of the genome, or the genome contains a lot of repetitive elements
that are interferring with assembly.

Mapping rate in large contigs I am assuming means percentage of total
reads that mapped to large contigs. This metric is likely constrained by
how many “large contigs” are in the assembly. So Shasta having fewer
might be a result of not having very many “large contigs”. It could also
be a consequence of there being a lot of assembly errors in the the
shasta assembly.

+---------------+---------+-------------+--------------+-------------+
| Error type    | Pu      | Raven       | Shasta       | Flye        |
|               | blished | Assembly    | Assembly     | Assembly    |
+===============+=========+=============+==============+=============+
| Structural    | 8086    | 4587        | 347          | 729         |
| error         |         |             |              |             |
+---------------+---------+-------------+--------------+-------------+
| Expansion     | 381     | 379         | 220          | 38          |
+---------------+---------+-------------+--------------+-------------+
| Collapse      | 376     | 288         | 48           | 4           |
+---------------+---------+-------------+--------------+-------------+
| Haplotype     | 7323    | 3920        | 79           | 686         |
| switch        |         |             |              |             |
+---------------+---------+-------------+--------------+-------------+
| Inversion     | 6       | 0           | 0            | 1           |
+---------------+---------+-------------+--------------+-------------+

Now for the juicy part. The raven assembly, by a large margin, has the
most large scale errors in the assembly. However, the majority of these
errors are “haplotype switchs”. Once those are removed from the total,
it appears that the raven assembly is not that much worse than the
others, but still worse. These errors can also be easily corrected using
polishing tools such as Hapo-g or the one built into Inspector.

+-----------------------+----------+-----------+-----------+-----------+
| Indels                | P        | Raven     | Shasta    | Flye      |
|                       | ublished | Assembly  | Assembly  | Assembly  |
+=======================+==========+===========+===========+===========+
| Small-scale assembly  | 1650.42  | 2161.13   | 6993.43   | 188.174   |
| error /per Mbp        | 73110638 | 566796317 | 411054656 | 131326123 |
+-----------------------+----------+-----------+-----------+-----------+
| Total small-scale     | 1817624  | 2640740   | 8366545   | 286287    |
| assembly error        |          |           |           |           |
+-----------------------+----------+-----------+-----------+-----------+
| Base substitution     | 344520   | 1240424   | 2714250   | 134786    |
+-----------------------+----------+-----------+-----------+-----------+
| Small-scale expansion | 1418895  | 818823    | 4165783   | 103520    |
+-----------------------+----------+-----------+-----------+-----------+
| Small-scale collapse  | 54209    | 581493    | 1486512   | 47981     |
+-----------------------+----------+-----------+-----------+-----------+
|                       |          |           |           |           |
+-----------------------+----------+-----------+-----------+-----------+
| QV                    | 24.8871  | 25.2858   | 21.3835   | 34.6460   |
|                       | 80495927 | 950332793 | 831443883 | 926406124 |
+-----------------------+----------+-----------+-----------+-----------+

This is probably the most important section regarding genome quality. In
my experience, small scale errors such as insertions, deletions, and
base substitutions significantly impact gene prediction. These small
errors can change the reading frames of the gene which results in gene
truncations or wrong gene models. Keeping that in mind, Flye is the
clear winner with regards to genome quality. This is reflected in the
much higher QV score than the other two. The QV score is a `PHRED
score <https://en.wikipedia.org/wiki/Phred_quality_score>`__ assigned to
the genome which tells you how frequently errors occur in the genome. In
this case a phred score of 20 corresponds to roughly 1 error every 100
bases and a phred score of 30 suggests an error every 1000 bases. For
context, the best human genome has a phred score somewhere around 60 to
70.

To summarize the results: While the raven assembly is longer and more
contigious, it contains a large number of errors whereas the Flye
assembly is more fragmented, suspciously longer, but contains fewer
errors. Before declaring a winning genome assembly though we need to do
more analysis. Next up is Merqury.

Note: need to run this on the original genome too.

Merqury
=======

`Merqury <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9>`__
compares assembly k-mers to k-mers found in the short-read data that was
used in creating the assembly. It is able to estimate assembly accuracy
and completeness in addition to phase block continuity and haplotype
switch errors. It also provides a phred score (QV) that is similar to
the one produced by Inspector.

.. _installation-1:

Installation
------------

Installation is fairly straight forward using conda. Additionally, if
you want to install directly, more instructions can be found at their
`github repository <https://github.com/marbl/merqury>`__

.. code:: bash

   conda create -n merqury
   conda activate merqury
   conda create -n merqury -c conda-forge -c bioconda merqury openjdk=11

Running Merqury
---------------

.. code:: bash

   # this is a simple script to calculate what the ideal k-mer size is based on the estimated genome size
   $MERQURY/best_k.sh 1100000000 

   # results
   genome: 1100000000
   tolerable collision rate: 0.001
   19.9996

   # iterate over the fastq files in that specific folder and create meryl databases
   for file in /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/*.fastq
   do
       # 1. Build meryl dbs
       meryl k=20 count output $file.meryl $file
   done

   # 2. Merge the paired-end fastq meryl databases
   meryl union-sum \
     output c_heheva.rawdata.meryl \
     /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/*.meryl

   # Running Merqury on the genome assemblies

   # Raven assembly
   merqury.sh \
       /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/c_heheva.rawdata.meryl \
       /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/raven/raven_c_heheva.fasta \
       raven_c_heheva_merqury_analysis

   # shasta assembly
   merqury.sh \
       /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/c_heheva.rawdata.meryl \
     /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta/Assembly.fasta \
       shasta_c_heheva_merqury_analysis

   # Flye assembly
   merqury.sh \
       /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/c_heheva.rawdata.meryl \
     /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye/assembly.fasta \
       flye_c_heheva_merqury_analysis

   # Published assembly
   merqury.sh \
       /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/c_heheva.rawdata.meryl \
     /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/15302229/Chiridota_heheva.fa \
       og_c_heheva_merqury_analysis

.. _results-1:

Results
-------

+----------------+-------------------+------------+-------+----------+
| Genome         | Assembly only     | Shared     | QV    | Error    |
| Assembly       | kmers             | Kmers      |       | Rate     |
+================+===================+============+=======+==========+
| Raven          | 187445697         | 1221896050 | 20    | 0.       |
|                |                   |            | .8134 | 00829211 |
+----------------+-------------------+------------+-------+----------+
| Shasta         | 287105937         | 1209594249 | 18    | 0        |
|                |                   |            | .7105 | .0134569 |
+----------------+-------------------+------------+-------+----------+
| Flye           | 139046663         | 1540506161 | 23    | 0.       |
|                |                   |            | .2618 | 00471867 |
+----------------+-------------------+------------+-------+----------+
| Published      | 45930005          | 1106849963 | 26    | 0.       |
| genome         |                   |            | .7431 | 00211684 |
+----------------+-------------------+------------+-------+----------+

Shared kmers are those kmers that are found in both the read dataset and
the genome assembly. Consensus quality (QV) is the Mercury Phred quality
score. The error rate is calculated using presense/absence of k-mers in
the assembly vs raw data. See the Merqury vs Inspector QV scores section
for more info on this is calculated. It looks like shasta has a lower QV
and higher error rate than the Raven assembly. This agrees with the
Inspector results. Although they are a little off from each other.

+-----------------+----------------+------------+---------------------+
| Genome Assembly | Assembly       | Read       | Completeness        |
|                 | K-mers         | K-mers     | Percent             |
+=================+================+============+=====================+
| Raven           | 597225562      | 904653194  | 66.0171             |
+-----------------+----------------+------------+---------------------+
| Shasta          | 545857355      | 904653194  | 60.3389             |
+-----------------+----------------+------------+---------------------+
| Flye            | 702337152      | 904653194  | 77.6361             |
+-----------------+----------------+------------+---------------------+
| Published       | 645834340      | 904653194  | 71.3903             |
| genome          |                |            |                     |
+-----------------+----------------+------------+---------------------+

Per the
`paper <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9#Sec10>`__,
completeness is defined as the “the fraction of reliable k-mers in the
read set that are also found in the assembly. For repetitive genomes,
erroneous read set k-mers can sometimes appear above this threshold due
to recurring errors in high-copy repeat families, but this is rare.”

{% include gallery id=“gallery_merqury” layout=“half” caption=“Merqury
Spectra graphs of the published genome, Raven, Flye, and Shasta
assemblies” %}

Per the `paper <>`__, “A typical k-mer spectrum for a heterozygous
diploid genome consists of two primary peaks, one representing k-mers
that are 1-copy in the diploid genome (heterozygous, on a single
haplotype) and one representing those that are 2-copy in the diploid
genome (homozygous, on both haplotypes or two copies on one haplotype).
The 2-copy k-mers appear with a frequency approximately equal to the
average depth of sequencing coverage, where the 1-copy k-mers appear
with frequency approximately equal to half the sequencing coverage. **If
a genome is entirely homozygous, only the 2-copy peak may appear, and if
the genome is extremely heterozygous, only the 1-copy peak may appear**.
With sufficient sequencing coverage (to separate the peaks along the
axis), and a proper choice of k, both peaks are visible for most
genomes.”

So it appears, not suprisingly, that this sea cucumber genome is highly
hetrozygous. When comparing the graphs, note that the axis scales are
different. I need to see if this can be fixed so the scales are the
same, otherwise it is hard to determine how different the kmer
distribution is between assemblies.

Merqury vs Inspector QV scores
==============================

QV scores are `Phred quality
scores <https://en.wikipedia.org/wiki/Phred_quality_score>`__ assigned
to a genome assembly and are traditionally calculated using

.. math:: \mathrm{QV}=-10{\log}_{10}\ E

Where E is the error rate per a base in the genome assembly or data
being sequenced. Both Inspector and Merqury have different ways of
calculating the error rate in an assembly. Merqury compares the presence
of k-mers in the assembly to those in the raw data. Inspector aligns
long reads to the assembly, identifies and sums the assembly errors
which is then used to create a probability of error per a base. See
below for a more detailed look at the math behind these calculations.

Inspector QV
------------

Inspector calculates the Phred score differently than Merqury. Although
they state is correlates well with Merqury QV results.

.. math:: {N}_{Err}={N}_{Exp}+{N}_{Col}+{N}_{Her}+{N}_{Small}+{n}_{Inv}

.. math:: {N}_{Err}

\ is the sum total of errors (number of bases) inspector found.

.. math:: E=\frac{N_{\mathrm{Err}}}{N_{\mathrm{asm}}}=\frac{N_{\mathrm{Exp}}+{N}_{\mathrm{Col}}+{N}_{\mathrm{Her}}+{N}_{\mathrm{Small}}+{n}_{\mathrm{Inv}}}{N_{\mathrm{asm}}}

.. math:: E

\ or error rate is the number of incorrect bases over the total number
bases in the assembly.

.. math:: \mathrm{QV}=-10{\log}_{10}\ E

Phred scores (QV) are the log of the error rate as shown above. For more
detailed information, check out the methods section of the inspector
`paper <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02527-4#Sec12>`__

Merqury QV
----------

.. math:: \mathrm{QV}=-10{\log}_{10}\ E

As stated earlier, a phred score is the -10log of the error rate as
shown above. The probability that a base is correct is calculated as
shown below where E is

.. math:: E=1-P=1-{\left(1-\frac{K_{\mathrm{asm}}}{K_{\mathrm{total}}}\right)}^{\frac{1}{\mathrm{k}}}

which is 1 minus the probability that a base is correct. The probability
that is a base is correct is calculated as

.. math:: P={\left(\frac{K_{\mathrm{shared}}}{K_{\mathrm{total}}}\right)}^{\frac{1}{\mathrm{k}}}

As noted in the `paper <>`__, “the read set is assumed to completely
cover the genome, any k-mer found only in the assembly

.. math:: {(K_{asm} = K_{total} − K_{shared})}

\ likely reflects a base error in the assembly consensus.” Some basic
math is done to rearrange the equation with

.. math:: {(K_{asm} = K_{total} − K_{shared})}

\ plugged in and viola

.. math:: P={\left(1-\frac{K_{\mathrm{asm}}}{K_{\mathrm{total}}}\right)}^{\frac{1}{\mathrm{k}}}

While Inspector ane Merqury have very different methods of calculating
error rates, the Inspector paper notes they correlate well. Although in
this example, the QV for the assemblies is lower than that calculated
using Inspector, but fairly close. For more information on Merqury
methods see their
`paper <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9#Sec10>`__
