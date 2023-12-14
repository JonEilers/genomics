==========================
Genome Assembly Statistics
==========================


.. contents::
   :depth: 3
..

Introduction
============

Summary statistics are a great way to get a broad sense of genome
assembly quality. They also lack a lot of important information that is
critical to deciding if a genome assembly can be useful or not. However,
as a starting point for evaluating an assembly, they are great.

Most tools such as genometools
`seqstat <http://genometools.org/tools/gt_seqstat.html>`__ and bbduk’s
`stats.sh <https://jgi.doe.gov/data-and-tools/software-tools/bbtools/bb-tools-user-guide/statistics-guide/>`__
will produce numbers arranged in an easy to read format, but they aren’t
great for getting a holistics view of what the assembly actually looks
like. This is where snail plots produced by blobtoolkit really shine.

`Blobtoolkit <https://blobtoolkit.genomehubs.org/>`__ is an amazing
suite of tools for analyzing and producing publication grade figures of
genome assemblies. There
`publication <https://academic.oup.com/g3journal/article/10/4/1361/6026202>`__
contains examples and explanations of the various figures and analysis’
that can be performed. This includes snail plots for visualizing genome
summary statistics, blobplots of contamination, taxonomy hits, and
cumulative assembly span plots of multiple genome assemblies. Below I
have use blobtoolkit to produce snail plots of three sea cucumber genome
assemblies. One is published, the other two are a high and low
confidence masurca assemblies.

Installing Blobtoolkit
======================

There are a number of options for how to install blobtoolkit. There is a
`docker
image <https://blobtoolkit.genomehubs.org/install/use-docker/>`__
available. Additionally, you can install it directly by following these
`instruction <https://blobtoolkit.genomehubs.org/install/>`__, they also
note you can make your life easier by installing most of the
dependencies using conda.

I have used both the docker image and installing directly. Both are a
bit of a pain to do. Thankfully, they introduced an alternative recently
- using `pip <https://pypi.org/project/pip/>`__. pip is a package
installer for python. It is akin to using conda to install stuff. At the
time of writing this I suggest you use pip.

Installing using pip

.. code:: bash

   pip install blobtoolkit

So easy. Love it.

Running Blobtoolkit
===================

Generating snail plots of the genome assemblies

.. code:: bash

   blobtools create \
     --fasta /home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
     --taxid 307972 \
     --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
     /home/jon/Working_Files/blobtoolkit/datasets/ajap

   blobtools create \
     --fasta /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
     --taxid 307972 \
     --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
     /home/jon/Working_Files/blobtoolkit/datasets/ajap-masurca

   blobtools create \
     --fasta /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/scaffolds.ref.fa \
     --taxid 307972 \
     --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
     /home/jon/Working_Files/blobtoolkit/datasets/ajap-masurca-scaffolds.ref

Note: the taxid and taxdump are not required to run the above commands.
However, they will be used later when running other blobtools commands.

viewing the results locally

.. code:: bash

   blobtools view --local /home/jon/Working_Files/blobtoolkit/

This will load an interactive locally host webpage in your browser. This
is a great way to explore the genome assembly and also tweak the graphs
to your liking. It also lets you download the images for use in
publications, or in my case, uploading to websites.

Snail plot Results
==================

I love snail plots for visualizing genome assemblies. It offers much
more information than just looking at numbers such as the N50.
Unfortunately, blobtools isn’t set up to view multiple snail plots side
by side. If they did, I would likely host the interactive version of
these images.

.. image:: /assembly_qc/images/summary_statistics/ajap-masurca-scaffolds.ref.fa.snail.png
   :align: left
   :width: 30%

.. image:: /assembly_qc/images/summary_statistics/ajap-masurca.snail.png
   :align: left
   :width: 30%

.. image:: /assembly_qc/images/summary_statistics/ajap.snail.png
   :align: left
   :width: 30%

{% include gallery caption=“Snails plots of three *Apostichopus
japonicus* genome assemblies. The Far left snail plot is the Published
genome assembly. The middle is the final MaSURCA assembly. The far right
snail plot is the MaSURCA assembly plus lower confidence contigs.” %}

So what do I see here? The first thing is that the published genome has
some gaps in it (denoted by the percent N in the bottom right of the
image). The masurca assembly does not have gaps. The GC content is
pretty similar between all three (again, bottom right corner)

Looking at the top left you’ll see abunch more numbers. Most of those
are self explanatory. What’s cool about those numbers though is they are
easily seen in the image. The red line in the plot is the longest
contig. The grey represents the assembly scaffolds and the “height” or
distance to the center of the circle represents the length of those
scaffolds. There is a scale line in the center of the circle that goes
up. The dark orange represents the scaffolds contained in the N50 metric
and the light orange are the scaffolds in the N90. In highly contigious
assemblies there will usually be less than a hundred scaffolds so the
snail swirl thingy will look very jagged. When there are a lot of
scaffolds the grey line will appear smooth such as in this case.

The outer light and dark blue represent the GC/AT content. In an ideal
assembly that should be fairly consistent - meaning the line between the
two colors shouldn’t be squigglely. However, you’ll notice that near the
end of the assembly there is a little squiggle. This could be because
they are contamination from other organisms or maybe the assemblers have
some bias in assembling reads and the short unassembled ones are
difficult to place? It could also be short reads have more variance in
the GC content and these are the shortest reads. Not sure. Additionally,
if there are a lot of gaps in the assembly they will show up in the
light/dark blue as white. This a good way to check how contigious the
assembly is.

The published assembly is more complete (contains more nucleotides) than
the final masurca assembly, but not as complete as the masurca assembly
containing lower confidence contigs/scaffolds. However, the published
assembly is significantly more contiguous than the masurca assembly
containing lower confidence contigs. It is interesting that the masurca
assembly does have way more gapless scaffolds longer than 1 megabase.
This makes me think the Masurca assemblies are likely of higher quality.
However, I won’t know until I take a look at how many assembly errors
are in it using `Inspector and Merqury </genome_quality/>`__, check for
`contamination </contamination/>`__ using blobtoolkit, and check
`busco <busco/>`__ content.
