Genome Assembly
===============

.. _Genome Assembly:

.. warning:: This page is currently under construction with new content and mistakes added regularly.

Introduction
----------

Genome Assembly is `hard<https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2005894>`. Even now with long read sequencing, [there is still a long ways to go](https://www.nature.com/articles/s41592-021-01057-y). In 1999, the human genome sequence was announced. In 2021, a telomere to telomere genome assembly, with no gaps, was announced. Even with the advances in technology and reduction in cost, many genomes cannot realistically be sequenced and assembled without a significant investment. For example, most salamander and newt species have genomes that are estimated to be 100 gigabase. The largest genome sequenced so far was the [lungfish](https://www.nature.com/articles/s41586-021-03198-8?#Sec2), with a genome size of 43 gigabases. Lungfish have a diploid genome, which is fairly normal for a large animal. But plants and many other organisms are not diploid and require extra caution when assembling. One notable example is [wheat](https://www.sciencedirect.com/science/article/pii/S1672022920300590) which has chromosomes from three different genomes and is an [allohexaploid](https://www.nature.com/articles/s41586-020-2961-x0), meaning it has six sets of two chromsomes. Most genome assembler would not know what to do with this and would likely collapse the three sets in a psuedohaploid assembly, loosing a great deal of information in the process. 

Genome assembly approaches largely depend on the type and quality of data you have available to you. Sanger sequencing used to be the golden standard. Then Illumina shotgun sequencing took over for awhile. Now we have long read sequencing using either PacBio or Oxford Nanopore Technologies platforms. Additional wet lab techniques such as [Hi-C](https://en.wikipedia.org/wiki/Chromosome_conformation_capture#Hi-C_(all-vs-all)) or [optical mapping](https://en.wikipedia.org/wiki/Optical_mapping) can further bring the assembly pieces together into chromosome length scaffolds. Any combination of the above can be found in most current genome assembly papers. However, if Illumina is all you have and genomes of closely related species are available, then reference assisted genome assembly is a possibility, but with caveats. 

.. note:: `Mind the Gap <https://www.sciencedirect.com/science/article/abs/pii/S1087184515300220>`_ reasons to produce gapless genome assemblies
.. note:: `Errors in long-read assemblies can critically affect protein prediction <https://www.nature.com/articles/s41587-018-0004-z>`_ for why it is critical to check assembly quality and fix errors
.. note:: `A comprehensive review of scaffolding methods in genome assembly <https://academic.oup.com/bib/article-abstract/22/5/bbab033/6149347?redirectedFrom=fulltext>`_ on ways to improve assemblies scaffolding

Assembly
--------

Short Read
----------

[Details about Illumina sequencing...]

.. note:: `Short read assembly using ABySS and SPAdes <short-read-assembly/>`_

[Description of Platanus-Allee and MaSuRCA...]

.. note:: Short-read assembly using `Platanus-Allee <platanus-allee/>` and `MaSuRCA <masurca/>`_

Long Read
---------

[Details about long-read sequencing technology...]

.. note:: `Long read assembly of Chiridota heheva using Raven, Flye, and Shasta assemblers <longread_genome_assembly/>`_

Hybrid
------

Hybrid genome assembly is the predominant approach these days. [Further explanation...]

.. note:: `Hybrid genome assembly of Apostichopus japonicus using Platanus-allee and MaSURCA <hybrid_genome_assembly/>`_
.. warning:: To-do: Using Hi-C data to achieve chromosome scale scaffolds using salsa, yahs, 3d-dna and hirise for the Stichopus chloronotus genome

Assembly QC
===========

[Description of assembly quality control...]

Summary Statistics
-------------------

[Description about the importance of N50 and other metrics...]

.. note:: `Summary statistics via Blobtoolkit <summary-stats/>`_

Assembly QC with BUSCOs
------------------------

[Details about using BUSCOs for assembly quality assessment...]

.. note:: `Assembly quality assessment using BUSCO analysis <busco/>`_

Assembly Contamination and Quality
----------------------------------

[Discussion about checking for contamination in genome assembly...]

.. note:: `Assembly contamination and quality via Blobtoolkit <contamination/>`_

Assembly Evaluation using K-mers and Long Reads
-----------------------------------------------

[Information on using K-mers and long reads for assembly evaluation...]

.. note:: `Assembly Quality Assessment using Inspector and Merqury <genome_quality/>`_

Polishing and Gap Closing
-------------------------

[Description about the importance and methods of gap closing and polishing...]

Gap Closing
-----------

[Details about gap closing methods...]

.. warning:: To-do: Gap closing using Dentist, SAMBA and Longstitch

Polishing
---------

[Explanation about the polishing process...]

.. note:: `Genome assembly polishing using hapo-g <genome_polishing/>`_

Manual Assembly Correction
--------------------------

[Information about manual assembly correction...]

.. note:: `Significantly improving the quality of genome assemblies through curation <https://academic.oup.com/gigascience/article/10/1/giaa153/6072294>`_

.. note:: Hmmm, maybe play with `AGB <https://github.com/almiheenko/AGB>`_ for visualizing long read assembly?

.. note:: Note to self: Consider future genome analysis and reassembly for `Australostichopus mollis <https://www.ncbi.nlm.nih.gov/bioproject/PRJEB10682/>`, `Apostichopus leukothele <https://www.ncbi.nlm.nih.gov/sra/SRX8086344[accn]>`, and `Actinopyga echinites <https://www.ncbi.nlm.nih.gov/assembly/GCA_010015985.1#/st>` genomes.

